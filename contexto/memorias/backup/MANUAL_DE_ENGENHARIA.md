# MANUAL DE ENGENHARIA E ARQUITETURA (ECOSSISTEMA EXODRIVE)
**Data:** 29 de Outubro de 2026
**Status:** Consolidação de Histórico (533 interações)
**Operador:** Alex P. (Lexpaker)

## 1. FILOSOFIA DO PROJETO
- **Segurança Física:** A proteção do carro (VIGILANTE) é por invisibilidade (falso fusível), não por criptografia no app.
- **100% Offline (quando possível):** O EXODASH não depende da nuvem para OBD ou telemetria. O BOSS é o único que usa rede (DeepSeek API).
- **Simplicidade Sênior:** Não criar arquiteturas complexas (SurfaceView, Hilt, etc.) se o XML e o Kotlin puro resolvem.

## 2. ARQUITETURA DO BOSS (A IA CONSULTORA)
- **Frontend:** `docs/index.html` no repositório `CHEFE-` (GitHub Pages). Tem dois layouts (Jarvis Orb e Chat Discreto) e dois temas (Ardósia e AMOLED).
- **Backend (Cérebro):** Cloudflare Worker (`boss-proxy`).
  - Motor Primário: **DeepSeek API** (`deepseek-chat`). Chave configurada nas variáveis do Worker.
  - Motor de Reserva (Fallback): Groq API (`qwen/qwen3.8-27b`).
- **Memória:** O BOSS lê e escreve arquivos `.md` no GitHub.
  - Memória diária: `contexto/memorias/chefe.md` (Compilada via botão "Encerrar Sessão").
  - HD Externo (Backup): `contexto/memorias/backup/` (Lido sob demanda sem gastar tokens de IA).
- **Regras de Prompt:** O arquivo `contexto/diretrizes_ia.md` dita que prompts para o Codespace devem ter `@workspace` e comandos bash devem ser puros.
- **Voz:** TTS nativo do navegador. Bug do Chrome Android exige o Samsung Browser para funcionar 100%.

## 3. EXODASH (O LAUNCHER AUTOMOTIVO)
- **Status:** Estável. V3.2.1.
- **Hardware:** OBD2 USB (ELM327) via protocolo ISO 9141-2.
- **UI:** Layout premium (fundo metálico, botão EXO arc reactor). Fontes Inter (títulos) e Roboto Mono (dados). Tela fixa em landscape na multimídia.
- **Funcionalidades:**
  - Leitura de OBD: Velocidade, RPM, Temperatura, Combustível.
  - DTCs: Histórico local em JSON, triângulo de alerta na tela, limpeza de DTC via app.
  - **EXO (Assistente):** Voz com TTS limpo (remove markdown). **Modo Follow-up de 10s:** após o TTS falar, o microfone abre por 10s sozinho. Se o usuário não falar, fecha.
- **Auto-Update:** Workflow do GitHub Actions compila o APK e gera Release. O app baixa e instala sozinho.
- **Erro comum:** O BOSS (DeepSeek) pode achar que o EXODASH precisa de refatoração de rotação. **IGNORE.** O EXODASH é um launcher fixo de multimídia.

## 4. EXO VIGILANTE (A SEGURANÇA AUTOMOTIVA)
- **Status:** Em desenvolvimento local (o GitHub deu erro 403 no push).
- **Hardware:** Relé Bluetooth HC-05 (4 canais). O relé é escondido na caixa de fusíveis do carro. O app envia comandos hexadecimais (A0 01 01 A2 para armar, A0 01 00 A1 para desarmar).
- **UI:** Splash Screen com anel giratório (Cofre) e a marca EXODRIVE. Tela principal com diagrama visual de circuito (Bateria -> Relé -> ECU). Linha contínua (Desarmado) e linha quebrada dourada (Armado).
- **Código:**
  - `BluetoothService.kt`: Busca dispositivos pareados com nome "HC-05" ou "HC-06" e usa UUID SPP padrão.
  - `MainActivity.kt`: Pede permissão BLUETOOTH_CONNECT (Android 12+) e gerencia o estado do circuito.
  - `AndroidManifest.xml`: Removemos `screenOrientation="landscape"` e o tema foi alterado para não parecer um Launcher.
- **Problema Atual:** A tela no G7 não está adaptando ao tamanho do celular e a rotação parece falhar. O layout usa ScrollView, mas pode precisar de ajustes no ConstraintLayout.
- **Problema de Infra:** O Codespace não tem permissão para dar push no repositório `EXO-VIGILANTE` no GitHub (Erro 403). Precisa configurar um Token Classic com escopo `repo`.

## 5. LIÇÕES APRENDIDAS (ERROS HISTÓRICOS)
1. Não confiar em modelos de IA antigos (Qwen) para refatorações grandes. Eles inventam SurfaceView e quebram o código. Usar DeepSeek.
2. Nunca colocar a chave de API no `index.html`. Sempre usar o Cloudflare Worker (Secrets).
3. O Chrome Android bloqueia TTS. O usuário deve usar o Samsung Browser no celular.
4. O `splash.mp4.mp4` (extensão dupla) quebra o build do GitHub. Cuidado ao subir vídeos.
5. O Token Guard do BOSS corta textos acima de 20.000 caracteres para não estourar a API.
