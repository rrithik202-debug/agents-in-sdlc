---
title: "Lição 8 - Criar e mesclar o PR da funcionalidade"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes. Depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora, revise tudo em conjunto e abra um pull request. Você mesclou o pull request (PR) das classificações por estrelas manualmente; desta vez, deixe que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Em geral, separaríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch, e incluiremos todo esse trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar toda a alteração da funcionalidade e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Durante o fluxo de trabalho de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização para mesclar sob o controle da pessoa desenvolvedora.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request. Quando você o habilita, o Copilot trata os impedimentos do PR — corrige verificações de integração contínua (CI) que falharam, responde a comentários de revisão e faz rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub se o repositório permitir.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código pronto, revise-o, crie o PR e deixe que o Agent Merge gerencie o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Digite `/diff` e revise todos os arquivos alterados, inclusive as atualizações das instruções, da skill, do perfil de QA e dos testes.
5. Leia o relatório do agente de QA e confirme as evidências de verificação antes de continuar.
6. Crie um novo PR com o comando `/pr create`.
7. Ative o Agent Merge com `/pr agentmerge`.
8. O Copilot acompanhará o processo de integração contínua do PR e fará a mesclagem quando todas as verificações forem concluídas com êxito.
9. Confirme que o Copilot exibe uma mensagem semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório ou permissões ausentes. Se o processo for bloqueado, leia o motivo informado e, quando o repositório permitir, faça manualmente a mesclagem já revisada.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração e validação de código e, agora, o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou toda a alteração da funcionalidade e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente após a revisão e confirmou que o PR foi mesclado.

Em seguida, [explore outros comandos slash úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
