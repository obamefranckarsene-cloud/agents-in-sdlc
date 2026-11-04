---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に適した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の種類の作業を効果的に実行するための命令、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠をチェックするアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどです。

カスタム エージェントはプロジェクト内の `.github/agents` にある Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI が作業を実行する方法を示します。最も分かりやすい違いは、**カスタム エージェントは作業者、スキルはツール**だということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルや他のエージェントを調整します。このラボでは、アクセシビリティのカスタム エージェントがサイトをアクセシビリティ ガイドラインに照らしてレビューし、更新します。その際、プルリクエストのワークフロー用スキルやテスト実行用スキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI の他の機能と同様に、環境やシナリオに合わせて試し、反復的に改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。ユーザーからのフィードバックによると、現在のダーク テーマではテキストと背景色のコントラストが不十分で、読みにくい場合があります。デザイン チームは、オン/オフを切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot を導く方法を把握しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用のブランチを作成します。フィルター PR は演習 3 のブランチに残します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成での主な責任、アクセシビリティのベスト プラクティス、HTML、CSS、JavaScript のコード例、よくある落とし穴や間違いが説明されています。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義されているカスタム エージェントが表示されます。カスタム エージェントを選択すると、そのエージェントの命令とツールに合わせてチャット セッションが設定されます。

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で指定します。

1. まだ選択されていない場合は、Chat ビューのエージェント ドロップダウンから **Agent** を選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。既定では、アクティブなエージェントとして **default** が表示されます。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。編集ごとに、ファイル パスとレビューできる差分がチャットに表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> 実際にリポジトリを変更するため、数分かかる場合があります。Astro コンポーネント、CSS、関連テストなどが編集されます。

作業中の内容は次の演習でレビューし、追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや分野に適した専門的な AI アシスタントです。カスタム エージェントを使うと、チームの専門知識や標準を再利用できる形にして、Copilot の作業を導けます。

このレッスンでは、次のことを確認しました。

- カスタム エージェントの定義方法。
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法。

次は、同じセッションにライト モードの切り替えを追加するよう依頼し、作業を[監視して案内します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
