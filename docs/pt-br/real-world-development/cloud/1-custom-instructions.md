---
title: "Exercício 1 - Instruções personalizadas (cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: GitHub Copilot cloud agent →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica ou o Copilot deve conhecer informações de contexto, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* fazer, mas também *como* fazer.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções com escopo de caminho;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes do VS Code e da CLI, você não executará um prompt antes e depois para comparar os resultados. O GitHub Copilot cloud agent trabalha de forma assíncrona em issues do GitHub, o que dificulta uma demonstração lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests gerados pelo cloud agent.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação em comentários TSDoc.
- Cada arquivo deve começar com um bloco de comentários que descreva sua finalidade.

Com os arquivos de instruções, você garante que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que deseja incluir. Também pode criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot para **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas pelo chat ou pela CLI, como a pilha de tecnologias, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações globais.
- Os arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Use-os para orientar linguagens como TypeScript ou Astro, ou tarefas como criar um componente de interface ou um conjunto de testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados somente para gerar código no Copilot Chat — não para conclusões de código nem sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o GitHub Copilot cloud agent usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções baseados em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma explicação completa sobre como criar arquivos de instruções está fora do escopo deste workshop. Os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado em orientações para todo o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de codificação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Mostre exemplos de como o código deve ser e do que deve ser evitado.

Não há uma única maneira certa de criar arquivos de instruções nem de usar IA. Experimente para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você encontrará instruções para várias tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode gerar arquivos de instruções. Cada ambiente oferece essa opção de uma maneira diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição do ambiente em uso indicará quando essa opção for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o repositório [awesome-copilot][awesome-copilot], que reúne arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Este repositório inclui um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub para conhecê-los.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as diretrizes aninhadas de **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados do Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo, que define um glob relativo à raiz do repositório para determinar a quais arquivos as instruções se aplicam. Neste caso, ele corresponde a arquivos de teste em TypeScript, como os que correspondem a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir ao gerar código. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicione um novo padrão ao repositório

Agora você fará a única edição deste exercício: adicionar uma regra para todo o projeto que determine que a documentação seja incluída no código em comentários TSDoc e em um cabeçalho de comentário no nível do arquivo. O cloud agent usará essa regra mais adiante ao trabalhar nas issues que você atribuir.

Antes de editar, prepare uma branch de trabalho. O cloud agent lerá as instruções da branch de destino da issue, mas você fará o commit das alterações separadamente.

1. No terminal do Codespace, crie uma nova branch e mude para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como explicado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Documente os padrões de codificação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de codificação, mas ainda não contém uma regra para documentação no código.
3. Adicione o Markdown a seguir logo abaixo dos padrões existentes, orientando o Copilot a incluir cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Cada função exportada deve ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes das importações ou de qualquer código, adicione ao arquivo um bloco de comentários que explique sua finalidade.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Faça commit, push e merge da atualização das instruções

O cloud agent lê os arquivos de instruções da branch de destino da issue. No próximo exercício, ao atribuir uma issue ao Copilot, ele criará uma branch a partir de `main`. Portanto, as alterações precisam chegar a `main` para que o cloud agent possa usá-las.

1. Prepare e faça commit das alterações:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. Em github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. O cloud agent usará essas instruções quando trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a criação da branch e fazer commit diretamente em `main`. A etapa da branch demonstra como o trabalho seria feito em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um padrão para todo o repositório a `.github/copilot-instructions.md`. Você verá esse padrão aplicado nos pull requests que o cloud agent gerar durante o restante deste ambiente.

Em seguida, [atribua sua primeira issue ao GitHub Copilot cloud agent][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
