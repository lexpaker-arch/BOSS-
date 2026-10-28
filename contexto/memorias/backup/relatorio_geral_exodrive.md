# RELATÓRIO TÉCNICO E ESTRATÉGICO: ECOSSISTEMA EXODRIVE
**Data de Fechamento:** 28 de Outubro de 2026
**Arquiteto:** Alex P. (Lexpaker)
**Status:** Fase de Validação e Distribuição

## 1. VISÃO GERAL E ESTRATÉGIA DE NEGÓCIOS
A EXODRIVE Sistemas Automotivos é o guarda-chuva da marca. A estratégia é o modelo "Cavalo de Troia":
- **O Produto Físico (Isca):** O app EXO VIGILANTE será distribuído gratuitamente em fóruns de automotivos. A ideia é gerar comoção, viralizar a ideia do "falso fusível" (invisibilidade física) e criar audiência.
- **O Produto Comercial:** O EXODASH será vendido (ex: R$ 10,00 + atualizações) na Play Store, focado em carros antigos (1996-2015) com interface premium e OBD2.

---

## 2. PROJETO 1: EXODASH (O Launcher Premium)
**Status:** Funcional, compilando via GitHub Actions e com Auto-Update configurado.
- **Interface:** Design Premium "Dark Mode Absoluto". Fundo metálico profundo (#0D1117), acentos em ciano (#00FFCC) e dourado (#D4A017). Fontes nativas: Inter (títulos) e Roboto Mono (dados técnicos).
- **OBD2 e DTCs:** Leitura via USB (ELM327 / ISO 9141-2). Parser próprio, incluindo código para ler DTCs via Mode 03 e limpar via Mode 04.
- **Inteligência de Voz:** O assistente EXO tem o "Modo Follow-up": após o TTS terminar de falar, o microfone abre automaticamente por 10 segundos.
- **Melhorias no Código:** Forçamos o encoding UTF-8 no GroqClient; implementamos limpeza de Markdown para o TTS não ler asteriscos; implementamos FLAG_KEEP_SCREEN_ON para o display não apagar no carro.
- **Dados Customizados:** O sistema suporta JSON específico por carro (ex: Focus 2001) via script `trocar-carro.sh`. Se não houver JSON, usa a base genérica `dtc_codes.json`.

---

## 3. PROJETO 2: EXO VIGILANTE (O Imobilizador Invisível)
**Status:** Código do app pronto, aguardando testes com a placa física (HC-05).
- **Hardware:** Relé Bluetooth 4 Canais (HC-05). A segurança não é criptográfica (refutamos a ideia de Token Ring), mas sim **física**: o relé fica oculto dentro de um falso fusível. O carro não tem energia no fusível, a placa não tem energia.
- **Comandos da Placa:** `A0 01 01 A2` (Armar / Abre Relé) e `A0 01 00 A1` (Desarmar / Fecha Relé).
- **Interface do App:** Splash Screen estilo "Cofre" giratório com a marca EXODRIVE no rodapé. Tela principal com um diagrama de circuito visual (Bateria -> Relé -> ECU).
  - *Estado Desarmado:* Linha contínua, cinza, "Relé Conectado".
  - *Estado Armado:* Linha quebrada, dourada, "Relé Desconectado".
- **Segurança do App:** O app foi blindado para não parecer um Launcher (roda em portrait/landscape nativo, exige permissões de BLUETOOTH_CONNECT no Android 12+). Ativamos V1/V2 Signing no Gradle para rodar em centrais multimídia antigas.

---

## 4. PROJETO 3: BOSS (A Inteligência Artificial Consultora)
**Status:** MVP Concluído e Operacional (com memória de longo prazo).
- **Arquitetura:** Frontend PWA (GitHub Pages) -> Proxy de Segurança (Cloudflare Worker) -> LLM (Groq / Qwen 3.8 27B) + Banco de Dados (GitHub).
- **Memória e Aprendizado:** O BOSS lê arquivos `.md` do GitHub em tempo real. Implementamos a **Memória Compilada**: ele lê a sessão do dia, compila um resumo (para não estourar tokens) e salva no arquivo `chefe.md`.
- **"HD Externo" (Zero Tokens):** O BOSS consegue ler arquivos da pasta `memorias/backup/` diretamente do GitHub sem enviar para a IA. É uma busca local de arquivo, que economiza 100% da cota da API.
- **Token Guard:** Proteção implementada no frontend para não exceder o limite de 8.000 TPM (Tokens por Minuto) da Groq.

---

## 5. LIÇÕES APRENDIDAS (ERROS E ACERTOS)
Nesta jornada, enfrentamos e resolvemos problemas que valem ouro:
1. **Limites de API:** Erro 413 (Request Too Large) no Groq foi resolvido reduzindo a leitura de código cru e adotando resumos e RAG.
2. **GitHub Actions:** O fluxo de trabalho foi corrigido para gerar releases automáticas (`v3.2.1`, etc.) em todo push na `main`.
3. **TTS no Android:** Descobrimos que o Chrome no Android bloqueia o áudio do TTS. A solução foi usar navegadores alternativos (como o Samsung Browser) e adicionar botões de "Ouvir" manuais.
4. **Crash no VIGILANTE:** A `SplashActivity` estava faltando, causando crash. A falta de permissão de Bluetooth no Android 12+ também travava o app. Ambos resolvidos.
5. **Distribuição:** A chave da Groq nunca deve ir para o Git. Usamos o Cloudflare Worker para esconder a `GROQ_API_KEY` e o `GITHUB_TOKEN` (que exige permissão de escrita `repo`).

---

## 6. PRÓXIMOS PASSOS (TO-DO)
- [ ] Testar a comunicação real do app EXO VIGILANTE com a placa relé HC-05.
- [ ] Blindar o APK do VIGILANTE (R8/ProGuard) antes de distribuir nos fóruns.
- [ ] Finalizar o esquema de animação (ObjectAnimator) dos anéis do botão EXO no EXODASH.
- [ ] Publicar o vídeo de demonstração (splash) no ecossistema.
