---
title: "演習 2 - GitHub Copilot クラウド エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム命令][previous-lesson]

多くの組織が、未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能リクエストなど、技術的負債に直面しています。GitHub Copilot クラウド エージェントは、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、時間のかかる作業を委任し、開発者はより大きな課題に集中できます。

Copilot クラウド エージェントで、次の内容を確認します。

- コード生成環境のカスタマイズ。
- 操作が安全に行われるようにする方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した委託先は、ドキュメントを適切に整備しませんでした。まず、アプリケーションのエクスポート関数すべてに TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、ユーザーが閲覧を続けられるようにします。洗練された実装はまだ必要なく、UX の受け入れテストに使える状態を目指します。これは現在のブロッカーですが、他に優先度の高い issue があります。

どちらも、優先順位が下がりやすく、Copilot クラウド エージェントに適したタスクです。非同期で作業させている間に別のタスクに取り組み、後で結果をレビューできます。

## GitHub Copilot クラウド エージェントとは

[GitHub Copilot クラウド エージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備して作業を開始します。作業中や完了後に Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確に定めた命令の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot クラウド エージェントに依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階を踏み、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令が Copilot を導く方法を学びました。すでにカスタム命令の演習を完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、Copilot クラウド エージェントに作業を割り当てる前に、リポジトリに含まれる命令ファイルを読み取り専用で確認しておきましょう。

GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` のファイルに命令を適用することを確認します。

次のセクションで **Code lacks documentation** issue をクラウド エージェントに割り当てたら、これらの命令ファイルに由来する TSDoc コメント、TypeScript の規約、コメント ヘッダーを確認してください。後の演習で PR をレビューするときにも取り上げます。

## Copilot クラウド エージェント用の開発環境を設定する

コードを作成する際は、通常、環境の準備とセットアップ スクリプトの実行が必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェア エンジニアのように作業します。

クラウド エージェントは作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に [セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。演習の進行を簡単にし、学習に集中できるよう、サンプルではすでに構成されています。単一の Astro アプリ用に Node.js、プロジェクトの依存関係、E2E テスト用のブラウザー バイナリ、マイグレーションとシード済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # このセットアップ手順に必要な最小限の権限を設定します。Copilot の操作には専用トークンが付与されます。
    permissions:
      # セットアップ手順でリポジトリをクローンする場合 (依存関係のインストールなど) は contents: read が必要です。クローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリは Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションしてシードし、ビルドとテスト用のデータを準備します。
      - name: Set up the database
        run: npm run db:setup
```

通常の GitHub ワークフローと同様ですが、重要な点が 2 つあります。

- `copilot-setup-steps` というジョブが 1 つあります。これは Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動で実行できます。Copilot の実行を待たずに、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり不足していたりするプロジェクトがほとんどです。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、クラウド エージェントに適した issue です。バックグラウンドでドキュメントを生成してもらい、後の演習で結果を確認します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは **[WIP]: Code lacks documentation** のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作成した To Do リストが表示されます。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかる場合があります。前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を判断しています。

13. リストを確認し、実行するタスクを把握します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウの更新が必要な場合があります。

16. ライブ セッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を理解し、考えながら適切な計画を立て、コードを作成していきます。

数分かかることがあります。Copilot クラウド エージェントの大きな利点は、タスクを非同期に実行し、別の作業に集中できることです。さらに別のタスクをクラウド エージェントに割り当て、アプリケーションに機能を追加する作業を進めましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot クラウド エージェントを使う利点は、作業を分担できることです。デザイン チーム向けの関連ゲームの追加に時間がかからないとしても、その時間を別の作業に使えます。クラウド エージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択します。
9. **Assign** を選択します。

まもなく、Copilot が issue の最初のコメントに 👀 を表示し、作業を開始したことが分かります。

![Copilot が目の絵文字で issue の作業中であることを示している](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot クラウド エージェントはソフトウェア エンジニアと同じように作業するので、常に監視する必要はありません。完了後にレビューします。次は、カスタム エージェントの作成と使用を確認しましょう。

## まとめと次のステップ

[GitHub Copilot クラウド エージェント][copilot-agents]について学びました。issue を Copilot に割り当てると、非同期に作業できます。技術的負債の解消、新機能の追加、別のフレームワークへのコード移行などに活用できます。

次の内容を確認しました。

- コード生成環境のカスタマイズ。
- 操作が安全に行われるようにする方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

クラウド エージェントがバックグラウンドで作業している間に、カスタム エージェントの作成と利用に取り組みましょう。[Copilot クラウド エージェントは MCP サーバー][cloud-agent-mcp]と、前のモジュールで扱ったカスタム命令も利用できます。

## リソース

- [Copilot クラウド エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウド エージェントのセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
