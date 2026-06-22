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

<img src="./imagens/linear_ratchet_pawl_v1_lateral.jpg" width="500" alt="Linear Ratchet and Pawl - Vista Lateral">

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

### 2.5 Protótipo Alpha

#### Mecanismo com Servo Montado

<table>
  <tr>
    <td><img src="./imagens/ratchet_alpha_pawl_frontal.jpg" width="300" alt="Mecanismo - Vista Frontal"></td>
    <td><img src="./imagens/ratchet_alpha_pawl_traseira.jpg" width="300" alt="Mecanismo - Vista Traseira"></td>
    <td><img src="./imagens/ratchet_alpha_pawl_lateral.jpg" width="300" alt="Mecanismo - Vista Lateral"></td>
  </tr>
</table>

Mecanismo completo com servo motor montado. Vista frontal mostra o servo e o linkage. Vista traseira evidencia o pawl engatando nos dentes do ratchet. Vista lateral mostra o perfil completo do conjunto.

#### Pawl (Lingueta)

<img src="./imagens/ratchet_alpha_pawl_detalhe.jpg" width="500" alt="Pawl - Detalhe">

Detalhe do pawl isolado. A ponta engata nos dentes do ratchet para reter a porta.

#### Montagem no Módulo

<table>
  <tr>
    <td><img src="./imagens/ratchet_alpha_pawl_modulo_traseira.jpg" width="300" alt="Mecanismo no Módulo"></td>
    <td><img src="./imagens/ratchet_alpha_porta_modulo_detalhe.jpg" width="300" alt="Zoom - Porta no Módulo"></td>
    <td><img src="./imagens/ratchet_alpha_porta_trilho.jpg" width="300" alt="Porta e Trilho"></td>
  </tr>
</table>

Mecanismo montado no módulo do paraquedas. Primeira foto: vista traseira com pawl visível. Segunda: zoom na porta entrando no módulo. Terceira: porta deslizando no trilho linear.

**Teste inicial (alpha)** — Validação de conceito com impressão 3D.

---

## 3. Acionamento do Pawl

### 3.1 Servo Motor (atual)

<!-- TODO: Documentar por que servo, limitações, implementação -->

### 3.2 Solenoide (futuro)

**Vantagens:**
- Movimento linear direto
- Resposta rápida (~10-50ms)
- Simplicidade mecânica

**Desafios:**
- Curso curto típico (5-15mm)
- Força vs. consumo de corrente
- Necessita driver específico (não PWM direto)

**Orientação do Solenoide**

O solenoide deve ser montado **perpendicularmente** ao eixo do foguete. Durante o lançamento, a aceleração pode gerar forças inerciais no pino do solenoide (que é metálico e mais pesado que as partes poliméricas do mecanismo). Se o solenoide estiver alinhado com o eixo do foguete, a força de aceleração pode vencer a mola de retorno e abrir a porta prematuramente.

Para mudar a direção da força do solenoide de perpendicular para linear (ao longo da porta), utiliza-se um **bell crank** (alavanca angular):

<img src="./imagens/ratchet_alpha_bell_crank.jpg" width="500" alt="Bell Crank - Mudança de direção de força">

O bell crank converte o movimento linear perpendicular do solenoide no movimento linear necessário para desengatar o pawl, mantendo o solenoide protegido das forças de aceleração do voo.

**Condições para migração:**
- Validar que o curso do solenoide é suficiente para desengatar o pawl via bell crank
- Garantir força adequada contra pressão aerodinâmica na porta
- Teste de vibração com solenoide perpendicular

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
