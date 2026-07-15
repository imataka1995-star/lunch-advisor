# 月間昼食アドバイザー

つくかね麻布の月間献立画像を月に1回登録し、各日のA/B弁当とセブン-イレブンの固定候補を比較するローカルアプリです。175cm・64kg、在宅勤務、午後の眠気を考慮し、たんぱく質、野菜、調理法、主食量、カロリーからおすすめを表示します。

最初に読む案内は [PROJECT.md](PROJECT.md)、これまでの設定状況と次回以降の更新手順は [HANDOFF.md](HANDOFF.md) を参照してください。

毎日の自動実行は行いません。月間レポートは毎回同じファイル名 `reports\current.html` に更新されます。PCではこのファイルをブックマークすれば、翌月も同じブックマークから確認できます。

## アプリを開く

プロジェクトフォルダーで次を実行します。

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\start-app.ps1
```

ブラウザで `http://127.0.0.1:8765/` が開きます。画面を閉じたあと、PowerShellで `Ctrl+C` を押すとアプリが終了します。

アプリ画面では次の操作ができます。

- 登録済みの月を選んで月間一覧を表示
- A/B双方の主菜、カロリー、副菜・記載食材を確認
- つくかね内のおすすめと、セブンを含む総合おすすめを確認
- 日ごとに異なる詳しい選定理由を確認
- 翌月の献立画像をアップロードして登録

固定ページ上部のジャンプメニューから、今日、月間ベスト、各週、献立登録へ直接移動できます。右下の「↑」でページ上部へ戻れます。

固定ページの「献立登録画面を開く」ボタンは、Windowsに登録した `lunchadvisor://admin` リンクでローカル管理画面を起動します。PowerShellを手作業で開く必要はありません。登録操作はPCから行い、スマホは閲覧用として使います。

2026年7月は登録済みです。生成済みファイルを直接開く場合は `reports\current.html` を開きます。スマホ幅では、横に長い表ではなく日ごとのカード表示へ自動で切り替わります。

## スマホでの確認

公開ページは次の固定URLです。

https://imataka1995-star.github.io/lunch-advisor/index.html

このURLをPCとスマホのブラウザでブックマークしてください。PCでは一覧表、スマホでは日ごとのカード表示になります。上部の「今日」「月間ベスト」「第1週〜第5週」から目的の場所へ移動できます。

`publish\index.html` は、GitHub Pagesへ公開する元ファイルです。翌月の献立を登録して月間レポートを生成すると、このファイルも同じ名前で更新されます。

毎月の公開更新は、献立登録後にCodexへ「昼食アドバイザーの公開ページを更新して」と依頼してください。GitHubの `imataka1995-star/lunch-advisor` にある `index.html` を差し替えるため、ブックマーク先のURLは変わりません。

公開リポジトリには閲覧用HTMLだけを置き、OpenAI APIキーなどの秘密情報は保存しません。献立画像の読み取りと登録はPC内の管理画面で行います。

## セブンの固定候補

固定候補は次の組み合わせです。

- [ブロッコリーチキンエッグ](https://www.sej.co.jp/products/a/item/105387/)：141kcal、たんぱく質18.4g、脂質5.5g、炭水化物6.0g
- もち麦おにぎり2個：1個170〜210kcal、たんぱく質3〜5gとして概算
- 合計：481〜561kcal（中心値約521kcal）
- たんぱく質：概算24.4〜28.4g

もち麦おにぎりは具材が変わるため、範囲で表示します。ツナマヨなど脂質の多い具は上限寄り、梅・昆布などは下限寄りと考えます。

指定サラダは公式ページ上で北海道販売の商品です。東京の店舗では同一商品がない可能性があるため、実際の栄養表示が異なる場合は `data\seven_candidates.json` を更新してください。

## 翌月の献立を登録

画像の自動読み取りにはOpenAI APIキーが必要です。

1. `.env.example` を `.env` にコピーします。
2. `.env` の `OPENAI_API_KEY` を設定します。
3. アプリを開き、「対象月」と「献立画像」を選びます。
4. 「画像を読み取って登録」を押します。
5. 読み取り後、日付、A/B、主菜名、カロリー、副菜を一度確認します。

PowerShellから登録することもできます。

```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\run.ps1 --import-month-image "C:\path\to\menu.png" --month "2026-08"
powershell -ExecutionPolicy Bypass -File .\scripts\run.ps1 --monthly "2026-08"
```

読み取り結果は `data\monthly_menus\YYYY-MM.json`、元画像は `data\monthly_menu_images`、表示ページは `reports\monthly\YYYY-MM.html` に保存されます。

## 判定基準

- 600〜750kcalを昼食の基本目安として加点
- 魚・肉・卵・大豆などの主菜を加点
- 副菜の野菜種類を加点
- 揚げ物、丼・ピラフなどの主食偏重、750kcal超を減点
- セブンは約481〜561kcalで軽め、たんぱく質は約24.4〜28.4gとして比較
- つくかねが600〜750kcalの非揚げ物なら、夕方の空腹まで考えて優先しやすくする
- 高カロリー・揚げ物中心の日はセブンを優先する

手作り弁当のたんぱく質量や野菜量は、献立名と記載食材による推定です。医療的な栄養指導ではありません。

## 動作確認

```powershell
$python = "$env:USERPROFILE\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe"
& $python -m unittest discover -s .\tests -v
```

