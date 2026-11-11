---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para criar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para funcionar com o fluxo de trabalho existente da organização e ajudar a construir o que é necessário da maneira correta. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: partirá de uma issue para criar um recurso, verificará o código, validará o comportamento e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch durante o fluxo de trabalho desse recurso. Normalmente, haveria branches ou PRs diferentes para os vários tipos de arquivos, mas vamos simplificar algumas etapas para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa com o Copilot a partir de uma issue do GitHub;
- definir os requisitos no modo Plan;
- implementar o recurso com o modo Autopilot;
- revisar o código;
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar esse trabalho, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas alguns detalhes, como combinar categorias, precisam ser definidos antes de escrever o código. Você usará o modo Plan para tomar essas decisões e, depois, autorizará uma implementação com escopo definido no Autopilot.

## Contexto

Adicionar agentes de codificação com IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles ficam ainda mais importantes. A maioria dos desenvolvedores segue um fluxo semelhante a este:

1. Começar por uma issue registrada que descreva o trabalho necessário.
2. Planejar o que precisa ser criado.
3. Criar e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Fazer o merge do código depois da revisão e da aprovação da integração contínua.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma variação desse fluxo.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão do código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua resposta antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e, depois, use o Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR da avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` ou um número semelhante. O caractere `#` permite incluir uma issue ou um pull request (PR) do GitHub no contexto.

## Planeje o recurso de filtragem

O planejamento permite definir como implementar um recurso ou executar uma tarefa antes de delegá-la ao Copilot. Para qualquer trabalho complexo, é uma boa ideia reservar um tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt:

   ```plaintext
   Crie um plano para implementar esse recurso.
   ```

O Copilot explorará o projeto e determinará a melhor abordagem para montar o plano.

3. O Copilot poderá fazer perguntas sobre o funcionamento dos filtros. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano para revisá-lo.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Deixe o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere no problema até considerar que ele está concluído.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme se o indicador abaixo do prompt mostra **Autopilot**.
3. Observe o Copilot seguir o plano definido, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Assim como qualquer código, o código gerado precisa ser revisado antes do merge. Vamos revisar o código e executar o site para verificar se tudo está correto.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois da revisão, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija as falhas.

## Explore o novo recurso

O código parece correto, mas será que funciona? Inicie o aplicativo como antes e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa experimentar o recurso de filtragem no navegador. Informe a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente os filtros de categoria e editora, além das combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às decisões tomadas.
5. Volte ao Codespace e peça ao Copilot para parar o servidor de desenvolvimento que iniciou.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa com o Copilot a partir de uma issue do GitHub;
- definiu os requisitos no modo Plan;
- implementou o recurso com o modo Autopilot;
- revisou o código;
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, vamos aprofundar como gerar código que siga práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
