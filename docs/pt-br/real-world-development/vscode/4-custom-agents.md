---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de código e conhecimento do domínio para orientar o trabalho. As equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há certa sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA na execução de tarefas. A distinção mais simples é: **o agente personalizado é quem executa o trabalho; as skills são as ferramentas**.

Agentes personalizados têm sua própria janela de contexto e podem coordenar skills e até outros agentes durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site conforme as diretrizes de acessibilidade. Para isso, ele poderia usar skills de fluxo de trabalho de pull request ou de execução e gerenciamento de testes.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em qualquer uso de IA, teste e faça iterações para descobrir o que funciona em cada ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para gerar essa funcionalidade rapidamente.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Uma branch nova baseada em `main` mantém esse trabalho separado do PR de filtragem do Exercício 3.

1. No Codespace, alterne para uma nova branch baseada em `main`:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código em linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript.
   - armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` no seletor de agentes, na parte inferior da exibição Copilot Chat. Você pode selecionar um agente personalizado para aplicar as instruções e as ferramentas dele à sessão de chat.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Prepare uma conversa limpa no Codespace antes de usar o agente. O modo e o modelo variam conforme o exercício; cada etapa informa o que selecionar quando isso for importante.
>
> 1. Volte ao Codespace.
> 2. Abra o painel do Copilot Chat.
> 3. Selecione **New Chat**.

1. Se ainda não estiver selecionado, escolha **Agent** no seletor de agentes da exibição Chat.

   ![Captura de tela mostrando o seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abra o seletor de agentes na parte inferior da exibição Chat. Ele mostra o agente ativo, que por padrão é **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

   ```plaintext
   Adicione ao site um modo de alto contraste. Deve haver um controle para ativar e desativar o alto contraste, e a preferência deve ser mantida após recarregar a página usando o armazenamento local do navegador.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. Acompanhe o indicador **Files changed** para ver os arquivos alterados pelo agente.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot está fazendo alterações reais no repositório: ele pode editar componentes Astro, CSS e testes relacionados.

Você acompanhará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com agentes personalizados, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis para orientar o trabalho do Copilot.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações enquanto são feitas e solicite a inclusão de um controle de modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Exercício anterior: Testar a funcionalidade com o servidor Playwright MCP][previous-lesson] | [Próximo exercício: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
