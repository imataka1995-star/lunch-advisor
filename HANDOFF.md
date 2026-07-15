# 月間昼食アドバイザー 引き継ぎ

最終更新：2026-07-15

## このプロジェクトの目的

つくかね麻布の月間献立（A/B弁当）とセブン-イレブンの固定候補を比較し、175cm・64kg、在宅勤務、午後の眠気を考慮した昼食のおすすめを表示する。

毎日の自動実行は**行わない**。月に一度、翌月の献立を登録して固定URLの公開ページを更新する運用。

## いま使うURL

- 公開ページ（PC・スマホ共通）：https://imataka1995-star.github.io/lunch-advisor/index.html
- 公開リポジトリ：https://github.com/imataka1995-star/lunch-advisor
- GitHub Pages：`main` ブランチのルートを公開元に設定済み

`index.html` を明示したURLをブックマークする。月が変わってもURLは変えない。

## ローカルの重要な場所

- 公開用HTML：`publish\\index.html`
- PC用の固定レポート：`reports\\current.html`
- 月ごとのレポート：`reports\\monthly\\YYYY-MM.html`
- 月間献立データ：`data\\monthly_menus\\YYYY-MM.json`
- 元の献立画像：`data\\monthly_menu_images`
- ローカル管理画面：`monthly_app.py`

2026年7月の献立は登録済み。現在の公開ページは2026年7月版。

## 毎月の更新手順

1. PCで `lunchadvisor://admin` を開く（または `scripts\\start-app.ps1` で管理画面を開く）。
2. 翌月の献立画像を登録する。画像の自動読み取りにはローカルの `.env` に `OPENAI_API_KEY` が必要。
3. A/B候補、カロリー、副菜・記載食材、日別のおすすめ理由を確認する。
4. `publish\\index.html` が新しい月の内容に更新されたことを確認する。
5. Codexに「昼食アドバイザーの公開ページを更新して」と依頼する。
6. CodexがGitHubの `imataka1995-star/lunch-advisor` の `index.html` を完全に差し替え、公開URLでPC・スマホ表示を確認する。

公開リポジトリには `index.html` と `.nojekyll` だけを置く。APIキー、`.env`、元画像、個人用のローカルデータは公開しない。

## 公開ページの仕様

- PC：日ごとのA/B候補、副菜、つくかね内の推奨、総合おすすめを表で表示
- スマホ：表を隠し、日ごとのカードを表示
- 上部ジャンプ：「上部」「今日」「月間ベスト」「第1週〜第5週」「献立登録」
- 右下の `↑`：ページ上部へ戻る
- セブン固定候補：ブロッコリーチキンエッグ＋もち麦おにぎり2個
  - 481〜561kcal、中心値約521kcal、たんぱく質24.4〜28.4g

## GitHub連携の状態

- GitHubアカウント：`imataka1995-star`
- ChatGPT Codex Connector：インストール・認可済み
- リポジトリへの読み書き：許可済み
- GitHub CLI（`gh`）はこのPCに未導入。公開更新はCodexのGitHub連携を使う。

## 動作確認済みの内容

- Pythonテスト：11件すべて成功
- 公開URLでタイトル・セブン候補・月間ベスト・A/B比較・副菜・日別理由を確認
- スマホ幅390pxで日別カード22件を確認
- 横方向のレイアウト崩れなし
- 「今日」で `#day-2026-07-15` へのジャンプを確認

## 注意点

- GitHub Pagesの更新直後はルートURLが一時的に旧キャッシュを返すことがある。確認・ブックマークともに `index.html` を含む固定URLを使う。
- `publish\\index.html` は日本語を含む大きめの単一HTML。GitHubへ更新する際は内容が途中で欠けないよう完全なファイルとして差し替え、公開後に日別カード数とスマホ表示を確認する。
- 栄養評価は献立名・記載食材からの目安であり、医療的な栄養指導ではない。

