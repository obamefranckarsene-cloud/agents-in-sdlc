---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みのフィルター issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールを使えます。ターミナルからタスクを任せたり、変更を依頼したりできるため、作業の流れを保てます。まずはツールをインストールしましょう。使い慣れたツールを使って簡単にインストールできます。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーでフィルター issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルにその機能をもたらします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイス コードの手順に従って認証し、Copilot CLI にアクセスを許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常のシェル プロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動すると、自然言語はエージェントに送信され、スラッシュ コマンドは会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択します。<kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストール済みバージョンで利用できるコマンドを確認します。<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問で Copilot が正しく動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot の回答を読み、応答の前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のように Copilot にフィルター issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、issue を読みます。最初の小さな変更を終えた後に使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree を作成することはありません。変更ごとに機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、許可済みの操作以外でツールを使うときに確認を求めます。このワークショップでは `--yolo` を付けて再起動します。ユーザーが許可したこのショートカットを使うと、Codespace 内では承認の確認が省略され、演習に集中できます。

> [!CAUTION]
> `--yolo` は、すべてのツール、パス、URL に対する自動許可 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離された環境でのみ使用し、日常の開発で既定の設定にしないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

後のレッスンで issue やプルリクエストを操作するため、`--enable-all-github-mcp-tools` を指定して読み取り/書き込みの GitHub MCP ツールも有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証された GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問を送り、会話が動作することを確認してから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で命令を変更したりエージェントを追加したりした後、`copilot --resume` で同じ会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルター issue を見つけました。

次は、[最初の小さな変更][next-lesson]として、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
