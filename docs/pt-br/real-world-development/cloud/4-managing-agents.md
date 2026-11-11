---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos dois exercícios anteriores, você pediu ao Copilot cloud agent que realizasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o cloud agent trabalhe de forma autônoma e assíncrona, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao cloud agent, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas dos agentes com pull requests (PRs) abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do cloud agent;
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar uma opção de alternância, pareceu lógico incluir esse recurso. Você vai orientar o agente para que ele adicione também o modo claro.

## Revise as tarefas do GitHub Copilot cloud agent

Vamos verificar o status atual de todas as tarefas atribuídas ao Copilot cloud agent.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código;
   - gerar interfaces de programação de aplicações (APIs) para modificar jogos;
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações realizadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório;
   - preparar o ambiente para o Copilot trabalhar;
   - configurar servidores do Model Context Protocol (MCP);
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exatas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull requests, exibido à direita. Ele permite ver o PR e os arquivos alterados para continuar o monitoramento.

## Oriente o cloud agent

Agora que você viu as tarefas ativas, vamos pedir ao Copilot que adicione o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato dependerá do nome escolhido pelo Copilot e do estado atual do trabalho.

   ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você saberá que isso aconteceu quando aparecerem mensagens semelhantes às seguintes.
3. Na caixa de diálogo **Steer active session while Copilot is working**, insira o prompt:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Esse modo também deve ter um controle para que as pessoas escolham como preferem exibir o site.
   ```

   ![Captura da tarefa do cloud agent na página de agentes, com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento e inclui a solicitação no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot continuará trabalhando na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Como isso levará alguns minutos, é um bom momento para fazer uma pausa e refletir sobre tudo o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro de controle para monitorar e orientar as tarefas do GitHub Copilot cloud agent. Nela, você acompanha tarefas ativas e concluídas, revisa o trabalho realizado e redireciona sessões em andamento para ajustar o escopo ou fornecer novas orientações.

Você explorou estes conceitos:

- usar o centro de controle e a página de agentes para monitorar tarefas do cloud agent;
- redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, você pode [iterar sobre os pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
