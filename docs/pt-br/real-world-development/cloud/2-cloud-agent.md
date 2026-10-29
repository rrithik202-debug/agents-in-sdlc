---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações não enfrentam algum tipo de dívida técnica. Pode ser uma questão de segurança ainda não resolvida, código legado que precisa de atualização ou uma solicitação de funcionalidade que ficou no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request (PR) em rascunho para uma pessoa desenvolvedora revisar. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em objetivos mais amplos.

Neste exercício, você vai explorar como usar o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir a segurança das operações.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver parte da dívida técnica. A documentação deixada pelas pessoas contratadas para criar a primeira versão do site está em estado inadequado: na prática, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. A equipe gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando o catálogo. Por enquanto, não é necessária uma implementação refinada; basta algo que permita testar a experiência do usuário (UX). Embora essa tarefa seja um impedimento, há issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e que são boas candidatas para atribuição ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras tarefas e, depois, você revisa o trabalho concluído.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho em equipe, há várias maneiras de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois que ela terminar. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não faz mágica: você ainda precisa pensar no problema. Até operações aparentemente simples podem ter complexidades escondidas, então [delimite cuidadosamente as tarefas atribuídas ao agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez o exercício prático sobre instruções personalizadas, você viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos no repositório para observar seus efeitos mais adiante.

Para observar mais tarde a influência das instruções, leia estes arquivos na interface web do GitHub ou em um Codespace que já esteja em execução:

1. Abra `.github/copilot-instructions.md` e examine a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
2. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções e serão mencionadas novamente em um exercício posterior de revisão de PRs.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O desenvolvimento de código costuma exigir um ambiente específico e scripts de configuração para garantir que tudo esteja em boas condições. Isso também se aplica às tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente com um [fluxo de trabalho especial de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do agente começar a trabalhar. Esse fluxo dá acesso às ferramentas de desenvolvimento e dependências necessárias. Ele já está configurado neste laboratório para facilitar as atividades e garantir que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos navegadores do Playwright para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para estas etapas. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se as etapas de configuração clonarem o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não houver clonagem, o Copilot fará isso automaticamente depois da conclusão destas etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend e aplicativo: Node.js (o aplicativo agora é todo em Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local com dados de teste.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`. Esse trabalho é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se ele funciona, sem esperar o Copilot executá-lo.

## Adicionar documentação

Embora todo mundo reconheça a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica pode reduzir a produtividade, dificultar a manutenção do código e tornar mais difícil integrar novas pessoas à equipe. O Copilot pode ajudar a criar documentação, então esta é uma boa issue para atribuir ao agente de nuvem. Ele trabalhará em segundo plano, e você revisará o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Nossa organização exige que funções e métodos tenham comentários TSDoc quando forem úteis. No entanto, atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação nos casos em que eles esclareçam o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-criado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde alguns instantes e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente ao executar o fluxo de trabalho descrito anteriormente, analisando o projeto e definindo a melhor abordagem.

13. Revise a lista e as tarefas que o agente pretende executar.
14. Role a linha do tempo do pull request até encontrar uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve o problema: ele explora o código e o estado do projeto, faz uma pausa para pensar e definir um plano e, em seguida, cria o código.

O trabalho pode levar alguns minutos. Um dos objetivos principais do agente de nuvem do Copilot é executar tarefas de forma assíncrona, deixando você livre para cuidar de outras atividades. Aproveite esse recurso atribuindo outra tarefa ao agente e, enquanto ele trabalha, desenvolva outras funcionalidades do aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho: você pode se concentrar em uma tarefa enquanto o agente cuida de outra. Mesmo que a seção de jogos relacionados não leve muito tempo para ser implementada, esse tempo pode ser usado em outras atividades. Atribua essa tarefa ao agente de nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer testes de aceitação. Estes são os requisitos:

   - Adicione em `src/lib/` um helper de acesso a dados que retorne outros jogos da mesma categoria de um jogo, sem incluir o próprio jogo.
   - Exiba na página de detalhes do jogo uma seção "Related games" que use esse helper.
   - Trate o caso em que um jogo não tenha jogos relacionados.
   - Crie testes unitários para o novo helper.
   - Antes de criar o PR, confirme que todos os testes foram aprovados.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, um símbolo 👀 deverá aparecer no primeiro comentário da issue, indicando que o Copilot começou a trabalhar nela.

![Emoji de olhos indicando que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de maneira semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente; revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. Use-o para resolver dívidas técnicas, criar funcionalidades ou ajudar na migração de código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir a segurança das operações.
- entender a importância de issues bem delimitadas.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, você pode criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e receber instruções personalizadas, que foram abordadas em exercícios anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próximo exercício: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
