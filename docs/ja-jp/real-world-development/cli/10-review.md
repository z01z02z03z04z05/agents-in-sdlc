---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルター issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認し、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで方針を決め、Autopilot モードで作成し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テスト カバレッジ、スキルの結果、ブラウザーでの検証根拠を確認しました。
- フィルター機能全体の変更をレビューし、フィルター PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、更新済みの `main` から作成した各自のブランチで行う、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と、評価がない場合の明示的な表示をゲーム カードに追加します。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルターの計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを簡潔にするため、この作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、ブラウザーでの手動確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA はその結果を要件とテスト カバレッジのレビューにまとめてから PR を作成できるようにしました。

追加するテストは、実際に不足しているカバレッジを補う必要があります。新しいテストが不要という QA の結果が正しい場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける検証結果を再取得します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果に影響します。このワークショップでは指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに応じて調整してください。指示は標準を定め、スキルは反復可能なタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル**を選びます。作成前の検討には **Plan**、範囲を絞った変更を一緒に進めるには **Interactive**、明確な範囲のタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

コンテキストはインフラストラクチャと同じくらい重要です。作成するもの、その理由、方法を明確に伝えると、出力は大きく変わります。

## さらに試す

基本的なワークフローを学びました。ほかにも、次の CLI 機能を試せます。

- `/review` でコード レビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を相談し、別の視点から考えます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。実運用コード、趣味のコード、長年作ろうと思っていた小さなアプリなどで使ってみてください。学んだことをチームで共有し、ほかの人から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認します。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
