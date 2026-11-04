---
title: "Lição 5 - Personalizar e usar uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas programar. Já verificamos manualmente se o código funciona e usamos arquivos de instruções para garantir que ele siga nossos padrões. Mas e os testes? E o lint? E as outras partes da integração contínua (CI)?

Para esses tipos de tarefa, as **skills de agente** são ideais. As skills ajudam o Copilot a entender como executar operações como essas.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes de unidade, lint e verificação de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que um agente carrega sob demanda. No centro de uma skill há uma pasta com o nome da skill e um arquivo Markdown chamado `SKILL.md`. O arquivo Markdown contém frontmatter com um nome e uma descrição que definem a skill, uma visão geral do que ela faz e orientações sobre quando deve ser usada. A pasta também pode conter subpastas com scripts e outros recursos que a skill utiliza quando é chamada.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A skill `quality-checks` da Tailspin Toys contém somente `SKILL.md` porque usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` de um projeto e ser compartilhadas pela equipe como um recurso do repositório, ou na pasta de skills do usuário `~/.copilot/skills`.

## Explorar a skill

Vamos explorar a skill `quality-checks` criada pela equipe da Tailspin Toys para executar testes de unidade, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia os campos `name` e `description` no início do arquivo. A descrição ajuda o Copilot a entender quando deve chamar a skill.
3. Leia as instruções e observe como elas orientam o Copilot durante os testes e o lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Executar a skill antes de alterá-la

Você pode chamar skills diretamente pelo Copilot CLI ou usando linguagem natural. Vamos pedir ao Copilot para executar as três verificações da skill.

1. Volte à conversa sobre filtragem no modo Interactive.
2. Use o prompt a seguir:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalizar o relatório

Queremos um relatório melhor, que informe o que foi executado, se houve sucesso e o que as ferramentas relataram. Vamos atualizar a skill para gerar esse relatório.

1. Volte para `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

Agora que fizemos a alteração, vamos ver como ela funciona. O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes de unidade, lint e verificação de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
