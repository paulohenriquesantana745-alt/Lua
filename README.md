# Lua — Personal AI System

Primeira geração do aplicativo desktop para Windows, construída com Tauri 2, Rust, React e TypeScript.

## Recursos funcionais

- Central de comando com histórico e conversas persistidas em SQLite.
- Chat com endpoint configurável compatível com `POST /chat/completions` da API OpenAI.
- Teste de conexão pelo endpoint `/models`.
- Chaves de API e PIN guardados pelo Gerenciador de Credenciais do Windows.
- Segunda janela flutuante, independente, compartilhando o mesmo núcleo e banco de dados.
- Conversa ativa sincronizada entre as duas janelas, inclusive novas mensagens.
- Atalho global `Alt + Espaço` para abrir a janela flutuante; fechá-la apenas a oculta.
- Comandos de texto “Lua, entre no modo assistente flutuante” e “Lua, volte para a central de comando”.
- Memória local editável, configuração MCP importável sem execução e registro de operações.
- Interrupção de emergência no núcleo Rust; políticas e autenticação de PIN fora do modelo de IA.

## Requisitos para desenvolvimento

- Windows 10 ou 11 com WebView2 Runtime.
- Node.js 20+ e pnpm.
- Rust (MSVC) e a carga de trabalho de desenvolvimento C++ para desktop do Visual Studio 2022.

## Executar em desenvolvimento

```powershell
pnpm install
pnpm dev:app
```

Somente a interface web pode ser iniciada com `pnpm dev` em `http://127.0.0.1:1420`.

## Compilar

```powershell
pnpm build
pnpm build:app
```

O executável fica em `src-tauri/target/release/lua.exe`.

Os testes do núcleo Rust podem ser executados com:

```powershell
cargo test --manifest-path src-tauri/Cargo.toml
```

## Conectar seu modelo

Abra **Configurações → Modelo de inteligência artificial** e informe o endereço base e o identificador do modelo. Para um servidor local como LM Studio, por exemplo, use `http://localhost:1234/v1`. A chave é opcional para servidores locais. Chaves em endpoints remotos exigem HTTPS e ficam no Gerenciador de Credenciais do Windows.

O botão **Testar conexão** faz uma consulta autenticada a `/models`. Ao enviar uma conversa, a Lua envia o histórico daquela conversa para o endpoint configurado. Não há transmissão de áudio em espera nesta versão.

## Dados locais

Conversas, memórias, preferências, tarefas e auditoria ficam no banco `lua.sqlite3` dentro da pasta de dados do aplicativo do Windows. Segredos não são gravados nesse banco.

## Recursos ainda não disponíveis

- Captura de áudio, palavra de ativação, identificação de locutor, TTS e seleção de voz.
- Execução de ferramentas do Windows, plugins e servidores MCP.
- Planejamento e execução de tarefas por agentes.
- Memória injetada automaticamente no contexto, inicialização com o Windows e personalização do atalho.

Importar uma configuração MCP guarda metadados apenas: nenhum pacote é instalado ou iniciado. O núcleo ainda não expõe ferramentas de sistema ao modelo.
