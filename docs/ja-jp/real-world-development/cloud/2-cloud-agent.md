---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどが該当します。GitHub Copilot の cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せ、開発を加速させることで、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作が安全に実行されるようにします。
- issue の範囲を明確にする重要性を確認します。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。最初のサイトを作成した委託業者がドキュメントを残していません。まず、アプリケーション内のすべてのエクスポートされた関数に TSDoc ドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのタイトルなど、関連ゲームを表示します。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを希望しています。現在はブロッカーですが、優先度がより高い issue がほかにあります。

どちらも、優先順位が下がりやすいタスクの例です。Copilot cloud agent に割り当てて、非同期に処理させるのに適しています。開発者はほかの作業に集中し、後から戻って Copilot の成果をレビューし、期待どおりかを確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。人間の開発者と作業するときと同様に、[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を設定して、タスクに取り組み始めます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見単純な操作も、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に設定してください。AI ペア プログラマーとして考え、段階的に進め、学び、試し、調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。すでにカスタム指示を扱う演習を完了していれば、`.github/copilot-instructions.md` が機能する様子を確認できています。まだの場合は、ここで簡単に見ておきましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を確認できるようにします。

リポジトリの GitHub Web UI、またはすでに起動している codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認します。特に、TypeScript の規約と TSDoc ドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。これにより、指示が `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てるときは、生成されるプルリクエストで TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認します。これらは指示ファイルによって適用されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードを作成するには通常、特定の環境とセットアップ スクリプトが必要です。Copilot にタスクを割り当てる場合も同様です。Copilot もソフトウェア エンジニアのように作業します。

Cloud agent は、作業環境として [GitHub Actions][github-actions] を使います。作業開始前に実行される専用のセットアップ ワークフローを `.github/workflows/copilot-setup-steps.yml` に作成して、環境をカスタマイズできます。このワークフローを使って、必要な開発ツールや依存関係を利用可能にします。演習の流れを円滑にするため、あらかじめ設定してあります。単一の Astro アプリのビルドとテストに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを Copilot が使えるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # セットアップ手順の一部としてリポジトリを複製して依存関係をインストールする場合は、`contents: read` 権限が必要です。セットアップ手順で複製しない場合、手順完了後に Copilot が自動的に複製します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL に移行済み)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと同じ形式ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブが含まれます。このジョブは、Copilot がプルリクエストの作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを確認してください。これを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するまで待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、情報が古かったり、ドキュメントがなかったりするプロジェクトは少なくありません。このような技術的負債は後回しにされ、作業効率を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、Copilot cloud agent に割り当てるのに適した issue です。Copilot はバックグラウンドでドキュメントを作成し、後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が ToDo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで、数分かかる場合があります。Copilot は前述のワークフローを実行して環境を作成し、プロジェクトを分析して、問題に対処する方法を決めています。

13. リストと、これから実行されるタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールして、Copilot がどのように問題を解決しているか確認します。コードを調べて状態を把握し、計画を考えて決定し、コードを作成する様子を確認できます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、非同期にタスクを実行し、ほかの作業に集中できるようにすることです。この機能を活用し、cloud agent に別のタスクも割り当ててから、アプリケーションに機能を追加するコードの作成に取り組みます。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームを追加する作業にそれほど時間がかからない場合でも、その時間をほかのタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![issue で Copilot が作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目のリクエストにも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同様に作業するため、常に監視する必要はありません。完了後にレビューできます。次に、カスタムエージェントを作成して使用する方法を確認します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について確認しました。cloud agent に issue を割り当てて、非同期に実行させることができます。技術的負債への対応、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに Copilot を活用できます。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作が安全に実行されるようにしました。
- issue の範囲を明確にする重要性を確認しました。
- issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と利用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。前のモジュールで確認したように、カスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
