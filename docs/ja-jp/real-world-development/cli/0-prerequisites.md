---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。次のレッスンでは、統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。
- Codespace を作成し、プロジェクトを使う準備ができていることを確認します。

## 演習用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーで作業します。[テンプレート リポジトリ][tailspin-template]からコピーを作成します。新しいリポジトリにはラボに必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]に移動します。
2. **Use this template** を選択し、次に **Create a new repository** を選択します。
3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。
4. 後で参照するため、作成したリポジトリのパス (`organization-or-user-name/repository-name`) をメモします。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使うため、自分で登録する必要はありません。

ワークショップのテンプレートの新しいコピーを使用してください。リポジトリの命令、アプリケーションのコード、テスト、`quality-checks` スキル、バックログが含まれます。古いコピーを使用する場合は、必要なファイルが含まれていることを進行役に確認してください。

## Codespace を作成する

次に、Codespace を使ってワークショップに取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーでコードを記述、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、すべての機能を備えたエディターが提供されます。

1. 作成したリポジトリに移動します。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示された場合は **Trust Folder & Continue** を選択します。
6. リポジトリ ルートのターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、次のことを行いました。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成しました。
- Codespace を作成し、プロジェクトの準備ができていることを確認しました。

次は、Codespace に[GitHub Copilot CLI をインストールし][next-lesson]、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
