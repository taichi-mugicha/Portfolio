# Portfolio

伊藤太一（UI/UXデザイナー）のポートフォリオサイト。
ビルドツールなし、静的HTML1枚 + Markdownの原稿という構成。

**やり取りは日本語で行う。**

## リポジトリ構成

| パス | 役割 |
|---|---|
| `index.html` | 実装本体。GitHub Pagesの公開エントリ。index（作品一覧）と作品詳細（ボトムシート）が1ファイルに同居。CSS/JSもインライン。 |
| `works/*.md` | 各作品の原稿。**テキストの正**。1作品1ファイル、ファイル名は `{通し番号}-{slug}.md`（例 `01-asean-refrigerator-app.md`）。通し番号は `WORKS` 配列の並び順＝一覧の `0X` と一致させる。 |
| `works/images/{slug}/` | 作品画像。 |
| `docs/*.md` | 仕様・ルール。**書き始める前に読む対象はすべてここにある。** |
| `archive/` | 旧デザイン検討の残骸。**参照も編集もしない**（明示的に頼まれた場合を除く）。 |

`docs/` = ルール、`works/` = コンテンツ。仕様を書き足すなら `docs/`、作品の中身なら `works/`。

## 作業前に読むもの

該当ファイルを読んでから着手すること。推測で書かない。

| やること | 読むファイル |
|---|---|
| 作品の文章を書く / 直す / 新作品を追加 | `docs/work-detail-text-format.md`（項目・本数・分量）と `docs/writing-style.md`（文体）の**両方** |
| indexのレイアウト・見た目を変える | `docs/index-layout.md` |
| 作品詳細（シート）のレイアウト・見た目を変える | `docs/work-detail-layout.md` |

文章に関するルールは `docs/` 側が正。このファイルには要約を置かない（ずれるため）。

## 原稿とHTMLの同期（重要）

作品テキストは `works/*.md` と `index.html` 内の `WORKS` 配列の**両方**に存在する。
`works/*.md` が正で、`WORKS` はその写し。片方だけ直すと表示と原稿がずれる。

**手順**
1. `works/{通し番号}-{slug}.md` を編集する。
2. **同じターンのうちに** `WORKS` から同じ `slug` の要素を探し、変更を反映する。
3. ずれたままターンを終えない。「あとで反映しますか？」と聞かず、そのまま両方やる。

`works/*.md` を編集すると PostToolUse フック（`.claude/settings.json`）が 2 のリマインドを出す。

**`WORKS` の1要素の形**

```js
{
  title, slug, tags:[], period:"YYYY.MM ~ MM",
  ext:"jpg",                      // サムネ（thumb）が .png 以外のときのみ指定（既定 png）
  pickup: true,                   // ヒーローカルーセルに載せる作品にのみ付与（現在5件。付けない作品は省略）
  carouselImg:"carousel.png",     // カルーセル専用のヒーロー画像（省略時は thumb.{ext} を使う）。pickup作品のみ想定
  carouselLegacyRatio: true,      // trueなら画像を16:9 coverで切り抜かず、縦長比率(5:6固定・全幅共通)+containで見せる
  overview:"…",
  pov:    [{heading, body}],      // 1件のみ
  design: [{heading, body, img}], // img は "design-01.png" or ["design-01.png","design-02.png"]、省略可
  process:[{heading, body, img}], // 空配列ならセクションごと非表示
  outcome:[{value, label}],       // 同上
  hue: 215,                       // 画像未設置時のプレースホルダー色
}
```

- 画像未確定のブロックには `placeholder:true` を付ける（DESIGN末尾の「全体像」など）。
- 作品を増減・並べ替えたら、`works/*.md` の通し番号を `WORKS` の並び順に合わせてリネームする（画像フォルダ `works/images/{slug}/` は番号なしのまま）。

## 画像

- 置き場所は `works/images/{slug}/`。ファイル名は**用途＋セクション内の連番**で付ける。

  | ファイル名 | 用途 |
  |---|---|
  | `thumb.{ext}` | 一覧カードのサムネ兼、作品詳細のヒーロー |
  | `carousel.png` | カルーセル専用のヒーロー画像（`carouselImg`。pickup作品のみ） |
  | `overview-01`, `overview-02`… | OVERVIEW の画像 |
  | `pov-01` | POINT OF VIEW の画像 |
  | `design-01`, `design-02`… | DESIGN の画像（上から順。末尾の「全体像」も含む） |
  | `process-01`, `process-02`… | PROCESS の画像 |

- 連番は原稿（`works/*.md`）での登場順。画像未設置（`placeholder:true`）のブロックも番号を1つ消費する。
- 拡張子は画像ごとに異なってよい（`img` には拡張子込みで書く）。`thumb` が png 以外なら上記 `ext` を指定。
- 差し替えはファイル名を変えず上書きするのが基本。HTML側の参照を触らずに済む。
- `_` 始まりのファイル（`_99.png` など）は作業中の下書き。参照しない。

## 要望の記録

レイアウトや体験について新しい要望・意図が出たら、該当する `docs/*-layout.md` の
「要望・意図（随時追記）」に追記する。仕様として確定したものは同ファイル上部の各項へ反映する。

## 動作確認

`.claude/launch.json` の `static` 設定でプレビューを起動し、
`http://localhost:4599/index.html` を開いて確認する。**Bashでサーバーを立てない。**

見た目に関わる変更をしたら、スクリーンショットまで撮って結果を示すこと。
モバイル幅（375〜430px）が主戦場なので、そこで崩れていないかを優先的に見る。

## 方針

- モバイルファースト。PCは破綻しなければ良い。
- 装飾を足すより作品そのものを見せる。既存のトーン（余白多め・装飾少なめ・モノトーン）を崩さない。
- ビルドツール・フレームワーク・npm依存は追加しない。プレーンなHTML/CSS/JSを維持する。
- コミットは頼まれたときだけ行う。
