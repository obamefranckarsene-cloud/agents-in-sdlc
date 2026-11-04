---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に伝えておく背景情報がある場合は、コンテキストを利用できるようにします。Copilot に特定のコンテキストを共有する方法はいくつかあります。主な方法の 1 つが[命令ファイル][instruction-files]です。コード生成の指針を提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス指定の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の命令を使って、フィルター用の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が適用される様子を確認します。
- 命令の更新とフィルターの一部を `main` にコミットし、次の演習で Copilot が更新したガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の出力は確率的です。この演習では、命令を更新した後の動作の**傾向**を確認します。生成結果が文字単位で完全に一致することを期待するものではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- データ レイヤーには必ず単体テストを作成します。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件のコンテキストと設定を Copilot に渡すことができます。使用する規約やライブラリ、好みのコメント形式などを指定すれば、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のファイル向けに命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡す単一の命令ファイルです。技術スタック、プロジェクトの概要、ベスト プラクティス、全般的なガイダンスなど、ほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などのタスク向けにガイドラインを記載します。

> [!NOTE]
> IDE で作業する場合、命令ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウド エージェントは、コード生成時にリポジトリ レベルの命令と `applyTo` フロントマターを持つ `*.instructions.md` ファイルを使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準に基づく命令ファイルも[サポートしています][custom-instructions-support]。

### 命令ファイルを管理するためのベスト プラクティス

命令ファイルの作成について詳しく説明することは、この演習の範囲外です。サンプル プロジェクトには、代表的な方法が含まれています。

- `copilot-instructions.md` には、構築するものの説明、プロジェクトの構造、全般的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスク向けのガイダンスを記載します。
- 自然な言葉を使い、明確なガイダンスと、コードの良い例・悪い例を示します。

命令ファイルや AI の使い方に唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した命令ファイルを用意してください。このプロジェクトでは、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスク向けのファイルを確認できます。
>
> Copilot に命令ファイルを生成してもらうこともできます。利用する環境によって操作は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。関連するレッスンで手順を説明します。
>
> テンプレートや出発点を探すには、命令ファイル、カスタム エージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに用意された命令ファイル (中心となる `copilot-instructions.md` と、タスク向けの `*.instructions.md`) をエディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ レイヤー、テストなどの命令を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドがリポジトリ ルートからの glob であり、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されることを確認します。
5. このプロジェクトの単体テスト作成に関する命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の命令ファイル (例: `unit-tests.instructions.md`) やプロジェクト ファイルへのリンクを確認します。命令を再利用できる小さなファイルに分け、Copilot に例を示せます (パスはリポジトリ ルートではなく命令ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントについてはまだ指定していません。次の手順で、TSDoc コメントとファイル ヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で指定します。

カスタム命令の影響を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。その後、命令を更新して、同じプロンプトを再送信し、違いを見ます。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけを与えます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、ファイル変更を適用できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパー ファイルとテストなど、複数のファイルに変更が及ぶ場合があります。
6. 提案されたヘルパーが、型付けされた関数であり、最初の引数に `db` クライアントを受け取り、型付けされた発行元の配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。
7. 提案されたコードに TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示しなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。命令を更新した後に動作の一貫性が高まることがこの演習の目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` を更新し、コード提案の改善に役立つリポジトリ全体の標準を記述します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在、コード内のドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc ドキュメント コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ全体の `.github/copilot-instructions.md` や、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` で命令を作成できます。追加したドキュメント コメントのルールのような、プロジェクト全体の標準はリポジトリ レベルのファイルに記載します。

## プロンプトを再実行し、変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot に対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を新しいプロジェクト標準に従わせ、次の演習で利用する実際のコードに適用できました。

## 命令とブランチをコミット、プッシュする

命令ファイルもリポジトリ内の他のファイルと同じようにソース管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、命令の更新とヘルパーの変更が含まれることを確認します。
3. 命令の更新とヘルパーの土台として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (例: テスト) がある場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けたプルリクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼します。issue を探し、issue をクローズする PR を作成してからマージするよう指示します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートには GitHub ツールがすでに設定されています。Copilot が初めてツールを使う際、VS Code から GitHub へのサインインを求められることがあります。表示される手順に従ってください。後の演習で Model Context Protocol (MCP) による接続方法を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの一覧で **Agent** が選択されていることを確認します。
3. 命令ファイル更新の issue を探し、現在のブランチから PR を作成して、命令の更新と発行元ヘルパーの追加の両方を説明するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が GitHub 操作の許可を求めたら、実行内容を確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカル ブランチを `main` に戻します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作やシェル コマンドの許可を求められたら、実行内容を確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の命令を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認しました。
- 命令の更新とヘルパーの土台を `main` にコミットし、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェント モード][next-lesson]で命令を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令のベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
