---
title: "Lição 0 - Pré-requisitos"
description: "Prepare-se para as lições do aplicativo GitHub Copilot: instale o Node.js para o projeto Tailspin Toys e crie sua cópia do repositório a partir do modelo."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O aplicativo GitHub Copilot é um aplicativo para desktop que funciona como seu espaço central para o Copilot e o GitHub. Ele oferece acesso rápido a issues e pull requests e, é claro, permite desenvolver com o GitHub Copilot. Neste workshop, você trabalhará localmente no aplicativo Tailspin Toys, desenvolvido com Astro, usando o aplicativo GitHub Copilot. Antes de começar, instale o Node.js localmente e depois instale o aplicativo Copilot.

Nesta lição, você vai:

- instalar o Node.js para executar os testes do projeto no computador.
- criar sua cópia do projeto Tailspin Toys a partir do modelo.

## Instale o Node.js

Várias lições pedirão que um agente implemente recursos e execute localmente os testes do Tailspin Toys. Para isso, é necessário o **[Node.js][nodejs]**, o único runtime exigido pelo projeto. Instale a versão **LTS** atual.

A opção mais simples em todas as plataformas é usar o instalador oficial:

1. Abra um terminal no sistema operacional. Use o Windows Terminal, o Terminal do macOS ou o aplicativo que você costuma usar.
2. Execute os comandos a seguir para verificar a versão instalada do Node.js:

   ```shell
   node --version
   npm --version
   ```

3. Se os dois comandos forem executados e o Node.js atender aos requisitos descritos no README e em `package.json` do projeto, avance para a próxima seção.

> [!TIP]
> Conclua estas etapas somente se ainda não tiver o Node.js instalado ou se precisar atualizá-lo.

4. Acesse a [página de download do Node.js][node-download].
5. Baixe a versão **LTS** para seu sistema operacional.
6. Execute o instalador e aceite as opções padrão. No Windows, mantenha selecionada a opção **Add to PATH**.
7. Depois da instalação, abra uma nova janela do terminal.
8. Na nova janela, confirme a instalação:

   ```bash
   node --version
   npm --version
   ```

9. Você deverá ver as versões instaladas do Node.js e do npm.

> [!IMPORTANT]
> O agente do aplicativo Copilot também precisa executar `node --version` e `npm --version`. O fato de os comandos funcionarem no terminal habitual não garante que estejam disponíveis para o aplicativo. Feche o aplicativo completamente e abra-o novamente depois de instalar o Node.js ou alterar o PATH. Se algum comando continuar falhando em uma sessão, siga as [etapas para solucionar problemas do ambiente da sessão][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree também precisa das dependências do projeto e do Chromium do Playwright para as verificações de ponta a ponta. Siga o README do repositório Tailspin Toys ao preparar uma worktree e revise as solicitações de instalação antes de aprová-las.

## Configure o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório modelo][template-repository]. O novo repositório contém todos os arquivos necessários para o laboratório e será conectado ao instalar o aplicativo.

1. Em uma nova janela do navegador, acesse o repositório do laboratório: `https://github.com/github-samples/tailspin-toys`.
2. Crie sua cópia selecionando **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

   ![Botão Use this template com Create a new repository selecionado no menu](../../../_images/app-0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

   ![Formulário Create a new repository com github-samples/tailspin-toys selecionado como modelo e o nome do repositório preenchido](../../../_images/app-0-create-repository.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você precisará dele mais adiante no laboratório.

> [!NOTE]
> Ao criar seu repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você usará essas issues durante o workshop — não precisa criar nenhuma.

Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, código do aplicativo, testes, uma skill `quality-checks` e uma extensão de canvas existente. Durante o workshop, você personalizará a skill e criará um agente de QA. Se usar uma cópia antiga, confira com a pessoa que conduz o workshop se ela contém os arquivos necessários.

## Resumo e próximos passos

Está tudo pronto! Nesta lição, você:

- instalou o Node.js para compilar e testar o projeto no computador.
- criou sua cópia do repositório Tailspin Toys a partir do modelo.

Em seguida, [instale o aplicativo GitHub Copilot][next-lesson], conecte o repositório criado e conheça o espaço de trabalho.

## Recursos

- [Baixar o Node.js][node-download]
- [Criar um repositório a partir de um modelo][template-repository]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicie-uma-sessao-a-partir-da-issue
