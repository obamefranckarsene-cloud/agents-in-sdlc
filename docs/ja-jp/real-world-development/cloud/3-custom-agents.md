---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に適した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の種類の作業を効果的に実行するための命令、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントにできます。たとえば、[WCAG][wcag] 準拠をチェックするアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントなどです。

カスタム エージェントはプロジェクト内の `.github/agents` にある Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI が作業を実行する方法を示します。最も分かりやすい違いは、**カスタム エージェントは作業者、スキルはツール**だということです。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルや他のエージェントを調整します。この演習では、アクセシビリティのカスタム エージェントがサイトをアクセシビリティ ガイドラインに照らしてレビューし、更新します。その際、プルリクエストのワークフロー用スキルやテスト実行用スキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI の他の機能と同様に、環境やシナリオに合わせて試し、反復的に改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次のことを確認します。

- カスタム エージェントの定義方法。
- カスタム エージェントにタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。ユーザーからのフィードバックによると、現在のダーク テーマではテキストと背景色のコントラストが不十分で、読みにくい場合があります。デザイン チームは、オン/オフを切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot を導く方法を把握しましょう。

Codespace に戻り、次のファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成での主な責任、アクセシビリティのベスト プラクティス、HTML、CSS、JavaScript のコード例、よくある落とし穴や間違いが説明されています。

## issue を作成して割り当てる

Mission Control は環境内のエージェントを操作する場所です。Copilot クラウド エージェントにタスクを割り当て、監視し、追加の指示を出すことができます。まず、ハイ コントラスト モードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択します。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントと Accessibility が選択されているクラウド エージェント割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや分野に適した専門的な AI アシスタントです。カスタム エージェントを使うと、チームの専門知識や標準を再利用できる形にして、Copilot の作業を導けます。

次の内容を確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントにタスクを割り当てる方法。

Copilot がハイ コントラスト モードを実装する間に、Mission Control から[エージェント セッションを監視して案内します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot クラウド エージェント][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
