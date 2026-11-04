---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo à medida que ocorrem. Você pode revisar cada diff no contexto, aceitar ou rejeitar alterações individuais e direcionar a conversa com prompts complementares para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições de arquivo propostas;
- orientar o agente durante a sessão para adicionar um modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado na [Lição 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez uma solicitação adicional: também gostaria de oferecer um modo claro para usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa do zero, você vai orientar a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização do Copilot Chat mostra cada etapa executada pelo agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização do Copilot Chat da Lição 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados pelo agente nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente pausará e exibirá uma solicitação **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Você verá uma mensagem final que resume as alterações feitas.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar uma opção de modo claro. Como está continuando a sessão existente, o agente mantém todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão usado para armazenar preferências localmente.

1. No campo de entrada do Copilot Chat, envie o prompt complementar a seguir:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo atualizações para o modo claro.
3. Revise cada nova edição à medida que aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar a interface das opções de alternância de uma forma que você não queira — interrompa-o com o botão **Stop** ao lado do campo de entrada do chat e envie uma mensagem complementar para esclarecer o que deseja.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para solicitar ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione uma terceira opção; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione atalhos de teclado para ambas as opções").

## Revisar o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns arquivos alterados e percorra os diffs. Confirme se as práticas de acessibilidade do agente personalizado foram aplicadas — atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- monitorar o trabalho de um agente em tempo real;
- aceitar e rejeitar edições individuais de arquivos;
- orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
