---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial (IA) especializados em tarefas ou áreas específicas do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de codificação e conhecimento de domínio para ajudá-lo a executar determinados tipos de trabalho com mais eficiência. As equipes podem transformar sua experiência em agentes reutilizáveis: um agente de acessibilidade que garante conformidade com as diretrizes de acessibilidade para conteúdo Web (WCAG), um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem um frontmatter em YAML (YAML Ain't Markup Language) com pelo menos `name` e `description`, seguido por um prompt em Markdown que define o comportamento, a experiência e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm alguns pontos em comum. Ambos são definidos principalmente em arquivos Markdown e dizem à IA como executar operações. A distinção mais simples é: o **agente personalizado** é quem executa o trabalho, e as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e coordenam skills — e até outros agentes — como parte do trabalho. Neste exercício, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Como parte desse trabalho, ele poderá chamar skills como uma skill de workflow de pull request ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere até encontrar o que funciona no seu ambiente e nos seus cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com os agentes personalizados, você vai explorar:

- como os agentes personalizados são definidos;
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre texto e fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você quer implementar esse recurso o mais rápido possível. Para gerar a funcionalidade, usará um agente personalizado.

## Revise o agente personalizado de acessibilidade

Um agente personalizado para acessibilidade já foi criado. Revise o conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace e examine o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - as principais responsabilidades ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript;
   - problemas e erros comuns.

## Crie e atribua uma issue

O painel de agentes é o centro de controle para trabalhar com todos os agentes do seu ambiente. Você pode atribuir tarefas ao Copilot cloud agent, acompanhar o trabalho e até redirecionar uma sessão em andamento ou fornecer orientações adicionais. Vamos começar atribuindo ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de uma nova issue.
4. Selecione **Blank issue** para criar a issue.
5. Defina `Add high contrast mode to website` como **Title**.
6. Em **Description**, insira:

   ```plaintext
   Precisamos de um modo de alto contraste para o site. Deve haver um controle que o usuário possa usar para ativá-lo. A configuração deve ser armazenada no local storage do navegador.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

   ![Captura da atribuição do cloud agent, com o agente personalizado e a opção de acessibilidade destacados](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e áreas específicas. Com agentes personalizados, você pode transformar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos;
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha no modo de alto contraste, você pode [monitorar e orientar a sessão do agente pelo painel de controle][next-lesson].

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: GitHub Copilot cloud agent][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
