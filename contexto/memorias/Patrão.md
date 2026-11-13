# 📖 Diário de Bordo — Patrão.md

> **Fonte da verdade temporal do projeto EXODRIVE.**
> Append-only. Nunca reescrever entradas antigas. Correções viram novas entradas.
> Fatos técnicos vão para o Firestore. Decisões, narrativas e marcos vão para cá.

---

## 🏛️ MARCO 0 — PRÉ-HISTÓRICO (data desconhecida)

**Status:** reconstruído a partir de `contexto/decisoes.md` e `contexto/memoria_codigo.md`. Datas reais perdidas — este é o custo de não ter tido diário até agora.

### Origem do projeto
- **EXODRIVE** nasce como projeto guarda-chuva. Contém dois produtos ativos: **EXODASH** e **VIGILANTE**.
- **BOSS** é a inteligência de engenharia que opera sobre os repositórios do Patrão (`lexpaker-arch`).
- Projeto ativo no momento: `lexpaker-arch/BOSS-`.

### Decisões travadas do EXODASH (herdadas)
- **Arquitetura de modos:** EXODASH opera em modos distintos com convenções próprias (ver `memoria_codigo.md`).
- **Convenções de código:** documentadas em `memoria_codigo.md`, seção de convenções.
- **Fluxos principais:** mapeados em `memoria_codigo.md`.

### Decisões travadas do VIGILANTE (herdadas)
- **Modelo de distribuição:** grátis em fóruns (decisão de negócio).
- **Segurança:** sem PIN (decisão de UX/segurança — ver justificativa em `decisoes.md`).
- **Latch:** comportamento de latch definido em `decisoes.md`.
- **Comunicação:** AT RV em vez de PID 02 42 (decisão de protocolo).

### Decisões travadas do BOSS (herdadas)
- **Groq como provider primário** de inferência.
- **Modo follow-up de 10s** para continuidade de contexto.
- **Governança:** decisões em `contexto/decisoes.md`, diretrizes em `contexto/diretrizes_ia.md`.

> ⚠️ **Nota de auditoria:** todas as decisões acima foram **recuperadas de snapshot**, não de log. O "quando" e o "porquê" originais se perderam. Este Marco 0 é a tentativa de reconstruir o filme a partir de fotos.

---

## 📅 [DATA DA SESSÃO ATUAL] — Auditoria de memória e aprovação da Opção C

**Contexto:** o Patrão pediu auditoria do sistema de memória do BOSS. Diagnóstico apontou 8 erros estruturais, sendo os 3 críticos: memória write-only (Firestore sem leitura), fonte de verdade duplicada e ambas vazias, e diário prometido mas nunca escrito.

**Decisões aprovadas nesta sessão:**

1. **Opção C — Híbrido Honesto** aprovada como arquitetura de memória:
   - Fatos técnicos → Firestore (via `[MEMORIA_ADD]`).
   - Decisões, narrativas e marcos → este arquivo (`Patrão.md`) via `[GITHUB_PATCH]` append.
   - Regra: toda decisão de arquitetura aprovada ou etapa concluída gera **uma linha aqui no mesmo turno**. Sem exceção.

2. **`decisoes.md` vira índice temático** (estado atual), com cabeçalho apontando para este diário como fonte histórica.

3. **`memoria_codigo.md` ganha carimbo** "Última verificação: [data]" no topo.

4. **Regra Absoluta nova:** toda vez que o BOSS ler `memoria_codigo.md` e encontrar divergência com o código real, deve **avisar e atualizar no mesmo turno**.

5. **Firestore fica como buraco negro por enquanto.** Prioridade é o GitHub. O `index.html` na máquina do Patrão já tem `carregarMemoriaFirebase()` ativa — leitura será resolvida no próximo boot.

**Arquivos afetados:** `contexto/memorias/Patrão.md` (criado), `contexto/decisoes.md` (cabeçalho a adicionar), `contexto/memoria_codigo.md` (carimbo a adicionar).

**Pendências abertas:**
- [ ] Adicionar cabeçalho de índice temático em `decisoes.md`.
- [ ] Adicionar carimbo "Última verificação" em `memoria_codigo.md`.
- [ ] Confirmar leitura do Firestore no próximo boot do `index.html`.
- [ ] Popular este diário com marcos retroativos conforme forem lembrados.

---

## 🧭 PENDÊNCIAS GERAIS (abertas)

| # | Pendência | Origem | Status |
|---|-----------|--------|--------|
| 1 | Firestore read (buraco negro) | Auditoria de memória | ⏳ Aguardando boot |
| 2 | Cabeçalho em `decisoes.md` | Opção C | 🔜 Próximo turno |
| 3 | Carimbo em `memoria_codigo.md` | Opção C | 🔜 Próximo turno |
| 4 | RAG traz pergunta sem resposta | Auditoria | 🔴 Aberto |
| 5 | Patch sem grep de unicidade | Auditoria | 🔴 Aberto |
| 6 | Commit sem verificação pós-escrita | Auditoria | 🔴 Aberto |
| 7 | Alucinação de ação (simulação de MEMORIA_ADD) | Auditoria | 🟡 Mitigado por regra nova |

---

## 📅 2026-11-13 — v8.2: DeepSeek primário, Mermaid render(), robustez mantida

**Contexto:** Patrão atualizou o `index.html` para **v8.2**, aplicando correções auditadas e novas integrações.

### 1. DeepSeek V4.1 Flash (provider primário)
- `deepseek-flash` vira o **modelo principal** de inferência.
- Fluxo de fallback: **DeepSeek → Worker (Groq) → Groq Direto**.
- Novo campo de chave DeepSeek no menu de configurações.
- `[WEB_SEARCH]` e `[WEB_FETCH: url]` migradas para APIs nativas do DeepSeek (Search/Fetch) — economiza chamadas do Worker e melhora o RAG.

### 2. Correção do Mermaid (bug visual)
- `mermaid.run()` substituído por `mermaid.render()` — SVG gerado direto no container.
- CSS ajustado: `min-height` + centralização; barra de rolagem vazia eliminada.
- Código Mermaid inválido agora cai no `try/catch` e exibe caixa vermelha — não finge sucesso.

### 3. Segurança e robustez (herdadas da v7.3, mantidas)
- Firebase com sintaxe nativa correta.
- `GITHUB_PATCH`: `escapeRegex`, delimitadores `<<<OLD>>>/