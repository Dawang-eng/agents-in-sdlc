---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações tenham dificuldades com dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos que ficaram no backlog por falta de tempo para implementá-los. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request de rascunho para uma pessoa desenvolvedora revisar. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em atividades mais estratégicas.

Neste exercício, você explorará o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de delimitar claramente o escopo das issues.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas para criar a primeira versão do site não deixaram a documentação em um estado ideal — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Cada página de detalhes deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar explorando. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência. Isso é um bloqueio, mas outras issues têm prioridade maior no momento.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e são ideais para o agente de nuvem do Copilot. O agente pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras atividades. Depois, você revisa o trabalho para confirmar se está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Há várias maneiras de trabalhar com uma pessoa desenvolvedora, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que você atribui uma issue, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto o Copilot trabalha ou depois que ele termina. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — o trabalho de pensar continua sendo seu. Até operações aparentemente simples se tornam complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas do agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma ferramenta de programação em pares com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a adição de IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Se ainda não fez esse exercício, aproveite para examinar o arquivo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções existentes neste repositório para reconhecer o efeito deles mais adiante.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace ativo:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as expectativas para convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir ao agente de nuvem a issue `Code lacks documentation` na próxima seção, observe no pull request os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentário. Essas instruções vêm dos arquivos que você examinou. Vamos destacar isso novamente ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [fluxo de trabalho de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Assim, ele terá acesso às ferramentas e dependências necessárias para o desenvolvimento. Esse fluxo já foi configurado para facilitar o laboratório e garantir uma boa experiência. Ele fornece ao Copilot Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para estas etapas. O Copilot recebe um token próprio para as operações dele.
    permissions:
      # Se as etapas de configuração precisarem clonar o repositório, por exemplo, para instalar dependências, será necessário conceder a permissão `contents: read`. Se o repositório não for clonado nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo e do front-end: Node.js (o aplicativo agora é totalmente Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se ele funciona, em vez de aguardar o Copilot executá-lo.

## Adicione a documentação

Todos reconhecem a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, o que reduz a produtividade e dificulta a manutenção da base de código e a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação e esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Em **Title**, digite `Code lacks documentation`.
6. Em **Description**, digite:

   ```plaintext
   Nossa organização exige que funções e métodos tenham comentários TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação onde eles esclarecerem o comportamento.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se o novo PR ainda não aparecer, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o fluxo de trabalho mencionado acima), analisando o projeto e definindo a melhor abordagem para a tarefa.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código e analisa o estado do projeto, pausa para definir a abordagem e cria código.

Isso provavelmente levará alguns minutos. Um dos objetivos principais do agente de nuvem do Copilot é executar tarefas de forma assíncrona e permitir que você se concentre em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, escrever código para adicionar recursos ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho: você pode se concentrar em uma tarefa enquanto o agente trabalha em outra. Mesmo que a seção de jogos relacionados para a equipe de design não leve muito tempo, você poderia usar esse período para outras atividades. Vamos atribuí-la ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Em **Title**, digite `Show related games on the game details page`.
6. Em **Description**, digite:

   ```markdown
   Queremos ajudar as pessoas a descobrir mais jogos, exibindo jogos relacionados em cada página de detalhes. A equipe de design quer explorar a experiência e fazer testes de aceitação. Os requisitos são:

   - Adicionar um helper de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de determinado jogo, sem incluir o próprio jogo.
   - Exibir uma seção "Related games" na página de detalhes do jogo usando o helper.
   - Tratar o caso em que um jogo não tem jogos relacionados.
   - Criar testes de unidade para o novo helper.
   - Confirmar que todos os testes passam antes de criar o PR.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Logo depois, um emoji 👀 deverá aparecer no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O agente de nuvem do Copilot funciona de maneira semelhante a uma pessoa desenvolvedora de software. Não é necessário acompanhá-lo ativamente: revise o trabalho quando ele terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, você pode atribuir issues ao Copilot para execução assíncrona. Use-o para resolver dívidas técnicas, criar recursos ou ajudar na migração de código entre estruturas.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de delimitar claramente o escopo das issues.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, que já exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
