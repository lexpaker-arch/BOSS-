# MANUAL DE DIAGNÓSTICO ELÉTRICO AUTOMOTIVO (EXODRIVE)

## 1. FUNDAMENTOS DE MEDIÇÃO

### 1.1 Tensão de Referência (VRef)
- Sensores passivos (potenciômetros, NTC) recebem uma tensão de referência da ECU.
- **Valor padrão:** 5.0V (± 0.2V)[reference:0].
- Se a VRef estiver baixa, o sensor gerará leituras errôneas. Verifique o chicote por alta resistência ou curto-circuito[reference:1].

### 1.2 Queda de Tensão (Voltage Drop) - Limites Máximos
A queda de tensão é o teste definitivo para resistência parasitária. Um ohmímetro pode não detectar oxidação; a queda de tensão sob carga, sim[reference:2].

| Componente / Trecho | Queda Máxima (12V) |
| :--- | :--- |
| **Por conexão/terminal** | **0.1 V** (100 mV)[reference:3] |
| **Por cabo/fio** | **0.2 V** (200 mV)[reference:4] |
| **Circuito total (lado positivo)** | **0.5 V**[reference:5] |
| **Circuito total (lado terra)** | **0.1 V a 0.2 V** (tolerância mais rígida)[reference:6] |
| **Circuito de partida (positivo)** | **0.5 V** (sob carga de arranque)[reference:7] |
| **Circuito de partida (terra)** | **0.2 V** (sob carga de arranque)[reference:8] |

**Tabela de referência rápida (OCTools):**
- Interruptores: **300 mV**
- Condutor/cabo: **200 mV**
- Terra: **100 mV**
- Conectores do cabo da bateria: **50 mV**
- Conexões: **0.0 V**[reference:9]

## 2. SISTEMA DE CARGA E BATERIA

### 2.1 Teste de Bateria em Repouso
- **12.6 V** = 100% de carga (bateria saudável)[reference:10].
- **12.3 V a 12.8 V** = Carga suficiente para teste[reference:11].
- **Abaixo de 12.3 V** = Menos de 50% de carga; recarregar antes de testar[reference:12].
- **Abaixo de 12.0 V** = Descarga profunda; risco de sulfatação.

**Atenção:** A tensão de repouso **não** indica capacidade de corrente (CCA). Uma bateria pode marcar 12.6V e colapsar sob carga de arranque[reference:13].

### 2.2 Teste do Alternador (Sistema de Carga)
Com o motor em marcha lenta e todos os acessórios desligados:
- **Leitura normal:** **13.2 V a 15.2 V**[reference:14].
- **Leitura acima de 15.0 V:** Regulador de tensão defeituoso (risco de ferver a bateria)[reference:15].
- **Leitura abaixo de 13.0 V (com cargas ligadas):** Alternador incapaz de suprir a demanda; verificar correia, regulador ou diodos[reference:16].

**Teste de queda de tensão no cabo do alternador:**
- Positivo do alternador para positivo da bateria: **< 0.5 V** (com todas as cargas ligadas)[reference:17].
- Negativo do alternador para negativo da bateria: **< 0.5 V**[reference:18].
- Se a leitura for **> -0.25 V** (mais negativo que -0.25V), há problema no cabo positivo[reference:19].

## 3. SISTEMA DE PARTIDA

- **Consumo do motor de arranque:** 150 A a 250 A (veículos de passeio).
- **Queda de tensão durante o arranque:** A tensão da bateria não deve cair abaixo de **9.6 V**[reference:20].
- **Teste de corrente de arranque:** Se a leitura for **inferior a 150 A**, pode indicar solenoide defeituoso ou resistência excessiva no circuito[reference:21].

## 4. SENSORES E ATUADORES

### 4.1 Sensores Passivos (Resistivos)
- **Teste:** Medir resistência com o sensor desconectado (ohmímetro) **ou** medir a queda de tensão com a VRef de 5V aplicada[reference:22].
- **Sensores de temperatura (NTC):** A resistência **diminui** com o aumento da temperatura. Verifique a tabela do fabricante para o par temperatura × resistência[reference:23].

### 4.2 Sensores Ativos (Efeito Hall / Indutivos)
- **CKP (Rotações):** Indutivo gera tensão AC; Hall gera onda quadrada de 5V. Medir em **VAC** durante o arranque para o indutivo; **VDC** para o Hall[reference:24].
- **CMP (Fase):** Hall de 3 fios. Medir sinal em **VDC**; se o multímetro não for conclusivo, usar osciloscópio para ver a forma de onda[reference:25].

### 4.3 Sonda Lambda (O2)
- **Sinal oscilante:** **0.1 V a 0.9 V** (alternando rapidamente).
- **Travada em 0.45 V:** Sonda inativa (circuito aberto ou aquecimento queimado).
- **Travada acima de 0.8 V:** Mistura rica.
- **Travada abaixo de 0.2 V:** Mistura pobre.

### 4.4 Bobinas de Ignição
- **Primário:** 0.4 Ω a 1.5 Ω.
- **Secundário:** 5.000 Ω a 15.000 Ω.
- **Bobina de 3 pinos (ignitor embutido):** Pino 1 = 12V, Pino 2 = Terra, Pino 3 = Sinal de pulso da ECU.
- **Bobina de 2 pinos (centelha perdida):** Pino 1 = 12V, Pino 2 = Pulso negativo da ECU.

## 5. REDE CAN BUS

### 5.1 Teste de Resistência (Sistema Desligado)
- Medir entre **CAN_H** e **CAN_L** com a bateria desconectada.
- **Valor esperado:** **≈ 60 Ω** (dois resistores de terminação de 120 Ω em paralelo)[reference:26].
- **≈ 120 Ω:** Uma terminação está faltando (circuito aberto)[reference:27].
- **≈ 40 Ω ou menos:** Terminação extra ou curto-circuito[reference:28].

### 5.2 Teste de Tensão (Sistema Ligado, Barramento Ocioso)
- **CAN_H:** ≈ **2.5 V**
- **CAN_L:** ≈ **2.5 V**[reference:29].

### 5.3 Tensão Durante Estado Dominante
- **CAN_H:** ≈ **3.5 V**
- **CAN_L:** ≈ **1.5 V**[reference:30].

### 5.4 Osciloscópio
- Sinal diferencial limpo: aproximadamente **2 V de swing** (CAN_H - CAN_L)[reference:31].
- Reflexões e ringing indicam terminações inadequadas ou cabos de má qualidade[reference:32].

## 6. TERMINAIS DIN 72552 (Padrão Alemão)

| Terminal | Função |
| :--- | :--- |
| **30** | Positivo direto da bateria (12V contínuo)[reference:33] |
| **15** | Positivo após chave de ignição[reference:34] |
| **31** | Terra (massa) |
| **50** | Sinal de partida (solenoide do motor de arranque) |
| **61** | Indicador de carga (D+ do alternador) |
| **85** | Terra da bobina do relé |
| **86** | Sinal de acionamento da bobina do relé |
| **87** | Saída de potência do relé (contato NA) |
| **87a** | Saída de potência do relé (contato NF) |
| **B+** | Positivo do alternador (carga) |

## 7. CONSUMO PARASITA (Parasitic Draw)

- **Procedimento:** Desligar tudo, retirar o cabo negativo e conectar o multímetro em série (escala de Ampères).
- **Aguardar:** A rede CAN pode levar até **40 minutos** para "dormir" em veículos modernos.
- **Leitura aceitável:** **0.030 A a 0.050 A** (30 a 50 mA).
- **Acima de 0.100 A:** Consumo excessivo; isolar circuitos removendo fusíveis um a um[reference:35].

## 8. CÓDIGOS DE FALHA (DTC) - PADRÃO OBD-II

- **P0xxx:** Powertrain (motor/transmissão).
- **P1xxx:** Códigos específicos do fabricante.
- **B0xxx:** Carroceria (airbag, conforto).
- **C0xxx:** Chassi (ABS, suspensão).
- **U0xxx:** Rede de comunicação (CAN).
