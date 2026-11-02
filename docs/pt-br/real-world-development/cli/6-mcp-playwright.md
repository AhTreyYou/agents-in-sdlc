---
title: "Lição 6 - Valide a funcionalidade com Playwright MCP"
description: "Inspecione ou configure Playwright MCP no Copilot CLI e use-o para explorar a funcionalidade de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. É preciso trabalhar com dados, serviços externos e automações adicionais que o Copilot possa usar. É aí que entram os servidores MCP. Eles permitem que o Copilot vá além dos recursos incluídos na CLI e fornecem mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é Model Context Protocol (MCP) e como o Copilot CLI o utiliza;
- adicionar o servidor Playwright MCP, caso ele ainda não esteja disponível;
- pedir ao agente que controle um navegador e explore a funcionalidade de filtragem.

## Cenário

Testes de unidade e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria, automatizando ainda mais o processo de alteração e aumentando a confiança de que as atualizações funcionam como esperado.

## O que é Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP bastante usados:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um deles, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas de configuração e continue na próxima seção.

1. Volte ao Codespace.
2. Abra a caixa de diálogo para adicionar um servidor MCP digitando o comando a seguir no Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. No campo de nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot que explore a funcionalidade com Playwright

Anteriormente, você confirmou manualmente que a funcionalidade funciona como esperado. Agora, vamos usar o servidor Playwright MCP disponível no Copilot para fazer o mesmo.

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário dizer explicitamente ao Copilot que use o servidor MCP; normalmente, ele identifica sozinho a ferramenta adequada. Mas, como você sabe qual ferramenta deve ser usada, indicar a direção pode ajudar a obter resultados mais consistentes e economizar alguns tokens.

2. Acompanhe as etapas que o Copilot executa no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará Playwright para interagir com o site, parará o servidor e apresentará um relatório.

## Resumo e próximas etapas

Parabéns, você usou o servidor Playwright MCP para explorar a funcionalidade em um navegador real a partir do Copilot CLI. Recapitulando, você:

- aprendeu o que é Model Context Protocol (MCP) e como o Copilot CLI o utiliza;
- adicionou o servidor Playwright MCP, caso ainda não estivesse disponível;
- pediu ao agente que controlasse um navegador e explorasse a funcionalidade de filtragem.

Em seguida, você vai [criar um agente de QA personalizado][next-lesson] que combinará a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Microsoft Playwright MCP][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
