# BOSS-

> Inteligência de engenharia da EXODRIVE. Assistente técnico do Patrão.

---

## 📁 Estrutura do repositório

O cérebro do BOSS vive em `contexto/`. É de lá que ele lê as regras, decisões e o mapa de código dos projetos.

| Arquivo | Função |
|---|---|
| `contexto/diretrizes_ia.md` | Regras de operação e personalidade do BOSS |
| `contexto/decisoes.md` | Decisões técnicas travadas (não reabrir sem motivo) |
| `contexto/projetos.md` | Estado atual de cada projeto da EXODRIVE |
| `contexto/memoria_codigo.md` | Mapa de código do ExoDash (arquivos, classes, fluxos) |
| `contexto/memorias/` | Diário de bordo e memórias auxiliares |

---

## 🚗 Projetos

### 1. ExoDash

App Android de dashboard automotivo.

- **Stack:** Kotlin + XML puro. Sem Compose, sem OpenGL.
- **OBD:** via USB Serial (`usb-serial-for-android`).
- **Voz:** `MainVoiceInteractionService` → `BossService` → `BossBrain` → `GroqClient`.
- **Decisões travadas:**
  - Nome do carro: manual, via `Prefs`.
  - Bateria: leitura via comando `AT RV`.
  - DTC: via Mode 03 + regex.
  - Follow-up de voz: 10s após `TTS onDone`.
  - Tela: `FLAG_KEEP_SCREEN_ON` ativo.

### 2. Vigilante

Sistema de alarme veicular com relé Bluetooth.

- **Stack:** Kotlin, `minSdk 24`.
- **Comunicação:** Bluetooth SPP (HC-05, UUID SPP padrão).
- **Relé:** 4 canais.
- **Comandos hex:**
  - Armar: `A0 01 01 A2`
  - Desarmar: `A0 01 00 A1`
- **Senha padrão:** `1234`.
- **Cores:** `#D4A017` (destaque) / `#0D1117` (fundo).
- **Status do hardware:** *A definir* — último registro indicava "aguardando placa de relé Bluetooth 4 canais". Confirmar se já chegou.

### 3. BOSS

Assistente de engenharia (este repositório).

- **LLM:** Groq (primário), OpenRouter (reserva).
- **Memória de longo prazo:** Firebase Firestore.
- **Infraestrutura:** Cloudflare Worker faz proxy da Groq — a chave principal fica protegida no Worker. Fallback: cliente chama a Groq direta usando a chave do `localStorage`.
- **Chave da Groq no cliente:** `localStorage`.
- **Interface:** PWA.

---

## ⚙️ Regras de operação

Extraídas de `contexto/diretrizes_ia.md`:

- Tratar o usuário como **Patrão**. Nunca "Chefe".
- Ser técnico e direto. Sem enrolação.
- Não inventar arquitetura. Se não há fonte, marcar como **"A definir"**.
- Toda edição em arquivo grande: `GITHUB_PATCH` com trecho `OLD` único.
- Toda resposta termina com `` (resumo ≤ 180 caracteres).

---

## 📊 Estado do repositório

| Item | Estado |
|---|---|
| Estrutura `contexto/` | ✅ Definida |
| Regras de operação | ✅ Documentadas |
| Mapa de código ExoDash | ✅ Documentado |
| Versão atual do ExoDash | ⚠️ **A definir** (sem tag/versão nos arquivos) |
| Status do hardware Vigilante | ⚠️ **A definir** (confirmar placa de relé) |
| Config Firebase / Firestore | ⚠️ **A definir** (sem arquivo de config neste repo) |

---

## 🛠️ Desenvolvimento

Este repositório é **documentação viva** do BOSS. Não há build, não há deploy — o conteúdo de `contexto/` é lido pelo BOSS em tempo de execução.

Para alterar comportamento: editar os arquivos em `contexto/` e commitar. O BOSS recarrega no próximo ciclo.