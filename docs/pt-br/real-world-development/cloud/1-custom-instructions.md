---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Exercício anterior: Pré-requisitos][previous-lesson] · [Próximo exercício: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica ou o Copilot precisar conhecer informações adicionais, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não apenas *o que* você quer, mas também *como* deseja que o trabalho seja feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos com escopo por caminho.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos do VS Code e da CLI, você não executará um prompt *antes e depois* nesta atividade. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, por isso é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções ao revisar os pull requests gerados pelo agente de nuvem neste percurso.

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

Os exemplos no projeto mostram uma abordagem possível para criar arquivos de instruções:

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

Leia os arquivos de instruções incluídos neste repositório: há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início, que define um glob relativo à raiz do repositório. Por exemplo, ele inclui arquivos de teste TypeScript correspondentes a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicionar um novo padrão ao repositório

Agora você adicionará uma regra válida para todo o projeto: a documentação deve ficar no código, em comentários TSDoc e em um cabeçalho de comentário no nível do arquivo. O agente de nuvem usará essa regra nas issues que você atribuir a ele mais adiante.

Antes de editar o arquivo, crie uma branch. O agente de nuvem lerá as instruções da branch de destino da issue, mas você fará o commit das alterações separadamente.

1. No terminal do Codespace, crie uma branch e alterne para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

O arquivo `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Atualize os padrões do repositório para orientar melhor as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões de código, mas não inclui regras de documentação no código.
3. Adicione estas linhas de Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu no exercício anterior, os arquivos de instruções podem ficar no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicos. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Fazer commit, push e mesclar a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. No próximo exercício, ao atribuir uma issue ao Copilot, ele criará uma branch a partir de `main`. Portanto, as atualizações das instruções precisam ser mescladas em `main` para que o agente de nuvem as use.

1. Prepare as alterações e faça o commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça a mesclagem. O agente de nuvem lerá essas instruções ao trabalhar nas issues que você atribuir no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main`, você pode pular a etapa da branch e fazer o commit diretamente nessa branch. A etapa com a branch serve para reproduzir o fluxo de trabalho usado em projetos reais.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções deste projeto e adicionou um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem gerar neste percurso.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
