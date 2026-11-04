---
title: "演習 1 - カスタム命令 (クラウド エージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウド エージェント →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に伝えておく背景情報がある場合は、コンテキストを利用できるようにします。[命令ファイル][instruction-files]を使うと、Copilot が何を行うかだけでなく、どのように実行するかを案内できます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス指定の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、この演習では変更前後のプロンプトを比較しません。Copilot クラウド エージェントは GitHub issue に対して非同期で動作するため、リアルタイムで影響を比較することは困難です。後続の演習で、クラウド エージェントが作成するプルリクエストを確認し、命令ファイルの影響を見ます。

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

命令ファイルの作成について詳しく説明することは、この演習の範囲外です。サンプル プロジェクトには、代表的な方法が含まれています。基本的には、次の点を意識します。

- `copilot-instructions.md` には、構築するものの説明、プロジェクトの構造、全般的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、ファイルの種類 (単体テスト、Astro コンポーネント、データ レイヤー) や特定のタスク向けのガイダンスを記載します。
- 自然な言葉を使い、明確なガイダンスと、コードの良い例・悪い例を示します。

命令ファイルや AI の使い方に、唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

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

このリポジトリには、中心となる `copilot-instructions.md` と、タスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認します。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリ ルートからの glob で、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトの単体テスト作成に関する命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の命令ファイル (例: `unit-tests.instructions.md`) やプロジェクト ファイルへのリンクを確認します。命令を再利用できる小さなファイルに分け、Copilot に例を示せます (パスはリポジトリ ルートではなく命令ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントについてはまだ指定していません。次の手順で、TSDoc コメントとファイル ヘッダーのルールを追加します。

## リポジトリ標準を追加する

次に編集するファイルは `.github/copilot-instructions.md` だけです。コードに TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーを追加するルールを設定します。後続の演習で issue を割り当てるとき、クラウド エージェントがこのルールを読み込みます。

編集の前にブランチを作成します。クラウド エージェントは issue が対象とするブランチから命令を読み込みますが、この変更もコミットしておきます。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に提供します。コーディング標準を記述し、コード提案を改善しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown を追加します。ファイル コメント ヘッダーと TSDoc のドキュメント コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリ レベルの `.github/copilot-instructions.md` や、特定の言語、ファイルの種類、タスク向けの `*.instructions.md` で命令を作成できます。追加したドキュメント コメントのルールのような、プロジェクト全体の標準はリポジトリ レベルのファイルに記載します。

## 命令をコミット、プッシュ、マージする

クラウド エージェントは、issue が対象とするブランチの命令ファイルを読み込みます。次の演習で Copilot に issue を割り当てるとき、Copilot は `main` からブランチを作成します。そのため、命令を `main` にマージしておく必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。次の演習で issue に割り当てたとき、クラウド エージェントはこれらの命令を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略して、`main` に直接コミットできます。ブランチを使う手順は、実際のプロジェクトの進め方を再現しています。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。残りの演習では、クラウド エージェントが作成するプルリクエストでその標準を確認します。

次は、最初の issue を Copilot クラウド エージェントに[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令のベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
