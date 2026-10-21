---
title: "演習 6 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
| :-- |

## 作業をレビューする

この演習では、ユーザー エクスペリエンスの改善を目指し、GitHub Copilot と複数のタスクに取り組みました。エージェント モードでクライアントとサーバーにまたがるフィルター機能を追加し、Playwright MCP サーバーでブラウザーから手動テストした後、カスタムエージェントでハイコントラスト モードとライト モードの切り替えを実装して、作業中のセッションに指示を追加しました。これらのローカル変更を公開し、チームと同じ方法でレビューします。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。Copilot が作成した成果を実際にレビューする必要があります。codespace からアクセシビリティ機能の変更をプッシュし、プルリクエストを開き、チームに共有する前に差分を確認します。

## アクセシビリティ機能を公開する

[演習 4][exercise-4] と [演習 5][exercise-5] でアクセシビリティ カスタムエージェントを使って実装したハイコントラストとライト モードの切り替えは、コミット済みの変更として codespace にあります。チームがレビューできるよう、変更をブランチにプッシュしてプルリクエストを作成します。

1. codespace に戻ります。
2. VS Code の **Source Control** ビューを開きます。
3. アクセシビリティ機能の変更がコミット済みであることを確認します。演習 5 でコミットしていない変更がある場合は、`Add high-contrast and light-mode toggles` のような説明的なメッセージでステージしてコミットします。
4. **Publish Branch** を選択してブランチを公開します (または **...** メニューから **Push** を選択します)。
5. VS Code から github.com で新しいブランチを開くか確認されたら許可します。またはリポジトリに移動し、ブランチのバナーで **Compare & pull request** を選択します。
6. 明確なタイトル (例: `Add high-contrast and light-mode toggles`) と、実施内容と理由をまとめた簡潔な説明を入力します。
7. **Create pull request** を選択します。
8. PR が作成されたら **Files changed** タブを選択して変更全体をレビューします。特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保持する local storage の使い方。
   - ハイコントラスト モードとライト モードの CSS またはスタイル変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モードの切り替えを管理する JavaScript または TypeScript コード。

9. **Conversation** タブに戻ります。
10. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

    ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

11. ワークフローが完了するまで待ちます。問題がなければ、成功したことを確認できます。

> [!TIP]
> アクセシビリティ機能について別の意見を聞きたい場合は、PR のコメントで `@copilot` にメンションし、「WCAG の問題がほかにないかレビューしてください」や「キーボード操作の改善案を提案してください」などと依頼します。Copilot はコメントに対応する新しいセッションを開始します。

## 省略可能な演習 - ローカルでさらに試す

IDE でエージェントと反復して作業するには、繰り返し練習する必要があります。VS Code から開始できる追加セッションの例を紹介します。

- ゲーム詳細ページに支援者の関心フォームを追加します。
- ゲーム一覧ページにページネーションを実装します。
- `src/lib/` のデータ アクセス ヘルパーに入力検証とエラー処理を追加します。
- アクセシビリティ エージェントの対象を広げ、サイト全体のキーボード フォーカス順序を監査させます。

## まとめ

VS Code のハーネスを完了しました。この演習では、次のことを行いました。

- **Playwright MCP で機能を手動テストしました。** Playwright MCP サーバーを追加し、Copilot にブラウザーを操作させて、プルリクエストを作成する前にフィルター機能を検証しました。
- **エージェント モードでスタック全体にまたがる変更を行いました。** クライアント、サーバー、テストを 1 つのセッションで変更するフィルター機能を追加しました。
- **カスタムエージェントを使いました。** エージェント ピッカーでアクセシビリティ用のカスタムエージェントを選択し、リポジトリにハイコントラスト モードを実装する様子を確認しました。
- **エージェント セッションを管理して誘導しました。** 提案された変更をインラインでレビューし、必要なものを承認して、ライト モードの追加プロンプトでセッションを拡張しました。
- **プルリクエストで作業を完了しました。** ローカルの変更を公開し、チームと同じ方法で全体をレビューしました。

## レビューと次のステップ

必須の VS Code ハーネスは完了です。ここでワークショップを終了できます。

Copilot のエージェント機能についてさらに学ぶ場合は、ほかの環境向けのハーネスで関連シナリオを確認できます。

- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、コンテキストを踏まえた引き継ぎを使い、ターミナルからレビュー済みの変更を 2 つリリースします。
- ☁️ **[Cloud agent ハーネス](../../cloud/)** — cloud agent への issue 割り当て、エージェント ページを使ったセッション監視、プルリクエストの非同期反復に取り組みます。

今回の作業を発展させることもできます。独自のプロジェクトに適応できる指示ファイル、カスタムエージェント、スキルを探すには、[awesome-copilot][awesome-copilot] が役立ちます。

省略可能な拡張として、[省略可能: Foundry を組み込む][exercise-7]では VS Code と Microsoft Foundry Toolkit を使ってモデルを準備し、Backer Concierge をデプロイしてサイトに接続します。

## リソース

- [GitHub Copilot][github-copilot]
- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェント モードを使う][agent-mode]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
