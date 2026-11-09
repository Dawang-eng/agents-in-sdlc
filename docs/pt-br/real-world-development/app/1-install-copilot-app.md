---
title: "Lição 1 - Instale o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop que permite trabalhar com agentes. Desenvolvido com base no GitHub Copilot CLI, ele se integra nativamente ao GitHub, para que seus repositórios, branches e pipelines de CI funcionem imediatamente. O aplicativo foi criado para fluxos de trabalho em que você orienta vários agentes em paralelo — cada um em seu próprio espaço de trabalho isolado — em vez de executar todo o trabalho por conta própria e automatiza tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, o próximo passo é instalar o aplicativo e conectar esse repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar o projeto ao aplicativo a partir do repositório no GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo.
- experimentar um chat rápido para conhecer melhor o aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único espaço para orientar esse trabalho: escolher issues, executar agentes, revisar alterações e mesclar pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). No Copilot Business ou no Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instale e configure o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro instale-o. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar e adicionar o repositório Tailspin Toys.

1. No navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois da instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe o nome do host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial solicitar a seleção de um repositório, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Selecione um tema quando solicitado e conclua a configuração inicial.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o nome do proprietário e do repositório anotado na lição de pré-requisitos.
9. Confirme se o repositório aparece na barra lateral. No layout usado neste workshop, as pastas dos repositórios, como `tailspin-toys`, ficam agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conheça o espaço de trabalho

Com o projeto conectado, conheça a organização do aplicativo. A barra lateral agrupa tudo em algumas áreas:

- **New**: inicie uma sessão de chat com o Copilot.
- **Pull requests**: seus pull requests, integrados nativamente ao GitHub. Navegue, filtre, confira o status de CI e revise PRs sem sair do aplicativo.
- **Issues**: suas issues, também integradas ao GitHub. Navegue e filtre issues e inicie uma sessão a partir de uma issue.
- **Automations**: tarefas salvas de agentes que são executadas em uma programação definida ou sob demanda. Elas são úteis para gerenciar listas de tarefas, manutenção periódica do projeto ou outras atividades repetitivas. A seção de conclusão as apresenta como próximo passo, não como outro exercício do workshop.
- **Customize**: adicione recursos ao aplicativo Copilot, como servidores MCP, plug-ins, skills e outros componentes. Você usará esta opção para configurar o MCP do Playwright.
- **Chats**: conversas leves para perguntas e ideias que não precisam de uma branch ou de um espaço de trabalho próprio. Esta seção aparece depois que você inicia o primeiro chat. Se não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions**: agrupa as pastas dos repositórios e as sessões dos agentes. Selecione um nome de repositório, como `tailspin-toys`, para abrir a exibição do repositório ou selecione uma sessão abaixo dele para retomar a conversa. Cada sessão tem seu próprio espaço de trabalho isolado, então você pode executar várias ao mesmo tempo sem conflito entre as alterações. Você iniciará a primeira sessão ao adicionar avaliações por estrelas.

Durante o workshop, você explorará o espaço de trabalho.

> [!TIP]
> Em caso de dúvida, pergunte ao Copilot! Se não souber como fazer algo ou se não tiver certeza de que é possível, pergunte ao Copilot. Ele ajudará você.

### Encontre o backlog criado pelo modelo

É difícil encontrar um projeto sem backlog, e a Tailspin Toys não é diferente. Vamos explorar o backlog criado quando você usou o modelo.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual dentro dela. Depois, na navegação superior do repositório, selecione **Issues**.
2. Procure estas issues pelo título, sem presumir que os números serão os mesmos:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma pequena alteração inicial.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimente um chat rápido

Uma ótima maneira de conhecer o aplicativo é usá-lo para aprender sobre o *próprio aplicativo*. Um **chat rápido** é ideal para isso. Faça uma pergunta ou troque ideias sem criar uma branch nem uma worktree. É perfeito para uma pergunta rápida e descartável — não é necessário iniciar uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte como funcionam as sessões do aplicativo:

   ```plaintext
   Como o aplicativo GitHub Copilot usa worktrees?
   ```

3. Leia a resposta na exibição da conversa. Cada sessão usa uma worktree e uma branch Git isoladas — assim, você pode executar vários agentes em paralelo sem conflito entre as alterações. Continue a conversa ou inicie um novo chat quando quiser.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar na sua conta do GitHub.
- adicionar um projeto a partir do repositório no GitHub.
- conhecer o espaço de trabalho e encontrar o backlog criado pelo modelo em **Issues**.
- usar um chat rápido para fazer uma pergunta simples e descartável.

Em seguida, [inicie sua primeira sessão de agente][next-lesson] e use-a para exibir uma avaliação por estrelas nos cartões de jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Primeiros passos com o aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agentes no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
