---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このハーネスでは、ユーザー エクスペリエンスの改善や機能追加に関する複数のタスクを GitHub Copilot と進めてきました。コードへのドキュメント追加、デザイン チームが反復して改善できる関連ゲーム機能の作成、ハイコントラスト モードとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って、改善していきましょう。

### シナリオ

これまで何度も確認したように、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。生成されたコードを必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム向けにレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督者なしで非同期にタスクを実行するため、安全性を確保するためのセキュリティ制約があります。次のような制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コードを書き込めるのは作業用のブランチ **のみ** です。
- Cloud agent は GitHub Actions 内で実行され、作業用の別個の一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が生成した最初のプルリクエスト (PR) で、コードへのドキュメント追加を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされているという警告が表示される場合があります。既定では Copilot の外部リソース (外部 MCP サーバーへの呼び出しなど) へのアクセスが制限されているため、これは **想定どおり** です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、Copilot が作業を完了するまで待ってください。数分かかる場合があるので、休憩したり、ここまでに学んだことを振り返ったりできます。

4. PR の準備ができたら、**Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc ドキュメント コメントなど、更新されたコードを確認します。実際の変更内容は異なる場合があります。

    変更を確認しながら、演習 2 の最初に確認した指示ファイルによる TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーがあるか確認します。

6. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック セクションで、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに関するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とのプルリクエスト作業は一方通行ではありません。チームのほかのメンバーと同じように、プルリクエストのコメントやコード行へのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は決定論的ではないため、依頼文を厳密には指定できません。Copilot に依頼できる変更の例を次に示します。

- 各コード ファイルの先頭に、そのファイルの役割を簡潔に説明するコメント ヘッダーを追加します。
- TypeScript と Astro のファイルに TSDoc ドキュメント コメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. 上記の例またはほかの案を使って、コードベースに追加したいドキュメントの変更をコメントで依頼します。ほかのユーザーと同じように **@copilot** をメンションしてください。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot が更新のために新しいセッションを開始することを確認します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る画面](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足できるまで反復して改善できます。問題がなければ、ドラフト PR をレビュー可能な状態にして、`main` ブランチにマージします。

![PR をレビュー可能な状態にする画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)
8. プルリクエストのチェック セクションで、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに関するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に合わせて `<branch-name>` を置き換え、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc ドキュメント コメントを追加したりできます (このタスクは、カスタム指示を更新する **前** に割り当てたことを思い出してください)。前の手順と同じように、**Conversation** タブで新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 で mission control から依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が表示されます。その場合は、Copilot が作業を完了するまで待ってください。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保持するための local storage の利用。
   - ハイコントラスト モードとライト モードの CSS またはスタイル変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)。
   - モードの切り替えを管理する JavaScript または TypeScript コード。

6. 更新内容に問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. ワークフローの承認待ちがある場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)
9. プルリクエストのチェック セクションで、ワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに関するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に合わせて `<branch-name>` を置き換え、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラスト モードとライト モードを切り替えて、ページを再読み込みした後も設定が保持されることを確認します。

アクセシビリティ カスタムエージェントが、アクセシビリティのベスト プラクティスに従って Copilot の機能実装を導いたことを確認してください。アクセシビリティ上の問題や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せてほかの作業に取り組むときに役立ちます。さらに練習するには、リポジトリで issue をいくつか作成し、Copilot に割り当てます。たとえば、次のようなタスクがあります。

- ゲームの詳細ページに支援者の関心フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データ アクセス ヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- このリポジトリに含まれるカスタム指示ファイルを確認し、後で cloud agent の出力に反映される様子を把握しました。
- Copilot cloud agent に issue を割り当て、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラスト モードとライト モードの切り替えを追加しました。
- Copilot agents ページを mission control として使い、アクセシビリティ セッションを監視して途中で誘導しました。
- Copilot のプルリクエストをレビューして反復し、**@copilot** に変更を依頼したり、ワークフローを承認したりしました。

## レビューと次のステップ

Cloud agent のハーネスを完了しました。学習を続けたい場合は、ほかのハーネスで関連する内容を実践できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を直接試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` による cloud agent への連携を使って、レビュー済みの変更をターミナルから 2 つリリースします。

自身のリポジトリでは、次のようなことを試してください。

- リファクタリングやテスト カバレッジに関する issue を cloud agent に割り当てます。
- 別の分野向けにカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

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
