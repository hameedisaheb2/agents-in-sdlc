---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este workshop, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse melhorias de acessibilidade, incluindo opções de alto contraste e modo claro. Vamos revisar as alterações no código e, se necessário, enviar comentários para o Copilot aprimorar o trabalho.

### Cenário

Como já foi destacado várias vezes, os fundamentos do design de software e do desenvolvimento e operações (DevOps) não mudam com a inclusão da inteligência artificial (IA) generativa. Sempre revisamos o código gerado e seguimos nosso processo habitual de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e as melhorias de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e GitHub Copilot cloud agent

Como o Copilot cloud agent executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter tudo protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O cloud agent é executado no GitHub Actions e cria um ambiente separado e temporário para trabalhar.
- Uma pessoa precisa aprovar todos os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores do Model Context Protocol (MCP).

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo GitHub Copilot cloud agent: a adição de documentação ao código. Você fará isso na interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você talvez veja um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, inclusive a chamadas para servidores externos do Model Context Protocol (MCP). Se quiser, você pode [personalizar ou desabilitar o firewall do Copilot cloud agent][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o PR com um título semelhante a **Add missing documentation** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários de documentação de TypeScript (TSDoc) e outros conteúdos. As alterações exatas podem variar.

   Ao examinar as alterações, procure comentários de documentação TSDoc, convenções de TypeScript e cabeçalhos de comentários. Eles vêm dos arquivos de instruções personalizadas que você examinou no início do exercício 2.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na fila da seção de verificações do PR. Se tudo correr bem, as verificações do projeto passarão para o único aplicativo Astro. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um PR não é uma via de mão única. Você também pode mencioná-lo em comentários no PR ou em comentários embutidos no código, como faria com qualquer outra pessoa da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos fornecer um texto exato para o pedido. Veja algumas ideias do que você pode pedir:

- adicionar cabeçalhos de comentários no início de cada arquivo de código, com uma breve descrição do conteúdo;
- adicionar comentários de documentação TSDoc aos arquivos TypeScript e Astro;
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no PR.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR em rascunho como pronto para revisão e fazer o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos voltar ao PR que o Copilot criou para resolver a issue sobre exibir jogos relacionados na página de detalhes de um jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro título mais descritivo.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na fila da seção de verificações do PR. Se tudo correr bem, as verificações do projeto passarão para o único aplicativo Astro. Isso pode levar alguns minutos.
9. **Opcional:** você pode trocar para essa branch no Codespace para testar o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode iterar com o Copilot cloud agent e solicitar atualizações. Por exemplo, pode pedir que ajuste a quantidade de jogos relacionados exibidos ou adicione cabeçalhos de comentários e comentários de documentação TSDoc (lembre-se: a issue foi atribuída **antes** da atualização das instruções personalizadas). Você também pode fazer esses pedidos em um novo comentário na guia **Conversation**; o Copilot verá o comentário e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no exercício 3 e o modo claro solicitado no centro de controle no exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre modos;
   - uso do armazenamento local para manter as preferências das pessoas;
   - alterações de folhas de estilo em cascata (CSS) ou estilo dos modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos Accessible Rich Internet Applications (ARIA), navegação por teclado etc.);
   - código JavaScript/TypeScript que controla a alternância de modos.

6. Depois de revisar as atualizações e confirmar que estão corretas, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na fila da seção de verificações do PR. Se tudo correr bem, as verificações do projeto passarão para o único aplicativo Astro. Isso pode levar alguns minutos.
10. **Opcional:** você pode trocar para essa branch no Codespace para testar os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute estes comandos (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Depois, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências permanecem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo as práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O cloud agent funciona melhor quando você pode atribuir itens reais do backlog e se concentrar em outras atividades. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na página de listagem de jogos;
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o ambiente do cloud agent. Ao longo destas lições, você:

- **Examinou os arquivos de instruções personalizadas deste repositório** para ver como influenciariam mais tarde o trabalho do cloud agent.
- **Atribuiu issues ao Copilot cloud agent** e acompanhou a preparação do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como centro de controle** para monitorar e orientar a sessão de acessibilidade.
- **Revisou e iterou sobre os pull requests do cloud agent**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o ambiente do cloud agent. Para continuar explorando, os outros ambientes complementam o que você praticou:

- 🖥️ **[Ambiente do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com Model Context Protocol (MCP) diretamente no IDE.
- 💻 **[Ambiente da interface de linha de comando (CLI)](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de garantia de qualidade (QA) personalizado, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` de volta para o cloud agent.

No seu próprio repositório, experimente estas ideias:

- atribuir ao cloud agent uma issue de refatoração ou de cobertura de testes;
- criar um agente personalizado para outro domínio;
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do Copilot cloud agent][cloud-agent-best-practices]
- [Configurar o firewall do Copilot cloud agent][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
