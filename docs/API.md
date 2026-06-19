# API - Interface com Flight-Computer

> Especificação da interface entre o sistema de recuperação e o computador de bordo

[← Voltar ao índice](../README.md)

---

## Visão Geral

O sistema de recuperação é controlado pelo **flight-computer** via sinal PWM. Este documento define a interface elétrica e lógica entre os dois sistemas.

```
┌─────────────────┐     PWM     ┌─────────────────┐
│                 │ ──────────→ │                 │
│ Flight-Computer │             │ Recovery        │
│                 │ ←───────── │                 │
└─────────────────┘   Status    └─────────────────┘
     (outro repo)                (este repo)
```

---

## Interface Elétrica

### Conexão

```
Flight-Computer                    Recovery
┌──────────────┐                   ┌──────────────┐
│              │                   │              │
│  Pino PWM  ──┼───────────────────┼── Sinal      │
│  5V        ──┼───────────────────┼── VCC        │
│  GND       ──┼───────────────────┼── GND        │
│              │                   │              │
└──────────────┘                   └──────────────┘

Conector: JST-XH 3 pinos
Fio: 22AWG silicone
Comprimento: 10-30cm
```

### Pinagem do Conector JST-XH

```
┌─────────────────┐
│  1. Sinal (PWM) │ ← Marrom ou Laranja
│  2. VCC (5V)    │ ← Vermelho
│  3. GND         │ ← Preto
└─────────────────┘
```

### Especificações Elétricas

| Parâmetro | Valor | Observação |
|-----------|-------|------------|
| Tensão de alimentação | 5V ± 0.5V | Do flight-computer ou bateria dedicada |
| Corrente máxima | 500mA | Durante movimento do servo |
| Sinal PWM | 3.3V ou 5V | Lógica compatível com ESP32/Arduino |
| Frequência PWM | 50Hz | Padrão para servos |
| Largura de pulso | 500-2500μs | Posição do servo |

---

## Protocolo de Comunicação

### Sinal PWM

O flight-computer controla o servo através de **sinal PWM** (Pulse Width Modulation):

```
Posição do servo = f(largura do pulso)

┌─────────────────────────────────────────────┐
│           MAPEAMENTO DO SINAL               │
├─────────────────────────────────────────────┤
│                                              │
│  500μs  ──── Posição 0°   (fechado)         │
│  1000μs ──── Posição 45°                    │
│  1500μs ──── Posição 90°  (neutro)          │
│  2000μs ──── Posição 135°                   │
│  2500μs ──── Posição 180° (aberto)          │
│                                              │
│  Para recuperação:                           │
│  - Fechado: 500-1000μs (pino retém NC)      │
│  - Aberto: 2000-2500μs (pino recua)         │
│                                              │
└─────────────────────────────────────────────┘
```

### Sequência de Operação

```
┌─────────────────────────────────────────────┐
│         SEQUÊNCIA DE DEPLOY                  │
├─────────────────────────────────────────────┤
│                                              │
│  1. Voo ascendente                           │
│     → Servo em posição FECHADA (700μs)      │
│     → Pino retém NC                          │
│                                              │
│  2. Apogeu detectado                         │
│     → Flight-computer calcula Δh ≈ 0        │
│     → Velocidade vertical ≈ 0               │
│                                              │
│  3. Comando de deploy                        │
│     → Flight-computer envia 2300μs          │
│     → Servo gira para posição ABERTA        │
│     → Pino recua                            │
│                                              │
│  4. Liberação                               │
│     → Mola empurra NC                        │
│     → NC se separa do tubo                   │
│     → Paraquedas exposto ao fluxo de ar      │
│                                              │
│  5. Inflação                                │
│     → Paraquedas infla                       │
│     → Descida controlada (~5 m/s)           │
│                                              │
│  6. (Opcional) Reset                        │
│     → Servo volta para FECHADA (700μs)      │
│     → Pronto para novo voo                   │
│                                              │
└─────────────────────────────────────────────┘
```

---

## Exemplo de Código (Flight-Computer)

### Arduino / ESP32

```cpp
// recovery.h
#ifndef RECOVERY_H
#define RECOVERY_H

#include <Servo.h>

// Pinos
#define RECOVERY_SERVO_PIN 16  // Pino PWM para servo

// Posições do servo (em microssegundos)
#define SERVO_CLOSED  700   // Posição fechada (pino retém NC)
#define SERVO_OPEN    2300  // Posição aberta (pino recua)

// Tempo mínimo entre deploys (ms)
#define DEPLOY_COOLDOWN 5000

class Recovery {
public:
    void begin();
    void arm();
    void deploy();
    void reset();
    bool isDeployed();
    
private:
    Servo _servo;
    bool _deployed = false;
    unsigned long _lastDeployTime = 0;
};

#endif
```

```cpp
// recovery.cpp
#include "recovery.h"

void Recovery::begin() {
    _servo.attach(RECOVERY_SERVO_PIN);
    _servo.writeMicroseconds(SERVO_CLOSED);
    _deployed = false;
}

void Recovery::arm() {
    // Garante que servo está na posição fechada
    _servo.writeMicroseconds(SERVO_CLOSED);
    _deployed = false;
}

void Recovery::deploy() {
    // Proteção contra deploy múltiplo
    if (millis() - _lastDeployTime < DEPLOY_COOLDOWN) {
        return;
    }
    
    // Aciona deploy
    _servo.writeMicroseconds(SERVO_OPEN);
    _deployed = true;
    _lastDeployTime = millis();
}

void Recovery::reset() {
    // Reseta para posição fechada (para novo voo)
    _servo.writeMicroseconds(SERVO_CLOSED);
    _deployed = false;
}

bool Recovery::isDeployed() {
    return _deployed;
}
```

### Uso no Flight-Computer

```cpp
// main.cpp (exemplo simplificado)
#include "recovery.h"

Recovery recovery;

void setup() {
    Serial.begin(115200);
    recovery.begin();
    recovery.arm();
    
    Serial.println("Sistema de recuperação armado");
}

void loop() {
    // ... leitura de sensores ...
    
    // Detecção de apogeu (exemplo simplificado)
    if (isApogeeDetected()) {
        Serial.println("APOGEU DETECTADO - DEPLOY!");
        recovery.deploy();
    }
    
    // ... telemetria e logging ...
}
```

---

## Detecção de Apogeu

### Critérios de Detecção

O flight-computer deve detectar o apogeu usando **múltiplos critérios** para evitar falso positivo:

```
┌─────────────────────────────────────────────┐
│         CRITÉRIOS DE APOGEU                  │
├─────────────────────────────────────────────┤
│                                              │
│  1. Barômetro (primário)                     │
│     → Δaltitude < 0 por N amostras          │
│     → Velocidade vertical ≈ 0               │
│     → Altitude máxima atingida              │
│                                              │
│  2. IMU (secundário)                         │
│     → Aceleração vertical ≈ 0               │
│     → (descontando gravidade)               │
│                                              │
│  3. Timeout (fallback)                       │
│     → Tempo máximo de voo atingido          │
│     → Deploy de segurança                   │
│                                              │
│  Combinação:                                 │
│     (Barômetro) AND (IMU OR Timeout)        │
│                                              │
└─────────────────────────────────────────────┘
```

### Exemplo de Algoritmo

```cpp
// apogee_detection.cpp
bool isApogeeDetected() {
    static float lastAltitude = 0;
    static int descendCount = 0;
    
    float currentAltitude = readBarometer();
    float velocity = currentAltitude - lastAltitude;
    
    // Critério 1: Descida detectada
    if (velocity < -1.0) {  // Descendo > 1m/amostra
        descendCount++;
    } else {
        descendCount = 0;
    }
    
    // Critério 2: Descida consistente
    if (descendCount >= 5) {  // 5 amostras consecutivas
        return true;
    }
    
    // Critério 3: Timeout de segurança
    if (millis() - launchTime > MAX_FLIGHT_TIME) {
        return true;
    }
    
    lastAltitude = currentAltitude;
    return false;
}
```

---

## Testes de Integração

### Teste em Bancada

```bash
# 1. Conectar flight-computer ao recovery
# 2. Alimentar com 5V
# 3. Simular detecção de apogeu:
#    - Alterar pressão no sensor (seringa)
#    - Ou enviar comando via serial
# 4. Verificar:
#    - [ ] Servo se move na posição correta
#    - [ ] Pino recua completamente
#    - [ ] NC se solta
#    - [ ] Tempo de deploy < 500ms
```

### Teste de Integração Completo

```bash
# 1. Montar sistema completo no foguete
# 2. Ligar flight-computer
# 3. Verificar status via telemetria:
#    - [ ] Recovery armado
#    - [ ] Servo na posição fechada
#    - [ ] Sensor de apogeu funcionando
# 4. Simular voo:
#    - Subir foguete (fio/polia)
#    - Soltar para simular apogeu
#    - Verificar deploy automático
```

---

## Troubleshooting de Interface

### Servo não responde ao flight-computer

```bash
# Verificar:
1. Conexão física (continuidade)
2. Pino PWM correto no firmware
3. Alimentação 5V presente
4. Sinal PWM com osciloscópio/LED
5. Servo funciona com Arduino direto?
```

### Falso positivo (deploy prematuro)

```bash
# Verificar:
1. Lógica de detecção de apogeu
2. Ruído no barômetro (filtragem)
3. Vibração afetando IMU
4. Threshold muito sensível
```

### Deploy não acontece

```bash
# Verificar:
1. Logs do flight-computer
2. Sensor de apogeu funcionou?
3. Sinal PWM enviado?
4. Servo recebeu sinal?
5. Mecanismo travou?
```

---

## Referências

- [Guia de Montagem](./INSTALACAO.md)
- [Calibração](./CALIBRACAO.md)
- [Troubleshooting](./TROUBLESHOOTING.md)
- [Flight-Computer (outro repo)](https://github.com/Serra-Rocketry/flight-computer)

---

[← Voltar ao índice](../README.md)
