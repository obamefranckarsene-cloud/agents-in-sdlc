---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que adicionasse documentação ao código, criasse uma funcionalidade de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo opções de modo de alto contraste e modo claro. Vamos explorar as alterações no código e, se necessário, enviar comentários ao Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre devemos revisar o código gerado e seguir nosso processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar documentação, a funcionalidade de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, foram definidas algumas restrições de segurança para manter tudo protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions e cria um ambiente separado e temporário para trabalhar.
- Qualquer workflow do GitHub Actions precisa da aprovação de uma pessoa antes de ser executado.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Você fará essa revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez você veja um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desativar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título parecido com **Add missing documentation** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a marca **[WIP]**. Nesse caso, aguarde o Copilot terminar. Isso pode levar alguns minutos, então você pode fazer uma pausa ou refletir sobre o que aprendeu até aqui.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc recém-criados e outros elementos de documentação. As alterações exatas podem variar.

    Ao examinar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários. Essas orientações vêm dos arquivos de instruções personalizadas que você revisou no início do Exercício 2.

6. Depois de revisar as atualizações e confirmar que estão adequadas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode marcar o Copilot em comentários no PR — como faria com outra pessoa da equipe — ou em comentários inseridos diretamente no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos indicar um texto exato para solicitar alterações. Algumas ideias:

- Adicionar cabeçalhos de comentários no início de cada arquivo de código, com uma breve descrição do conteúdo.
- Adicionar comentários TSDoc a arquivos TypeScript e Astro.
- Criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e marque **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe como ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, você verá um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando estiver tudo pronto, você poderá marcar o PR como pronto para revisão e fazer o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

Vamos voltar ao PR que o Copilot criou para resolver a issue sobre exibir jogos relacionados na página de detalhes de cada jogo.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Show related games on the game details page** ou outro título mais específico.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações e confirmar que estão adequadas, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão ser adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode mudar para essa branch no Codespace para testar manualmente a funcionalidade de jogos relacionados. Acesse seu Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou a funcionalidade de jogos relacionados! Como antes, você pode trabalhar de forma iterativa com o agente de nuvem do Copilot para solicitar atualizações. Por exemplo, talvez você queira ajustar a quantidade de jogos relacionados exibidos ou solicitar que ele adicione cabeçalhos de comentários e comentários TSDoc (lembre-se: essa tarefa foi atribuída **antes** de você atualizar as instruções personalizadas). Assim como antes, você pode fazer essas solicitações adicionando um comentário na guia **Conversation**. O Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir tanto o modo de alto contraste que você solicitou no Exercício 3 quanto o modo claro pedido no painel de controle do Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título parecido com **Add high contrast mode to website** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a marca **[WIP]**. Nesse caso, aguarde o Copilot terminar. Isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação e preste atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para manter as preferências do usuário;
   - alterações de CSS ou de estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.);
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.

6. Depois de revisar as atualizações e confirmar que estão adequadas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser adicionados à seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse seu Codespace, abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os modos de alto contraste e claro no navegador para confirmar se funcionam como esperado e se as preferências permanecem após recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou a orientar o Copilot para implementar esses recursos seguindo boas práticas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, você pode marcar **@copilot** em um comentário e solicitar atualizações, como fez nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras atividades. Para desenvolver esse hábito, crie mais algumas issues para seu repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário para que apoiadores demonstrem interesse na página de detalhes do jogo.
- Implementar paginação na página com a lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o roteiro do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizados incluídos no repositório** para ver o efeito deles nos resultados do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções de modo de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e direcionar a sessão de acessibilidade em andamento.
- **Revisou e refinou os pull requests do agente de nuvem**, marcando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o roteiro do agente de nuvem. Se quiser continuar explorando, os outros roteiros complementam o que você praticou:

- 🖥️ **[Roteiro do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Roteiro do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma integração opcional `/delegate` com o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- Atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- Crie um agente personalizado para outro domínio.
- Configure `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
