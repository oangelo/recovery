# Hardware - Sistema de Recuperação

> Especificações mecânicas, BOM e instruções de montagem do mecanismo de recuperação

[← Voltar ao README](../README.md)

---

## Documentação de Design

Para decisões de design e detalhes dos mecanismos:

- **[Design do Sistema](../docs/DESIGN.md)** — Visão geral da arquitetura e princípios de design
- **[Linear Ratchet and Pawl](../docs/MECANISMO_RATCHET.md)** — Mecanismo da porta lateral do paraquedas

---

## Diagrama de Operação

```
┌─────────────────────────────────────────────────────────────┐
│                    SEQUÊNCIA DE OPERAÇÃO                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. VOO ASCENDENTE                                           │
│  ┌──────────┐                                                │
│  │ ▲ Foguete│  Servo: POSIÇÃO FECHADA (pino retém NC)       │
│  │ │        │  Paraquedas: Dobrado dentro do tubo            │
│  │ │  NC    │  NC = Nose Cone                                │
│  │ │  ↕     │                                                │
│  │ └────────│                                                │
│  └──────────┘                                                │
│                                                              │
│  2. APOGEU (velocidade ≈ 0)                                  │
│  ┌──────────┐                                                │
│  │   NC     │  Flight-computer detecta apogeu                │
│  │   ↕      │  Envia sinal PWM ao servo                      │
│  │ ──────── │                                                │
│  │          │                                                │
│  └──────────┘                                                │
│                                                              │
│  3. LIBERAÇÃO                                                │
│  ┌──────────┐                                                │
│  │   NC ▲   │  Servo: POSIÇÃO ABERTA (pino recua)            │
│  │   ↕  │   │  Mola empurra nose cone                        │
│  │ ─────│── │  Paraquedas exposto ao fluxo de ar             │
│  │      │   │                                                │
│  └──────│───┘                                                │
│         │                                                    │
│  4. DEPLOY                                                   │
│  ┌──────│───┐                                                │
│  │   NC │   │  Paraquedas infla                              │
│  │      ▼   │  Shock cord absorve choque                     │
│  │   🪂     │  Descida controlada (~5 m/s)                   │
│  │          │                                                │
│  └──────────┘                                                │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Mecanismo de Liberação

### Princípio

O mecanismo utiliza um **servo mecânico** que retém o nose cone através de um **pino de retenção**. Quando o flight-computer detecta o apogeu, envia um sinal PWM que faz o servo recuar o pino, liberando o nose cone.

### Componentes do Mecanismo

```
┌─────────────────────────────────────────────────┐
│              CORTE DO MECANISMO                  │
│                                                  │
│    Nose Cone (NC)                                │
│    ┌──────────┐                                  │
│    │          │                                  │
│    │  Para-   │                                  │
│    │  quedas  │                                  │
│    │  dobrado │                                  │
│    │          │                                  │
│    ├──────────┤ ← Pino de retenção               │
│    │    ●━━━━━━━━━ Servo horn                    │
│    │          │                                  │
│    │  Mola    │                                  │
│    │  ║║║║║   │                                  │
│    │          │                                  │
│    ├──────────┤ ← Fixação da mola                │
│    │          │                                  │
│    │  Tubo    │                                  │
│    │  corpo   │                                  │
│    │          │                                  │
│    └──────────┘                                  │
│                                                  │
└─────────────────────────────────────────────────┘
```

### Tipos de Mecanismo

#### Opção 1: Pino Retrátil (Recomendado)

```
FECHADO (voo):              ABERTO (deploy):

    NC                          NC
    │                           │
    ├──●━━━ Servo               │
    │  ↑ Pino retém NC          │  ↑ Pino recuou
    │                           │
    ║║║ Mola                    ║║║ Mola expande
    │                           │  → empurra NC
```

**Como funciona:**
- Servo tem um "horn" (braço) com pino
- Na posição fechada, pino atravessa furo no tubo e retém NC
- Na posição aberta, servo gira, pino recua, NC liberado

#### Opção 2: Gancho/Lock

```
FECHADO:                    ABERTO:

    NC                          NC
    │                           │
    ┌─┐ ← Gancho                │  ← Gancho abriu
    │ │                          │
    └─┘                          │
    ║║║ Mola                    ║║║ Mola expande
```

---

## Bill of Materials (BOM)

### Servo

| Item | Especificação | Qtd | Observação |
|------|---------------|-----|------------|
| Servo micro | 9g, torque ≥ 1.5 kgf·cm | 1 | SG90 ou similar |
| Parafuso M2x8 | Fixação do servo | 2 | Com porca e arruela |
| Parafuso M3x10 | Fixação do suporte | 2 | Com porca e arruela |

**Alternativas de servo:**

| Modelo | Peso | Torque | Aplicação |
|--------|------|--------|-----------|
| SG90 | 9g | 1.6 kgf·cm | Foguetes até 500g |
| MG90S | 14g | 2.2 kgf·cm | Foguetes até 1kg |
| MG996R | 55g | 11 kgf·cm | Foguetes > 1kg |

### Mola

| Item | Especificação | Qtd | Observação |
|------|---------------|-----|------------|
| Mola de compressão | Diâmetro: 10-15mm, Comprimento: 30-50mm | 1 | Força: 2-5N |

**Seleção da mola:**
- Deve ser forte o suficiente para empurrar o NC com segurança
- Não tão forte que o servo não consiga comprimir
- Teste: servo deve conseguir fechar contra a mola sem travar

### Nose Cone e Tubo

| Item | Especificação | Qtd | Observação |
|------|---------------|-----|------------|
| Nose cone | Diâmetro do tubo corpo | 1 | Comercial ou impresso 3D |
| Tubo corpo | Diâmetro comercial (BT-50, BT-55, etc.) | 1 | Papel, fibra, ou plástico |
| Shock cord | Nylon, 3-5mm, 2-3x comprimento do foguete | 1 | Absorve choque do deploy |
| Paraquedas | Nylon, diâmetro conforme peso | 1 | Ver tabela abaixo |

### Fixação e Conectores

| Item | Especificação | Qtd | Observação |
|------|---------------|-----|------------|
| Parafuso M3x10 | Fixação geral | 4 | Inox ou latão |
| Porca M3 | Auto-travante | 4 | Nylon insert |
| Arruela M3 | Lisa | 8 | Aço inox |
| Conector JST-XH 3P | Conexão servo → flight-computer | 1 | 3 pinos (Sinal, VCC, GND) |
| Fio 22AWG | Extensão do servo | 20cm | Silicone, flexível |
| Hot glue | Strain relief e fixação | - | Pistola de cola quente |

---

## Tamanho do Paraquedas

### Fórmula

```
Área do paraquedas = (2 × massa × g) / (ρ × Cd × V²)

Onde:
- massa = massa do foguete (kg)
- g = 9.81 m/s²
- ρ = densidade do ar (1.225 kg/m³ ao nível do mar)
- Cd = coeficiente de arrasto do paraquedas (~1.5 para hemisférico)
- V = velocidade de descida desejada (m/s)
```

### Tabela Prática

| Massa do Foguete | Diâmetro do Paraquedas | Velocidade de Descida |
|------------------|------------------------|----------------------|
| 200g | 30cm | ~5 m/s |
| 500g | 45cm | ~5 m/s |
| 1kg | 60cm | ~5 m/s |
| 2kg | 85cm | ~5 m/s |
| 5kg | 130cm | ~5 m/s |

**Velocidade segura:** 3-7 m/s
- < 3 m/s: Paraquedas muito grande, foguete voa longe com vento
- \> 7 m/s: Impacto pode danificar o foguete

---

## Especificações do Servo

### Sinal PWM

```
┌─────────────────────────────────────────────┐
│           SINAL PWM DO SERVO                │
├─────────────────────────────────────────────┤
│                                              │
│  Posição 0° (fechado):  500-1000 μs         │
│  Posição 90° (meio):    1500 μs             │
│  Posição 180° (aberto): 2000-2500 μs        │
│                                              │
│  Frequência: 50 Hz (período = 20ms)         │
│                                              │
│  ┌───┐     ┌───┐     ┌───┐                  │
│  │   │     │   │     │   │                  │
│──┘   └─────┘   └─────┘   └──  5V            │
│  │←→│                                       │
│  1-2ms (largura do pulso = posição)         │
│                                              │
└─────────────────────────────────────────────┘
```

### Conexão Elétrica

```
Servo                    Flight-Computer
┌──────────┐             ┌──────────────┐
│          │             │              │
│  Sinal ──┼─────────────┼── Pino PWM   │
│  VCC   ──┼─────────────┼── 5V         │
│  GND   ──┼─────────────┼── GND        │
│          │             │              │
└──────────┘             └──────────────┘

Conector JST-XH 3 pinos:
┌─────────────┐
│ 1. Sinal    │ ← Marrom ou Laranja
│ 2. VCC (5V) │ ← Vermelho
│ 3. GND      │ ← Preto
└─────────────┘
```

### Consumo

| Estado | Corrente | Potência |
|--------|----------|----------|
| Repouso | ~10 mA | 0.05W |
| Movimento (sem carga) | ~100 mA | 0.5W |
| Movimento (com carga) | ~300-500 mA | 1.5-2.5W |
| Stall (travado) | ~700 mA | 3.5W |

**Atenção:** Se o servo travar contra a mola, consumirá corrente máxima. Garanta que a mola não seja forte demais.

---

## Montagem

### Ferramentas Necessárias

- Ferro de solda (350°C)
- Solda (60/40 ou lead-free)
- Flux
- Alicate de corte
- Chave Phillips pequena (PH0, PH1)
- Multímetro
- Régua/trena
- Marcador permanente

### Passo 1: Preparar o Suporte do Servo

```
1. Meça a posição do servo no tubo corpo
2. Marque a posição do furo para o pino de retenção
3. Corte o furo no tubo (diâmetro do pino + folga)
4. Teste o encaixe do servo
```

### Passo 2: Montar o Mecanismo

```
1. Fixe o servo no suporte com parafusos M2
2. Conecte o horn (braço) ao servo
3. Insira o pino no horn
4. Teste o movimento manualmente
5. Fixe a mola na base do tubo
```

### Passo 3: Conexão Elétrica

```
1. Solde fios 22AWG ao conector JST-XH
2. Conecte ao servo (ver pinagem acima)
3. Faça strain relief com hot glue
4. Teste a conexão com multímetro
```

### Passo 4: Teste do Mecanismo

```
1. Conecte ao flight-computer ou fonte de sinal PWM
2. Teste posição fechada (pino retém NC)
3. Teste posição aberta (pino recua, NC libera)
4. Repita 10 vezes para verificar consistência
5. Meça corrente consumida
```

### Passo 5: Montagem no Foguete

```
1. Insira a mola no tubo corpo
2. Posicione o mecanismo sobre a mola
3. Fixe com parafusos M3
4. Conecte o shock cord ao NC e ao tubo
5. Monte o paraquedas no shock cord
6. Teste o deploy completo
```

---

## Checklist Pré-Voo

### Mecanismo
- [ ] Servo fixado firmemente (sem movimento)
- [ ] Pino de retenção se move livremente
- [ ] Mola comprime e expande sem travar
- [ ] NC se encaixa perfeitamente no tubo
- [ ] Shock cord sem nós ou torções

### Conexões
- [ ] Conector JST-XH travado
- [ ] Strain relief aplicado (hot glue)
- [ ] Fios com folga suficiente
- [ ] Continuidade verificada (multímetro)

### Paraquedas
- [ ] Paraquedas dobrado corretamente
- [ ] Shock cord com comprimento adequado (2-3x tubo)
- [ ] Sem nós no shock cord
- [ ] Paraquedas não está preso na mola

### Teste Final
- [ ] Teste de deploy 3x consecutivas
- [ ] Corrente dentro do esperado
- [ ] Sem ruídos estranhos no servo
- [ ] Tempo de deploy < 500ms

---

## Modelos 3D

Diretório: `hardware/3d_models/`

**Arquivos esperados:**
- `servo_mount.stl` - Suporte do servo
- `retention_pin.stl` - Pino de retenção (se impresso)
- `spring_base.stl` - Base da mola
- `nosecone_adapter.stl` - Adaptador para NC

> **Nota:** Adicione os modelos 3D quando disponíveis.

---

## Fotos

Diretório: `hardware/images/`

**Fotos esperadas:**
- `mechanism_closed.jpg` - Mecanismo na posição fechada
- `mechanism_open.jpg` - Mecanismo na posição aberta
- `assembly_top.jpg` - Montagem vista de cima
- `assembly_side.jpg` - Montagem vista lateral
- `servo_detail.jpg` - Detalhe do servo e pino

> **Nota:** Adicione as fotos quando disponíveis.

---

## Recursos

- [Guia de Servos - Adafruit](https://learn.adafruit.com/adafruit-arduino-lesson-14-servo-motors)
- [Cálculo de Paraquedas](https://www.rocketryforum.com/)
- [Boas Práticas de Hardware - Serra Rocketry](https://github.com/Serra-Rocketry/best-practices/blob/main/hardware/boas-praticas-hardware.md)

---

[← Voltar ao README](../README.md)
