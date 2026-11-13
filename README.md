# BOSS- — Assistente de Engenharia da EXODRIVE

Assistente de IA e consultor técnico do ecossistema **EXODRIVE Sistemas Automotivos**. Opera sobre os projetos **EXODASH** e **EXO VIGILANTE**, com memória persistida no GitHub.

---

## 📚 Índice de Contexto

Toda a base de conhecimento do BOSS vive em `contexto/`:

| Arquivo | Função |
|---|---|
| `contexto/diretrizes_ia.md` | Regras de engenharia, arquitetura travada, gerador de prompts |
| `contexto/decisoes.md` | Decisões técnicas já tomadas (ExoDash, Vigilante, BOSS) |
| `contexto/memoria_codigo.md` | Mapa completo de arquivos do ExoDash |
| `contexto/projetos.md` | Descrição dos projetos + manuais de instalação/operação |
| `contexto/memorias/chefe.md` | Diário de bordo do operador |
| `contexto/memorias/backup/` | Histórico e relatórios anteriores |

Base de conhecimento adicional em `conhecimento/` (elétrica automotiva, veículos).

---

## 🚗 Projetos do Ecossistema

### 1. EXODASH
HUD automotivo Android para Ford Focus MK1 2001 (1.8 Zetec).

- **Stack:** Kotlin + Android XML puro (sem Compose, sem OpenGL/SurfaceView/Canvas).
- **Comunicação OBD:** USB Serial (`usb-serial-for-android`).
- **Voz:** `MainVoiceInteractionService` → `BossService` → `BossBrain` → `GroqClient`.
- **Funcionalidades confirmadas:** telemetria OBD-II, leitura de DTCs (Mode 03), assistente de voz com follow-up de 10s, auto-update via GitHub Releases.
- **Mapa de código completo:** ver `contexto/memoria_codigo.md`.

### 2. EXO VIGILANTE
Módulo de segurança veicular (imobilizador invisível via relé Bluetooth oculto no circuito do fusível).

- **Stack:** Kotlin, minSdk 24.
- **Comunicação:** Bluetooth SPP com módulo HC-05 (UUID SPP padrão).
- **Hardware:** relé Bluetooth 4 canais, falso fusível.
- **Comandos hex:** `A0 01 01 A2` (Armar) / `A0 01 00 A1` (Desarmar). Senha padrão: `1234`.
- **Tema:** `#0D1117` (fundo) + `#D4A017` (dourado).
- **Status do hardware:** *A definir* — aguardando placa de relé (última informação registrada em `contexto/projetos.md`).

### 3. BOSS
Este assistente.

- **IA:** Groq (primário), OpenRouter (reserva).
- **Memória:** GitHub (JSON append-only).
- **Interface:** PWA.
- **Infra:** sem servidor próprio; chave no `localStorage`.

---

## ⚠️ Regras de Operação do BOSS

1. **Regra de Ouro:** nunca gerar prompt de engenharia sem ter o código em mãos. Sem arquivo lido → pedir leitura primeiro.
2. **Modo Estrito:** prompt para Codespace = texto puro, começando com o aviso de `cd /workspaces/NOME && code -r .` seguido de `@workspace`.
3. **Arquitetura travada:** UI em XML puro, lógica em Kotlin, OBD via USB, Vigilante via Bluetooth SPP. Mudanças exigem decisão explícita registrada em `contexto/decisoes.md`.

---

## 🧩 Estado do Repositório

| Item | Status |
|---|---|
| Base de contexto (`contexto/`) | ✅ Populada |
| Mapa de código ExoDash | ✅ Completo |
| Manuais de instalação/operação | ✅ Em `contexto/projetos.md` |
| Memória do operador (`chefe.md`) | ⚠️ Placeholder — a popular |
| Config Firebase / RAG | ❓ A definir — sem fonte no repositório |
| Versão atual do ExoDash | ❓ A definir — sem tag/versão nos arquivos |
| Status real do hardware Vigilante | ❓ A definir — última info pode estar desatualizada |

---

## 🛠️ Desenvolvimento

- **Ambiente:** GitHub Codespaces (`.devcontainer/` no repo do ExoDash).
- **Build ExoDash:** Gradle Kotlin DSL (`build.gradle.kts`, `gradlew`).
- **CI:** `.github/workflows/release.yml` no repo do ExoDash.

---

*Documento mantido pelo BOSS. Toda alteração de arquitetura deve ser registrada em `contexto/decisoes.md` antes de refletir aqui.*