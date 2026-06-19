# Mecanismo Linear Ratchet and Pawl

> Mecanismo de retenção e liberação da porta lateral do módulo do paraquedas

[← Voltar ao DESIGN](./DESIGN.md)

---

## 1. Visão Geral

O mecanismo **linear ratchet and pawl** é responsável por reter e liberar a porta lateral do módulo do paraquedas. Diferente de um pino de retenção convencional, o ratchet permite fechamento dinâmico com maior tolerância de alinhamento.

**Posição no sistema:**
```
┌─────────────────────────────────────┐
│           MÓDULO DO PARAQUEDAS      │
│                                     │
│    ┌───────────┐                    │
│    │  Porta    │ ← Ratchet + Pawl  │
│    │  lateral  │   retém esta porta │
│    └───────────┘                    │
│                                     │
│    Paraquedas dobrado dentro        │
└─────────────────────────────────────┘
```

**Por que ratchet em vez de pino:**

| Aspecto | Pino de retenção | Linear ratchet |
|---------|------------------|----------------|
| Alinhamento | Precisão alta — pino deve coincidir com furo | Tolerante — dentes engatam progressivamente |
| Fechamento | Posição única de fechamento | Dinâmico — empurra e trava em qualquer ponto do curso |
| Robustez | Sensível a vibração e desgaste | Distribui carga ao longo dos dentes |
| Complexidade | Simples | Levemente mais complexo, mas mais confiável |

---

## 2. Descrição Técnica

### 2.1 Diagrama Geral

<img src="imagens/linear_ratchet_pawl_v1_diagrama.jpg" width="500" alt="Linear Ratchet and Pawl - Diagrama">

```
┌─────────────────────────────────────────────────────────────┐
│              LINEAR RATCHET AND PAWL - VISTA LATERAL         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│    PORTA (fechada)                                           │
│    ┌──────────────────────────┐                              │
│    │                          │                              │
│    │   ┌─┐ ┌─┐ ┌─┐ ┌─┐ ┌─┐  │ ← Dentes do ratchet         │
│    │   └─┘ └─┘ └─┘ └─┘ └─┘  │                              │
│    │         ↑                │                              │
│    │        ┌┴┐               │                              │
│    │        │P│ ← Pawl        │                              │
│    │        │A│   (engatado)  │                              │
│    │        │W│               │                              │
│    │        │L│               │                              │
│    │        └─┘               │                              │
│    │         │                │                              │
│    │     ┌───┴───┐            │                              │
│    │     │ SERVO │            │                              │
│    │     └───────┘            │                              │
│    └──────────────────────────┘                              │
│                                                              │
│    DIREÇÃO DE ABERTURA →                                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 2.2 Geometria do Ratchet

```
    PERFIL DO DENTE
    
         ← Ângulo de ataque (α)
        ┌──┐
       ╱│  │
      ╱ │  │ ← Altura (h)
     ╱  │  │
    ────┘  └────
    │← Largura →|
      (w)
    
    Ângulo de repouso (β) → \
```

**Parâmetros recomendados:**

| Parâmetro | Valor | Observação |
|-----------|-------|------------|
| Ângulo de ataque (α) | 30-45° | Facilita o fechamento dinâmico |
| Ângulo de repouso (β) | 80-90° | Trava contra movimento reverso |
| Altura do dente (h) | 2-3mm | Compromisso entre retenção e desgaste |
| Largura do dente (w) | 3-5mm | Passo do ratchet |
| Curso total | 30-50mm | Conforme dimensão da porta |

### 2.3 Detalhes do Pawl

```
    PAWL (LINGUETA)
    
    ┌─────────┐
    │         │ ← Ponta engata nos dentes
    │    ●    │ ← Pivô ou ponto de fixação
    │         │
    └────┬────┘
         │
    ┌────┴────┐
    │  ATUADOR│ ← Servo (atual) / Solenoide (futuro)
    └─────────┘
```

**Funcionamento:**
- **Engatado:** Ponta do pawl pressiona contra os dentes do ratchet — porta retida
- **Desengatado:** Atuador move o pawl para fora — porta livre para abrir

### 2.4 Mecanismo da Porta

```
    VISTA SUPERIOR - PORTA NO TRILHO
    
    ┌──────────────────────────────────────┐
    │           TRILHO/GUIA                │
    │  ═══════════════════════════════════  │
    │       ┌──────────────────┐           │
    │       │      PORTA       │           │
    │       │  (ratchet aqui)  │           │
    │       └──────────────────┘           │
    │  ═══════════════════════════════════  │
    └──────────────────────────────────────┘
    
    Curso: ←─────────────────────────→
           Fechada                 Aberta
```

---

## 3. Sequência de Operação

```
┌─────────────────────────────────────────────────────────────┐
│                    TIMELINE DE OPERAÇÃO                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  t=0        VOO ASCENDENTE                                  │
│  ┌──────┐   Porta fechada, pawl engatado                    │
│  │██████│   Ratchet segura porta contra pressão aerodinâmica │
│  └──────┘                                                    │
│      │                                                       │
│      ▼                                                       │
│  t=T1       APOGEU DETECTADO                                 │
│  ┌──────┐   Flight-computer envia sinal PWM ao servo        │
│  │→→→→→→│                                                   │
│  └──────┘                                                    │
│      │                                                       │
│      ▼                                                       │
│  t=T1+Δt    PAWL DESENGATA                                   │
│  ┌──────┐   Servo move, pawl recua dos dentes               │
│  │  ↺   │   Δt ≈ 100-300ms (tempo de resposta do servo)     │
│  └──────┘                                                    │
│      │                                                       │
│      ▼                                                       │
│  t=T2       PORTA ABRE                                       │
│  ┌──────┐   Mola empurra porta ao longo do ratchet           │
│  │  →   │   Porta desliza no trilho                          │
│  └──────┘                                                    │
│      │                                                       │
│      ▼                                                       │
│  t=T3       DEPLOY COMPLETO                                  │
│  ┌──────┐   Porta totalmente aberta                          │
│  │ 🪂   │   Paraquedas exposto ao fluxo de ar                │
│  └──────┘   Descida controlada (~5 m/s)                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Notas:**
- O ratchet permite que a porta se abra progressivamente — não precisa de posição exata
- Se houver vibração ou perturbação durante o voo, o pawl mantém a porta retida
- O fechamento dinâmico facilita a montagem pré-voo

---

## 4. Acionamento do Pawl

### 4.1 Situação Atual: Servo Motor

**Por que servo:**
- Controle preciso de posição
- Disponível no projeto (mesmo tipo usado em outros mecanismos)
- Interface PWM padrão com flight-computer

**Limitações:**
- Curso limitado (rotação angular, não linear)
- Tempo de resposta ~100-300ms
- Mecanismo de conversão angular→linear necessário

**Implementação:**
```
Servo horn → Linkage → Pawl
     ↺           →        ↕
  (rotação)   (linear)  (engate)
```

### 4.2 Objetivo Futuro: Solenoide

**Vantagens:**
- Movimento linear direto
- Resposta rápida (~10-50ms)
- Simplicidade mecânica

**Desafios:**
- Curso curto típico (5-15mm)
- Força vs. consumo de corrente
- Necessita driver específico (não PWM direto)

**Condições para migração:**
- Validar que o curso do solenoide é suficiente para desengatar o pawl
- Garantir força adequada contra pressão aerodinâmica na porta
- Teste de vibração com solenoide

---

## 5. Comparação e Trade-offs

### Linear Ratchet vs. Outros Mecanismos

| Critério | Linear Ratchet | Pino Retrátil | Baioneta | Pyro (eletroespoleta) |
|----------|----------------|---------------|----------|----------------------|
| **Tolerância de alinhamento** | Alta | Baixa | Média | N/A |
| **Fechamento dinâmico** | Sim | Não | Não | Não |
| **Simplicidade** | Média | Alta | Média | Baixa |
| **Confiabilidade** | Alta | Média | Alta | Alta |
| **Reusabilidade** | Sim | Sim | Sim | Não |
| **Custo** | Baixo | Baixo | Médio | Médio |
| **Peso** | Baixo | Baixo | Médio | Baixo |

### Vantagens do Ratchet

1. **Tolerância de alinhamento:** Dentes engatam progressivamente — não precisa de posição exata
2. **Fechamento dinâmico:** Empurra a porta e ela trava em qualquer ponto do curso
3. **Distribuição de carga:** Força distribuída ao longo dos dentes, não concentrada num pino
4. **Robustez a vibração:** Múltiplos pontos de contato reduzem risco de liberação acidental

### Desvantagens

1. **Complexidade ligeiramente maior:** Mais peças que um pino simples
2. **Desgaste progressivo:** Dentes podem desgastar com ciclos (mitigado por material adequado)
3. **Manufatura:** Requer precisão nos dentes (mitigado por impressão 3D/usinagem)

---

## 6. Especificações e Cálculos

### Forças Envolvidas

```
F_total = F_aero + F_atrito + F_mola

Onde:
- F_aero = Pressão aerodinâmica × Área da porta
- F_atrito = μ × N (atrito no trilho)
- F_mola = Força da mola de abertura (se houver)
```

### Cálculo Simplificado

**Pressão aerodinâmica (pior caso — voo sônico):**
```
q = 0.5 × ρ × V²
q = 0.5 × 1.225 × (340)² ≈ 70 kPa

F_aero = q × A_porta
F_aero = 70000 × 0.001 = 70 N (para porta de 10cm²)
```

**Força necessária no pawl:**
```
F_pawl ≥ F_total × fator_segurança
F_pawl ≥ 70 × 2 = 140 N (com margem 2x)
```

### Tolerâncias de Fabricação

| Componente | Tolerância | Método |
|------------|------------|--------|
| Dentes do ratchet | ±0.2mm | Impressão 3D / Usinagem |
| Pawl | ±0.1mm | Usinagem |
| Trilho da porta | ±0.3mm | Impressão 3D |
| Curso do pawl | ±0.5mm | Calibrado no servo |

---

## 7. Materiais e Manufatura

### 7.1 Protótipo (Impressão 3D)

| Material | Uso | Limitações |
|----------|-----|------------|
| PLA | Validação de forma e encaixe | Baixa resistência, deformação térmica |
| PETG | Testes funcionais | Melhor resistência, ainda limitado |
| PC (Policarbonato) | Protótipo resistente | Boa resistência, impressão mais difícil |

**Recomendação:** Começar com PLA para validação de geometria, depois PETG/PC para testes funcionais.

### 7.2 Versão Final

| Opção | Vantagens | Desvantagens |
|-------|-----------|--------------|
| Alumínio usinado | Alta resistência, durável | Custo, peso |
| PC impresso | Bom custo-benefício, leve | Resistência menor que alumínio |
| Nylon (POM) | Auto-lubrificante, resistente | Custo, disponibilidade |

**Critério de escolha:** Dependendo do resultado dos testes de vibração e desgaste.

---

## 8. Análise de Falhas

### Modos de Falha e Mitigações

| # | Modo de Falha | Causa Provável | Consequência | Mitigação |
|---|---------------|----------------|--------------|-----------|
| 1 | Pawl não desengata | Servo falha / linkage trava | Porta não abre | Redundância no acionamento; teste pré-voo |
| 2 | Dentes desgastam | Ciclos repetidos / material fraco | Ratchet não retém | Material adequado; inspeção periódica |
| 3 | Porta trava no trilho | Detritos / deformação térmica | Porta não abre mesmo com pawl solto | Tolerâncias adequadas; limpeza pré-voo |
| 4 | Deploy prematuro | Vibração excessiva / pawl fraco | Paraquedas libera durante subida | Fator de segurança no pawl; teste de vibração |
| 5 | Ratchet quebra | Impacto / material fraco | Perda de retenção | Material resistente; inspeção visual |

### Checklist Pré-Voo (Ratchet)

- [ ] Dentes do ratchet sem desgaste visível
- [ ] Pawl engata e desengata suavemente
- [ ] Porta desliza livremente no trilho
- [ ] Servo responde ao sinal PWM
- [ ] Curso do pawl suficiente para desengatar
- [ ] Sem detritos no mecanismo

---

## 9. Roadmap

- [x] Definição do mecanismo (linear ratchet and pawl)
- [ ] Protótipo impresso 3D (PLA/PETG)
- [ ] Teste de funcionalidade (engate/desengate)
- [ ] Teste de vibração (10-30G)
- [ ] Versão em alumínio ou PC
- [ ] Avaliação de solenoide como atuador
- [ ] Teste de campo com payload real

---

## 10. Referências

- [Working process of the ratchet mechanism (ResearchGate)](https://www.researchgate.net/figure/Working-process-of-the-ratchet-mechanism-a-Initial-position-b-Picking-c-Locking_fig2_286477211)
- [Ratchet mechanism analysis (ScienceDirect)](https://www.sciencedirect.com/science/article/pii/S0094114X17307474)
- [Adafruit Servo Guide](https://learn.adafruit.com/adafruit-arduino-lesson-14-servo-motors)

---

[← Voltar ao DESIGN](./DESIGN.md)
