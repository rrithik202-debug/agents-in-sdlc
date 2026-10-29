---
title: "Exercício 5: Fazer iterações no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo controles de modo de alto contraste e modo claro. Esta revisão permite avaliar as alterações e, se necessário, pedir melhorias ao Copilot.

### Cenário

Como já destacamos, os fundamentos de design de software e DevOps não mudam com a inclusão da IA generativa. O código gerado ainda precisa passar pelo processo normal de DevOps e ser revisado antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o ambiente protegido:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e temporário.
- Uma pessoa precisa aprovar a execução de qualquer fluxo de trabalho do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores do Model Context Protocol (MCP).

## Revisar a documentação gerada

A primeira revisão aborda o pull request (PR) gerado pelo agente de nuvem do GitHub Copilot para adicionar documentação ao código. Você fará a revisão pela interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso informando que o firewall bloqueou o GitHub Copilot. Isso **é esperado**, pois o acesso a recursos externos, incluindo servidores do Model Context Protocol (MCP), é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários de documentação TypeScript (TSDoc) e outra documentação. As alterações exatas variam.

    Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizados que você examinou no início do Exercício 2.

6. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo ocorrer como esperado, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários do PR ou em comentários no código, assim como faria com outra pessoa da equipe. O Copilot lerá esses comentários e iniciará outra sessão para tratar das solicitações. Como os resultados não são determinísticos, não há um texto específico a recomendar. Você pode pedir, por exemplo, para:

- adicionar um breve cabeçalho de comentário ao início de cada arquivo de código.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias acima ou outra sugestão de documentação para o código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, deverá aparecer um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Se quiser continuar fazendo iterações na documentação, você pode solicitar outras alterações e, quando estiver satisfeito com o resultado, preparar o PR para revisão.

6. **Opcional:** continue fazendo iterações até que o resultado esteja satisfatório.
7. Quando estiver satisfeito, converta o PR de rascunho em pronto para revisão e mescle-o à branch principal.

   ![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

O próximo PR a revisar foi gerado pelo Copilot para resolver a issue de exibição de jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
6. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo ocorrer como esperado, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** faça um teste manual da funcionalidade de jogos relacionados em seu Codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot implementou a funcionalidade de jogos relacionados. Se quiser solicitar ajustes, lembre-se de que essa tarefa foi atribuída **antes** da atualização das instruções personalizadas; por isso, o agente de nuvem não aplicará automaticamente essas instruções.

10. **Opcional:** solicite ao Copilot, por meio de um comentário na guia **Conversation**, que ajuste a quantidade de jogos exibidos ou adicione cabeçalhos de comentário e comentários TSDoc. O Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

O PR final contém os recursos de acessibilidade implementados pelo agente personalizado: o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde alguns minutos até que ele termine.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, especialmente:
   - os componentes de alternância entre modos.
   - o uso do armazenamento local para persistir as preferências.
   - as alterações de folhas de estilo em cascata (CSS) e estilo para os modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos de Aplicações de Internet Ricas e Acessíveis (ARIA) e navegação por teclado.
   - o código JavaScript/TypeScript que controla a alternância entre modos.

6. Depois de revisar as atualizações, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo ocorrer como esperado, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** teste manualmente os recursos de acessibilidade no Codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se as preferências são mantidas após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot para implementar esses recursos seguindo práticas recomendadas de acessibilidade.

11. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário e solicite atualizações, como nos PRs anteriores.

## Exercício opcional — continuar delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. O exercício opcional a seguir ajuda a desenvolver esse hábito:

1. **Opcional:** crie issues para tarefas como:
   - criar um formulário de interesse de apoiadores na página de detalhes do jogo.
   - implementar paginação na página de listagem de jogos.
   - adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Inspecionou os arquivos de instruções personalizados do repositório** para observar seus efeitos no trabalho futuro do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para implementar recursos de acessibilidade, incluindo os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como centro de controle** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e aprimorou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para voltar ao agente de nuvem.

No seu próprio repositório, experimente estas atividades:

- atribuir ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Exercício anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
