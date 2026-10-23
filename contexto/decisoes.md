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
