---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が動作することを確認します。

## 演習用リポジトリを設定する

コードの作業用にリポジトリのコピーを作成するには、[テンプレート][template-repository]からインスタンスを作成します。新しいインスタンスには演習に必要なファイルがすべて含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys のテンプレート][tailspin-template]に移動します。
2. 演習用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択してコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成できます。

   ![リポジトリ テンプレートの設定を入力する](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。この後の演習で参照します。

> [!NOTE]
> **バックログを準備する**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使うため、自分で登録する必要はありません。

## Codespace を作成する

次に、Codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーでコードを記述、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE が提供されます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりもはるかに短時間で済みます。作成中に GitHub Copilot の他の機能を確認しておきましょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。ここでは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できるように構成されています。どちらも必要な前提条件がインストールされた環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使っている場合、通常はすでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。Visual Studio Code の拡張機能ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択する方法もあります。

拡張機能をインストールしたら、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディター右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力して Enter キーを押します。Copilot Chat が有効になります。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。画面の指示に従って認証します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

演習用リポジトリのコピーを作成できました。また、コードを書くときに使用する Codespace の作成を開始しました。

## 次のステップ

Copilot を使って作業を始めましょう。[演習 1 - カスタム命令][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
