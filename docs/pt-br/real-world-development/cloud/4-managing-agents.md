---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Agentes personalizados][previous-lesson] |
|:--|

Nos últimos exercícios, você atribuiu ao agente de nuvem do Copilot três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê as tarefas dos agentes que têm pull requests (PRs) abertos, examina as operações realizadas e orienta sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe decidiu que também seria útil adicionar um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, pareceu natural incluir essa opção. Oriente o agente para que ele adicione o modo claro junto com o modo de alto contraste.

## Revisar as tarefas do agente de nuvem do Copilot

Confira o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Ela deve incluir as tarefas atribuídas ao Copilot, como:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações já realizadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores do Model Context Protocol (MCP).
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel do pull request à direita, que exibe o PR e os arquivos alterados para que você acompanhe o trabalho.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot para incluir a opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

    ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração terminou e o trabalho começou. Você verá mensagens semelhantes às descritas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver um controle para esse modo, permitindo que os usuários selecionem a aparência desejada.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar uma sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Aguardar o trabalho do Copilot

Como antes, o Copilot continuará trabalhando na tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa em andamento quando você enviou a mensagem.

O trabalho pode levar alguns minutos. Aproveite esse tempo para refletir sobre o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro de controle para monitorar e orientar as tarefas do agente de nuvem do GitHub Copilot. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para ajustar o escopo ou fornecer orientações.

Você explorou estes conceitos:

- usou o centro de controle e a página de agentes para acompanhar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, prepare-se para [fazer iterações nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Exercício anterior: Agentes personalizados][previous-lesson] | [Próximo exercício: Fazer iterações no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
