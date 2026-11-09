---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para implementar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Mas mudanças maiores exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para funcionar com o fluxo de trabalho existente da organização e ajudar a implementar as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes, começando com uma issue para gerar um novo recurso, verificando se o código é válido e se o recurso funciona como esperado e, por fim, integrando-o ao projeto.

> [!NOTE]
> Você usará a mesma sessão durante todo o fluxo de trabalho do recurso. Normalmente, você usaria sessões ou PRs diferentes para cada tipo de arquivo, mas vamos simplificar para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar o novo recurso usando o modo **Autopilot**.
- revisar o código.
- validar manualmente o recurso em um canvas de navegador.

Ao continuar o trabalho no recurso, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de escrever o código. Você usará o modo Plan para tomar essas decisões e, em seguida, autorizará uma implementação com escopo definido pelo Autopilot.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Abrir uma issue registrada que descreva o trabalho necessário.
2. Criar um plano do que precisa ser implementado.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com êxito, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria segue uma variação do processo acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de sessão

O **modo da sessão** controla o nível de autonomia do agente. Você pode selecioná-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua resposta antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você revisa e aprova o plano antes que o agente o execute.
- **Autopilot**: o agente trabalha de forma totalmente autônoma: escreve código, executa testes e itera sem esperar sua resposta.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Confirme se o PR de avaliação por estrelas foi mesclado e se sua cópia local de `main` está atualizada antes de começar.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior do repositório, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada no modelo; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na versão atualizada de `main`.

   ![Exibição de issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente para executar `node --version` e `npm --version`. Se funcionarem no terminal, mas não na sessão, verifique o PATH do aplicativo ou a configuração do gerenciador de versões e feche e reabra o aplicativo completamente. Repita a operação e as verificações que falharam. Não reinstale o npm apenas porque o agente não consegue encontrá-lo.

## Planeje o recurso de filtragem

O planejamento permite revisar a abordagem antes que o Copilot escreva código. Como você iniciou pela issue, o Copilot já tem a solicitação de recurso no contexto. Envie:

```plaintext
Implemente este recurso.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele inclui filtragem por categoria e editora, controles acessíveis, alterações de acesso a dados e testes. Converse sobre qualquer comportamento que não estiver claro, como a combinação de várias categorias ou o resultado quando nenhum jogo corresponder.

O plano deve incluir lint, testes de unidade, testes de ponta a ponta e verificação de tipos com as ferramentas existentes no projeto. Mantenha o escopo na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e guarde o URL da issue e todas as decisões acordadas para validá-las mais tarde.

## Aprove o Autopilot explicitamente

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Verifique o modo ativo, em vez de presumir que a aprovação o alterou. Confirme se o indicador de modo mostra **Autopilot**.

O Copilot começará a implementar o recurso! Ele seguirá o plano definido, gerará código e executará testes em um processo iterativo.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Pare somente os servidores que você iniciou.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes de mesclar, como faria com qualquer outro código. Vamos revisar o código e executar o site para conferir se tudo está funcionando bem.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e com as decisões aprovadas, incluindo combinações de categorias e editoras. Confirme se as alterações seguem as instruções existentes do repositório.
3. Inspecione os resultados de lint, testes de unidade, testes de ponta a ponta e verificação de tipos. Uma verificação ignorada não significa aprovação.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração de testes de ponta a ponta do Playwright compila e fornece uma prévia e pode reutilizar um servidor local. Confirme se o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

   ```plaintext
   Inicie o aplicativo e abra-o no canvas de navegador.
   ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador aparecerá dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Remova o filtro e selecione uma editora. Confirme se os resultados correspondem a essa editora.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se várias categorias forem aceitas, confira o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

   ```plaintext
   Pare o servidor de desenvolvimento e feche o canvas de navegador.
   ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem mescle o PR de filtragem. Mantenha esta sessão, worktree e branch para as Lições 4 a 8. Em seguida, aplique instruções personalizadas ao código de filtragem e adicione melhorias à skill e ao perfil de QA antes de criar e mesclar o PR combinado na Lição 8. Você pode deixar a sessão ociosa depois de parar o servidor de desenvolvimento e voltar a ela na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou o novo recurso usando o modo **Autopilot**.
- revisou o código.
- validou manualmente o recurso em um canvas de navegador.

Em seguida, vamos explorar melhor como o código é gerado e garantir que siga as práticas documentadas ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agentes no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
