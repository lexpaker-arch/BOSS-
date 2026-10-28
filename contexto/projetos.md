# Projetos Ativos - EXODRIVE Sistemas Automotivos

A EXODRIVE é o ecossistema de tecnologia automotiva focado em otimização, segurança invisível e inteligência artificial local. 

## 1. EXODASH
- Interface (Launcher) e diagnóstico embarcado.
- Carro foco: Ford Focus MK1 2001 (1.8 Zetec).
- Funcionalidades: OBD2, DTCs, Assistente por voz e Follow-up contínuo de 10s.

## 2. EXO VIGILANTE
- Módulo de segurança (hardware + software).
- Conceito: Imobilizador invisível via relé Bluetooth oculto no circuito do fusível.
- UX: Sem tokens complexos no app. A segurança está no segredo físico (falso fusível).
- Status: Aguardando placa de relé Bluetooth (4 canais).

## 3. BOSS
- Assistente de IA e consultor de engenharia do ecossistema EXODRIVE.
## 4. MANUAL DE INSTALAÇÃO E OPERAÇÃO DO EXODASH (Para Auxiliares)

### Hardware Necessário
1. Central Multimídia Android (ou celular/tablet) com suporte a USB Host/OTG.
2. Adaptador OBD2 USB (recomendado: chips FTDI ou CH340, como OBDLink EX ou Vgate vLinker FS).
3. Cabo OTG (se a central não tiver porta USB tipo A direta).
4. Carro com porta OBD2 (padrão desde 1996).

### Instalação do App
1. Baixe o APK mais recente no GitHub Releases ou via Auto-Update.
2. Instale no Android da central.
3. Conceda as permissões solicitadas: Microfone (para o EXO), Armazenamento (para logs e MP3) e Notificações.
4. Abra o app.

### Configuração Inicial (Setup)
1. Conecte o adaptador ELM327 na porta OBD2 do carro (geralmente embaixo do volante).
2. Ligue a ignição (não precisa ligar o motor).
3. Vá em Configurações (⚙) > Veículo.
4. Digite o "Nome no painel" (ex: FOCUS 1.8 L). Este nome aparecerá em dourado no topo da tela principal.
5. Volte para a tela principal.

### Funcionamento Diário
1. **Telemetria:** Os dados (Velocidade, RPM, Temperatura, Combustível, Bateria) atualizam automaticamente ao ligar o carro.
2. **DTCs (Falhas):** Se houver erro no motor, um triângulo vermelho pisca no canto inferior esquerdo. Basta tocar nele para ler os códigos e a explicação.
3. **Assistente EXO (Voz):** 
   - Toque no botão central "EXO".
   - Fale a pergunta (ex: "Qual o código P0301?").
   - O EXO responde por voz.
   - **Modo Follow-up:** Após a resposta, o microfone abre sozinho por 10 segundos. Você pode fazer uma segunda pergunta sem tocar na tela. Se ficar em silêncio, ele desliga.

### Solução de Problemas (Troubleshooting)
1. **"OBD: OFFLINE" na tela:**
   - Verifique se o cabo USB está bem conectado.
   - Verifique se o adaptador tem uma luz acesa (significa que recebeu energia).
   - Tire o adaptador e coloque de novo com a ignição ligada.
2. **EXO não ouve:**
   - Vá nas configurações do Android > Apps > EXODASH > Permissões > Microfone e permita.
   - Verifique se o "Speech Services by Google" está instalado e configurado como motor de voz padrão.
3. **Erro ao atualizar (Auto-update):**
   - Verifique se a central tem acesso à internet (Wi-Fi ou 4G do celular).
   - O GitHub pode ter bloqueado a conexão temporariamente. Tente novamente mais tarde.
## 5. MANUAL DO TÉCNICO EXODRIVE (Avançado)

### 1. Compatibilidade de Hardware (OBD2)
- **Aprovados:** Adaptadores com chip FTDI (OBDLink EX) ou CH340/CP210x (Vgate vLinker FS). São os únicos que a biblioteca `usb-serial-for-android` reconhece sem falhar.
- **Proibidos:** Clones ELM327 "baratos" que usam chips genéricos ou firmware corrompido. Eles cortam a resposta do Mode 03 (leitura de DTCs) e quebram o app.

### 2. Como a Internet é usada no EXODASH
- **OBD e Telemetria:** 100% OFFLINE. Não precisa de internet para ler o motor.
- **EXO (Assistente de Voz):** Precisa de internet (Groq API). Se o carro não tiver chip 4G, o ajudante deve conectar a central no Hotspot do celular.
- **Consumo:** Cada pergunta ao EXO gasta cerca de 2KB de dados. O uso diário não estoura nenhum plano de celular.
- **Dica do Hotspot:** No celular, deixe o Hotspot ligado e desative a opção "Desativar quando não houver dispositivos conectados".

### 3. Como extrair e enviar Logs (Suporte)
Se o carro der algum erro estranho (ex: app travou, OBD não leu):
1. Abra o EXODASH.
2. Vá em Configurações > Logs de eventos.
3. Toque no botão "EXPORTAR".
4. Escolha o WhatsApp ou Google Drive e envie o arquivo `.txt` para o Chefe.
5. O log contém todos os comandos que o app enviou para a ECU do carro e as respostas.

### 4. Problemas com Voz (TTS e STT)
- **Sintoma:** O EXO não fala.
- **Solução:** Vá nas configurações do Android > Acessibilidade > Saída de texto para fala. Verifique se o "Speech Services by Google" está instalado e definido como motor preferido.
- **Sintoma:** O EXO não ouve (Microfone).
- **Solução:** O navegador/App precisa da permissão de microfone. Em celulares Samsung, o navegador nativo funciona melhor que o Chrome.

### 5. Atualização do Sistema (Auto-Update)
- **Como funciona:** O app verifica automaticamente se há um Release novo no GitHub (versão maior que a instalada).
- **Requisito:** A central precisa estar conectada à internet.
- **Atenção:** Nunca desinstale o app para atualizar. Apenas clique em "Atualizar" na tela do Android para não perder o Nome do Carro e os DTCs salvos.
