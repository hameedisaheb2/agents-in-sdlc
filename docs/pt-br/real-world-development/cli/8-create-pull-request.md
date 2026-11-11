---
title: "Lição 8 - Crie e mescle o PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora você os revisará juntos e abrirá um pull request. Você mesmo fez o merge do pull request (PR) da avaliação por estrelas; desta vez, deixará que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma conversa e branch e incluirá o trabalho neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge;
- inspecionar toda a alteração do recurso e as evidências de verificação;
- criar o PR de filtragem;
- habilitar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Ao longo do fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para incorporar um pull request. Quando ativado, o Copilot resolve os bloqueios do PR: corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário. Depois, habilita o merge automático do GitHub se as configurações do repositório permitirem.

Até agora, você mesmo selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que ele está pronto antes de habilitar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com todo o código pronto, revise as alterações, crie o PR e deixe o Agent Merge gerenciar o restante do processo.

1. Volte ao Codespace.
2. Digite `/agent` para abrir a caixa de diálogo de agentes.
3. Selecione **Default** na lista e pressione <kbd>Enter</kbd>.
4. Crie um novo PR usando o comando `/pr create`.
5. Ative o Agent Merge usando `/pr agentmerge`.
6. O Copilot monitorará a integração contínua do PR. Quando tudo passar, ele fará o merge.
7. Confirme se o Copilot exibiu uma mensagem semelhante a `"PR #14 was squash-merged successfully."`.

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e faça manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento, incluindo gerar, testar e validar código e, agora, gerenciar o processo do pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge;
- inspecionou toda a alteração do recurso e as evidências de verificação;
- criou o PR de filtragem;
- habilitou o Agent Merge somente após a revisão e confirmou que o PR foi mesclado.

Em seguida, [explore outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
