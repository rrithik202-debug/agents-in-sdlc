---
title: "Exercício 3 - Testar a funcionalidade com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Exercício anterior: Modo de agente][previous-lesson] · [Próximo exercício: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor Playwright MCP** e deixe o Copilot controlar um navegador real para testar a funcionalidade. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor Playwright MCP ao espaço de trabalho.
- pedir ao Copilot para usá-lo a fim de testar manualmente a filtragem no navegador.
- publicar a branch e abrir um pull request com o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite que o Copilot se comunique com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e como ele interage com contexto, modelo de linguagem grande (LLM) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

O acesso a essas ferramentas e recursos é feito por um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e os serviços externos, como APIs existentes ou ferramentas locais instaladas como pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: fornece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios do GitHub e permite criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: fornece recursos de automação de navegador com o Playwright, como acessar páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis para diferentes ferramentas e recursos. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Em termos de segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente seu código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores em que você confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Observe que já há um servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` vem do modelo do projeto, por isso o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor remoto do GitHub MCP][remote-github-mcp-server], então não é necessário instalar nada localmente: o VS Code se conecta por HTTP e você autentica sua conta do GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicionar o servidor Playwright MCP

Agora, adicione um segundo servidor. O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador controlável, necessário para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` executa o navegador sem uma janela visível, o que é necessário em um Codespace sem ambiente gráfico.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme a confiança no servidor, revise a configuração e escolha a opção para confiar nele.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code o iniciará e pedirá confirmação quando o Copilot precisar das ferramentas no próximo exercício.

## Testar a funcionalidade de filtragem

O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador real. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou.

Nos bastidores, o servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente trabalha com elementos estruturados e rotulados, como botões, links e itens de lista, da mesma forma que a tecnologia assistiva. Uma verificação funcional rápida também pode servir como uma análise básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, você poderá pedir ao Copilot que teste a filtragem:

1. Envie este prompt:

   ```text
   Usando o servidor Playwright MCP, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique a nova funcionalidade de filtragem de jogos:

   1. Acesse a página de jogos e informe quantos jogos aparecem na lista.
   2. Aplique um filtro de categoria e confirme que a lista passa a exibir somente jogos dessa categoria.
   3. Limpe o filtro e aplique um filtro de editora. Confirme que a lista mostra os jogos dessa editora.
   4. Combine um filtro de categoria com um de editora e confirme que os resultados correspondem aos dois filtros.

   Informe o que observou em cada etapa e destaque qualquer comportamento inesperado.
   ```

O Copilot iniciará um navegador pelo servidor Playwright MCP, seguirá as etapas e apresentará os resultados. Compare o relatório com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> Para este teste, o aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor Playwright MCP, talvez seja necessário baixar um navegador. Se o Copilot informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que a funcionalidade funciona, publique a branch `filtering-vscode` para abrir um pull request (PR) e permitir que a equipe revise as alterações.

1. No Codespace, abra o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias formas de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está usando o GitHub Copilot, peça que ele crie o PR. Ele pode localizar a issue correspondente e associá-la ao pull request.

1. Abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma sessão nova.
2. No seletor de agentes, confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot colete informações e execute as operações necessárias. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja solicitado que você autentique sua conta; siga as instruções.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link exibido pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor Playwright MCP ao espaço de trabalho.
- usou o servidor para testar manualmente a funcionalidade de filtragem no navegador antes de publicá-la.
- publicou a branch e abriu um pull request com o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP também pode ampliar o Copilot de outras formas. Em seguida, [crie um agente personalizado][next-lesson] para facilitar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Servidor GitHub MCP][github-mcp-server]
- [Registro de servidores MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
