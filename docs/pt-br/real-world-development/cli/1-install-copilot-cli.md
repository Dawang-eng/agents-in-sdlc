---
title: "Lição 1 - Instale o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça o ambiente e localize a issue de filtragem criada pelo modelo."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um poderoso assistente de programação baseado em agentes executado no terminal. Ele explora bases de código, gera código, executa comandos e interage com ferramentas externas — tudo pela linha de comando. Assim, você pode delegar tarefas, solicitar alterações e manter o foco. Como você pode imaginar, o primeiro passo é instalar a ferramenta! Felizmente, isso pode ser feito com ferramentas que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI com npm.
- autenticar sua conta do GitHub.
- confiar no repositório do workshop e experimentar uma conversa rápida.
- localizar a issue de filtragem usando o servidor MCP do GitHub integrado.

## Cenário

Sua equipe está começando a usar agentes de IA para avançar em um backlog crescente. O Copilot CLI traz esses recursos para o terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você instalará e autenticará a ferramenta para usá-la no restante do workshop.

## Instale o Copilot CLI

Você pode instalar o Copilot CLI com [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, você usará o npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme se o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão deve ser 24 ou posterior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifique a instalação:

   ```bash
   copilot --version
   ```

   Você deverá ver o número da versão.

## Autentique sua conta do GitHub

Na primeira inicialização, o Copilot CLI solicitará que você autentique sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções de código de dispositivo para autenticar e autorizar o Copilot CLI.
3. O Copilot CLI exibirá o seguinte prompt:

   ```plaintext
   O Copilot pode ler arquivos desta pasta e, com sua permissão, editá-los ou executar código e comandos de shell. Ele lembrará suas permissões durante o restante desta sessão.

   Você confia nos arquivos desta pasta?
   ```

4. Confirme se o caminho corresponde ao repositório Tailspin Toys e selecione **Yes, and remember this folder for future sessions** para responder que sim.

> [!NOTE]
> Em um Codespace, talvez sua sessão do GitHub já tenha autenticado o Copilot. Se o Copilot CLI iniciar sem pedir autenticação, está tudo certo.

## Conheça o ambiente

Os comandos digitados no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI é iniciado, a linguagem natural é enviada ao agente, e os comandos de barra controlam a conversa.

1. Digite `/model`, use as teclas de seta para selecionar **Auto**, pressione <kbd>Enter</kbd> e pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a tela de ajuda.
3. Faça uma pergunta simples para confirmar se tudo está funcionando:

   ```plaintext
   Quais são os principais arquivos deste projeto?
   ```

4. Leia a resposta e observe o Copilot explorar o repositório antes de responder.
5. Digite `/mcp list` e confirme se o servidor MCP do GitHub integrado está disponível.
6. Peça ao Copilot para localizar a issue de filtragem:

   ```plaintext
   Usando o MCP do GitHub, encontre neste repositório a issue intitulada "Allow users to filter games by category and publisher". Informe o URL e um breve resumo. Não faça alterações.
   ```

7. Abra o URL e leia a issue. Você a usará depois de fazer uma pequena alteração inicial.

> [!TIP]
> Uma sessão normal do Copilot CLI usa a branch selecionada no terminal; ela não cria uma worktree automaticamente. Você criará uma branch de recurso antes de cada alteração.

## Use o atalho do workshop

Normalmente, o Copilot CLI pede permissão antes de usar ferramentas fora das permissões já concedidas. Neste workshop, você reiniciará o Copilot CLI com `--yolo`, um atalho aprovado pelo usuário que remove essas solicitações no Codespace para que você se concentre nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões automáticas completas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma VM, e nunca o defina como alias padrão no desenvolvimento diário. Consulte [Allowing and denying tool use][allow-all-warning] para saber mais.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas MCP de leitura e gravação do GitHub usadas nas próximas lições para trabalhar com issues e pull requests. O Codespace limita o acesso ao computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as alterações antes de publicá-las ou fazer merge.

1. Saia do Copilot CLI com `/exit`.
2. Reinicie-o na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar se a conversa está funcionando e saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, depois de alterar instruções ou adicionar um agente, você usará `copilot --resume` para voltar à mesma conversa e branch do recurso.

## Resumo e próximos passos

Parabéns! Nesta lição, você:

- instalou o GitHub Copilot CLI com npm.
- autenticou sua conta do GitHub.
- confiou no repositório do workshop e experimentou uma conversa rápida.
- localizou a issue de filtragem usando o servidor MCP do GitHub integrado.

Em seguida, [faça sua primeira alteração específica][next-lesson] e use o Copilot CLI para exibir uma avaliação por estrelas nos cartões de jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
