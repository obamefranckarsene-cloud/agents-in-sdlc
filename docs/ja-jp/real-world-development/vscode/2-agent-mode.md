---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを探し、変更を行い、テストを実行し、問題をデバッグし、漏れていたファイルを特定して、さらに更新することになります。作業は次々に増えていきます。

ここで Copilot エージェント モードが役立ちます。

Copilot エージェント モードは IDE 内で、より自律的に作業します。開発者と同様に、既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェント モードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードとクラウド エージェントは異なる作業方法を提供します。エージェント モードは IDE 内で作業し、すばやいフィードバックとやりとりを可能にします。クラウド エージェントはチーム メンバーのように非同期で動作し、issue とプルリクエストを通じてやりとりします。

この演習では、次のことを学びます。

- Copilot エージェント モードがプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- GitHub Copilot エージェント モードで UI とデータ レイヤーに新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。次は、残りのデータ レイヤー、UI、テストの作業を Copilot エージェント モードで完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示します。単一の Astro アプリで、ビルド時に静的 HTML ページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、対話機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Codespace の Web サーバーをホストするプライベート トンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認します。主なページは次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボード ゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で指定します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトを設定すると issue のバックログが自動的に作成されます。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** はファイル間の変更を自律的に計画して実装し、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高めることが目的ですが、LLM の出力は確率的なため、手順とは異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目を優先順位付けしたことを確認します。

## 命令ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える命令ファイルを確認します。[ユーザー インターフェイス (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトに機能を追加する方法のコンテキストが記載されています。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加のガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計・テスト容易性・アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などのファイル向け命令へのリンクが含まれます。

> [!TIP]
> 命令ファイルから、他の命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルからの相対パスです。命令を再利用し、複雑な内容を管理しやすい単位に分け、Copilot に例やテンプレートを示せます。

## フィルター機能を実装する

フィルターを完成させるには、アプリケーションに少なくとも 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りのフィルター ロジックを追加または改善する。
- フィルターの動作をテストするテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot エージェント モードにこれらの作業を依頼しましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、命令ファイルを追加します。

   ![UI 命令ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関係するファイルを見つける様子を確認します。データ レイヤーのヘルパー、UI、テストを特定し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べている様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストの実行など、複数の操作を行います。タスクやコードベースの複雑さによって数分かかる場合があります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。完了後はすべての変更に対して選択肢が表示されるため、作業中に選択する必要はありません。

7. Copilot に確認されたら、**Continue** を選択してテストを実行します。

   ![テストの実行を確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に処理が一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了し、エラーがないと判断するまでコード生成とテストを繰り返します。

   ![Copilot エージェント モードのチャット セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェント モードを使って Tailspin Toys Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot エージェント モードで UI とデータ レイヤーに機能を実装する方法。
- Copilot エージェント モードがプロジェクトを調べ、関連ファイルを特定し、複数ファイルを連携して変更する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを開きます。

### ボーナスの演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割の実装を依頼してみましょう。

- 既存の命令ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズをユーザーが選べるようにするか、固定にするかなど、ページ分割の方法を検討します。
- 希望する実装に必要なガイダンスを、プロンプトで Copilot に伝えます。
- Copilot に変更を依頼し、コンテキストを伝えながら何度かやりとりすることがあります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
