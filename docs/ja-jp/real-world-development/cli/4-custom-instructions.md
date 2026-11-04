---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[命令ファイル][instruction-files]では、求めるコードの内容だけでなく、構成方法も説明できます。フィルターを作成したので、Copilot が使った命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパス指定の命令ファイルがエージェントに適用される方法を確認します。
- コーディング標準が守られるように命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば次のとおりです。

- コメントでは、コードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変更されたらコメントを最新状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

命令ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件のコンテキストと設定を Copilot に渡すことができます。使用する規約やライブラリ、好みのコメント形式などを指定すれば、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のファイル向けに命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に渡す単一の命令ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、またはタスク向けのガイドラインを記載します。

> [!NOTE]
> その他の命令形式とサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム命令のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、すでに命令ファイルが含まれています。変更を加える前に内容を確認して、影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この命令は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、命令を適用するファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の命令ファイルや既存プロジェクト ファイルへのリンクを確認します。これにより、大きな命令セットを再利用可能な小さなファイルに分割し、参考例を Copilot に示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存ファイルは良い出発点ですが、追加したいガイダンスがあります。新しく生成する TypeScript に TSDoc コメントを追加するよう、`copilot-instructions.md` を更新しましょう。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。更新した命令を反映させるには、会話を終了して再開してください。機能のコンテキストは維持できます。

1. フィルターの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   必要に応じてフィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく生成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令を読み込む方法を確認し、機能に新しい標準を適用しました。具体的には、次のことを行いました。

- リポジトリの命令とパス指定の命令ファイルがエージェントに適用される方法を確認しました。
- コーディング標準が守られるように命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI 用のカスタム命令を追加する][instruction-files]
- [カスタム命令のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
