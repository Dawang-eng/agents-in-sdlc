---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a publicação, mas cabe a você revisar as alterações e confirmar se o site está disponível.

Nesta lição, você vai:

- pedir ao Copilot que configure a publicação no GitHub Pages.
- revisar o fluxo de trabalho e o pull request antes de fazer o merge.
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas ainda não há uma maneira simples para outras pessoas testá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em um URL público, sem adicionar um servidor nem uma dependência de runtime.

## Peça ao Copilot para publicar o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Habilite o GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e o publique no GitHub Pages. Adicione um README que descreva o aplicativo e explique como executá-lo localmente, caso ainda não exista um. Crie um pull request com essas alterações e faça o merge depois que as verificações obrigatórias forem aprovadas. Acompanhe a publicação até ela ser concluída e informe o URL final do site ativo. Se você não tiver permissão para habilitar o Pages ou fazer o merge, diga exatamente o que preciso aprovar ou fazer; não informe que a tarefa foi concluída até o site estar no ar.
```

![Diagrama em três etapas: um pull request mesclado do space-quiz; uma execução bem-sucedida de Deploy website no GitHub Actions com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz ativo em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e faça o merge de um pull request e altere as configurações do Pages do repositório. Revise as alterações, as permissões de publicação e todas as solicitações de aprovação do GitHub antes de continuar.

## Revise o fluxo de trabalho e verifique a publicação

1. Confirme se o README foi adicionado somente se o repositório ainda não tinha um.
2. Revise as alterações do pull request, incluindo as etapas de compilação e publicação do fluxo de trabalho.
3. Faça o merge somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de publicação do GitHub Actions até a conclusão.
5. Abra o URL informado e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou fazer o merge do pull request, siga as instruções específicas fornecidas por ele. Não considere a tarefa concluída até a publicação ser concluída e o site ativo carregar.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site ativo, e você verificou a publicação em vez de confiar apenas no relatório do agente. Continue para a [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
