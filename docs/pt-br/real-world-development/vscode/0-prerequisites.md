---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare tudo o que você precisa. Você criará uma cópia do repositório Tailspin Toys, iniciará um [Codespace][codespaces] para trabalhar e confirmará que o GitHub Copilot Chat está funcionando no editor.

## Configure o repositório do laboratório

Para criar uma cópia do repositório em que você desenvolverá o código, crie uma instância a partir do [repositório de modelo][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Na página do repositório do laboratório, selecione **Use this template** e, em seguida, **Create a new repository**.

   ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um workshop organizado pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

   ![Configurações do repositório de modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele mais adiante.

> [!NOTE]
> **O backlog está pronto**
>
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você trabalhará nessas issues durante todo o workshop — não precisa criar nenhuma.

## Crie um Codespace

Em seguida, use um Codespace para concluir os exercícios do laboratório.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione o botão verde **Code**.

   ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, em seguida, o botão **+** para criar um Codespace.

   ![Criar um Codespace](../../../_images/ex0-create-codespace.png)

A criação do Codespace levará alguns minutos, mas ainda será muito mais rápida do que instalar todos os serviços manualmente. Enquanto isso, você pode explorar outros recursos do GitHub Copilot, que serão abordados a seguir.

> [!CAUTION]
> Você voltará ao Codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi preparado para ser executado em um Codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Ambos incluem os pré-requisitos necessários para uma experiência tranquila. Para executar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner de desenvolvimento usado pelo Codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Use o GitHub Copilot Chat e o modo de agente

Para usar o modo de agente do GitHub Copilot Chat, você precisa ter a extensão GitHub Copilot Chat instalada no IDE. Ela já deve estar instalada se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, você poderá [instalá-la pelo Visual Studio Code Marketplace][copilot-chat-extension]. Outra opção é abrir a visualização **Extensions** no Visual Studio Code, pesquisar **GitHub Copilot Chat** e selecionar **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar sua conta do GitHub.

1. Volte ao Codespace.
2. Se o Copilot Chat não aparecer à direita do editor, selecione o ícone **Copilot Chat** na parte superior do Codespace.
3. Digite uma mensagem, como `Hello world`, na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deve ativar o Copilot Chat.
4. Se você ainda não estiver autenticado, será solicitado que entre na sua conta do GitHub. Siga as instruções para autenticar.

   ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Após a autenticação, a janela do Copilot Chat será exibida.

## Resumo

Você criou uma cópia do repositório do laboratório e iniciou a criação de um Codespace, que usará quando começar a escrever código.

## Próxima etapa

Vamos começar a usar o Copilot. Continue para o [exercício 1: Instruções personalizadas][next-lesson], no qual você ensinará ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
