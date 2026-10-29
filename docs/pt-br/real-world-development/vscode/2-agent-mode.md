---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir alterações em vários arquivos e operações como executar testes. Em geral, a pessoa desenvolvedora precisa localizar os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra alteração e repetir o processo.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar com mais autonomia no ambiente de desenvolvimento integrado (IDE). Ele se comporta de forma semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Veja como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage por meio de issues e pull requests.

Neste exercício, você vai:

- explorar como o modo de agente do Copilot identifica arquivos relevantes e faz alterações coordenadas.
- usar o modo de agente do GitHub Copilot para implementar funcionalidades na interface do usuário e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. Você já adicionou um helper de editoras no exercício anterior. Agora, conclua com o modo de agente do Copilot o trabalho restante na camada de dados, na interface do usuário e nos testes.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para entender suas funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro, que renderiza as páginas como linguagem de marcação de hipertexto (HTML) estática durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle, um mapeador objeto-relacional (ORM). Não há uma interface de programação de aplicações (API) de backend separada nem um framework de interface do usuário no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com pequenos elementos `<script>` do Astro, limitados ao escopo necessário, que usam as APIs padrão do DOM.

### Iniciar o site

O projeto oferece a tarefa `npm run dev`, que inicia o servidor de desenvolvimento do único aplicativo Astro. Execute-a no GitHub Codespaces:

1. Volte ao Codespace. Depois do exercício anterior, você deverá estar na branch `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute este comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, deverá aparecer uma mensagem semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora está hospedando o servidor web.

### Explorar o site

Com o site em execução, explore suas principais funcionalidades:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página de detalhes com mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Prepare uma conversa limpa no Codespace antes de continuar. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar quando isso for importante.
>
> 1. Volte ao Codespace.
> 2. Abra o painel do Copilot Chat.
> 3. Selecione **New Chat**.

A implementação inicial do site funciona, mas queremos adicionar recursos. Comece revisando o backlog. A configuração do projeto a partir do modelo criou automaticamente um conjunto de issues. Peça ao GitHub Copilot que as liste.

1. Selecione **Agent** no seletor de agentes da exibição Chat. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e chama ferramentas.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Este workshop não recomenda um modelo específico. Durante sua criação, usamos Claude Sonnet 4.5 e o incluímos nas instruções para tentar manter as sugestões relativamente consistentes. Como os modelos de linguagem de grande porte (LLMs) são probabilísticos, as sugestões podem ser diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Como os modelos de linguagem de grande porte (LLMs) são probabilísticos, o Copilot pode usar um comando do Model Context Protocol (MCP) diferente, mas ainda deverá conseguir concluir a tarefa.

3. Envie este prompt para perguntar sobre o backlog:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los de acordo com o que será mais útil para os usuários.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot priorizou os itens de acordo com o que considera mais útil para os usuários.

## Revisar os arquivos de instruções

Antes de pedir ao agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso permite reutilizar orientações, dividir conjuntos complexos de instruções em partes menores e fornecer exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, você precisará fazer pelo menos três alterações no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes para o comportamento dos filtros.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes da mesclagem. O modo de agente do Copilot pode executar essas tarefas.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como o arquivo de instruções.

   ![Captura de tela mostrando como selecionar o arquivo de instruções da interface do usuário](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no seletor de agentes da exibição Chat.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade da issue relacionada no backlog:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria, de acordo com os requisitos da issue relacionada no backlog do GitHub. Já existe um helper de editoras do exercício anterior; preserve e aprimore esse trabalho conforme necessário para concluir as alterações restantes na camada de dados, na interface do usuário e nos testes. Confirme que todos os testes foram aprovados antes de concluir. O servidor já está em execução, então não precisa iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo os helpers da camada de dados, a interface do usuário e os testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela mostrando uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e alguns testes falhem durante o processo. Não há problema: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão concluída do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você explorou como usar o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu a:

- explorar o projeto, identificar arquivos relevantes e fazer alterações coordenadas com o modo de agente do Copilot.
- usar o modo de agente do GitHub Copilot para implementar funcionalidades na interface do usuário e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora, [teste a funcionalidade com o servidor Playwright MCP][next-lesson] e abra um pull request.

### Exercício de exploração bônus — implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação.

1. **Opcional:** peça ao Copilot para adicionar paginação ao site. Ao preparar a solicitação:
   - siga as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções disponíveis.
   - decida como a paginação funcionará: se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
   - forneça ao Copilot as orientações necessárias para implementar o comportamento desejado.
   - faça iterações com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
