---
title: "Lição 6 - Valide a funcionalidade com o MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. É necessário trabalhar com dados, serviços externos e até disponibilizar outras automações para o Copilot. É aí que entram os servidores MCP. Eles permitem que o Copilot vá além dos recursos integrados à CLI, oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor MCP do Playwright, se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Os testes de unidade e de ponta a ponta são importantes, mas validar alterações na interface do usuário exige interagir com ela. Você quer permitir que o Copilot use o site em desenvolvimento como uma pessoa usuária faria, automatizando também a validação das alterações e aumentando a confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem acessar informações atualizadas e realizar ações em seu nome.

Essas ferramentas e recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP bastante usados:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore estas etapas de configuração e continue na próxima seção.

1. Volte ao Codespace.
2. Abra a caixa de diálogo para adicionar um servidor MCP digitando o seguinte comando no Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot para explorar o recurso com o Playwright

Antes, você confirmou manualmente que a funcionalidade está correta. Agora, vamos usar o servidor MCP do Playwright disponível no Copilot para fazer o mesmo!

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Inicie o aplicativo e use o MCP do Playwright para verificar a filtragem com base na issue e no nosso plano. Informe o que funciona e o que não funciona, sem fazer alterações. Quando terminar, pare o servidor iniciado.
   ```

> [!NOTE]
> Não é necessário pedir ao Copilot que use o servidor MCP, pois ele geralmente o encontra sozinho. Mas, como você sabe qual servidor deve ser usado, vale a pena indicar isso no prompt! Assim, os resultados tendem a ser mais consistentes, e você economiza alguns tokens.

2. Acompanhe as etapas que o Copilot executar no navegador para confirmar se a funcionalidade está correta.
3. Leia o relatório e confira se tudo está funcionando como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns! Você usou o servidor MCP do Playwright para explorar o recurso do Copilot CLI em um navegador real. Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor MCP do Playwright, se ele ainda não estava disponível.
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que combinará a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
