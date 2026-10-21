---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー エクスペリエンスを改善し、機能を追加するために、Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するためのツールは複数あります。たとえば、GitHub.com の[エージェント ページ][agents-page]を使うと、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を出したりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を追加し、機能をリクエストします。

## シナリオ

エージェントにハイコントラスト モードの作成を依頼した後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイルを更新し、切り替え機能を追加する作業がすでに進んでいたため、ライト モードも含めるのが合理的です。ハイコントラスト モードに加えてライト モードも実装するよう、エージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインにあるタスクの一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
    - コードベースのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトにハイコントラスト モードを追加する。
3. 実行中のタスクを 1 つ選択し、Copilot が実施した作業を確認します。次のような作業が含まれます。
    - リポジトリをチェックアウトする。
    - Copilot が作業するための環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するためのさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実施方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業をさらに監視できます。

## cloud agent に指示を出す

実行中のタスクを確認できたので、ハイコントラスト モードの作業中にライト モードも追加するよう Copilot に依頼します。

1. ハイコントラスト モードの追加に関するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot エージェントのタスクを表示するページで、Steer active session while Copilot is working ダイアログが強調表示されている画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot の作業を待つ

前回と同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実施中だった手順が終わると、新しいリクエストが作業に反映されます。

今回も完了まで数分かかります。その間に、ここまでに学んだ内容を振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して誘導するための中心的な場所である Copilot agents ページを確認しました。この mission control で、進行中と完了済みのタスクを追跡し、作業内容をレビューし、進行中のタスクをリダイレクトして範囲を調整したり、追加のガイダンスを伝えたりできます。

このレッスンでは、次のことを行いました。

- mission control とエージェント ページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに指示を出し、追加機能をリクエストしました。

Copilot がアクセシビリティ機能の作業を続けている間に、Copilot が作成したプルリクエストを[反復して改善する][next-lesson]方法を確認しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
