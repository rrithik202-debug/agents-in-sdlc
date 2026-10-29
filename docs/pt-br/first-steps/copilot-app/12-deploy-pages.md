---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Implante o Space Quiz com o GitHub Pages para disponibilizá-lo em uma página pública da Web, acessível por um localizador uniforme de recursos (URL). O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas cabe a você revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot para configurar a implantação do GitHub Pages.
- revisar o fluxo de trabalho e o pull request antes da mesclagem.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma forma simples de experimentá-lo. Publicar o site em um único arquivo com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar um servidor ou uma dependência de ambiente de execução (runtime).

## Pedir ao Copilot para implantar o site

O Copilot pode configurar a implantação e informar a URL do site publicado. Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

1. Envie este prompt:

   ```plaintext
   Habilite o GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e o implante no GitHub Pages. Se ainda não houver um README, adicione um que descreva o aplicativo e explique como executá-lo localmente. Crie um pull request com essas alterações e faça a mesclagem depois que as verificações obrigatórias forem aprovadas. Acompanhe a implantação até que ela seja concluída com êxito e informe a URL final do site publicado. Se você não tiver permissão para habilitar o Pages ou mesclar o PR, diga exatamente o que preciso aprovar ou fazer; não informe que a tarefa foi concluída até que o site esteja no ar.
   ```

![Diagrama em três etapas: um pull request do space-quiz mesclado; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz no ar em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e mescle um pull request e altere as configurações do Pages do repositório. Revise as diferenças, as permissões de implantação e todas as solicitações de aprovação do GitHub antes de continuar.

## Revisar o fluxo de trabalho e verificar a implantação

Revise as alterações propostas antes de mesclar e acompanhe a execução até confirmar que o site está disponível.

1. Confirme que o README só foi adicionado se o repositório ainda não tinha um.
2. Revise as alterações do pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho.
3. Mescle somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com êxito.
5. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não conseguir habilitar o Pages ou mesclar o pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site esteja no ar.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site no ar. Nesta lição, você:

- pediu ao Copilot para configurar a implantação do GitHub Pages.
- revisou o fluxo de trabalho e o pull request antes da mesclagem.
- acompanhou a implantação e verificou o site publicado.

Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
