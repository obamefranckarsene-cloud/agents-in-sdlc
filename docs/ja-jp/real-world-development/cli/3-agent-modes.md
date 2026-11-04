---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を合意し、Autopilot で issue からフィルターを構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まずは小さな機能をプロジェクトに追加しました。より大きな変更には、さらに堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存のフローに沿って動作できるため、適切な方法で必要な機能を構築できます。ここから複数のレッスンにわたり、一般的なエージェント駆動型の開発プロセスに取り組みます。issue を使って機能を作成し、コードが有効で、機能が期待どおりに動作することを確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能の作業を続ける間は、同じ会話とブランチを使います。通常は作業するファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するために簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送ブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで方針を決めた後、範囲を限定した実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のようなフローで作業しています。

1. 必要な作業を記述した登録済みの issue から始めます。
2. 実装する内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードのレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、ほとんどは上記の流れを基本にしています。

この標準的な方法に従えば、AI が生成したコードが要件を満たし、手書きのコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントの自律性を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと共同で作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルは編集できません。
- **Autopilot**: エージェントは自律的に作業し、タスクが完了するまでコードを書き、テストを実行し、反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から開始する

フィルターの作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルをまだ開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押し、**Issues** タブを開きます。
7. <kbd>A</kbd> を押して、すべての issue を表示します。
8. 矢印キーを使って、**Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が追加されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てることで、Copilot に任せる前に、機能やタスクの実装方法を定義できます。複雑な作業では、時間をかけて計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画を作成するよう依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 先ほど追加した issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査して、最適な方法を判断しながら計画を作成します。

3. フィルターの動作について質問された場合は、希望に応じて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を上下にスクロールしてレビューします。
6. 自分の決定に合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot が完了したと判断するまで問題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様の名前のオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、セットアップの問題を解決してからチェック結果を確認します。

## 実装をレビューして検証する

他のコードと同じように、生成されたコードはマージする前にレビューする必要があります。コードをレビューし、サイトを実行して問題がないことを確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** や **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. issue と計画で決めた内容に結果が合っていることを確認します。
4. コードをレビューしたら、<kbd>Esc</kbd> を押して差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードが良さそうでも、実際に動くでしょうか。以前と同様にアプリを起動し、Codespaces の転送ポートを使ってサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カテゴリ、発行元、および計画で合意した組み合わせを試します。
4. リセットと結果が空の場合の動作が、issue と合意内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使い、機能を構築してレビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新しい機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動で検証しました。

次は、[カスタム命令を使って][next-lesson]コードの生成方法を指定し、文書化されたプラクティスに従っていることを確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
