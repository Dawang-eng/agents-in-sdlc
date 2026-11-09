---
title: "Lição 8 - Crie e mescle o PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes em conjunto; depois, use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações de instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você mesclou o pull request (PR) de avaliação por estrelas por conta própria; desta vez, deixe que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações de instruções e da skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e incluiremos tudo neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar o PR do recurso completo e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar se o PR foi mesclado.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização para o merge sob o controle do desenvolvedor.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o pull request e resolve o que está impedindo o merge — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e faz o merge assim que o GitHub permitir. Ele funciona em segundo plano, continua ativo depois que o aplicativo é reiniciado e é desativado quando o pull request é mesclado.

Até agora, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de autorização explícita para editar o código e fazer o merge. Revise as ações permitidas e o trabalho antes de conceder permissão para mesclar.

## Use o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, vamos permitir que o Agent Merge gerencie o processo do PR.

1. Use o seletor de agentes para escolher **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia o processo.

O processo do Agent Merge será iniciado. Ele:

- criará o pull request com título e descrição.
- se a sessão tiver começado com uma issue, fará referência à issue na descrição.
- fará rebase ou resolverá possíveis conflitos com a branch de destino.
- acompanhará o processo de CI para garantir que todas as verificações sejam aprovadas.
- acompanhará comentários de outros desenvolvedores ou da revisão de código do Copilot e fará atualizações para resolvê-los.
- poderá fazer o merge automaticamente quando tudo estiver aprovado.

Vamos permitir que o Agent Merge também faça o merge do PR depois que todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão para mesclar.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se a opção **Merge pull request** está marcada.
8. Peça explicitamente ao agente para fazer o merge:

   ```plaintext
   Faça o merge do PR quando todas as verificações e aprovações obrigatórias forem aprovadas. Não ignore as proteções do repositório.
   ```

9. Acompanhe o progresso e resolva todos os bloqueios relatados. Antes de continuar, confirme se o PR está com o estado **Merged**. Ter habilitado a permissão de merge ou um agente em execução não confirma que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem a falta de permissões. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento, como gerar código, testar e validar o código e gerenciar o processo de pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou o PR do recurso completo e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou se o PR foi mesclado.

Em seguida, você [usará um canvas existente e criará um canvas de triagem][next-lesson] para explorar uma maneira mais rica de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
