---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業をより効果的に実施するための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供します。チームの専門知識を再利用可能なエージェントとしてまとめられます。たとえば、[WCAG][wcag] 準拠を徹底するアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown のプロンプトが必要です。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、論理的に重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。最も簡単な区別は、**カスタムエージェントは作業者、スキルはツール**ということです。

カスタムエージェントには独自のコンテキスト ウィンドウがあり、作業の一環としてスキル (さらには別のエージェント) を調整して実行できます。この演習では、アクセシビリティ カスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフローやテストの実行・管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダーク テーマはテキストと背景色のコントラストが不十分で、読みづらいと感じるユーザーがいることが分かりました。このアクセシビリティ上の問題に対応するため、デザイン チームはユーザーが切り替えられるハイコントラスト モードの実装を依頼しました。

アクセシビリティは重要なので、この機能をできるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot への指示内容を確認しましょう。

codespace に戻り、ターミナルを開いて `main` から新しいブランチを作成し、アクセシビリティ機能の作業に切り替えます (演習 3 のフィルター PR とは分けて管理します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必須です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成する際の主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` で定義したすべてのカスタムエージェントが Copilot Chat ビューの下部にあるエージェント ドロップダウンに表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールを使うようにチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。該当する手順で説明します。

1. Chat ビューのエージェント ドロップダウンで、まだ選択されていなければ **Agent** を選択します。

   ![Chat ビューのエージェント ピッカー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビューの下部にあるエージェント ドロップダウンを選択します。現在のエージェントが表示されます (既定は **default**)。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクトのファイルに適用します。各編集は、ファイル パスとレビュー用の差分とともにチャットに表示されます。
6. 編集が反映されるたびに、**Files changed** の表示が更新され、エージェントが変更した作業セットを確認できます。

> [!NOTE]
> 作業には数分かかる場合があります。Copilot はリポジトリのファイルを実際に変更します。Astro コンポーネント、CSS、関連テストなどを更新することがあります。

次の演習では、進行中の作業を確認して指示を追加します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門的な AI アシスタントである、GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。チームの専門知識や標準を再利用可能な形にまとめ、Copilot がより効果的に作業できるように導けます。

このレッスンでは、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat のエージェント モードでカスタムエージェントを使いました。

次は、[エージェントの作業を監視して指示を追加します][next-lesson]。変更を確認しながら、同じセッションにライト モードの切り替えを依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
