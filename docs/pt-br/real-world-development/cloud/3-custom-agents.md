---
title: "Exercício 3 - Agentes personalizados"
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

Neste exercício, você vai explorar:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para gerar essa funcionalidade rapidamente.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Revise o arquivo para entender como ele orientará o Copilot.

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código em linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript.
   - armadilhas e erros comuns.

## Criar e atribuir uma issue

O centro de controle de agentes permite atribuir tarefas ao agente de nuvem do Copilot, acompanhar as atividades e redirecionar o trabalho ou fornecer orientações adicionais. Atribua ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse o repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

    ```plaintext
    Precisamos de um modo de alto contraste para o site. Deve haver um controle para ativá-lo, e a preferência deve ser armazenada no armazenamento local do navegador.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição do agente de nuvem, com o agente personalizado Accessibility selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com agentes personalizados, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis para orientar o trabalho do Copilot.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, [acompanhe e oriente a sessão do agente][next-lesson] pelo centro de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Exercício anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próximo exercício: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
