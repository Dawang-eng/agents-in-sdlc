---
title: "Lição 2 - Adicione avaliações por estrelas: uma melhoria rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cartões de jogos e mescle-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você conheceu o espaço de trabalho e experimentou um chat rápido, é hora de iniciar uma **sessão de agente** e fazer sua primeira alteração no projeto. Você começará com algo simples: os jogos já têm uma avaliação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Peça ao agente que a mostre, revise a alteração e faça o merge como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e aprender como ela é estruturada.
- pedir ao agente que faça uma pequena alteração específica no projeto.
- revisar a alteração na exibição de diff do espaço de trabalho.
- executar o aplicativo localmente para confirmar a alteração no navegador.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Porém, os cartões de jogos da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente para exibir em cada cartão a avaliação existente: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma sessão

Uma **sessão** é uma conversa com um agente executada em um espaço de trabalho isolado. Cada sessão tem uma **worktree e uma branch Git dedicadas**, o que permite executar várias sessões ao mesmo tempo — uma para adicionar um recurso e outra para corrigir um bug — sem conflito entre as alterações. Suas sessões aparecem agrupadas por repositório na barra lateral. Selecione uma delas para mudar de sessão.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade das ferramentas** enquanto ele explora e edita arquivos e a lista de **arquivos alterados** com seus diffs.

## Inicie uma sessão e solicite a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar o recurso. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto a partir do repositório GitHub. Agora criaremos uma sessão para esse repositório e solicitaremos a alteração.

1. Abra ou volte ao aplicativo GitHub Copilot.
2. Na parte superior da barra lateral, selecione **+** para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Abaixo da caixa de prompt, selecione uma **new working tree** e o modo **Interactive**. Use o prompt a seguir:

   ```plaintext
   Exiba em cada cartão de jogo da página de listagem a avaliação starRating de 0 a 5. Se a avaliação for null, mostre "No rating yet". Mantenha o layout atual dos cartões, adicione testes e execute as verificações relevantes.
   ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando uma nova worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam ser atualizados para adicionar o recurso e cria o código necessário. Você acabou de adicionar um recurso com o aplicativo Copilot!

## Revise o diff

Revise todas as alterações geradas por IA antes de fazer o merge, mesmo as pequenas. Vamos explorar as alterações no próprio aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para exibir os arquivos alterados e seus diffs.

   ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel, à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você deverá ver código em `GameCard.astro`, o arquivo principal usado para exibir os detalhes do jogo. Ele deve ser semelhante ao exemplo: um bloco pequeno que exibe a avaliação quando ela existe e usa "No rating yet" como alternativa quando `starRating` é `null`.

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então o código exato pode variar. A implementação deve ser relativamente parecida.

## Confira as alterações

Antes de abrir o navegador, revise os resultados das verificações automatizadas do agente. Confirme se os testes cobrem uma avaliação `starRating` numérica e a alternativa `null`. Uma verificação ignorada ou um pré-requisito ausente não significa aprovação; revise qualquer solicitação de instalação antes de aprová-la.

Não basta ler o código e presumir que funciona. Peça ao Copilot para abrir o site e examine a interface atualizada em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível diretamente no aplicativo Copilot. Você explorará canvases personalizados e criará um mais adiante, mas agora usaremos o canvas de navegador integrado.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

   ```plaintext
   Inicie o aplicativo e abra-o no canvas de navegador.
   ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador aparecerá dentro do aplicativo Copilot.
3. Confirme se os cartões dos jogos avaliados mostram o valor de 0 a 5.
4. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

   ```plaintext
   Pare o servidor de desenvolvimento e feche o canvas de navegador.
   ```

## Abra e mescle seu primeiro pull request

Você implementou o recurso! Agora, crie um pull request (PR) para mesclar o novo código à base de código existente.

1. No canto superior direito, selecione **Create PR**.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para autenticar.
3. Se o Copilot explicar que a política do repositório exige uma issue correspondente antes de criar o PR, confirme que ele deve seguir esse processo.
4. O Copilot criará o PR.
5. Selecione o indicador **PR** acima do chat para abrir o PR no painel de revisão e examiná-lo.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela, selecione **Merge pull request** para mesclar o PR.

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o aplicativo GitHub Copilot. Especificamente, você:

- iniciou uma sessão de agente e aprendeu como ela é estruturada.
- orientou o agente a fazer uma alteração pequena e específica nos cartões de jogos.
- revisou a alteração na exibição de diff do espaço de trabalho.
- executou o aplicativo localmente para confirmar a avaliação por estrelas no navegador.
- abriu o PR 1, revisou as verificações e fez o merge explicitamente.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para implementar um recurso maior.

## Recursos

- [Trabalhar com sessões de agentes no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instale-e-configure-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
