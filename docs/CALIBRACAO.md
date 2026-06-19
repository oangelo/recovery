# Calibração - Sistema de Recuperação

> Procedimentos de calibração e ajuste fino do mecanismo de recuperação

[← Voltar ao índice](../README.md)

---

## Objetivo

Calibrar o sistema de recuperação garante que:
- Servo se move com precisão
- Mola tem força adequada
- Deploy é consistente e confiável
- Consumo de energia é otimizado

---

## Pré-requisitos

### Equipamento
- Multímetro (corrente e tensão)
- Fonte de alimentação 5V (ou bateria)
- Arduino ou flight-computer para gerar sinal PWM
- Régua ou paquímetro
- Dinamômetro (opcional, mas recomendado)
- Cronômetro ou osciloscópio

### Software
- Arduino IDE ou PlatformIO
- Serial monitor configurado (115200 baud)

---

## Calibração do Servo

### Passo 1: Encontrar Limites do Servo

Cada servo tem limites ligeiramente diferentes. Calibre os limites:

```cpp
// servo_calibration.ino
#include <Servo.h>

Servo servo;
int pos = 500;  // Começa em 500μs

void setup() {
    Serial.begin(115200);
    servo.attach(16);  // Pino do servo
    
    Serial.println("=== CALIBRAÇÃO DO SERVO ===");
    Serial.println("Use 'a' e 'd' para ajustar posição");
    Serial.println("Use 's' para salvar posição atual");
    Serial.println();
}

void loop() {
    if (Serial.available()) {
        char cmd = Serial.read();
        
        switch (cmd) {
            case 'a':  // Diminuir (fechar)
                pos -= 10;
                if (pos < 500) pos = 500;
                break;
            case 'd':  // Aumentar (abrir)
                pos += 10;
                if (pos > 2500) pos = 2500;
                break;
            case 's':  // Salvar
                Serial.print("Posição salva: ");
                Serial.println(pos);
                break;
        }
        
        servo.writeMicroseconds(pos);
        Serial.print("Posição: ");
        Serial.print(pos);
        Serial.println(" μs");
    }
}
```

### Passo 2: Determinar Posições de Trabalho

```
1. Execute o sketch de calibração
2. Encontre a posição FECHADA:
   - Decremente até o pino retém o NC
   - Anote o valor (ex: 700μs)
   - Decremente mais 20μs como margem

3. Encontre a posição ABERTA:
   - Incremente até o pino recua completamente
   - Anote o valor (ex: 2300μs)
   - Incremente mais 20μs como margem

4. Teste repetidamente:
   - Alterne entre as posições 10 vezes
   - Verifique consistência
   - Ajuste se necessário
```

### Passo 3: Registrar Valores

```markdown
## Calibração do Servo

Data: YYYY-MM-DD
Modelo: SG90 / MG90S / MG996R

Posição FECHADA: ___μs (recomendado: 600-800μs)
Posição ABERTA: ___μs (recomendado: 2200-2400μs)

Teste de consistência (10 ciclos):
- [ ] Todas as vezes fechou corretamente
- [ ] Todas as vezes abriu corretamente
- [ ] Sem ruídos estranhos
- [ ] Movimento suave

Corrente medida:
- Repouso: ___mA
- Movimento: ___mA
- Stall: ___mA
```

---

## Calibração da Mola

### Passo 1: Medir Força da Mola

```
1. Comprimento livre (sem carga): ___mm
2. Comprimento comprimido (dentro do mecanismo): ___mm
3. Compressão: ___mm

4. Medir força (com dinamômetro):
   - Comprima a mola ao comprimento de trabalho
   - Meça a força necessária
   - Anote: ___N
```

### Passo 2: Verificar Compatibilidade com Servo

```
Torque do servo: ___kgf·cm
Raio do horn: ___cm
Força máxima do servo: ___N (torque / raio)

Força da mola: ___N

Margem de segurança: ___% (deve ser > 50%)

Se força da mola > força do servo:
→ Troque mola mais fraca
→ Ou use servo com mais torque
```

### Passo 3: Ajustar Comprimento

```
Se mola muito forte:
→ Corte espirais (com cuidado)
→ Ou troque por mola mais fraca

Se mola muito fraca:
→ Adicione espirais (se possível)
→ Ou troque por mola mais forte
```

---

## Calibração do Paraquedas

### Passo 1: Verificar Tamanho

```
Massa do foguete: ___g = ___kg
Velocidade desejada: 5 m/s

Área necessária = (2 × massa × g) / (ρ × Cd × V²)
Área = (2 × ___ × 9.81) / (1.225 × 1.5 × 5²)
Área = ___m²

Diâmetro = 2 × √(Área / π)
Diâmetro = ___m = ___cm

Diâmetro do paraquedas: ___cm
Status: [ ] Adequado [ ] Muito pequeno [ ] Muito grande
```

### Passo 2: Teste de Inflação

```
1. Dobre o paraquedas conforme procedimento
2. Solte de 2 metros de altura
3. Verifique:
   - [ ] Paraquedas se infla em < 1 segundo
   - [ ] Inflação completa
   - [ ] Sem torções
   - [ ] Descida estável

4. Repita 5 vezes
5. Se falhar > 1x, ajuste dobra ou tamanho
```

---

## Calibração do Mecanismo Completo

### Teste de Deploy

```
1. Monte o mecanismo completo no foguete
2. Conecte ao flight-computer
3. Execute 10 deploys consecutivos
4. Meça para cada deploy:
   - Tempo de resposta (ms)
   - Corrente consumida (mA)
   - Sucesso: [ ] Sim [ ] Não

5. Calcule:
   - Tempo médio: ___ms
   - Corrente média: ___mA
   - Taxa de sucesso: ___%
```

### Teste de Vibração

```
1. Ligue o sistema
2. Posicione servo em FECHADO
3. Aplique vibração (furadeira/subwoofer) por 5 minutos
4. Verifique:
   - [ ] Servo não mudou de posição
   - [ ] NC continua retido
   - [ ] Sem ruídos estranhos
   - [ ] Corrente estável
```

### Teste de Temperatura

```
1. Teste em temperatura ambiente (~25°C)
2. Teste em frio (freezer, -10°C, 30 min)
3. Teste em calor (sol direto, ~50°C, 30 min)
4. Para cada temperatura, verifique:
   - [ ] Servo responde
   - [ ] Mola funciona
   - [ ] Deploy funciona
   - [ ] Corrente dentro do esperado
```

---

## Registro de Calibração

### Template

```markdown
# Registro de Calibração - Recovery

Data: YYYY-MM-DD
Responsável: Nome

## Servo
- Modelo: ___
- Posição FECHADA: ___μs
- Posição ABERTA: ___μs
- Corrente (repouso): ___mA
- Corrente (movimento): ___mA
- Corrente (stall): ___mA

## Mola
- Comprimento livre: ___mm
- Comprimento comprimido: ___mm
- Força: ___N
- Compatível com servo: [ ] Sim [ ] Não

## Paraquedas
- Massa do foguete: ___g
- Diâmetro: ___cm
- Velocidade de descida: ___m/s
- Inflação OK: [ ] Sim [ ] Não

## Mecanismo Completo
- Tempo médio de deploy: ___ms
- Corrente média: ___mA
- Taxa de sucesso: ___%
- Teste de vibração: [ ] Passou [ ] Falhou
- Teste de temperatura: [ ] Passou [ ] Falhou

## Notas
- ___
```

---

## Recalibração

### Quando Recalibrar

Recalibre o sistema quando:
- Substituir componente (servo, mola, NC)
- Após falha em campo
- Após teste de vibração/temperatura
- Antes de cada competição
- A cada 10 voos

### Procedimento de Recalibração

```
1. Repita todos os passos de calibração
2. Compare com registro anterior
3. Se valores mudaram > 10%:
   - Investigar causa
   - Substituir componente se necessário
4. Atualize registro
```

---

## Troubleshooting de Calibração

### Servo não atinge posição desejada

```
Causas possíveis:
- Curso mecânico limitado
- Atrito excessivo
- Mola forte demais

Soluções:
- Verificar limites mecânicos
- Lubrificar mecanismo
- Trocar mola mais fraca
```

### Valores inconsistentes

```
Causas possíveis:
- Conexão frouxa
- Alimentação instável
- Servo com defeito

Soluções:
- Verificar conexões
- Usar fonte estável
- Trocar servo
```

### Corrente muito alta

```
Causas possíveis:
- Mola forte demais
- Atrito no mecanismo
- Servo travando

Soluções:
- Trocar mola mais fraca
- Limpar/lubrificar mecanismo
- Verificar alinhamento
```

---

## Referências

- [Guia de Montagem](./INSTALACAO.md)
- [Interface com Flight-Computer](./API.md)
- [Troubleshooting](./TROUBLESHOOTING.md)

---

[← Voltar ao índice](../README.md)
