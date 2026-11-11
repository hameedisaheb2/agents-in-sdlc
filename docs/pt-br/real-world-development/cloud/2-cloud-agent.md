---
title: "Exercício 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas com dívida técnica. Isso pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo para implementá-las. O GitHub Copilot cloud agent foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele gera um pull request (PR) em rascunho para revisão de um desenvolvedor. Assim, você pode delegar tarefas tediosas, acelerar o desenvolvimento e se concentrar em questões mais amplas.

Com o Copilot cloud agent, você vai explorar:

- como personalizar o ambiente para gerar código;
- como garantir que as operações sejam realizadas com segurança;
- a importância de definir claramente o escopo das issues;
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica que gostaria de resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — você vai perceber que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação de TypeScript (TSDoc) a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. Ainda não é necessária uma implementação refinada; basta algo que a equipe possa usar para testes de aceitação da experiência do usuário (UX). Isso está bloqueando o trabalho, mas há outras issues de prioridade mais alta no momento.

Essas são tarefas que podem ser facilmente adiadas e que são ótimas para atribuir ao Copilot cloud agent. Ele pode trabalhar nelas de forma assíncrona enquanto você se concentra em outras atividades. Depois, você pode voltar, revisar o trabalho e confirmar se está como esperado.

## Conheça o GitHub Copilot cloud agent

O [GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a um desenvolvedor. Assim como no trabalho com uma pessoa, há várias maneiras de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber uma issue, o Copilot cria um PR em rascunho para acompanhar o progresso, prepara um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto ela está em andamento ou depois que termina. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você no PR.

## A importância de definir bem o escopo das instruções

O GitHub Copilot não faz mágica: não dá para pular a etapa de pensar. Mesmo tarefas aparentemente simples podem ser complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas para o Copilot cloud agent][cloud-agent-best-practices]. Trate-o como um parceiro de programação com inteligência artificial (IA): trabalhe por etapas, aprenda, experimente e adapte a abordagem. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez uma atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; se ainda não, este é um bom momento para conferir. Antes de atribuir trabalho ao Copilot cloud agent, faça uma leitura dos arquivos de instruções já incluídos neste repositório, sem alterá-los. Assim, você poderá identificar o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub do seu repositório ou em um Codespace que já esteja em execução:

- `.github/copilot-instructions.md` — examine a seção **Code standards**, principalmente as expectativas sobre convenções de TypeScript e comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que limita estas instruções aos arquivos `**/*.test.ts`.

Quando atribuir a issue `Code lacks documentation` ao cloud agent na próxima seção, observe se o PR gerado inclui comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários. Essas orientações vêm dos arquivos de instruções e serão retomadas na revisão dos PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do Copilot cloud agent

Criar código normalmente exige um ambiente específico e alguns scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O cloud agent usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [workflow especial de configuração][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes de o agente começar. Isso permite acessar as ferramentas e dependências necessárias. O workflow já foi configurado para este workshop. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos navegadores para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados iniciais para o único aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá seu próprio token para as operações.
    permissions:
      # Para clonar o repositório como parte da configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não fizer isso nas etapas de configuração, o Copilot clonará o repositório automaticamente depois que elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend e do aplicativo - Node.js (agora todo o aplicativo usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. O GitHub Actions o executa antes de o Copilot começar a trabalhar no PR.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente pela guia **Actions** do repositório. Isso é útil para testar se ele funciona corretamente, sem precisar esperar o Copilot executá-lo.

## Adicione documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção do código ou a integração de novos desenvolvedores. O Copilot é especialmente bom em criar documentação, por isso esta é uma issue ideal para atribuir a ele. O agente trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Abra uma nova guia do navegador e acesse seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de uma nova issue.
4. Selecione **Blank issue** para criar a issue.
5. Defina `Code lacks documentation` como **Title**.
6. Em **Description**, insira:

   ```plaintext
   Nossa organização exige que funções e métodos tenham comentários de documentação TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação nos pontos em que esclareçam o comportamento.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-criado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se nenhum PR novo aparecer, aguarde um pouco e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente — executando o workflow mencionado anteriormente —, analisando o projeto e definindo a melhor abordagem.

13. Examine a lista e as tarefas que o Copilot vai concluir.
14. Role a linha do tempo do PR para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado atual, faz uma pausa para pensar e decidir a abordagem adequada e também cria código.

Isso provavelmente levará alguns minutos. Uma das principais vantagens do Copilot cloud agent é poder realizar tarefas de forma assíncrona, deixando você livre para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao cloud agent enquanto voltamos a atenção para a criação de funcionalidades no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já foi destacado, uma das grandes vantagens do GitHub Copilot cloud agent é dividir o trabalho: você pode se concentrar em algumas tarefas enquanto ele cuida de outras. Mesmo que a seção de jogos relacionados para a equipe de design não leve muito tempo, você ainda pode aproveitar esse período para outras atividades. Vamos atribuí-la ao Copilot cloud agent.

1. Volte ao seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de uma nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina `Show related games on the game details page` como **Title**.
6. Em **Description**, insira:

   ```markdown
   Queremos ajudar as pessoas a descobrir mais jogos, exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a UX e fazer testes de aceitação. Estes são os requisitos:

   - Adicionar um helper de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de um jogo, sem incluir o próprio jogo
   - Mostrar uma seção "Related games" na página de detalhes do jogo usando o helper
   - Tratar o caso em que um jogo não tem jogos relacionados
   - Criar testes unitários para o novo helper
   - Antes de criar o PR, confirmar que todos os testes passam
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá alguns 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O Copilot cloud agent trabalha de maneira semelhante a um engenheiro de software. Não é necessário monitorá-lo continuamente; basta revisar o trabalho quando terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [GitHub Copilot cloud agent][copilot-agents]. É possível atribuir issues ao cloud agent para que ele trabalhe de forma assíncrona. O Copilot pode ajudar a resolver dívida técnica, criar funcionalidades ou migrar código de um framework para outro.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de definir claramente o escopo das issues;
- atribuir issues ao Copilot.

Enquanto o Copilot cloud agent trabalha em segundo plano, você pode se concentrar em criar e usar agentes personalizados. [O Copilot cloud agent também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e tem acesso a instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o Copilot cloud agent][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
