---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare o ambiente. Você criará uma cópia própria do repositório Tailspin Toys, iniciará um [Codespace][codespaces] e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

O laboratório usa uma cópia própria do [repositório de modelo][template-repository], que contém todos os arquivos necessários para as atividades.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Na página do repositório do laboratório, selecione **Use this template** e, em seguida, **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do repositório de modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que você criou (**organization-or-user-name/repository-name**); você precisará dele mais adiante no laboratório.

> [!NOTE]
> **Seu backlog está pronto**
>
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub será criado automaticamente. Você trabalhará com essas issues durante o workshop; não precisa criar nenhuma.

## Criar um Codespace

Use um Codespace para concluir os exercícios do laboratório.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um ambiente de desenvolvimento integrado (IDE) completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione o botão verde **Code**.

    ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um Codespace.

    ![Criar um Codespace](../../../_images/ex0-create-codespace.png)

A criação do Codespace levará alguns minutos, mas ainda é mais rápida do que instalar todos os serviços manualmente. Enquanto isso, explore outros recursos do GitHub Copilot, que serão abordados em seguida.

> [!CAUTION]
> Você voltará ao Codespace em um exercício futuro. Por enquanto, mantenha-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um Codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Ambos fornecem os pré-requisitos necessários para facilitar o trabalho. Para executá-lo localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner de desenvolvimento usado pelo Codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, você precisa ter a extensão GitHub Copilot Chat instalada no IDE. Ela já deve estar disponível se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada:
>
> 1. [Instale-a pelo Marketplace do Visual Studio Code][copilot-chat-extension]. Ou abra a exibição Extensions no Visual Studio Code.
> 2. Pesquise **GitHub Copilot Chat** e selecione **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar sua conta do GitHub para habilitá-la.

1. Volte ao Codespace.
2. Se o Copilot Chat não aparecer à direita do editor, selecione o ícone **Copilot Chat** na parte superior do Codespace.
3. Digite uma mensagem, como `Olá, mundo`, na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deve ativar o Copilot Chat.
4. Se você ainda não estiver autenticado, será solicitado que entre na sua conta do GitHub. Siga as instruções para autenticar.

    ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Depois da autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório e iniciou a criação de um Codespace para usar ao começar a escrever código.

## Próxima etapa

Agora, comece a usar o Copilot. Continue para [Exercício 1: Instruções personalizadas][next-lesson], no qual você ensinará ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
