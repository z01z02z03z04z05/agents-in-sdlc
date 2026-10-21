---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を合意し、Autopilot で issue のフィルター機能を作成し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しましたが、大きな変更にはより確実なプロセスが必要です。GitHub Copilot CLI は組織の既存の流れに合わせて作業できるため、適切な方法で適切なものを作成できます。このレッスンから、一般的なエージェント駆動の開発プロセスを進めます。issue から新機能を作り、コードの妥当性と動作を確認し、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常は作業するファイルの種類ごとにブランチや PR を分けますが、ここでは主要な概念に集中できるよう簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot 会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが拡大し、訪問者がカテゴリーと発行元でゲームを絞り込める必要があります。バックログの issue には機能が記載されていますが、カテゴリーの組み合わせ方など、コーディング前に合意すべき詳細があります。Plan モードで決定事項を整理し、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、いっそう重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容を説明する issue から始めます。
2. 作成するものを計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功した後、コードをマージします。

> [!NOTE]
> チームや組織によって具体的な手順は異なりますが、多くは上記の流れを応用しています。

この標準的な方法に沿って進めると、AI が生成したコードが要件を満たし、手書きのコードと同じ確認プロセスを経ることを保証できます。

## 会話モード

**会話モード**は、エージェントに与える自律性の程度を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントはまず計画を作成し、プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントは自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から開始する

フィルター機能に取りかかる前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、Copilot に依頼する前に、機能実装やタスクの進め方を定義できます。複雑な作業では事前に計画を立てることをお勧めします。Plan モードに切り替えて、計画を作成するよう Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下のモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を判断して計画を作成します。

3. フィルター機能の動作について質問されたら、希望に沿って回答します。どの回答も間違いではありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画をスクロールして確認します。
6. 決定内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画が作成されレビューも終わったので、実装に進みます。Autopilot を使って Copilot に作業してもらいましょう。

Autopilot では、Copilot がタスクを完了したと判断するまで、自律的に作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様の名前が付いた項目を選択します。
2. プロンプトの下のモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェックを完了と見なす前に環境の問題を解決します。

## 実装をレビューして検証する

コードが生成された後、ほかのコードと同様にマージ前のレビューが必要です。コードを確認し、サイトを実行して問題がないことを確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を実行し、フィルター機能とテストを確認します。
3. 結果を issue と計画中に決めた内容と照合します。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードに問題がなくても、実際に動作するか確認しましょう。前と同じようにアプリを起動し、Codespaces の転送ポート経由でサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリーの絞り込み、発行元の絞り込み、その組み合わせを試します。
4. リセットと検索結果が空の場合の動作が、issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

さまざまな会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot 会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、[カスタム指示を使って][next-lesson]コード生成をさらに詳しく確認し、文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
