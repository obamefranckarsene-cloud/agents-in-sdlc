---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer — é importante garantir que ele tenha acesso a elas. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções associados a caminhos;
- gerar o primeiro conjunto de dados para filtragem (um auxiliar para editoras) com as instruções *atuais*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o efeito do novo padrão no código gerado;
- fazer commit das instruções atualizadas e do conjunto de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após a atualização das instruções, não obter exatamente o mesmo código, caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa ter testes de unidade.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- O início de cada arquivo deve conter um bloco de comentários que descreva a finalidade dele.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para que ele realize as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a entender melhor seu estilo de código e seus requisitos. Esse recurso pode orientar as sugestões para que sejam mais relevantes, inclusive especificando convenções de código, bibliotecas e os tipos de comentários que você prefere. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas para o repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações enviadas ao Copilot Chat ou ao CLI, como a pilha de tecnologias, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para orientar linguagens como TypeScript ou Astro, ou tarefas como criar um componente de interface ou novos testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para conclusões de código nem sugestões de edição seguinte.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto o arquivo de instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [aceita arquivos de instruções de outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

## Explorar os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui alguns arquivos de instruções. Vamos explorá-los antes de fazer alterações para observar o efeito dessas orientações.

1. Volte ao seu Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Essas informações se aplicam a qualquer interação com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, ele corresponde a arquivos de teste TypeScript, como os que seguem o padrão `**/*.test.ts`.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Assim, você pode dividir conjuntos de instruções maiores em arquivos menores e reutilizáveis, além de indicar exemplos que o Copilot deve seguir ao gerar código. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Criar uma branch para nossas alterações

Vamos seguir boas práticas e criar uma branch para nossas alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício — as etapas indicarão o que selecionar quando necessário.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar o resultado.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot receba apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. Selecione **Agent** na lista de agentes da visualização do Chat para permitir que o Copilot altere arquivos.

   ![Captura de tela mostrando a seleção de agente na visualização do Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt a seguir:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará as alterações de código, geralmente incluindo o arquivo auxiliar e seus testes.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — essas características vêm das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicadas a `src/lib/*.ts`).
7. Observe que o código proposto **não tem** comentários TSDoc nem um cabeçalho de comentários no nível do arquivo.

> [!CAUTION]
> Como as respostas do Copilot são probabilísticas, talvez ele adicione comentários mesmo sem essa orientação. Não há problema: o objetivo é observar a melhoria de *consistência* depois da atualização das instruções.

## Adicionar um novo padrão para o repositório

Como mencionado anteriormente, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de código para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de código, mas não contém orientações sobre documentação no código — por isso, o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`), para orientações gerais, ou como arquivos `*.instructions.md`, para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais, como a regra sobre comentários de documentação que você acabou de adicionar.

## Execute novamente o prompt e observe a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` novamente para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** usado antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo a código real que servirá de base para o exercício de filtragem seguinte.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são como qualquer outro recurso: eles são gerenciados pelo mesmo processo de controle de versão. Vamos fazer commit das alterações e enviar a branch para o repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as alterações nas instruções e no auxiliar estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit das instruções atualizadas e de todos os arquivos alterados pelo Copilot para criar a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Agora que a branch foi enviada, vamos criar um pull request associado à issue de padrões de documentação do backlog. Poderíamos fazer isso manualmente, mas o Copilot pode usar as ferramentas do GitHub conectadas ao projeto. Vamos pedir que ele encontre a issue, crie um PR para fechá-la e, em seguida, faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executará ações no GitHub em seu nome. O modelo do projeto já conecta as ferramentas do GitHub ao workspace, então não é preciso configurar nada. Na primeira vez que o Copilot usá-las, o VS Code talvez peça que você entre no GitHub. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no seu codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme se **Agent** está selecionado na lista de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR da branch atual, descrevendo tanto as atualizações de instruções quanto a nova base do auxiliar de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Se o Copilot pedir permissão para executar ações do GitHub em seu nome, revise o comando e selecione **Allow** quando apropriado.
5. Depois que o PR for criado, peça ao Copilot que faça o merge do PR e volte para `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Se o Copilot pedir permissão para executar ações do GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow** quando apropriado.

Você acabou de criar e mesclar um pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto por meio dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código proposto seguir o novo padrão;
- fazer commit das instruções atualizadas e da base do auxiliar em `main` para que o próximo exercício possa usá-las.

Agora você aplicará essas instruções no [modo agente][next-lesson], enquanto o Copilot adiciona uma nova funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para criar instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — conjunto de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
