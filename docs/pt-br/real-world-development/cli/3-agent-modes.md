---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para implementar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto, mas alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para funcionar com o fluxo de trabalho existente da organização e ajudar a construir as soluções certas da maneira adequada. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará por uma issue para gerar uma funcionalidade, verificará se o código é válido e se a funcionalidade se comporta como esperado e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e a mesma branch durante todo o fluxo de trabalho da funcionalidade. Normalmente, você usaria branches ou PRs diferentes para os vários tipos de arquivos, mas faremos esse atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub;
- definir requisitos no modo Plan;
- implementar a funcionalidade com o modo Autopilot;
- revisar o código;
- validar manualmente a funcionalidade em um navegador encaminhado.

Durante o desenvolvimento desta funcionalidade, você também atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará a validação via MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes de escrever o código. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação com escopo limitado usando o Autopilot.

## Contexto

Incluir agentes de programação com IA no fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. O processo de muitas equipes inclui estas etapas:

1. Começar com uma issue que descreva o trabalho necessário.
2. Planejar o que será criado.
3. Criar e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão e da aprovação da integração contínua, fazer o merge do código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria segue alguma variação desse processo.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria primeiro um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando até concluir a tarefa.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Começar pela issue

Antes de começar o trabalho de filtragem, volte ao Codespace e confirme se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o antes de continuar.
2. Confirme se o PR de avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite adicionar ao contexto uma issue ou um pull request (PR) do GitHub.

## Planejar a filtragem

O planejamento permite definir a abordagem para implementar uma funcionalidade ou executar tarefas antes de delegá-las ao Copilot. Para tarefas complexas, é sempre bom reservar um tempo para planejar. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a explorar o projeto e a definir a melhor abordagem.

3. Durante o processo, talvez o Copilot pergunte como a filtragem deve funcionar. Responda de acordo com suas preferências. Não há respostas certas ou erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com nome semelhante na versão instalada.
2. Confirme se o indicador de modo abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot executar o plano, gerar código e rodar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Depois que o código for gerado, ele precisa ser revisado antes do merge, como qualquer outro código. Vamos revisar o código e iniciar o site para confirmar se tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme se o indicador de modo não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija eventuais falhas.

## Explorar a nova funcionalidade

O código parece correto — mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria e editora e as combinações acordadas no plano.
4. Confirme se a redefinição dos filtros e o comportamento sem resultados correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que encerre o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub;
- definiu requisitos no modo Plan;
- implementou a funcionalidade com o modo Autopilot;
- revisou o código;
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, vamos explorar como garantir que o código siga as práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
