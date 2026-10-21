---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデルの選択、セッション共有、省略可能なクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールと同様に、GitHub Copilot CLI にもさまざまなスラッシュ コマンドが用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを完了しました。次に、コンテキスト管理、モデルの切り替え、セッション共有、省略可能な [Copilot cloud agent][about-cloud-agent] への委任など、追加の機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュ コマンドで状況を確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI がまだ開いていない場合は、リポジトリ ルートから起動します。
2. 次を実行します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、コンテキストがシステム指示、ツール、メッセージ、空き領域にどのように分配されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して結果を比較します。会話が小さい場合は、大きな変化がないことがあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的に圧縮します。タイミングを自分で決めたい場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルによって得意なことが異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して、使用するモデルを選択できます。

1. 次を実行します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールをより効果的に活用できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を実行し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合った方法を選びます。たとえば `/share file` は、ローカルに Markdown をエクスポートします。
3. エクスポートした内容を誰かに送信したり公開したりする前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は省略可能です。チームが共有する意図のないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業を拡張または委任する

ワークショップの基本部分は完了です。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分割し、並列に実行します。
- `/worktree` は別のタスク用に、分離された Git worktree を作成します。
- `/delegate` はタスクを Copilot cloud agent に送信します。cloud agent は非同期に作業し、プルリクエストを作成する場合があります。

これらのコマンドを使うと新たな worktree やリモート作業が発生するため、省略可能な内容としています。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定、セッションの共有、内部の動作の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュ コマンドがあります。最後に[学んだことと次のステップを確認し][next-lesson]、学習を続けましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
