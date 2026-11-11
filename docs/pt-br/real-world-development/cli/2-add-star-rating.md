---
title: "Lição 2 - Adicione avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revise-a em um navegador encaminhado e faça o merge do seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os jogos já têm uma avaliação por estrelas nos dados, mas ela ainda não aparece nos cards da página inicial. Você pedirá ao agente que a exiba, revisará a alteração e fará o merge do seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada com o Copilot em uma branch de recurso;
- pedir ao agente que faça uma pequena alteração no projeto;
- revisar a alteração com `/diff`;
- executar o aplicativo e confirmar a alteração em um navegador encaminhado;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como primeiro exercício, peça ao agente para exibir em cada card a avaliação existente — uma alteração pequena e independente, ideal para sua primeira sessão.

## Como funciona uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a branch do Git atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar o mesmo diálogo mais tarde, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas enquanto ele explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Comece uma nova conversa para implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use o comando a seguir:

   ```bash
   copilot --yolo
   ```

4. Digite o comando de barra `/new` e pressione <kbd>Enter</kbd> para iniciar uma nova sessão.
5. Envie o prompt a seguir para solicitar a alteração:

   ```plaintext
   Exiba a starRating de cada jogo nos cards da página de listagem, com avaliação de no máximo cinco estrelas. Se a avaliação for null, exiba "No rating yet". Mantenha o layout atual dos cards, adicione testes e execute as verificações pertinentes.
   ```

O Copilot explorará o projeto, localizará os arquivos que exibem os detalhes dos jogos e criará o código necessário. Você adicionou um recurso com o Copilot CLI!

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Vamos examinar as alterações no próprio Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o card exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija quaisquer falhas.
5. Ao terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então o código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verifique as alterações

Não basta ler o código e presumir que funciona. Peça ao Copilot para iniciar o site para que você examine a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração da avaliação por estrelas. Informe a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards dos jogos exibem as avaliações com no máximo cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Pare o servidor de desenvolvimento que você iniciou.
   ```

## Abra e mescle seu primeiro pull request

Você criou o recurso. Agora crie um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão para fazer commit da alteração:

   ```plaintext
   Faça commit das alterações revisadas na avaliação por estrelas usando uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI envia o commit existente e exibe a URL do PR.
3. Para abrir o PR, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize a `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Nesta lição, você:

- iniciou uma conversa focada com o Copilot em uma branch de recurso;
- orientou o agente a fazer uma pequena alteração nos cards de jogos;
- revisou a alteração com `/diff`;
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador encaminhado;
- abriu e mesclou seu primeiro pull request.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
