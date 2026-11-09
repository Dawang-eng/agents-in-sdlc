---
title: "Exercício 3 - Teste o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ele funciona no navegador. Em vez de testar o aplicativo por conta própria, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Em seguida, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem em um navegador.
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é uma maneira padronizada de permitir que o agente se comunique com ferramentas e serviços externos.

![Diagrama que mostra como o modo de agente interage com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas (usando recursos) e realizar ações em seu nome (usando ferramentas).

Essas ferramentas e recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP bastante usados:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub. O agente pode criar novos repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright. O agente pode acessar páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que dão acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Por segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a operações ou informações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Observe que já há um servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` já está no modelo do projeto, por isso o Copilot conseguiu ler as issues do backlog no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]. Não é preciso instalá-lo localmente: o VS Code se conecta por HTTP e, na primeira vez que o Copilot usa uma ferramenta, você se autentica no GitHub.

## Adicione o servidor MCP do Playwright

Agora, você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, ideal para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia em seu computador executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário dentro de um Codespace sem ambiente gráfico.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para os testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code inicia esse servidor e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, a ação **Start** aparece acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e escolha confiar nele para permitir que seja iniciado.

Iniciá-lo manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e pedirá que você confirme se confia nele na próxima etapa, quando o Copilot precisar das ferramentas.

## Teste o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece um navegador real para o Copilot controlar. Em vez de testar o aplicativo por conta própria, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. Assim, você verifica rapidamente se o recurso funciona como esperado, sem sair da conversa.

Nos bastidores, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. O agente trabalha com elementos estruturados e rotulados, como botões, links e itens de lista, da mesma maneira que as tecnologias assistivas. Assim, uma verificação funcional rápida também pode servir como uma análise básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar o recurso de filtragem:

```text
Usando o servidor MCP do Playwright, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e informe quantos jogos aparecem.
2. Aplique um filtro de categoria e confirme se a lista mostra somente os jogos dessa categoria.
3. Remova o filtro e aplique um filtro de editora. Confirme se a lista é atualizada para mostrar apenas os jogos dessa editora.
4. Combine um filtro de categoria com um de editora e confirme se os resultados respeitam os dois filtros.

Informe o que observou em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e apresentará os resultados. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça ao Copilot que corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Agora que confirmou que o recurso funciona, abra um pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. Abra o painel **Source Control** no Codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, como usar github.com ou a interface de linha de comando do GitHub (CLI). Como você já está trabalhando com o GitHub Copilot, deixe que ele crie o PR. Ele pode localizar a issue relevante e associá-la ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. No menu suspenso de agentes, confirme se **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar sua conta. Siga as instruções para permitir o acesso.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link criado pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar o recurso de filtragem em um navegador antes de publicá-lo.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
