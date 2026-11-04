---
slug: pt-br/real-world-development/cloud
title: "Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O [**agente de nuvem do GitHub Copilot**](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent) permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui uma tarefa no GitHub, e o agente de nuvem a executa em segundo plano — explorando o repositório, fazendo alterações e abrindo um pull request — enquanto você se dedica a outras atividades.

Ao longo destes exercícios, você adicionará instruções personalizadas que o agente de nuvem seguirá e, em seguida, atribuirá uma issue do GitHub para que ele implemente a tarefa. Você vai revisar e usar agentes personalizados para orientar a abordagem, monitorar e direcionar sessões no painel de agentes e, por fim, revisar os pull requests e iterar sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione as instruções personalizadas que o agente de nuvem seguirá |
| [2. Agente de nuvem][ex2] | Agente assíncrono | Atribua issues ao agente de nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Monitore e direcione sessões de agentes |
| [5. Iterar][ex5] | Revisão | Revise PRs, itere sobre o trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, confira se você tem:

- [ ] Uma conta do GitHub com um plano **Copilot Student, Pro, Pro+, Business ou Enterprise** ativo
- [ ] O agente de nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem usar o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e os recursos do Copilot CLI usados neste workshop — assim, você pode concluir todos os roteiros com ele.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O agente de nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — em todos, exceto no Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para a organização.

> [!NOTE]
> Este roteiro não aborda MCP. Para aprender a usar servidores MCP com o Copilot, consulte o [roteiro do CLI](../cli/) ou o [roteiro do VS Code](../vscode/).

## Comece

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
