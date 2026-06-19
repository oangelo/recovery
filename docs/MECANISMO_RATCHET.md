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

<img src="imagens/linear_ratchet_pawl_v1_lateral.jpg" width="500" alt="Linear Ratchet and Pawl - Vista Lateral">

<!-- TODO: Adicionar descrição detalhada do diagrama -->

### 2.2 Geometria do Ratchet

<!-- TODO: Adicionar imagem do perfil do dente -->
<!-- TODO: Documentar parâmetros: ângulo de ataque, ângulo de repouso, altura, largura, curso -->

### 2.3 Detalhes do Pawl

<!-- TODO: Adicionar imagem do pawl -->
<!-- TODO: Documentar: formato, pivô, como engata/desengata -->

### 2.4 Mecanismo da Porta

<!-- TODO: Adicionar imagem da porta no trilho -->
<!-- TODO: Documentar: guia/trilho, curso de abertura, vedação -->

---

## 3. Acionamento do Pawl

### 3.1 Servo Motor (atual)

<!-- TODO: Documentar por que servo, limitações, implementação -->

### 3.2 Solenoide (futuro)

<!-- TODO: Documentar vantagens, desafios, condições para migração -->

---

## 4. Comparação e Trade-offs

<!-- TODO: Preencher tabela comparativa: ratchet vs pino vs baioneta vs pyro -->

---

## 5. Especificações e Cálculos

<!-- TODO: Documentar forças envolvidas, cálculos, tolerâncias de fabricação -->

---

## 6. Materiais e Manufatura

### 6.1 Protótipo (Impressão 3D)

<!-- TODO: Documentar materiais: PLA, PETG, PC -->

### 6.2 Versão Final

<!-- TODO: Documentar opções: alumínio, PC impresso, nylon/POM -->

---

## 7. Análise de Falhas

<!-- TODO: Documentar modos de falha e mitigações -->
<!-- TODO: Checklist pré-voo -->

---

## 8. Roadmap

- [x] Definição do mecanismo (linear ratchet and pawl)
- [ ] Protótipo impresso 3D (PLA/PETG)
- [ ] Teste de funcionalidade (engate/desengate)
- [ ] Teste de vibração (10-30G)
- [ ] Versão em alumínio ou PC
- [ ] Avaliação de solenoide como atuador
- [ ] Teste de campo com payload real

---

## 9. Referências

- [Working process of the ratchet mechanism (ResearchGate)](https://www.researchgate.net/figure/Working-process-of-the-ratchet-mechanism-a-Initial-position-b-Picking-c-Locking_fig2_286477211)
- [Ratchet mechanism analysis (ScienceDirect)](https://www.sciencedirect.com/science/article/pii/S0094114X17307474)
- [Adafruit Servo Guide](https://learn.adafruit.com/adafruit-arduino-lesson-14-servo-motors)

---

[← Voltar ao DESIGN](./DESIGN.md)
