# Troubleshooting - Sistema de Recuperação

> Soluções para problemas comuns do mecanismo de recuperação

[← Voltar ao índice](../README.md)

---

## Problemas Comuns

### Servo não se move

**Sintomas:**
- Servo não responde ao sinal PWM
- Sem som de motor
- LED do flight-computer indica que enviou sinal, mas servo não reage

**Causas possíveis:**
1. Conexão frouxa ou invertida
2. Alimentação insuficiente
3. Servo com defeito
4. Pino PWM incorreto no flight-computer

**Soluções:**
```bash
# 1. Verificar conexões
- Confirme pinagem: Sinal, VCC, GND
- Teste continuidade com multímetro
- Reaperte conectores

# 2. Verificar alimentação
- Meça tensão no conector do servo (deve ser ~5V)
- Meça corrente disponível (mínimo 500mA)
- Teste com bateria dedicada se necessário

# 3. Testar servo isoladamente
- Conecte servo a Arduino/fonte de sinal PWM
- Envie pulso de 1500μs (posição neutra)
- Se não mover, servo com defeito

# 4. Verificar pino PWM
- Confirme qual pino está configurado no firmware
- Teste com osciloscópio ou LED
```

---

### Servo move mas não libera NC

**Sintomas:**
- Servo se move, mas pino não recua o suficiente
- NC continua retido
- Servo faz som de "força" (stall)

**Causas possíveis:**
1. Curso insuficiente do servo
2. Pino muito curto
3. Mola forte demais
4. Atrito excessivo no mecanismo

**Soluções:**
```bash
# 1. Ajustar curso do servo
- Aumente amplitude do sinal PWM (de 500μs para 700μs)
- Teste diferentes posições

# 2. Verificar pino
- Meça comprimento do pino
- Pino deve recuar completamente para fora do tubo
- Troque por pino mais longo se necessário

# 3. Testar mola
- Meça força da mola com dinamômetro
- Compare com torque do servo
- Troque mola mais fraca se necessário

# 4. Verificar atrito
- Limpe pino e furo do tubo
- Verifique se não há rebarbas
- Aplique lubrificante (PTFE spray)
```

---

### Paraquedas não se infla

**Sintomas:**
- NC se solta, mas paraquedas não abre
- Foguete cai em queda livre
- Paraquedas sai mas não infla

**Causas possíveis:**
1. Paraquedas muito pequeno
2. Paraquedas preso na mola
3. Shock cord muito curto
4. Dobrado incorretamente

**Soluções:**
```bash
# 1. Verificar tamanho
- Calcule área conforme fórmula no README
- Paraquedas deve ser proporcional ao peso

# 2. Verificar obstruções
- Confirme que paraquedas não está preso na mola
- Verifique que NC não obstrui saída
- Teste deploy manualmente

# 3. Verificar shock cord
- Comprimento mínimo: 2x tubo
- Ideal: 3x comprimento do tubo
- Shock cord curto impede que NC se afaste

# 4. Verificar dobra
- Dobre em sanfona (zig-zag)
- Enrole levemente
- Não aperte demais
- Teste deploy 3x antes do voo
```

---

### Deploy prematuro (antes do apogeu)

**Sintomas:**
- Paraquedas abre durante subida
- Foguete perde estabilidade
- Voo interrompido prematuramente

**Causas possíveis:**
1. Mola forte demais (pressão constante)
2. Pino de retenção frouxo
3. Vibração solta o mecanismo
4. Falso positivo no sensor

**Soluções:**
```bash
# 1. Verificar mola
- Mola deve comprimir com servo fechado
- Não deve exercer pressão excessiva
- Teste: NC deve ficar retido sem esforço

# 2. Verificar pino
- Pino deve encaixar firmemente no furo
- Sem folga excessiva
- Teste puxando NC manualmente

# 3. Verificar vibração
- Teste mecanismo com furadeira (vibração)
- Deve permanecer fechado por 5 minutos
- Adicione trava se necessário (parafuso extra)

# 4. Verificar firmware
- Confirme lógica de detecção de apogeu
- Teste em simulador antes do voo
- Use múltiplos critérios (barômetro + IMU)
```

---

### Deploy não acontece (foguete não recupera)

**Sintomas:**
- Foguete atinge apogeu
- NC não se solta
- Foguete cai sem paraquedas

**Causas possíveis:**
1. Falha na detecção de apogeu
2. Conexão servo perdida durante voo
3. Servo falhou sob vibração
4. Bateria descarregou

**Soluções:**
```bash
# 1. Verificar detecção de apogeu
- Analise logs do flight-computer
- Verifique se barômetro funcionou
- Confirme se algoritmo detectou apogeu

# 2. Verificar conexões pós-voo
- Inspecione conectores (soltaram?)
- Verifique strain relief (falhou?)
- Teste continuidade

# 3. Verificar servo pós-voo
- Teste servo isoladamente
- Verifique se horn não soltou
- Inspecione pino (entortou?)

# 4. Verificar alimentação
- Meça tensão da bateria pós-voo
- Bateria pode ter descarregado durante voo
- Use bateria dedicada para servo
```

---

### Ruídos estranhos no servo

**Sintomas:**
- Servo faz som de "travando"
- Movimento irregular
- Corrente elevada

**Causas possíveis:**
1. Mecanismo com atrito
2. Servo com defeito
3. Alimentação instável
4. Mola desalinhada

**Soluções:**
```bash
# 1. Verificar atrito
- Desmonte e limpe mecanismo
- Verifique alinhamento
- Aplique lubrificante

# 2. Testar servo
- Conecte a Arduino
- Teste movimento suave
- Se travar, troque servo

# 3. Verificar alimentação
- Meça tensão durante movimento
- Deve ser estável (~5V)
- Adicione capacitor se necessário

# 4. Verificar mola
- Confirme alinhamento
- Verifique se não está torta
- Troque se necessário
```

---

## Diagnóstico Rápido

### Antes do Lançamento

```bash
# Teste rápido (2 minutos)
1. Conectar servo ao flight-computer
2. Ligar sistema
3. Acionar deploy manualmente
4. Verificar:
   - [ ] Servo se move
   - [ ] Pino recua
   - [ ] NC se solta
   - [ ] Paraquedas sai
   - [ ] Sem ruídos estranhos
5. Repetir 3 vezes
```

### Após Falha em Campo

```bash
# Diagnóstico pós-falha (5 minutos)
1. Recuperar foguete
2. Verificar estado físico:
   - [ ] NC solto ou preso?
   - [ ] Paraquedas saiu?
   - [ ] Servo intacto?
   - [ ] Conectores OK?

3. Teste elétrico:
   - [ ] Continuidade dos fios
   - [ ] Tensão na bateria
   - [ ] Sinal PWM presente?

4. Teste mecânico:
   - [ ] Servo funciona?
   - [ ] Mola OK?
   - [ ] Pino se move?
```

---

## Quando Substituir Componentes

| Componente | Substituir quando |
|------------|-------------------|
| Servo | Travamento persistente, ruído, corrente alta |
| Mola | Perda de elasticidade, deformação |
| Pino | Entortamento, desgaste excessivo |
| Shock cord | Desgaste, cortes, perda de elasticidade |
| Paraquedas | Rasgos, furos, desgaste do tecido |
| Conector | Contato frouxo, pinos danificados |

---

## Contato

Se não encontrar solução aqui:

1. Abra uma [Issue](https://github.com/Serra-Rocketry/recovery/issues)
2. Consulte as [Boas Práticas](https://github.com/Serra-Rocketry/best-practices)
3. Fale com a equipe no canal de comunicação

---

[← Voltar ao índice](../README.md)
