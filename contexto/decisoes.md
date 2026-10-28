# Decisões Travadas

## EXODASH
- Nome do carro vem do Prefs (manual), não de IA
- Bateria via AT RV (não PID 02 42)
- Tela não apaga (FLAG_KEEP_SCREEN_ON)
- DTC via Mode 03, parseDTCs com regex
- Groq system prompt ampliado (automotivo + geral)
- Encoding UTF-8 forçado no GroqClient

## EXO VIGILANTE
- Sem PIN, sem biometria (segurança está no hardware)
- Fonte da verdade do token ring: hardware
- Latch obrigatório (não momentary)
- Redundância: celular + multimídia + porta-fusível
- Cor: dourado #D4A017 + preto azulado #0D1117

## BOSS
- IA: Groq primário, OpenRouter reserva
- Memória: GitHub (JSON append-only)
- Interface: web (PWA)
- Sem servidor próprio
- Chave no localStorage
## Interação por Voz (EXODASH)
- O EXO possui "Modo Follow-up" de 10 segundos.
- Quando o EXO termina de falar (evento onDone do TTS), o microfone abre automaticamente por 10s para perguntas de acompanhamento.
- Se o usuário não falar nada em 10s, o microfone dorme em silêncio (evita gasto de bateria e falso positivo).
- O áudio do TTS tem um listener para avisar a MainActivity que a fala terminou.
## EXO VIGILANTE (Decisões)
- O app será distribuído gratuitamente em fóruns (isca de marketing), por isso não ficará no GitHub.
- O código passará por blindagem (R8/ProGuard) antes de ser lançado.
- A lógica de permissão exige BLUETOOTH_CONNECT (Android 12+).
- O status "Armado" corta o circuito visualmente na tela.
