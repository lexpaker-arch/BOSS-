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
