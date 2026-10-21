---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、さまざまな作業があります。手動でコードを検証し、指示ファイルを使って標準に従わせることはできました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が適しています。スキルを使うと、Copilot がこうした操作を適切に実行する方法を理解できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルは、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名のフォルダーにある `SKILL.md` で構成されます。この Markdown ファイルには、スキルの名前と説明を定義する frontmatter、機能の概要、呼び出すタイミングのガイダンスが含まれます。スキル フォルダーには、スクリプトなどのリソースを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルが必要とは限りません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルはプロジェクトの `.github/skills` フォルダーに配置すると、チーム全体で共有・再利用できるリポジトリの資産になります。または、ユーザー スキルのフォルダー `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は、Copilot がスキルをいつ呼び出すか判断するためのものです。
3. 指示を読み、テストと lint の実行手順を Copilot にどのように伝えているか確認します。
4. スキルにはまだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI からスキルを直接呼び出すことも、自然言語で実行を依頼することもできます。3 つのチェックを実行するよう Copilot に依頼します。

1. Interactive モードのフィルター会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行された内容、成功したかどうか、ツールが報告した内容を明確に示すレポートにしましょう。スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイル末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、結果を確認しましょう。Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。

1. 次を実行します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックを行う `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更をフィルター機能の PR に含めます。次は [Playwright MCP サーバーを介して][next-lesson] Copilot にサイトを直接操作させます。

## スキルの例

コミュニティの例を参考にできます。追加のタスクではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図表とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
