# Design do Sistema de Recuperação

> Decisões de design e arquitetura do sistema de recuperação da Serra Rocketry

[← Voltar ao README](../README.md)

---

## Visão Geral

O sistema de recuperação utiliza **dois mecanismos independentes** para garantir o deploy seguro do paraquedas:

```
┌─────────────────────────────────────────────────────────────┐
│              SISTEMA DE RECUPERAÇÃO - ARQUITETURA            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────┐         ┌──────────────────┐          │
│  │  MECANISMO 1     │         │  MECANISMO 2     │          │
│  │  Porta lateral   │         │  Expulsão NC     │          │
│  │                  │         │                  │          │
│  │  Linear Ratchet  │         │  (a definir)     │          │
│  │  and Pawl        │         │                  │          │
│  └────────┬─────────┘         └────────┬─────────┘          │
│           │                            │                     │
│           └──────────┬─────────────────┘                     │
│                      │                                       │
│              ┌───────▼───────┐                               │
│              │ FLIGHT        │                               │
│              │ COMPUTER      │                               │
│              │ (sinal PWM)   │                               │
│              └───────────────┘                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Mecanismos

### 1. Porta Lateral do Paraquedas — Linear Ratchet and Pawl

**Função:** Retém e libera a porta lateral que expõe o paraquedas.

**Características:**
- Fechamento dinâmico com tolerância de alinhamento
- Acionamento via servo motor (objetivo: solenoide)
- Unidirecional — permite abertura, trava no fechamento

**Documento completo:** [MECANISMO_RATCHET.md](./MECANISMO_RATCHET.md)

---

### 2. Expulsão do Nose Cone — (Em definição)

**Função:** Expulsa o nose cone para liberar o paraquedas.

**Status:** Em definição — documento próprio será criado.

**Candidatos:**
- Mola comprimida (mecânico)
- Servo com linkage
- Outro mecanismo

**Documento:** [MECANISMO_EXPULSAO.md](./MECANISMO_EXPULSAO.md) *(a criar)*

---

## Sequência de Operação Completa

```
┌─────────────────────────────────────────────────────────────┐
│                 SEQUÊNCIA TEMPORAL DE DEPLOY                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  t=0     APOGEU DETECTADO                                    │
│          Flight-computer identifica velocidade vertical ≈ 0  │
│                                                              │
│  t=0+Δ1  EXPULSÃO DO NOSE CONE                              │
│          Mecanismo 2 aciona → NC é empurrado para fora       │
│                                                              │
│  t=0+Δ2  ABERTURA DA PORTA                                   │
│          Mecanismo 1 aciona → pawl desengata                 │
│          Porta abre → paraquedas exposto                     │
│                                                              │
│  t=0+Δ3  INFLAÇÃO DO PARAQUEDAS                              │
│          Ar infla o paraquedas                               │
│          Descida controlada (~5 m/s)                         │
│                                                              │
│  Δ1, Δ2, Δ3 = tempos a calibrar nos testes                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Princípios de Design

### Independência dos Mecanismos
- Cada mecanismo é independente e pode ser testado separadamente
- Falha em um não compromete o outro necessariamente

### Tolerância e Robustez
- Linear ratchet permite fechamento sem alinhamento preciso
- Materiais dimensionados para vibração (10-30G) e pressão aerodinâmica

### Simplicidade
- Mínimo de peças móveis
- Sem carga pyro (sem eletroespoleta, sem black powder)
- Acionamento puramente mecânico (servo/solenoide)

### Reusabilidade
- Todos os componentes são reutilizáveis
- Sem consumíveis pirotécnicos

---

## Documentação Relacionada

| Documento | Conteúdo |
|-----------|----------|
| [MECANISMO_RATCHET.md](./MECANISMO_RATCHET.md) | Linear ratchet and pawl (porta lateral) |
| [MECANISMO_EXPULSAO.md](./MECANISMO_EXPULSAO.md) | Expulsão do nose cone *(a criar)* |
| [API.md](./API.md) | Interface com flight-computer |
| [INSTALACAO.md](./INSTALACAO.md) | Guia de montagem |
| [CALIBRACAO.md](./CALIBRACAO.md) | Procedimentos de calibração |

---

[← Voltar ao README](../README.md)
