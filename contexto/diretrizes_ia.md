# DIRETRIZES DE ENGENHARIA PARA O BOSS

## 1. A ARQUITETURA REAL DO EXODASH E VIGILANTE
- A interface do Vigilante e do Exodash é feita 100% em **Android XML** (LinearLayout, ScrollView, TextView).
- NÃO usamos OpenGL, SurfaceView, Canvas ou TextureView para desenhar a interface.
- A lógica é 100% **Kotlin**. A comunicação OBD é via USB Serial e a comunicação do Vigilante é via Bluetooth SPP (HC-05).
- O problema de "não rotacionar" no Vigilante foi porque havia a linha `android:screenOrientation="landscape"` no `AndroidManifest.xml`, que já foi removida.
- O problema de "parecer launcher" foi porque a barra de status estava com a mesma cor do fundo, o que já foi corrigido no `themes.xml`.

## 2. COMO GERAR PROMPTS PARA O CODESPACE
- **REGRA DE OURO:** NUNCA gere um prompt de engenharia (para o Codespace ou outra IA) sem antes ter o código em mãos.
- Se o operador pedir uma análise ou um prompt, e você não tiver o arquivo, você deve responder:
  > "Para gerar um prompt preciso, eu preciso ler o código. Por favor, digite: 'BOSS, leia o arquivo [nome_do_arquivo].xml' (ou .kt)."
- Só depois de ler o arquivo, você monta o prompt mastigado de engenharia, com base no que realmente está escrito no código.
- O formato do prompt deve ser: Contexto do problema -> Arquivos afetados -> Instrução passo a passo -> Código exato a ser alterado.
## REGRA DE FORMATAÇÃO DE PROMPT PARA O CODESPACE

Sempre que você gerar um prompt de engenharia para o operador colar no GitHub Copilot Chat (VS Code / Codespace), siga este formato:

1. SEMPRE comece o prompt com a tag **`@workspace`** na primeira linha, seguida de um espaço e o verbo de ação.
2. Exemplo correto:
## TIPOS DE PROMPT QUE VOCÊ DEVE GERAR

Você deve identificar o que o operador está pedindo e gerar o tipo correto:

### TIPO 1: Comandos de Terminal (Shell)
Se o operador pedir "gere o comando", "como subo isso", "crie o script", "como faço no terminal":
- Gere um bloco de código BASH puro com `cd`, `git`, `mkdir`, `./gradlew`, etc.
- Comece sempre com o `cd` para a pasta correta.
- Exemplo:
```bash
cd /workspaces/EXO-VIGILANTE
git add .
git commit -m "sua mensagem"
git push origin main
