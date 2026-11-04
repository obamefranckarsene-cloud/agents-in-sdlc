---
title: "Lição 8 - Criar e mesclar o PR da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Sua implementação de filtragem, as atualizações das instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. Agora é hora de revisar tudo em conjunto e abrir um pull request. Você mesmo fez o merge do pull request (PR) de avaliação por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, mantivemos todo o trabalho de filtragem e do fluxo de QA em uma conversa e uma branch, que serão incluídas neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionar todas as alterações da funcionalidade e as evidências de verificação;
- criar o PR de filtragem;
- habilitar o Agent Merge somente depois da revisão e confirmar se o PR foi mesclado.

## Cenário

Ao longo do fluxo de trabalho de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle do desenvolvedor.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir um pull request. Quando você o habilita, o Copilot resolve o que estiver bloqueando o PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub, se o repositório permitir.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme se está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado, vamos revisar as alterações em conjunto, criar o PR e deixar o Agent Merge gerenciar as próximas etapas.

1. Volte ao seu Codespace.
2. Abra a caixa de diálogo de agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie um PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua no PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme se o Copilot exibiu uma mensagem semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e, quando o repositório permitir, conclua manualmente o merge que você já revisou.

## Resumo e próximos passos

Você automatizou várias partes do processo de desenvolvimento: geração de código, testes e validação e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionou todas as alterações da funcionalidade e as evidências de verificação;
- criou o PR de filtragem;
- habilitou o Agent Merge somente após a revisão e confirmou se o PR foi mesclado.

Em seguida, você vai [explorar outros comandos slash úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
