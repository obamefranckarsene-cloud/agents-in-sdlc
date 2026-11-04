---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações — se é que alguma — não enfrentam dificuldades com dívidas técnicas. Elas podem incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi desenvolvido para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele cria um PR como rascunho para que um desenvolvedor possa revisar. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em atividades de maior impacto.

Neste exercício, você vai explorar como:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- definir issues com escopo claro;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. Por enquanto, a equipe não precisa de uma implementação refinada; quer apenas algo que possa usar em testes de aceitação da experiência. Essa tarefa está bloqueando o trabalho, mas há outras issues de prioridade mais alta no momento.

Esses são dois exemplos de tarefas que podem acabar sendo adiadas e que são ótimas candidatas para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, permitindo que o desenvolvedor se concentre em outras atividades e depois volte para revisar o trabalho do Copilot e confirmar se está adequado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de maneira semelhante a um desenvolvedor humano. Assim como no trabalho com outra pessoa desenvolvedora, há várias formas de atribuir tarefas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que você atribui uma issue, o Copilot cria um pull request como rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão do Copilot enquanto ela está em andamento ou depois que termina. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: você ainda precisa pensar. Mesmo operações aparentemente simples podem se tornar complexas quando analisadas em detalhe. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se você já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir uma tarefa ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos neste repositório. Isso ajudará você a perceber o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub para seu repositório ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que associa essas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir ao agente de nuvem a issue `Code lacks documentation`, na próxima seção, observe no pull request resultante os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários. Essas orientações vêm dos arquivos de instruções que você acabou de revisar. Voltaremos a esse assunto ao revisar PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

Criar código, independentemente de quem esteja fazendo o trabalho, normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow especial de configuração][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml`, para ser executado antes do início do trabalho. Isso permite disponibilizar as ferramentas e dependências de desenvolvimento necessárias. Esse workflow já foi configurado para este laboratório, para ajudar no andamento das atividades e oferecer uma oportunidade de aprendizado. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e populado do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se quiser clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migra e popula o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Assim, você pode verificar se ele funciona sem esperar uma execução do Copilot.

## Adicionar documentação

Todo mundo entende a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas à equipe. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue perfeita para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, cujo título será parecido com `[WIP]: Code lacks documentation`. Se não aparecer um novo PR na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado do projeto, pausa para pensar e decidir a abordagem adequada e também cria código.

O processo provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente de nuvem do Copilot e, enquanto isso, escreveremos código para adicionar funcionalidades ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes do jogo

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Talvez a inclusão de uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas esse tempo poderia ser usado em outras atividades. Vamos atribuir a tarefa ao agente de nuvem do Copilot!

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá alguns 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando na sua segunda solicitação! O agente de nuvem trabalha de maneira semelhante a um engenheiro de software, então você não precisa monitorá-lo continuamente. Basta revisar o trabalho quando ele terminar. Vamos voltar nossa atenção para a criação e o uso de agentes personalizados.

## Resumo e próximos passos

Nesta lição, você conheceu o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, você pode atribuir issues ao Copilot para que ele as execute de forma assíncrona. O Copilot pode ajudar a resolver dívidas técnicas, criar novas funcionalidades ou migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir que as operações sejam realizadas com segurança;
- definir issues com escopo claro;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, você pode se dedicar à criação e ao uso de agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e pode acessar instruções personalizadas, que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

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
