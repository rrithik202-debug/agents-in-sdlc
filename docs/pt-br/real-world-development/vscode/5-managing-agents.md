---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar diffs, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares, sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar um controle de modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design fez uma solicitação adicional: incluir um controle de modo claro para quem considera o tema padrão escuro demais. Em vez de começar uma conversa nova, oriente a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e acompanhe cada chamada de ferramenta, leitura e edição de arquivo e comando de terminal que ele solicitar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Abra também o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando do terminal, o agente fará uma pausa e exibirá uma solicitação **Allow**. Aprove apenas os comandos que você considera seguros.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Ele deverá exibir uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, amplie a mesma conversa para adicionar um controle de modo claro. Como você continuará na mesma sessão de chat, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o uso do armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

   ```plaintext
   Bom trabalho! Agora adicione um controle de modo claro junto com o controle de alto contraste. O modo claro deve seguir o mesmo padrão: manter a preferência no armazenamento local e ficar disponível na mesma área da interface do usuário. Deve ser possível ativar o alto contraste e o modo claro de forma independente.
   ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição quando aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** para cada diff.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de modo de uma forma que você não quer, pare-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer o que precisa ser alterado.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes, como simplificar o CSS usando variáveis em vez de duplicar seletores; corrigir a abordagem, como adicionar o modo claro ao painel de configurações existente em vez de criar um terceiro controle; ou ampliar o escopo, como adicionar atalhos de teclado aos dois controles.

## Revisar o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Ela deve incluir componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se a implementação segue os padrões de acessibilidade do agente personalizado: atributos de Aplicações de Internet Ricas e Acessíveis (ARIA), navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará [no próximo exercício][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais de arquivos.
- orientou a sessão durante a execução com prompts complementares.

No [próximo exercício][next-lesson], publique seu trabalho de acessibilidade em um pull request e revise as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Exercício anterior: Agentes personalizados][previous-lesson] | [Próximo exercício: Fazer iterações no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
