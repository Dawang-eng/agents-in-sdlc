---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem seja assíncrono e autônomo, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas do agente com pull requests abertos. Explore as operações realizadas e oriente uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois que a equipe atribuiu ao agente a tarefa de criar um modo de alto contraste, percebeu que seria uma boa oportunidade para adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, pareceu natural incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente também o modo claro.

## Revise as tarefas do agente de nuvem do Copilot

Vamos conferir o estado atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações executadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exatas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel de pull requests (PR) exibido à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que adicione um controle de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à implementação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o agente comece a trabalhar. Você perceberá quando as mensagens correspondentes começarem a aparecer.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver uma opção para esse modo que permita às pessoas selecionar o modo de exibição desejado.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar uma sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot confirmar o prompt e incluí-lo no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada! Ele incluirá a solicitação quando terminar a etapa em andamento no momento em que você enviou a mensagem.

Isso levará alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que você aprendeu até agora.

## Resumo e próximos passos

Este exercício apresentou a página de agentes do Copilot, o painel central para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- usou o painel central e a página de agentes para acompanhar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Com o Copilot concluindo o trabalho nos recursos de acessibilidade, agora podemos [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
