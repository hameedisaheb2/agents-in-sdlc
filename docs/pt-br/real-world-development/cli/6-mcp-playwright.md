---
title: "Lição 6 - Valide a funcionalidade com Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código não é a única parte do desenvolvimento. Também precisamos trabalhar com dados, serviços externos e automações adicionais disponíveis para o Copilot. É aí que entram os servidores MCP. Eles ampliam as ferramentas e os serviços disponíveis para o Copilot além dos recursos integrados à CLI.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionar o servidor Playwright MCP caso ele ainda não esteja disponível;
- pedir ao agente para controlar um navegador e explorar o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface do usuário (UI) exige interagir com ela. Você quer permitir que o Copilot use o site em desenvolvimento como uma pessoa usaria, automatizando mais verificações e aumentando a confiança de que as alterações funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

As ferramentas e os recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP fornece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos são:

- **[GitHub MCP Server][github-mcp]**: fornece acesso às APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: oferece recursos de automação de navegador com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa faria.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, digite o comando a seguir para abrir a caixa de diálogo de adição de servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Pressione <kbd>Enter</kbd> para confirmar o tipo de servidor **STDIO** e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o texto a seguir na caixa **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Peça ao Copilot para explorar o recurso com o Playwright

Antes, você confirmou manualmente que a funcionalidade funciona como esperado. Agora vamos usar o servidor Playwright MCP disponível no Copilot para fazer a mesma verificação.

1. Use o prompt a seguir para pedir que o Copilot valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Inicie o aplicativo e use o Playwright MCP para verificar a filtragem de acordo com a issue e nosso plano. Diga o que funciona e o que não funciona, sem fazer alterações. Quando terminar, pare o servidor que iniciou.
   ```

> [!NOTE]
> Normalmente, não é necessário dizer ao Copilot para usar o servidor MCP, pois ele costuma descobrir isso sozinho. Mas, se você sabe qual servidor deve ser usado, pode indicá-lo para obter resultados mais consistentes e economizar alguns tokens.

2. Observe as etapas executadas pelo Copilot no navegador para confirmar se o recurso funciona.
3. Leia o relatório e confirme que tudo está funcionando como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximos passos

Você usou o servidor Playwright MCP para explorar seu recurso em um navegador real a partir do Copilot CLI. Nesta lição, você:

- aprendeu o que é o MCP e como o Copilot CLI o usa;
- adicionou o servidor Playwright MCP caso ele ainda não estivesse disponível;
- pediu ao agente para controlar um navegador e explorar o recurso de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que reúne skills e ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
