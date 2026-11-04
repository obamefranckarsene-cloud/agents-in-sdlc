---
title: "Lição 6 - Validar funcionalidades com o MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar a funcionalidade de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código é apenas uma parte do trabalho. Também precisamos trabalhar com dados, serviços externos e automações adicionais disponíveis para o Copilot. É aí que entram os servidores MCP: eles permitem que o Copilot vá além dos recursos incluídos no CLI, oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionar o servidor MCP do Playwright, se ele ainda não estiver disponível;
- pedir ao agente que controle um navegador e explore a funcionalidade de filtragem.

## Cenário

Testes de unidade e de ponta a ponta são importantes, mas validar atualizações da interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria, automatizando ainda mais o processo de alteração e aumentando a confiança de que as mudanças funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP bastante usados:

- **[Servidor MCP do GitHub][github-mcp]**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador por meio do Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub hospeda um [registro MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor, revise o código-fonte, verifique o editor e avalie as implicações de segurança.

## Adicionar o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore as etapas de configuração e continue para a próxima seção.

1. Volte ao seu Codespace.
2. No Copilot CLI, digite o comando a seguir para abrir a caixa de diálogo de adição de servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o comando a seguir na caixa **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Pedir ao Copilot que explore a funcionalidade com o Playwright

Anteriormente, você confirmou manualmente se a funcionalidade se comportava como esperado. Agora vamos usar o servidor MCP do Playwright disponível no Copilot para fazer o mesmo.

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário dizer ao Copilot que use o servidor MCP: normalmente, ele descobre qual ferramenta usar. Mas, como você sabe qual ferramenta deve ser usada, indicar isso pode tornar os resultados mais consistentes e economizar tokens.

2. Acompanhe o Copilot executar as etapas no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor MCP do Playwright para explorar sua funcionalidade em um navegador real por meio do Copilot CLI. Para recapitular, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível;
- pediu ao agente que controlasse um navegador e explorasse a funcionalidade de filtragem.

Em seguida, você vai [criar um agente personalizado de QA][next-lesson] que combina a skill e as ferramentas do navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
