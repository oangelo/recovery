# Sistema de Recuperação - Serra Rocketry

![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)
![Versão](https://img.shields.io/badge/versão-0.1.0-blue)

## Sobre

Sistema de recuperação mecânica para foguetes de média/alta potência da Serra Rocketry. Utiliza **paraquedas único** acionado no apogeu por **servo mecânico**, controlado pelo computador de bordo.

**Princípio de operação:**
1. Foguete atinge apogeu (velocidade vertical ≈ 0)
2. Computador de bordo detecta apogeu via barômetro/IMU
3. Sinal enviado ao servo mecânico
4. Servo libera mecanismo de retenção (pino/gancho)
5. Mola/ar comprimido empurra paraquedas para fora do corpo do foguete
6. Paraquedas se infla e freia a descida

> **Nota:** PCB, firmware e software de controle estão documentados no repositório [flight-computer](https://github.com/Serra-Rocketry/flight-computer).

---

## Quick Start

```bash
# Clone o repositório
git clone https://github.com/Serra-Rocketry/recovery.git
cd recovery

# Consulte a documentação
# Visão geral do mecanismo:
cat hardware/README.md

# Guia de montagem:
cat docs/INSTALACAO.md

# Problemas comuns:
cat docs/TROUBLESHOOTING.md
```

---

## Estrutura do Projeto

```
recovery/
├── docs/                       # Documentação técnica
│   ├── hardware/              # Esquemáticos mecânicos
│   ├── diagrams/              # Fluxogramas e diagramas
│   ├── INSTALACAO.md          # Guia de montagem
│   ├── API.md                 # Interface com flight-computer
│   ├── CALIBRACAO.md          # Procedimentos de calibração
│   └── TROUBLESHOOTING.md     # Problemas comuns
│
├── hardware/                   # Arquivos de hardware físico
│   ├── images/                # Fotos do mecanismo
│   ├── 3d_models/             # Modelos 3D (STL, STEP)
│   └── README.md              # BOM, especificações, montagem
│
├── .gitignore
├── LICENSE
├── CHANGELOG.md
└── README.md                  # Este arquivo
```

---

## Princípios de Design

### Fungibilidade
- Servos com interface padrão PWM (qualquer servo padrão funciona)
- Conectores JST-XH para conexão com flight-computer
- Parafusos M3 padronizados para fixação

### Simplicidade
- Mínimo de peças móveis
- Sem carga pyro (sem eletroespoleta, sem black powder)
- Acionamento puramente mecânico via servo

### Robustez
- Mecanismo testado contra vibração (10-30G)
- Servo com torque adequado para contra-mola
- Strain relief em todos os conectores

---

## Pré-requisitos

**Hardware (mecânico):**
- Servo micro/médio (9g a 25g, torque mínimo 1.5 kgf·cm)
- Mola de compressão ou ar comprimido
- Tubo corpo do foguete (diâmetro comercial)
- Paraquedas (tamanho conforme peso do foguete)
- Shock cord (linha de choque)
- Parafusos M3, porcas, arruelas

**Conexão com flight-computer:**
- Sinal PWM (pino do computador de bordo)
- Alimentação 5V (do flight-computer ou bateria dedicada)
- Ver [docs/API.md](./docs/API.md) para detalhes

---

## Documentação

- [Guia de Montagem Detalhado](./docs/INSTALACAO.md)
- [Hardware e Especificações](./hardware/README.md)
- [Interface com Flight-Computer](./docs/API.md)
- [Calibração do Mecanismo](./docs/CALIBRACAO.md)
- [Troubleshooting](./docs/TROUBLESHOOTING.md)

---

## Status do Projeto

- [x] Definição do mecanismo (servo + mola)
- [x] Seleção de componentes
- [ ] Protótipo mecânico
- [ ] Testes de vibração
- [ ] Testes de campo
- [ ] Documentação final

---

## Contribuindo

Leia [Boas Práticas Serra Rocketry](https://github.com/Serra-Rocketry/best-practices)

**Workflow:**
1. Fork este repositório
2. Crie uma branch: `git checkout -b feature/sua-funcionalidade`
3. Commit: `git commit -m 'Adiciona funcionalidade X'`
4. Push: `git push origin feature/sua-funcionalidade`
5. Abra um Pull Request

---

## Autores

- [@oangelo](https://github.com/oangelo) - Mecanismo e documentação

## Licença

Este projeto está sob a licença MIT - veja [LICENSE](LICENSE) para detalhes.

## Agradecimentos

- Equipe Serra Rocketry - IPRJ/UERJ
- [Boas Práticas Serra Rocketry](https://github.com/Serra-Rocketry/best-practices)
