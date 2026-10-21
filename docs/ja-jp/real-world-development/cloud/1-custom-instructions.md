---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実施する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報にアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように作業してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、この演習ではプロンプトの *実行前と実行後* を比較しません。Copilot cloud agent は GitHub issue に対して非同期に動作するため、リアルタイムに並べて影響を示すことが難しいためです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ず単体テストが必要です。
- UI はダーク モードにして、モダンな印象にします。
- TSDoc のドキュメント コメントを使って、コードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、ここで示したプラクティスに沿ってタスクを行うために必要な情報を Copilot に伝えます。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件をより適切に理解させることができます。コーディング規約、ライブラリ、含めたいコメントの種類などを指定して、関連性の高い提案やコード スニペットを生成するように導けます。リポジトリ全体に適用する指示も、特定の種類のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する **すべての** リクエストで Copilot に送信される、単一の指示ファイルです。Copilot に送信するチャットや CLI リクエストの多くに関係するプロジェクト レベルの情報を記載します。使用する技術スタック、開発対象の概要、ベスト プラクティス、全体的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に作成できる指示ファイルです。TypeScript などの言語や、UI コンポーネントや単体テストの作成などのタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルのファイルと `applyTo` frontmatter を含む `*.instructions.md` ファイルの両方を、コード生成時に使用します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準に沿った指示ファイルにも対応しています。[対応している指示ファイル形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法の詳細はこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な方法が示されています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発対象の説明、プロジェクトの構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- 単体テスト、Astro コンポーネント、データ層など、ファイルの種類や特定のタスクについては `*.instructions.md` ファイルを使います。
- 自然な言葉で、明確なガイダンスを記載します。コードの良い例と悪い例を示します。

指示ファイルの作り方は 1 つではありません。AI の使い方と同じように、試しながらプロジェクトに適した方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。各環境で手順は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。該当するレッスンで詳しく説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、入れ子になった **GitHub Actions Workflows** のガイダンスがあります。これらの指示は、Copilot を使うすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示の適用対象となるファイルをリポジトリ ルートからの glob で指定します。ここでは、`**/*.test.ts` に一致するファイルなど、TypeScript のテスト ファイルが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。この方法を使うと、大きな指示セットを再利用できる小さなファイルに分割し、コード生成時の参考例を Copilot に示せます (リンク先のパスはリポジトリ ルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc ドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントは TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーで記述するという、プロジェクト全体のルールを追加します。このハーネスの後半で issue を割り当てると、cloud agent はこのルールに従います。

編集する前に、作業用のブランチを作成します (cloud agent は issue の対象ブランチにある指示を読みますが、ここでは変更をきちんとコミットします)。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に提供します。コードの提案を改善できるように、リポジトリのコーディング標準にルールを追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。通常は 27 行目付近にあります。このセクションではプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールはまだありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準のすぐ下に、次の Markdown 行を追加します。これにより、Copilot にファイル コメント ヘッダーと TSDoc ドキュメント コメントを追加するよう指示できます。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` は全体的なガイダンスに、`*.instructions.md` は特定の言語、ファイルの種類、タスクに関する指示に使用できます。今回追加したドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は issue の対象ブランチにある指示ファイルを読みます。次の演習で Copilot に issue を割り当てる際、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を使えるように、この変更を `main` に取り込む必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で issue を割り当てるとき、cloud agent はこの指示を読みます。

> [!TIP]
> このワークショップでは `main` に直接変更を加えてもかまいません。その場合は、ブランチの手順を省略して `main` に直接コミットできます。ここでブランチを作成するのは、実際のプロジェクトにおける作業方法を再現するためです。

## まとめと次のステップ

このプロジェクトの指示ファイルを通じて、Copilot がコンテキストをどのように取得するかを確認し、リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で cloud agent が作成するプルリクエストに、その標準が反映されることを確認します。

次に、[Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
