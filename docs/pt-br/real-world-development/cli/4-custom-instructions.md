---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *qual* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos com escopo de caminho chegam ao agente;
- atualizar o arquivo de instruções para garantir que os padrões de codificação sejam seguidos;
- observar o impacto dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento:

- Os comentários devem explicar a intenção e decisões não óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar a finalidade, os parâmetros e os valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Os componentes reutilizáveis do Astro devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instruções, você garante que o Copilot tenha as informações necessárias para seguir essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências para que o Copilot entenda melhor seu estilo de codificação e seus requisitos. Você pode especificar convenções, bibliotecas e os tipos de comentários a incluir. Também pode criar instruções para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot para **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações.
- Os arquivos `.github/instructions/*.instructions.md` fornecem diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> Os formatos de instrução e o suporte variam de acordo com o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui arquivos de instruções. Explore o conteúdo existente antes de fazer alterações para poder observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Examine a descrição do projeto e as orientações de codificação. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados do Drizzle, testes e muito mais.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início, que define um glob para determinar a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e para arquivos existentes do projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir.

## Atualize as instruções para corresponder às diretrizes da equipe

Os arquivos existentes são um bom começo, mas ainda há uma lacuna. Modifique o arquivo principal `copilot-instructions.md` para incluir comentários TSDoc no novo código TypeScript.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o item a seguir ao final da lista:

   ```markdown
   - Todo código TypeScript novo deve conter comentários TSDoc para fins de documentação.
   ```

O arquivo será salvo automaticamente.

## Use as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções atualizadas, saia da conversa de filtragem e retome-a para que o Copilot use as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está em uso.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   Acabamos de atualizar nossas instruções e orientações de código. Você pode atualizar o código gerado para seguir essas orientações?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI obtém contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Nesta lição, você:

- explorou como as instruções do repositório e os arquivos com escopo de caminho chegam ao agente;
- atualizou o arquivo de instruções para garantir que os padrões de codificação sejam seguidos;
- observou o impacto dos arquivos de instruções no código.

Em seguida, [personalize e execute a skill reutilizável quality-checks][next-lesson] para que lint e testes sejam executados de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
