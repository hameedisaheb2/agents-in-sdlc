---
title: "Lição 10 - Revisão e próximos passos"
description: "Revise o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem já criada;
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e fez o merge manual do primeiro pull request (PR);
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no modo Autopilot e revisou no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e a usou para executar testes unitários, lint e verificações de tipos;
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador;
- revisou toda a alteração de filtragem e autorizou o Agent Merge no PR de filtragem;
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop inclui dois marcos de PR, cada um em sua própria branch baseada na `main` atualizada:

1. **Avaliações por estrelas:** exiba o `starRating` existente e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implemente a filtragem, atualize as instruções e aplique-as ao recurso, personalize o relatório de `quality-checks`, crie um perfil de QA e inclua os testes relacionados.

Desde o planejamento da filtragem até a abertura do PR, você usou a mesma conversa e branch. Para simplificar o workshop, reunimos todo esse trabalho em um único PR.

## Diferentes formas de verificação

Você verificou o código de várias maneiras: com testes automatizados, inspecionando o navegador e usando o MCP para que o Copilot explorasse o site. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. Antes do PR, o QA reuniu esses resultados com a revisão dos requisitos e da cobertura de testes.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não precisa de novos testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de fazer alterações.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em diferentes conversas e ajuste-as às necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especializadas e servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para avaliar uma abordagem antes de implementar, **Interactive** para acompanhar de perto alterações específicas e **Autopilot** para tarefas com escopo bem definido. Escolha um modelo mais rápido para edições rotineiras e um modelo mais avançado para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Explicar claramente *o que* você quer criar, *por que* e *como* faz uma diferença significativa no resultado.

## Explore mais

Você concluiu o fluxo principal. Vale a pena conhecer mais alguns recursos da CLI:

- `/review` para pedir que o agente de revisão de código analise as alterações;
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva;
- `/fleet` para coordenar subtarefas independentes em paralelo;
- `/worktree` para isolar outra tarefa;
- `/delegate` para enviar uma tarefa ao GitHub Copilot cloud agent.

## Próximas etapas

A melhor maneira de melhorar no uso de qualquer ferramenta é continuar usando-a. Experimente em código de produção, projetos pessoais ou naquele pequeno aplicativo que você pensa em criar há anos. Compartilhe o que aprender com a equipe e aprenda com a experiência dos outros. E, como sempre, explore a documentação.

Para ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do Copilot cloud agent][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
