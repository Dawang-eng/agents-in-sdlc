---
title: "Lição 2 - Adicione avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revisá-la no navegador com encaminhamento de porta e mesclá-la como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e testou uma conversa, é hora de fazer sua primeira alteração no projeto. Você começará com algo simples: os jogos já têm uma avaliação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Peça ao agente que a mostre, revise a alteração e faça o merge como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de recurso.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo para confirmar a alteração em um navegador com encaminhamento de porta.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Porém, os cartões de jogos da página inicial mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente para exibir em cada cartão a avaliação existente: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão depois, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente, a atividade das ferramentas do agente enquanto ele explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver aberto, inicie-o com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use o prompt a seguir para solicitar a alteração:

   ```plaintext
   Exiba em cada cartão de jogo da página de listagem a avaliação starRating de 0 a 5. Se a avaliação for null, exiba "No rating yet". Mantenha o layout atual dos cartões, adicione testes e execute as verificações relevantes.
   ```

O Copilot explora o projeto, localiza os arquivos que exibem os detalhes dos jogos e cria o código necessário. Você acabou de adicionar um recurso com o Copilot CLI!

## Revise o diff

Revise todas as alterações geradas por IA antes de fazer o merge, mesmo as pequenas. Vamos explorar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme se o cartão do jogo mostra a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então o código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Confira as alterações

Não basta ler o código e presumir que funciona. Peça ao Copilot para iniciar o site e examine a interface atualizada no navegador com encaminhamento de porta do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar no navegador a alteração da avaliação por estrelas. Informe o URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cartões dos jogos mostram as avaliações de 0 a 5.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Pare o servidor de desenvolvimento que você iniciou.
   ```

## Abra e mescle seu primeiro pull request

Você implementou o recurso! Agora, crie um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça o commit da alteração:

   ```plaintext
   Faça o commit das alterações revisadas na avaliação por estrelas usando uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. O Copilot CLI envia o commit existente ao criar o PR e exibe o URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o URL exibido pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize a versão local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Especificamente, você:

- iniciou uma conversa focada do Copilot em uma branch de recurso.
- orientou o agente a fazer uma pequena alteração nos cartões de jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo para confirmar a avaliação por estrelas em um navegador com encaminhamento de porta.
- abriu e mesclou seu primeiro pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para implementar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
