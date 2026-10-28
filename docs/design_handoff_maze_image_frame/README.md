# ハンドオフ: MAZE 012345h ランディングページ（左エリア画像枠の配置ルール）

## 概要 (Overview)

MSX2 用 DISK プログラム「**MAZE 012345h**」の紹介用 1 ページ（ランディングページ）。

- 左エリア: スクリーンショット画像の配置枠（黒背景の枠）
- 右エリア: プログラムの説明とエミュレーターへのリンク
- 下部: 支援先ボタン 3 つ、免責文、フッター

このハンドオフの中心的な仕様は **「画像のファイル名だけで、左エリアの枠の形・大きさ・収め方を決める」** という点です。参照する `.png` の中身（画素・寸法）は一切使いません。したがって **ファイル名が同じであれば、画像の中身を別のものに差し替えても常に同じ配置** になります。

対象ファイル名: **`MSX2 Software Presentation.png`**（拡張子を除いた `MSX2 Software Presentation` で判定）

---

## デザインファイルについて (About the Design Files)

このバンドル内の HTML / CSS / JS は **HTML で作成されたデザインリファレンス（意図する見た目と挙動を示すプロトタイプ）** です。**そのまま本番コードとして出荷するものではありません。**

作業内容は、**この HTML デザインを、対象コードベースの既存環境（React / Vue / SwiftUI / Flutter / ネイティブなど）の確立されたパターンとライブラリで再現する** ことです。まだ環境が無い場合は、プロジェクトに最適なフレームワークを選定して実装してください。

特に **画像配置ルールは素の DOM を `document.querySelector` で探す実装** になっているため、対象環境では必ずその環境の流儀（React なら props / state、CSS なら `aspect-ratio` と CSS 変数など）に置き換えてください。ルールの **判定ロジックと数値は仕様としてそのまま移植** してください。

---

## 忠実度 (Fidelity)

**ハイフィデリティ (hifi)** — 最終的な色・タイポグラフィ・余白・角の処理まで確定したモックです。対象コードベースの既存ライブラリとパターンを使い、**ピクセル単位で** 再現してください。

- 基準キャンバス: **1280 × 720 px**（`transform: scale()` でビューポートにフィット）
- 左エリアの画像枠の四隅は **直角**（角丸なし / `border-radius: 0`）
- 枠内の画像は `object-fit: contain`（黒背景 `#000000` の枠に黒帯が出ないよう、枠の縦横比を画像に合わせて確定する）

---

## 画面 / ビュー (Screens / Views)

ページは 1 画面のみ。上から順に並ぶ縦 1 カラム構造です。

### 1. ヘッダー (Header)

- **目的**: サイト名の表示と GitHub へのダウンロード導線
- **レイアウト**: `display: flex` / `justify-content: space-between` / `align-items: center` / `padding: 16px 40px`
- **背景**: `#d9d4c7` / 下辺に `1px solid #bfb9a9`
- **左**: サイト名 `● MAZE 012345h`
  - `●` の色 `#a86a4b`、`MAZE 012345h` の色 `#4a5a6b`（`margin-left: 6px`）
  - フォントサイズ `22px` / `font-weight: 600` / `letter-spacing: 0.5px`
  - クリックでページを再読み込み（`location.reload()`）
- **右**: ラベル `Download`（`16px` / `#5a5a5a` / `font-weight: 500`）+ GitHub ボタン
  - ボタン: `padding: 0 24px` / `height: 38px` / 背景 `#6b7a8a` / 文字 `#ffffff` / `border-radius: 5px` / `font-size: 16px` / `font-weight: 500`
  - リンク先: **`https://github.com/rICT6F/MAZE-012345h`**（`target="_blank"` / `rel="noopener noreferrer"`）
  - ホバー: 背景 `#5a6878`（`transition: background 0.2s`）

### 2. メイン (Main)

`display: flex` / `flex-direction: column` / `gap: 28px` / `padding: 32px 56px`

#### 2-a. コンテンツ行（左: 画像枠 / 右: 説明）

- **レイアウト**: `display: grid` / `grid-template-columns: 1fr 1fr` / `gap: 44px` / `align-items: center`
- 1280px キャンバスでは、左右カラムはそれぞれ約 **562px** 幅になります。

**左エリア（画像配置枠）**

- **役割**: スクリーンショット画像を黒背景の枠内に配置する枠
- **配置**: 左カラムの中央に配置（`display: flex` / `align-items: center` / `justify-content: center`）
- **枠のスタイル**
  - 背景 `#000000`
  - `border-radius: 0`（**四隅は直角**）
  - `overflow: hidden`
  - `box-shadow: 0 6px 18px rgba(0, 0, 0, 0.15)`
- **枠の寸法**: 下の「画像配置ルール」で確定（1280×800 のビューポートでは **480 × 270 px**）
- **画像**: `width: 100%` / `height: 100%` / `object-fit: contain` / `object-position: center center` / `display: block`
- **現在の画像**: `MSX2 Software Presentation.png` / 実寸 **1024 × 576 px** / alt `MAZE 012345h screenshot`

**右エリア（説明）**

- `display: flex` / `flex-direction: column` / `justify-content: center` / `gap: 16px` / `padding-left: 16px`
- 本文: `font-family: "Meiryo", "メイリオ", sans-serif` / `font-size: 18px` / `line-height: 2.0` / `color: #3a3a3a`
- 1 行目（`lead`）のみ `font-size: 20px` / `margin-bottom: 18px`
- リンク: `color: #4a6a8a` / `text-decoration: none` / `border-bottom: 1px dotted #4a6a8a` / ホバーで `#2a4a6a`
- **正確な文言**
  - `MSX2 DISKプログラム`
  - `・SourceForge  blueMSX` → `https://sourceforge.net/projects/bluemsx/`
  - `・WebMSX` → `https://webmsx.org/`
  - いずれも `target="_blank"` / `rel="noopener noreferrer"`

#### 2-b. 区切り線 (Divider)

- `display: flex` / `align-items: center` / `gap: 12px`
- 中央に `◆`（`color: #a86a4b` / `font-size: 14px`）
- 左右に `1px` の線: `linear-gradient(to right, transparent, #bfb9a9, transparent)`

#### 2-c. 支援先ボタン (Support Buttons)

- `display: flex` / `gap: 18px` / `justify-content: center`
- 各ボタン: `padding: 0 28px` / `height: 42px` / `min-width: 200px` / `border-radius: 5px` / `font-size: 16px` / `font-weight: 500` / 文字 `#ffffff`
- ホバー: `opacity: 0.85`（`transition: opacity 0.2s`）

| ラベル | 背景色 | リンク先 |
| --- | --- | --- |
| Mozilla Foundation | `#161616` | `https://www.mozilla.org/` |
| LibreOffice | `#00a500` | `https://www.libreoffice.org/` |
| GIMP | `#0e2426` | `https://www.gimp.org/` |

#### 2-d. 免責文 (Disclaimer)

- `font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", Arial, sans-serif`
- `font-size: 12px` / `line-height: 1.75` / `color: #857d70` / `max-width: 960px` / 中央揃え / `margin: 12px auto 0`
- 2 段落（`p + p` は `margin-top: 10px`）

### 3. フッター (Footer)

- `padding: 14px 40px` / 背景 `#d9d4c7` / 上辺に `1px solid #bfb9a9` / 中央揃え
- `font-size: 12px` / `color: #9a9488`
- 文言: `© 2026-2*** rICT6F. All rights reserved.`

---

## 画像配置ルール（最重要仕様）

### 判定の流れ

1. `<img>` の `src` 属性から **ファイル名だけ** を取り出す（`imageFileStem`）
   - フォルダパスを除去
   - `?` クエリ / `#` ハッシュを除去
   - `decodeURIComponent`（URL エンコードの `%20` などを復元。失敗時はそのまま）
   - Unicode **NFC 正規化**
   - 拡張子（1〜6 文字の英数字）を除去
   - 前後の空白を除去
2. 得られた文字列を `IMAGE_PLACEMENT_RULES` の各 `match`（正規表現 or 文字列）に **上から順に** 照合
3. 最初に一致したルールを適用。どれにも一致しなければ `DEFAULT_IMAGE_PLACEMENT` を適用
4. 枠に `data-image-name`（判定したファイル名）と `data-placement`（適用したルールの `id`）を出力

**画像の画素数・`naturalWidth` / `naturalHeight`・`complete`・`srcset` は一切参照しません。** これにより「同じファイル名なら中身を差し替えても常に同じ配置」が保証されます。

### 現在のルール

```js
const IMAGE_PLACEMENT_RULES = [
  {
    id: "MSX2_Software_Presentation",
    // 区切り文字（空白 / アンダースコア / ハイフン）の差は許容
    match: /^MSX2[\s_\-]*Software[\s_\-]*Presentation$/i,
    ratio: [1024, 576],   // 16:9
    maxWidth: 480,
    maxHeight: 430,
    reservedHeight: 320,
    fit: "contain",
    pos: "center center",
    bg: "#000000"
  },
  {
    // 以前の対象ファイル名（src を戻せば元の配置に戻る）
    id: "MAZE_odp",
    match: /^MAZE_odp\s*\(\s*Gemini_Generated_Image\s*\)$/i,
    ratio: [512, 452],
    maxWidth: 480,
    maxHeight: 430,
    reservedHeight: 320,
    fit: "contain",
    pos: "center center",
    bg: "#000000"
  }
];
```

**一致するファイル名の例（すべて同一ルール）**

| 実際のファイル名 | 判定される文字列 | 適用ルール |
| --- | --- | --- |
| `MSX2 Software Presentation.png` | `MSX2 Software Presentation` | `MSX2_Software_Presentation` |
| `MSX2SoftwarePresentation.png` | `MSX2SoftwarePresentation` | `MSX2_Software_Presentation` |
| `MSX2_Software_Presentation.png` | `MSX2_Software_Presentation` | `MSX2_Software_Presentation` |
| `msx2-software-presentation.png` | `msx2-software-presentation` | `MSX2_Software_Presentation`（大文字小文字は無視） |
| `MSX2%20Software%20Presentation.png` | `MSX2 Software Presentation` | `MSX2_Software_Presentation` |
| `MAZE_odp (Gemini_Generated_Image).png` | `MAZE_odp (Gemini_Generated_Image)` | `MAZE_odp` |
| `other-image.png` | `other-image` | `default`（CSS 既定値） |

### 枠の実寸の決め方

```
ratio         = rule.ratio[0] / rule.ratio[1]
scale         = min(window.innerWidth / 1280, 1)
vhInternal    = window.innerHeight / scale          // ビューポート高さをページ内部座標へ換算
availH        = max(160, min(rule.maxHeight, vhInternal - rule.reservedHeight))
availW        = max(160, min(rule.maxWidth, 左カラムの clientWidth))
w = availW;  h = w / ratio;
if (h > availH) { h = availH; w = h * ratio; }       // 高さが収まらない場合は高さ基準で縮小
```

確定した値は枠のインライン CSS 変数として設定されます（CSS 側はこの変数を読むだけ）。

```
--shot-w: 480px      // 枠の幅
--shot-h: 270px      // 枠の高さ
--shot-fit: contain  // 画像の収め方（object-fit）
--shot-pos: center center  // object-position
--shot-bg: #000000   // 枠の背景色
```

`reservedHeight: 320` は「ページ内で画像以外が使う高さの目安」です。ビューポートが低いときは、この値を差し引いた分まで画像枠が縮み、ページ内の他要素（テキスト・ボタン・免責文）と衝突しません。

**実測例（ビューポート 1280 × 800 / `MSX2 Software Presentation.png`）**

| 項目 | 値 |
| --- | --- |
| `data-image-name` | `MSX2 Software Presentation` |
| `data-placement` | `MSX2_Software_Presentation` |
| 枠の実寸 | **480 × 270 px**（厳密に 16:9） |
| 縦横比 | 480 / 270 = 1.7778（= 1024 / 576） |
| 画像の実寸 | 1024 × 576 px |
| `object-fit` | `contain`（黒帯なし） |
| 枠の背景色 | `rgb(0, 0, 0)` |
| 横スクロールバー | 発生しない |

### 新しいファイル名を追加する手順

`IMAGE_PLACEMENT_RULES` に 1 件追加するだけです。`match` にそのファイル名（拡張子なし）を、`ratio` に **画像の実際の縦横比** を入れるのが基本です（比率を合わせると `contain` で黒帯が出ません）。

---

## インタラクションと挙動 (Interactions & Behavior)

- **ナビゲーション**: 外部リンクのみ（別タブで開く）。ページ内遷移はなし。
- **ヘッダーのサイト名クリック**: `location.reload()` で再読み込み。
- **ホバー**
  - GitHub ボタン: 背景 `#6b7a8a` → `#5a6878`（0.2s）
  - 支援先ボタン: `opacity: 1` → `0.85`（0.2s）
  - 本文リンク: 文字色 `#4a6a8a` → `#2a4a6a`
- **アニメーション / トランジション**: 上記 CSS トランジションのみ。スクロール連動アニメーションやローディング状態はありません。
- **フォーム / バリデーション / データ取得**: なし（静的ページ）。

### レスポンシブ挙動（固定サイズキャンバスのスケーリング）

基準キャンバスは **1280px 幅**。ページ全体を `transform: scale()` でフィットさせます。

1. `scale = min(window.innerWidth / 1280, 1)`
   - ビューポート幅 **1280px 以上**: 等倍表示（左右に黒レターボックス）
   - **1280px 未満**: 横幅に合わせて等比縮小
2. `transform: scale()` はレイアウト上の高さを変えないため、縮小分の余白を `margin-bottom` の負値で相殺し、`stage` の高さを実表示高に合わせる
3. 縦はコンテンツ高さに応じて伸び、収まらない場合はブラウザの縦スクロールで閲覧
4. **順序が重要**: `fit()` の中で必ず `layoutImagePlacement()` を先に呼び、画像枠の寸法を確定してから高さを測定する（確定後の高さでなければレターボックスの補正量がずれる）
5. `resize` は `requestAnimationFrame` でスロットリング。`load` と `document.fonts.ready` でも `fit()` を実行

---

## 状態管理 (State Management)

状態変数はありません。以下はすべて **DOM の属性 / CSS 変数に反映される派生値** です。

| 派生値 | 置き場所 | 値の例 |
| --- | --- | --- |
| 判定したファイル名 | 枠の `data-image-name` | `MSX2 Software Presentation` |
| 適用したルール ID | 枠の `data-placement` | `MSX2_Software_Presentation` |
| 枠の寸法・収め方 | 枠の `--shot-w` / `--shot-h` / `--shot-fit` / `--shot-pos` / `--shot-bg` | `480px` / `270px` / `contain` / `center center` / `#000000` |

- 入力（データ取得・ユーザー操作）: なし
- 永続化・サーバー通信: なし
- 画像を差し替えた場合は `layoutImagePlacement()` を再度呼ぶ必要があります（`img` の `load` 後に呼ぶ実装にすると安全です）

---

## デザイントークン (Design Tokens)

### 色

| 用途 | 値 |
| --- | --- |
| ページ外側（レターボックス） | `#000000` |
| ページ背景 | `#e8e4dc` |
| ヘッダー / フッター背景 | `#d9d4c7` |
| ヘッダー下辺 / フッター上辺 / 区切り線 | `#bfb9a9` |
| 本文テキスト | `#3a3a3a` |
| サイト名（記号） | `#a86a4b` |
| サイト名（文字） | `#4a5a6b` |
| `Download` ラベル | `#5a5a5a` |
| GitHub ボタン / ホバー | `#6b7a8a` / `#5a6878` |
| 本文リンク / ホバー | `#4a6a8a` / `#2a4a6a` |
| 支援先ボタン | `#161616` / `#00a500` / `#0e2426` |
| 免責文 | `#857d70` |
| フッター文字 | `#9a9488` |
| 画像枠の背景 | `#000000` |

### タイポグラフィ

| 用途 | フォント | サイズ | ウェイト | 行間 |
| --- | --- | --- | --- | --- |
| サイト名 | Meiryo | 22px | 600 | — |
| `Download` ラベル | Meiryo | 16px | 500 | 1（`line-height: 1`） |
| ボタン（GitHub / 支援先） | Meiryo | 16px | 500 | 1 |
| 説明の導入行（`lead`） | Meiryo | 20px | 既定 | 2.0 |
| 説明の本文 | Meiryo | 18px | 既定 | 2.0 |
| 免責文 | システムフォント | 12px | 既定 | 1.75 |
| フッター | Meiryo | 12px | 既定 | 既定 |
| 区切り記号 `◆` | Meiryo | 14px | 既定 | — |

フォントスタック:
- 本文 / 見出し: `"Meiryo", "メイリオ", -apple-system, BlinkMacSystemFont, "Helvetica Neue", Arial, sans-serif`
- 免責文のみ: `-apple-system, BlinkMacSystemFont, "Helvetica Neue", Arial, sans-serif`

### 余白スケール

`4 / 6 / 10 / 12 / 14 / 16 / 18 / 24 / 28 / 32 / 40 / 44 / 56`（px）。主な使用箇所:

- ヘッダー: `padding: 16px 40px`
- メイン: `padding: 32px 56px` / 要素間 `gap: 28px`
- コンテンツ行: 左右 `gap: 44px`
- 右エリア: `gap: 16px` / `padding-left: 16px`
- 支援先ボタン: `gap: 18px` / `padding: 0 28px`
- フッター: `padding: 14px 40px`

### 角丸 / 影

| 用途 | 値 |
| --- | --- |
| ボタン（GitHub / 支援先） | `border-radius: 5px` |
| **画像枠** | `border-radius: 0`（**四隅は直角**） |
| 画像枠の影 | `box-shadow: 0 6px 18px rgba(0, 0, 0, 0.15)` |

---

## アセット (Assets)

| ファイル | 内容 | 実寸 |
| --- | --- | --- |
| `MSX2 Software Presentation.png` | 左エリアに表示するスクリーンショット（**現在の対象**） | 1024 × 576 px / PNG / RGBA（透明画素なし） |
| `image_slot.js` | ドラッグ＆ドロップで画像を差し替える `<image-slot>` ウェブコンポーネント（31KB）。`index.html` が読み込んでいますが、**現在のマークアップでは `<image-slot>` を使用していません**（将来の差し替え用。実装時に不要なら外して構いません） | — |

外部アセット（画像・フォント・アイコン）はありません。アイコンは使用していません。文字記号 `●` `◆` `・` のみです。

---

## 変更履歴 (Change Log)

このデザインは以下の順で調整されています。実装時は **すべて反映済みの最新状態** を対象にしてください。

1. **画像配置ルールをファイル名基準に変更**: 画像の画素・寸法ではなく `src` のファイル名だけで枠の形・大きさ・収め方を決める方式に統一
2. **枠の四隅を直角化**: `border-radius: 5px` → `0`（枠の形・大きさ・収め方は変更なし）
3. **タイトル表記の修正**: `MAZE 12345h` → `MAZE 012345h`（0 を追加。文字種・大きさ・色は不変）
4. **GitHub リンク先の変更**: `https://github.com/rICT6F/MAZE-12345h` → `https://github.com/rICT6F/MAZE-012345h`
5. **対象画像の差し替え**: `MAZE_odp (Gemini_Generated_Image).png`（512×452）→ `MSX2 Software Presentation.png`（1024×576 / 16:9）。枠も 16:9 に追従
6. **導入文の変更**: `MSX2用のDISKプログラムです。` → `MSX2 DISKプログラム`（サイズ・色・行間は不変）

---

## ファイル (Files)

| ファイル | 役割 |
| --- | --- |
| `index.html` | ページ本体。CSS（`<style>`）と画像配置ロジック（インライン `<script>`）を含む |
| `image_slot.js` | 画像差し替え用ウェブコンポーネント（現在は未使用） |
| `MSX2 Software Presentation.png` | 左エリアのスクリーンショット |

### 実装時の要点（チェックリスト）

- [ ] 基準キャンバス **1280 × 720**、`transform: scale(min(vw / 1280, 1))` でフィット、縮小分を負の `margin-bottom` で相殺
- [ ] 画像枠: `border-radius: 0`（四隅直角） / 背景 `#000000` / `box-shadow: 0 6px 18px rgba(0,0,0,0.15)` / `overflow: hidden`
- [ ] 画像枠の寸法は **ファイル名から決まるルール** で確定し、`contain` で収める
- [ ] 判定に **画像の中身・寸法を使わない**（ファイル名のみ）
- [ ] `MSX2 Software Presentation` → 枠 **480 × 270**（16:9）が既定値
- [ ] GitHub ボタンのリンク先は `https://github.com/rICT6F/MAZE-012345h`
- [ ] サイト名・タイトル・alt は `MAZE 012345h`（0 を含む 6 桁）
- [ ] レスポンシブ計算では **画像枠の確定 → 高さ測定** の順を守る
