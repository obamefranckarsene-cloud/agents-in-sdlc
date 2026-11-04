---
title: "演習 5 - GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot とともに、ユーザー エクスペリエンスを改善して機能を追加する複数のタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが反復的に改善できる関連ゲーム機能を構築し、ハイ コントラストとライト モードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードを確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードを常にレビューし、通常の DevOps プロセスに従います。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot クラウド エージェント

Copilot クラウド エージェントは非同期かつ監視されずにタスクを実行するため、安全性を維持するための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスはコード作成に使うブランチに限定されます。
- クラウド エージェントは GitHub Actions 内で、作業用の独立した一時環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot クラウド エージェントが作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要であれば、[Copilot クラウド エージェントのファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。Copilot の作業が完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまで学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメント コメントやその他のドキュメントを確認します。変更内容は状況によって異なります。レビュー時には、演習 2 の最初に確認したカスタム命令ファイルに由来する TSDoc コメント、TypeScript の規約、コメント ヘッダーを確認します。
6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する場合、一方的に指示するだけではありません。チーム メンバーに依頼するように、プルリクエストのコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを読み、変更に取り組む新しいセッションを開始します。結果は確率的に生成されるため、依頼する内容を固定することはできません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、内容を簡単に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを参考に、ドキュメントの変更を依頼するコメントを **@copilot** にメンションして追加します。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot は更新のために新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

問題がなければ、納得できるまで反復します。その後、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

関連ゲーム機能が実装されました。Copilot クラウド エージェントに変更を依頼し、反復的に改善できます。たとえば、関連ゲームの表示件数を変えたり、カスタム命令を更新する前に依頼したため追加されなかったコメント ヘッダーや TSDoc のドキュメント コメントを加えたりできます。**Conversation** タブでコメントを追加すると、Copilot がそれを読み、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で Mission Control から依頼したライト モードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。作業が完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を維持するためのローカル ストレージの使用
   - ハイ コントラストとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード
6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えてアクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティ カスタム エージェントが、Copilot の機能実装をアクセシビリティのベスト プラクティスに沿うように導いたことを確認します。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — 委任を続ける

クラウド エージェントに実際のバックログ項目を渡して別の作業に取り組めると、効果的に使えます。いくつか issue を作成して Copilot に割り当て、習慣にしましょう。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウド エージェントのハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **リポジトリに含まれるカスタム命令ファイルを確認**し、クラウド エージェントの出力への影響を確認しました。
- **issue を Copilot クラウド エージェントに割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ向けの**カスタム エージェントを作成して使用**し、ハイ コントラストとライト モードを追加しました。
- Copilot エージェント ページを Mission Control として使い、アクセシビリティ セッションを監視して案内しました。
- クラウド エージェントのプルリクエストをレビューして反復し、**@copilot** に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

クラウド エージェントのハーネスを完了しました。他のハーネスでは、ここで学んだ内容を別の環境で確認できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、クラウド エージェントに戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次の案を試してください。

- リファクタリングまたはテスト カバレッジの issue をクラウド エージェントに割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 異なるスタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウド エージェントのセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot クラウド エージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
