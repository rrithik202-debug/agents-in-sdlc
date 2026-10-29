---
title: "Lição 5 - Personalizar e usar a skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas escrever código. Já validamos manualmente o funcionamento do código e usamos arquivos de instruções para garantir que ele siga os padrões. E quanto aos testes, ao lint e às outras partes da integração contínua (CI)?

As **skills de agente** são ideais para essas tarefas. Elas ajudam o Copilot a entender como executar operações desse tipo.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega quando necessário. No mínimo, elas consistem em uma pasta com o nome da skill e um arquivo Markdown chamado `SKILL.md`. Esse arquivo contém metadados com um nome e uma descrição, uma visão geral da skill e orientações sobre quando usá-la. A pasta também pode incluir subpastas com scripts e outros recursos.

> [!NOTE]
> Uma skill não precisa de outros arquivos ou pastas. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md` porque usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta de skills do usuário, em `~/.copilot/skills`.

## Explorar a skill

Explore a skill `quality-checks` que a equipe da Tailspin Toys criou para executar testes unitários, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia o `name` e a `description` no início do arquivo. A descrição ajuda o Copilot a decidir quando usar a skill.
3. Leia as instruções e observe como elas orientam o Copilot nas etapas de teste e lint.
4. Observe que a skill ainda não inclui uma seção **Results output formatting**.

## Executar a skill antes de alterá-la

As skills podem ser chamadas diretamente no Copilot CLI ou por meio de linguagem natural. Peça ao Copilot para executar as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Envie este prompt:

   ```plaintext
   Execute a skill quality-checks para rodar testes unitários, lint e verificações de tipos.
   ```

3. Observe o relatório ao final.

## Personalizar o relatório

Melhore o relatório para indicar o que foi executado, se houve êxito e quais resultados as ferramentas apresentaram.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione esta seção ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. Salve o arquivo; a alteração será salva automaticamente.

## Executar a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa. Execute novamente a skill e compare os relatórios.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Execute a skill quality-checks para rodar testes unitários, lint e verificações de tipos.
   ```

3. Observe o relatório e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR da funcionalidade. Em seguida, permita que o Copilot interaja diretamente com o site [usando o servidor Playwright MCP][next-lesson].

## Outros exemplos de skills

Estes exemplos da comunidade servem como referência, não como tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
