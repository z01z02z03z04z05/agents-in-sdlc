---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新であっても、通常は複数のファイルを変更し、テストなどの操作が必要になります。開発者は関連するファイルをすべて探して変更し、テストを実行し、問題をデバッグし、変更し忘れたファイルを見つけてさらに更新します。この繰り返しは続きます。

この作業を支援するのが Copilot Agent Mode です。

Copilot Agent Mode は IDE でより自律的に動作します。開発者と同様に、既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なるエクスペリエンスです。エージェント モードは IDE 内でタスクを実行し、すばやくフィードバックを得ながら対話できます。Cloud agent はチームの一員のように非同期で作業し、issue やプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけ、複数ファイルを協調して変更する方法。
- GitHub Copilot Agent Mode を使って、UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたので、カテゴリーと発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。今度は Copilot Agent Mode で、残りのデータ層、UI、テストの作業を完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に Tailspin Toys の Web サイトを調べ、現在の機能を確認します。

この Web サイトは、開発者向けテーマのボードゲーム クラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を確認できます。Astro の単一アプリであり、ビルド時にページを静的 HTML としてレンダリングします。ページは Drizzle ORM を通じて frontmatter 内からローカル SQLite データベースに直接クエリします。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、対話機能は標準 DOM API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を行います。

1. codespace に戻ります。前の演習後であれば `main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL のバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザー ウィンドウで開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> codespace でターミナルの localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストしている codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリーなど、詳細情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。該当する手順で説明します。

Web サイトの初期実装は機能していますが、新しい機能を追加して強化したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、issue のバックログが自動的に作成されます。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンで **Agent** を選択します。**Agent** は複数ファイルの変更を自律的に計画して実装し、ターミナル コマンドを実行して、ツールを呼び出します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。作成時に Claude Sonnet 4.5 を使用したため、手順に記載しています。提案をある程度一貫させ、使いやすくするためですが、大規模言語モデル (LLM) は確率的に動作するため、手順の例と異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使うことがありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログについて確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーにとって特に有用だと判断した項目を優先順位付けしていることを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に提供する指示ファイルを確認しましょう。Web サイトへの機能追加の方法に関するコンテキストが含まれる [ユーザー インターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、各ファイルの種類に関する指示へのリンク。

> [!TIP]
> 指示ファイルでは、ほかの指示ファイルとプロジェクト内のファイルの両方を参照できます。パスは指示ファイルの場所からの相対パスです。これを使うと、複雑な指示を再利用しやすい小さな単位に分割し、参考例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つを更新する必要があります。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探します。データ層のヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など複数の作業を行います。タスクやコードベースの複雑さによっては数分かかる場合があります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。作業が終わるとすべての変更に対して **Keep** または **Undo** を選べるため、処理中に選択する必要はありません。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![テストを実行してよいか Copilot Chat ペインが確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクを完了し、エラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode の会話が完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. 動作を確認し、コードをレビューしたら Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode で Tailspin Toys Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数ファイルを協調して変更する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 - ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションを実装するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページ サイズをユーザーが選択できるようにするか、固定値にするかなど、ページネーションの方式を決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot にコンテキストを提供して変更を依頼しながら、何度か反復する必要がある場合があります。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
