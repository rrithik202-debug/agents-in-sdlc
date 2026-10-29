---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Exercício anterior: Pré-requisitos][previous-lesson] · [Próximo exercício: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica ou o Copilot precisar conhecer informações adicionais, disponibilize esse contexto. Há várias formas de fornecer contexto específico ao Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que orientam a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo por caminho.
- gerar a primeira parte dos dados de filtragem — um helper de editoras — com as instruções *atuais* em vigor.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit das atualizações das instruções e da parte de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após a atualização das instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc (TypeScript Documentation).
- Cada arquivo deve começar com um bloco de comentários que descreva sua finalidade.

Use arquivos de instruções para fornecer ao Copilot as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Elas ajudam a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de código, bibliotecas e tipos de comentários, criando instruções para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações de chat ou da CLI, como a stack usada, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que fornecem orientações para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface do usuário ou novos conjuntos de testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código nem sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam arquivos no nível do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a outros formatos de arquivos de instruções, como AGENTS.md e CLAUDE.md; consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos no projeto mostram uma abordagem possível para criar arquivos de instruções. Em linhas gerais:

- Mantenha as orientações em `copilot-instructions.md` no nível do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões gerais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro e camada de dados) ou tarefas.
- Use linguagem natural e orientações claras. Inclua exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira certa de criar arquivos de instruções nem de usar IA. Experimente para descobrir o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste projeto, você encontrará arquivos para vários tipos de tarefa, incluindo [atualizações de interface do usuário][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa função de uma forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. O exercício para cada interface destacará esses recursos quando forem relevantes.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui arquivos de instruções. Explore o conteúdo existente antes de fazer alterações para perceber o efeito delas.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início, que define um glob relativo à raiz do repositório. Por exemplo, ele inclui arquivos de teste TypeScript correspondentes a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Criar uma branch para as alterações

Crie uma branch antes de começar a fazer alterações.

1. Volte ao Codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e alterne para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

Para observar o impacto das instruções personalizadas, envie um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar os resultados.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Prepare uma conversa limpa no Codespace antes de continuar. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar quando isso for importante.
>
> 1. Volte ao Codespace.
> 2. Abra o painel do Copilot Chat.
> 3. Selecione **New Chat**.

1. Feche as guias de editor abertas nas atividades anteriores para que o Copilot tenha apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Selecione **Agent** no seletor de agentes da exibição Chat para permitir que o Copilot altere arquivos.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e o id de cada uma. Aplique as alterações nos arquivos.
   ```

5. O Copilot explorará o projeto e aplicará alterações de código, possivelmente ao helper e aos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Não há problema: o objetivo continua sendo observar a melhora da *consistência* depois da atualização das instruções.

## Adicionar um novo padrão ao repositório

O arquivo `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Atualize os padrões do repositório para orientar melhor as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões de código, mas ainda não inclui regras de documentação no código.
3. Adicione estas linhas de Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu no exercício anterior, os arquivos de instruções podem ficar no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicos. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto da conversa.
2. Selecione `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o mesmo prompt de antes:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e o id de cada uma. Aplique as alterações nos arquivos.
   ```

4. Observe que o arquivo proposto começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão de projeto e aplicá-lo ao código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são recursos do repositório e devem ser gerenciados pelo controle do código-fonte, assim como os demais arquivos. Faça commit das alterações e envie a branch.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme se as atualizações das instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Depois de enviar a branch, crie um pull request e associe-o à issue do backlog sobre o padrão de documentação. O Copilot pode encontrar a issue, criar um PR para fechá-la e mesclar o PR.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub em seu nome. O modelo do projeto já configura as ferramentas do GitHub no espaço de trabalho, então não é necessário configurá-las. Na primeira vez que o Copilot usar uma delas, o VS Code talvez peça para você entrar no GitHub. Siga as instruções para permitir o acesso. Você conhecerá essa conexão, baseada no Model Context Protocol (MCP), em um exercício posterior.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot para encontrar a issue sobre a atualização dos arquivos de instruções e criar um PR da branch atual, descrevendo também a base do helper de editoras:

   ```plaintext
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um pull request da branch atual, informe que ele fecha essa issue e mencione também que adicionamos a base do helper de editoras para o trabalho de filtragem que faremos em seguida.
   ```

4. Aguarde enquanto o Copilot encontra a issue e cria o PR.
5. Quando o Copilot pedir permissão para executar ações do GitHub em seu nome, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois da criação do PR, peça ao Copilot para mesclá-lo e voltar à branch `main`:

   ```plaintext
   Mescle o PR em main. Depois, volte para main localmente e obtenha o código mais recente para atualizar o repositório.
   ```

7. Quando o Copilot pedir permissão para executar ações do GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow** se estiver de acordo.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções deste projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit da atualização das instruções e da base do helper em `main` para que o próximo exercício possa usá-las.

Em seguida, aplique essas instruções ao [modo de agente][next-lesson] enquanto o Copilot adiciona uma funcionalidade ao projeto.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
