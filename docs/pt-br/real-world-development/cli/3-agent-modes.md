---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para implementar a filtragem descrita em uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto, mas alterações maiores exigem um processo mais robusto. O GitHub Copilot CLI foi desenvolvido para se integrar ao fluxo de trabalho da organização e ajudar a desenvolver as funcionalidades certas da maneira certa. Nesta e nas próximas lições, você seguirá um processo típico de desenvolvimento com agentes: partirá de uma issue para criar uma funcionalidade, verificará o código e o comportamento e, por fim, mesclará as alterações ao projeto.

> [!NOTE]
> Você continuará o fluxo de trabalho da funcionalidade na mesma conversa e branch. Em geral, convém usar branches ou PRs diferentes para tipos distintos de alteração, mas vamos simplificar o processo para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar a funcionalidade no modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador encaminhado.

Ao continuar este trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação com MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como combinar categorias precisam ser definidos antes de escrever o código. Você usará o modo Plan para decidir esses detalhes e, em seguida, autorizará uma implementação delimitada com o Autopilot.

## Contexto

Adicionar agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos; na verdade, eles se tornam ainda mais importantes. Em geral, o fluxo de trabalho de desenvolvimento inclui:

1. Começar por uma issue registrada que descreva o trabalho necessário.
2. Criar um plano para o que será desenvolvido.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Mesclar o código depois que ele for revisado e o processo de integração contínua for concluído com êxito.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos fluxos segue alguma variação dessas etapas.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma: escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Antes de começar a filtragem, prepare o Codespace, o repositório e o terminal.

1. Volte ao seu Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR das classificações por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite adicionar ao contexto uma issue ou um pull request do GitHub.

## Planejar a filtragem

O planejamento permite definir a abordagem de implementação de uma funcionalidade ou de execução de uma tarefa antes de delegá-la ao Copilot. Para tarefas complexas, é recomendável dedicar algum tempo ao planejamento. Vamos pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite este prompt:

   ```plaintext
   Crie um plano para implementar esta funcionalidade.
   ```

O Copilot começará a explorar o projeto e a definir a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot para revisar as partes que não correspondam às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, implemente-o usando o Autopilot. Esse modo permite que o Copilot itere até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com nome semelhante na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Como qualquer outro código, o código gerado precisa ser revisado antes da mesclagem. Revise-o e execute o site para verificar se tudo está funcionando.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot para corrigir eventuais falhas.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Inicie o aplicativo como antes e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa testar a filtragem no navegador. Informe a URL e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria, por editora e as combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot para parar o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou a funcionalidade no modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, aprofunde-se em como gerar código que siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
