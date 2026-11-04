---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot アプリで最初のエージェント セッションを開始し、ゲーム カードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は、**エージェント セッション**を開始し、プロジェクトに初めて変更を加えます。小さな変更から始めます。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構成を学びます。
- エージェントにプロジェクトの小さく焦点を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードには、タイトル、カテゴリ、発行元、説明しか表示されていません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションにぴったりの、小さく完結した変更です。

## セッションの構造

**セッション**は、独立した専用ワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられます。これにより、たとえば一方のセッションで機能を追加しながら、別のセッションでバグを修正しても、変更が競合しません。セッションはサイドバーでリポジトリごとにまとめられ、選択すると切り替えられます。

セッション内では、エージェントとの**会話**、ファイルを調べて編集する**ツール アクティビティ**、差分付きの**変更ファイル一覧**を確認できます。

## セッションを開始し、変更を依頼する

新しいセッションを開始し、プロジェクトを調べて機能を実装します。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用に新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot アプリを開きます。すでに開いている場合はそのまま戻ります。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトを入力します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot アプリは、新しい worktree (プロジェクトの分離されたコピー) を作成して作業を開始します。プロジェクトを調べ、新機能に必要なファイルを見つけ、コードを作成します。Copilot アプリを使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージ前にレビューします。Copilot アプリ内で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択すると、変更ファイルと差分が表示されます。

   ![Create PR ボタンの右側に Toggle review panel が表示された GitHub Copilot アプリの上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細表示に使われる中心的なファイル `GameCard.astro` に、コードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" と表示する、次のような小さなブロックが追加されます。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは決定的ではなく、確率に基づいて結果を生成するため、コードは上記と異なる場合があります。ただし、比較的似たコードになるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と、`null` の場合の代替表示の両方をテストしていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認してください。

コードを読んだだけで動くと判断するのではなく、Copilot に Web サイトを起動させ、更新された UI を確認します。Web サイトを起動して、ブラウザー キャンバスで開くよう依頼します。

> [!TIP]
> キャンバスは、Copilot アプリ内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム キャンバスを調べて自分でも作成しますが、ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトを使い、アプリを起動してブラウザー キャンバスで開くよう Copilot に依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot アプリ内にブラウザー ウィンドウが開きます。
3. 評価があるゲーム カードに、5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトで、このセッションに対して起動した開発サーバーを停止するよう Copilot に依頼します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、既存のコードベースに新しいコードをマージするため、プルリクエスト (PR) を作成します。

1. 右上にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーにより PR の前に関連 issue が必要だと Copilot が説明した場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで PR を開きます。必要に応じてここでレビューします。
6. 準備ができたら **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

おめでとうございます。GitHub Copilot アプリを使い、最初の変更をリリースしました。具体的には、次のことを行いました。

- エージェント セッションを開始し、セッションの構成を学びました。
- エージェントにゲーム カードへの小さく焦点を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、明示的にマージしました。

次は、[フィルター issue から始め、Plan と Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot アプリでエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot アプリについて][about-copilot-app]
- [GitHub Copilot アプリで issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-アプリをインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
