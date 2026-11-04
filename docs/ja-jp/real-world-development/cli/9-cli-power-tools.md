---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウド委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にも操作に使えるスラッシュ コマンドが用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、さらに便利なコマンドをいくつか確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot が会話コンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポート、共有する方法を学びます。
- 並列作業、worktree、クラウド エージェントへの委任に使う任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローを終えました。次は、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot クラウド エージェント][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルのコンテキスト ウィンドウの上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドを使って自分で確認したり圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリ ルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域へのコンテキストの割り当てを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合、大きな違いはないことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。無関係なタスクに切り替えるとき、新しい会話を始めるには `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、モデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業し、学んだことを共有すると、AI ツールをより効果的に使えるようになります。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown 形式で出力する `/share file` など、適切な方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の拡張や委任

ワークショップの中心的な内容は完了しました。Copilot CLI には、大規模なタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分割し、並列で実行します。
- `/worktree` は、別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを作成することもある Copilot クラウド エージェントにタスクを送信できます。

これらのコマンドは、追加の worktree やリモート作業を作成する可能性があるため、任意です。試すときは、範囲が明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく確認するには、[クラウド エージェントのワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、構成、セッション共有、内部動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot が会話コンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポート、共有する方法を学びました。
- 並列作業、worktree、クラウド エージェントへの委任に使う任意のコマンドを確認しました。

Copilot CLI には他にも多くのスラッシュ コマンドがあります。次は、[学んだ内容を振り返り][next-lesson]、今後の学習方法を確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot クラウド エージェントについて][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
