---
title: "Exercício 2 - Adicionar funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir alterações em vários arquivos e a execução de operações como testes. Em geral, o fluxo de trabalho de uma pessoa desenvolvedora envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... e por aí vai.

É aí que entra o modo agente do Copilot.

O modo agente do Copilot foi criado para atuar de forma mais autônoma no IDE. Ele trabalha de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os erros encontrados. Vamos explorar como você pode usar o modo agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo agente e o agente de nuvem foram criados para experiências diferentes. O modo agente trabalha no seu IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem atua como uma pessoa parceira da equipe, trabalhando de forma assíncrona e interagindo com você por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o modo agente do Copilot pode explorar seu projeto, identificar arquivos relevantes e coordenar alterações;
- o modo agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados;
- revisar alterações e testes gerados pelo modo agente do Copilot antes de incorporá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. No exercício anterior, você adicionou um auxiliar para editoras; agora concluirá o trabalho restante na camada de dados, na interface e nos testes usando o modo agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir detalhes de cada um. O site é um único aplicativo Astro, que renderiza suas páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de back-end separada nem uma estrutura de interface no lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um pequeno elemento `<script>` do Astro, limitado ao escopo necessário e usando APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao seu codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra uma nova janela de terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo perguntar se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link para a URL local no terminal do Codespace redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para seu codespace, onde o servidor Web está sendo executado.

### Explorar o site

Depois que o site estiver em execução, você poderá explorar suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você abre uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício — as etapas indicarão o que selecionar quando necessário.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar consultando o backlog. Ao configurar o projeto a partir do modelo, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** na lista de agentes da visualização do Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos de terminal e chama ferramentas.

   ![Captura de tela mostrando a seleção de agente na visualização do Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em particular. Durante a criação do workshop, usamos Claude Sonnet 4.5 e, por isso, o incluímos nas instruções. A expectativa é que as sugestões de código recebidas sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como as respostas de LLMs são probabilísticas, talvez suas sugestões sejam diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot que mostre o backlog enviando o prompt a seguir:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot também priorizou os itens de acordo com o que considera mais útil para o usuário.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, vale revisar o arquivo de instruções que fornecerá contexto. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções específicos de vários tipos de arquivo, como:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar orientações, dividir conjuntos maiores em partes menores e gerenciáveis e apontar o Copilot para exemplos e modelos.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três atualizações distintas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento de filtragem;
- atualizar a página da lista de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge. O modo agente do Copilot pode fazer esse trabalho. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como o arquivo de instruções.

   ![Captura de tela mostrando um exemplo da seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado na lista de agentes da visualização do Chat.

   ![Captura de tela mostrando a seleção de agente na visualização do Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue correspondente do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe como o Copilot começa explorando o projeto e localizando os arquivos relacionados à funcionalidade desejada. Você verá que ele encontra os auxiliares da camada de dados, a interface e os testes. Em seguida, começa a modificar os arquivos e executar os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você terá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecionar nenhuma delas enquanto o trabalho estiver em andamento.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Captura de tela mostrando uma caixa de diálogo do painel do Copilot Chat pedindo confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e até falhas em alguns testes durante o processo. Não há problema: o Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão concluída do modo agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu que:

- o modo agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados;
- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- é importante revisar as alterações e os testes gerados pelo modo agente do Copilot antes de incorporá-los à base de código.

Agora vamos [testar sua funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus de exploração — implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Com base no que aprendeu neste exercício, peça ao Copilot que atualize o site com paginação. Ao planejar o código:

- siga as boas práticas existentes, incluindo o uso dos arquivos de instruções disponíveis;
- decida como a paginação deve funcionar, por exemplo, se o usuário poderá selecionar o tamanho da página ou se ele será fixo;
- inclua no prompt as orientações necessárias para que o Copilot implemente o comportamento desejado;
- talvez seja necessário iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agente do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
