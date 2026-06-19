# Guia de Montagem - Sistema de Recuperação

> Passo a passo para montar o mecanismo de recuperação com servo mecânico

[← Voltar ao índice](../README.md)

---

## Requisitos

### Hardware
- Servo micro/médio (SG90, MG90S, ou similar)
- Mola de compressão (10-15mm diâmetro, 30-50mm comprimento)
- Nose cone compatível com tubo corpo
- Tubo corpo do foguete
- Shock cord (nylon, 3-5mm)
- Paraquedas (nylon, diâmetro conforme peso)
- Parafusos M2 e M3 (com porcas e arruelas)
- Conector JST-XH 3 pinos
- Fio 22AWG silicone
- Hot glue

### Ferramentas
- Ferro de solda (350°C)
- Solda e flux
- Alicate de corte
- Chave Phillips (PH0, PH1)
- Multímetro
- Régua/trena
- Marcador permanente

---

## Passo 1: Verificar Componentes

Antes de começar, verifique se todos os componentes estão presentes e funcionais:

```bash
# Checklist de componentes
- [ ] Servo (testar com fonte 5V)
- [ ] Mola (verificar comprimento e força)
- [ ] Nose cone (encaixe no tubo)
- [ ] Tubo corpo (sem danos)
- [ ] Shock cord (sem nós ou defeitos)
- [ ] Paraquedas (sem rasgos)
- [ ] Parafusos e porcas
- [ ] Conector JST-XH
- [ ] Fio 22AWG
```

---

## Passo 2: Preparar o Suporte do Servo

### Medir e Marcar

```
1. Meça a posição do servo no tubo corpo
   - Posicione a 1/3 da base do tubo (próximo à base)
   - Marque a posição com marcador

2. Marque o furo para o pino de retenção
   - O furo deve estar alinhado com o horn do servo
   - Diâmetro: pino + 0.5mm de folga

3. Corte o furo no tubo
   - Use furadeira ou Dremel
   - Corte com cuidado para não rachar o tubo
```

### Testar Encaixe

```
1. Insira o servo no suporte
2. Verifique se o pino passa pelo furo do tubo
3. Teste o movimento manualmente
4. Ajuste se necessário
```

---

## Passo 3: Montar o Mecanismo

### Fixar o Servo

```
1. Posicione o servo no suporte
2. Alinhe o horn com o furo do tubo
3. Fixe com parafusos M2
4. Aperte firmemente, mas não demais (pode travar o servo)
```

### Instalar o Pino de Retenção

```
1. Conecte o horn ao servo (posicionando em 0°)
2. Insira o pino no horn
3. Teste o movimento:
   - Posição 0°: pino dentro do tubo (retém NC)
   - Posição 90°: pino recuado (libera NC)
4. Ajuste se necessário
```

### Instalar a Mola

```
1. Meça o comprimento da mola comprimida
2. Posicione a mola na base do tubo
3. Fixe a base da mola (parafuso ou cola)
4. Teste a compressão:
   - Deve comprimir o suficiente para empurrar o NC
   - Não deve ser tão forte que o servo não consiga fechar
```

---

## Passo 4: Conexão Elétrica

### Preparar os Fios

```
1. Corte 3 fios de 20cm cada
2. Descasque 5mm em cada ponta
3. Solde nos terminais do servo:
   - Marrom/Laranja → Sinal
   - Vermelho → VCC (5V)
   - Preto → GND
```

### Conector JST-XH

```
1. Solde os fios no conector JST-XH 3 pinos
2. Respeite a pinagem:
   - Pino 1: Sinal (marrom/laranja)
   - Pino 2: VCC (vermelho)
   - Pino 3: GND (preto)
3. Use flux para garantir boa solda
4. Isole com heat shrink ou fita
```

### Strain Relief

```
1. Aplique hot glue na raiz do conector
2. Aplique hot glue na raiz dos fios do servo
3. Deixe secar completamente
4. Teste a fixação puxando suavemente
```

---

## Passo 5: Teste do Mecanismo

### Teste Manual

```
1. Conecte o servo a uma fonte de sinal PWM
   - Ou use um Arduino com sketch de teste
   - Ou use o flight-computer

2. Teste posição fechada:
   - Envie pulso de 500-1000 μs
   - Pino deve recuar completamente
   - NC deve se soltar

3. Teste posição aberta:
   - Envie pulso de 2000-2500 μs
   - Pino deve avançar
   - NC deve ser retido

4. Repita 10 vezes:
   - Movimento deve ser consistente
   - Sem ruídos estranhos
   - Sem travamentos
```

### Teste de Corrente

```
1. Meça corrente com multímetro em série
2. Valores esperados:
   - Repouso: ~10 mA
   - Movimento: ~100-300 mA
   - Stall (travado): ~500-700 mA

3. Se corrente muito alta:
   - Mola pode estar forte demais
   - Servo pode estar travando
   - Verifique alinhamento
```

---

## Passo 6: Montagem no Foguete

### Instalar o Mecanismo

```
1. Insira a mola no tubo corpo
2. Posicione o mecanismo sobre a mola
3. Alinhe o pino com o furo do tubo
4. Fixe com parafusos M3
5. Aperte firmemente
```

### Conectar o Shock Cord

```
1. Prenda uma ponta do shock cord ao NC
   - Use nó seguro (nó de oito duplo)
   - Ou use conector metálico

2. Prenda a outra ponta ao tubo corpo
   - Use parafuso com arruela
   - Ou use conector metálico

3. Comprimento do shock cord:
   - Mínimo: 2x comprimento do tubo
   - Ideal: 3x comprimento do tubo
   - Evita que NC bata no foguete durante deploy
```

### Montar o Paraquedas

```
1. Prenda o paraquedas ao shock cord
   - Use nó seguro ou conector
   - Posicione entre NC e tubo

2. Dobre o paraquedas:
   - Dobre em sanfona (zig-zag)
   - Enrole levemente
   - Posicione dentro do tubo, acima da mola

3. Teste o deploy:
   - Acione o servo manualmente
   - Verifique se NC se solta
   - Verifique se paraquedas se infla
   - Repita 3 vezes
```

---

## Passo 7: Teste Final

### Checklist Pré-Voo

```markdown
## Mecanismo
- [ ] Servo fixado firmemente
- [ ] Pino se move livremente
- [ ] Mola comprime e expande sem travar
- [ ] NC se encaixa perfeitamente
- [ ] Shock cord sem nós ou torções

## Conexões
- [ ] Conector JST-XH travado
- [ ] Strain relief aplicado
- [ ] Fios com folga suficiente
- [ ] Continuidade verificada

## Paraquedas
- [ ] Paraquedas dobrado corretamente
- [ ] Shock cord com comprimento adequado
- [ ] Sem nós no shock cord
- [ ] Paraquedas não preso na mola

## Teste
- [ ] Deploy 3x consecutivas
- [ ] Corrente dentro do esperado
- [ ] Sem ruídos estranhos
- [ ] Tempo de deploy < 500ms
```

### Teste de Queda (Opcional)

```
1. Monte o sistema completo no foguete
2. Solte o foguete de 1-2 metros
3. Verifique se:
   - NC se solta no impacto
   - Paraquedas se infla
   - Sistema funciona após impacto
```

---

## Troubleshooting

Ver [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) para problemas comuns e soluções.

---

## Próximos Passos

1. [Calibrar o mecanismo](./CALIBRACAO.md)
2. [Integrar com flight-computer](./API.md)
3. [Testes de campo](../test/README.md)

---

[← Voltar ao índice](../README.md)
