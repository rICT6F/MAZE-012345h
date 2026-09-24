# Handoff: MAZE 12345h Landing Page

## Overview
「MAZE 12345h」(MSX2 用の DISK プログラム) のプロジェクト紹介 / ダウンロード用の
シングルページ Web サイトです。1 画面完結のランディングページで、以下を提供します。

- プロジェクト名 (ロゴ) と GitHub からのダウンロード導線
- プロジェクト概要 (スクリーンショット + 説明文 + 関連リンク)
- サポート対象のオープンソース団体への外部リンク (3 ボタン)
- 免責事項 (寄付は任意である旨) およびフッター (コピーライト表記)

デザインの一番のポイントはレスポンシブレイアウトの挙動です。**1280 × 720 の固定
デザインを基準**として、以下のように振る舞います。

- ブラウザ幅 **≥ 1280 px** → 実サイズで中央表示、左右に**黒のレターボックス**
- ブラウザ幅 **< 1280 px** → **横比を維持したまま縮小** (CSS `transform: scale()`)
- 縦方向 → コンテンツ (ヘッダー → 画像＋説明欄 → 3 ボタン → 免責文 → フッター)
  が全て収まるまで伸び、収まらない場合は**ブラウザの縦スクロール**で閲覧

## About the Design Files
このバンドルに含まれる `index.html` / `image_slot.js` は **HTML で作成された
デザインリファレンス**です。意図した見た目・振る舞いを示すプロトタイプであり、
そのまま本番環境にコピーして使うためのプロダクションコードではありません。

実装タスクは、これらの HTML デザインを**ターゲットとなるコードベースの既存環境**
(React / Vue / Next.js / SvelteKit / Astro / 静的 HTML など) 上で、
その環境の確立されたパターン・コンポーネント・スタイリング手法を用いて**再現**
することです。既存環境がまだ無い場合は、プロジェクトに最も適したフレームワークを
選択し、その中で実装してください。

`image_slot.js` は Genspark Designer 側の「ユーザーが画像をドロップして
差し替えられるプレースホルダー」コンポーネントで、**本番実装には不要**です
(スクリーンショットは静的な `<img>` タグに置き換えて構いません)。

## Fidelity
**High-fidelity (hifi)**: 本ハンドオフはピクセル単位のカラー・タイポグラフィ・
スペーシング・レイアウトが確定した高忠実度モックアップです。以下の Design
Tokens セクションに記載の値をそのまま使用し、既存コードベースの UI ライブラリ
/ Tailwind / CSS Modules / styled-components などのパターンで再現してください。

## Screens / Views

本デザインは 1 ページのみで構成されます。

### Screen: Landing Page (`index.html`)

- **Name**: MAZE 12345h Landing Page
- **Purpose**: 訪問者にプロジェクト概要を伝え、GitHub からのダウンロード導線と、
  支援したい OSS 団体への外部リンクを提供する。
- **Layout (base 1280 × 720)**:
  - ルートに **黒背景の外枠 (stage)** があり、その中央に **1280 px 幅の
    ページコンテナ (page)** が縦積みで配置される。
  - ページコンテナは縦フレックス (`display: flex; flex-direction: column`) で
    以下 3 セクションを上から順に配置:
    1. `<header>` (高さは内容依存、上下 `padding: 16px 40px`)
    2. `<main>` (`flex: 1`, `padding: 32px 56px`, セクション間 `gap: 28px`)
    3. `<footer>` (上下 `padding: 14px 40px`)
  - `<main>` 内は縦積みで:
    - `.content-row` (2 カラム CSS Grid, `grid-template-columns: 1fr 1fr`,
      `gap: 44px`, 画像 = 左 / 説明文 = 右, 縦中央揃え)
    - `.divider` (中央に ◆ を配置した水平罫線、左右に薄いグラデーション線)
    - `.support-row` (3 つのボタンを `gap: 18px` で横並び、中央揃え)
    - `.disclaimer` (最大幅 960 px、中央揃えの英文 2 段落)

- **Components**:

  1. **Header (`<header>`)**
     - Background: `#d9d4c7`
     - Border-bottom: `1px solid #bfb9a9`
     - Padding: `16px 40px`
     - Layout: `display: flex; justify-content: space-between; align-items: center`
     - 左: サイト名リンク `.site-name`
       - Font-size: 22px / weight 600 / letter-spacing: 0.5px
       - 装飾: 赤茶色の `●` (`.dot`, color `#a86a4b`) + サイト名テキスト
         `MAZE 12345h` (`.name`, color `#4a5a6b`, `margin-left: 6px`)
       - Behavior: クリックで `location.reload()` (自己リロード)
     - 右: `.download-area`
       - "Download" ラベル (font-size 16 / color `#5a5a5a` / weight 500)
       - GitHub ボタン `.btn-github`
         - Background: `#6b7a8a` / hover: `#5a6878`
         - Text: white / font-size 16 / weight 500 / line-height 1
         - Size: 高さ 38 px、`padding: 0 24px`、`border-radius: 5px`
         - href: `https://github.com/rICT6F/MAZE-12345h`
         - target: `_blank`, `rel="noopener noreferrer"`
         - Transition: `background 0.2s`

  2. **Content Row (`.content-row`)**
     - 2 カラム Grid、`gap: 44px`、`align-items: center`
     - **左カラム (`.image-col > .screenshot-frame`)**
       - 幅: 100% (最大 480 px、`aspect-ratio: 512 / 452`)
       - Background: `#000000` / Border-radius: 5px
       - Box-shadow: `0 6px 18px rgba(0,0,0,0.15)`
       - 中央にスクリーンショット画像を `object-fit: contain` で配置
       - Image src: `MAZE_0000 (Screenshot-2).png` (プロジェクト側で用意)
       - Alt: `"MAZE 12345h screenshot"`
     - **右カラム (`.text-col > .project-about`)**
       - `padding-left: 16px`, `gap: 16px`, 縦中央揃え
       - Font-family: Meiryo, メイリオ, sans-serif
       - Font-size: 18px / line-height 2.0 / color `#3a3a3a`
       - リード段落 (`.lead`): font-size 20px, `margin-bottom: 18px`
         - Text: 「MSX2用のDISKプログラムです。」
       - リンク付き段落 (行頭 `・` + `<a>`):
         - SourceForge  blueMSX → `https://sourceforge.net/projects/bluemsx/`
         - WebMSX → `https://webmsx.org/`
       - リンク: color `#4a6a8a` / `border-bottom: 1px dotted #4a6a8a`
         hover: color `#2a4a6a`

  3. **Divider (`.divider`)**
     - Flex 横並び、左右に線 (`::before` / `::after`)
     - 線: `height: 1px`, `linear-gradient(to right, transparent, #bfb9a9, transparent)`
     - 中央: ◆ 文字 (color `#a86a4b`, font-size 14px)

  4. **Support Buttons (`.support-row`)**
     - 中央揃え、`gap: 18px` で 3 つのボタンを横並び
     - 各ボタン `.btn-support` 共通:
       - 高さ 42 px、`padding: 0 28px`、`min-width: 200px`
       - `border-radius: 5px`、font-size 16 / weight 500 / line-height 1
       - Text: white、`transition: opacity 0.2s`、hover `opacity: 0.85`
       - target `_blank`, `rel="noopener noreferrer"`
     - 各バリエーション:
       - `.btn-mozilla` → background `#161616`
         label: `Mozilla Foundation` / href: `https://www.mozilla.org/`
       - `.btn-libre` → background `#00a500`
         label: `LibreOffice` / href: `https://www.libreoffice.org/`
       - `.btn-gimp` → background `#0e2426`
         label: `GIMP` / href: `https://www.gimp.org/`

  5. **Disclaimer (`.disclaimer`)**
     - Max-width 960 px、`margin: 12px auto 0 auto`、中央揃え
     - Font-family: `-apple-system, BlinkMacSystemFont, "Helvetica Neue", Arial, sans-serif`
     - Font-size 12 / line-height 1.75 / color `#857d70`
     - 段落 1: `This software is completely free to use. If you would like
       to support this project, please consider donating to the open-source
       organizations listed above or utilizing their related services to
       help sustain their development and operations.`
     - 段落 2: `Donations are entirely optional. You can access and use all
       features without any restrictions, even if you do not make a
       donation.`
     - 段落間: `margin-top: 14px` 相当 (`p + p { margin-top: 10px }`)

  6. **Footer (`<footer>`)**
     - Background `#d9d4c7`, `border-top: 1px solid #bfb9a9`
     - Padding `14px 40px`、中央揃え
     - Font-size 12 / color `#9a9488`
     - Text: `© 2026-2*** rICT6F. All rights reserved.`

## Interactions & Behavior

- **リンク遷移**
  - GitHub / OSS ボタン / 本文中の外部リンクは全て `target="_blank"` +
    `rel="noopener noreferrer"` で新規タブ遷移。
- **サイト名クリック**
  - ヘッダー左の `.site-name` は `onclick="location.reload(); return false;"`
    でページを再読み込み (SPA 実装時は代わりにトップへのルーティングで OK)。
- **Hover 状態**
  - `.btn-github`: 背景色を `#6b7a8a` → `#5a6878` に (transition 0.2s)
  - `.btn-support` (全種): `opacity: 1 → 0.85` に (transition 0.2s)
  - `.project-about a`: 文字色を `#4a6a8a` → `#2a4a6a` に
- **アニメーション**
  - ホバー以外のアニメーションは無し。
- **フォーム / バリデーション**
  - フォーム要素は無し。
- **Loading / Error 状態**
  - 特別なローディング / エラー画面は無し。スクリーンショット画像が読み込め
    ない場合は alt テキスト (`MAZE 12345h screenshot`) が表示される。

### Responsive Behavior (最重要)

- **基準サイズ**: 1280 px × 720 px (デザインは 1280 幅前提で組まれる)
- **ブラウザ幅 ≥ 1280 px**
  - コンテナは 1280 px 幅のまま画面中央に等倍配置
  - コンテナ左右の余りは `#000000` (黒) のレターボックス
- **ブラウザ幅 < 1280 px**
  - `scale = window.innerWidth / 1280` を `transform: scale(scale)` で
    ページコンテナに適用 (`transform-origin: top center`)
  - **横比を維持したまま**縮小する。文字サイズや余白も全て一律に縮む
- **縦方向**
  - ページコンテナは `min-height: 720px` だが、コンテンツ量で伸長する
  - `body` は `overflow-x: hidden; overflow-y: auto` で横スクロールは常に非表示、
    縦は必要に応じてブラウザ標準スクロールバーを利用
  - `transform: scale()` はレイアウト上の高さを変えないため、縮小後は
    `page.style.marginBottom = -(naturalH - scaledH)` を JS で毎回設定し、
    stage の高さと表示高さを一致させている (JS 内 `fit()` 関数を参照)
- **トリガ**
  - `window` の `resize` / `load` イベント、および `document.fonts.ready` の
    直後に `fit()` を再実行

React / Vue などで再現する場合は、`useEffect` / `mounted` で resize リスナーを
登録し、`ResizeObserver` を使うとより自然です。実装例:

```jsx
useEffect(() => {
  const page = pageRef.current;
  const BASE_W = 1280;
  const fit = () => {
    const scale = Math.min(window.innerWidth / BASE_W, 1);
    page.style.transform = `scale(${scale})`;
    const naturalH = page.offsetHeight;
    const gap = naturalH - naturalH * scale;
    page.style.marginBottom = `${-gap}px`;
  };
  window.addEventListener('resize', fit);
  fit();
  return () => window.removeEventListener('resize', fit);
}, []);
```

## State Management

- クライアント側 state は不要 (完全に静的なランディングページ)。
- レスポンシブ fit のスケール値のみ、DOM に対して直接書き込む (React 実装時は
  ref + inline style で十分。state 化する必要は無い)。
- データ取得 (fetch / API) は無し。

## Design Tokens

### Colors
| 用途 | Hex |
|---|---|
| Letterbox / screenshot frame background | `#000000` |
| Page background | `#e8e4dc` |
| Header / Footer background | `#d9d4c7` |
| Header / Footer border | `#bfb9a9` |
| Primary text | `#3a3a3a` |
| Accent (dot / diamond) | `#a86a4b` |
| Site name text | `#4a5a6b` |
| Download label | `#5a5a5a` |
| GitHub button background | `#6b7a8a` |
| GitHub button hover | `#5a6878` |
| Link default | `#4a6a8a` |
| Link hover | `#2a4a6a` |
| Disclaimer text | `#857d70` |
| Footer text | `#9a9488` |
| Support btn Mozilla bg | `#161616` |
| Support btn LibreOffice bg | `#00a500` |
| Support btn GIMP bg | `#0e2426` |
| Button text | `#ffffff` |

### Spacing scale
- Header padding: `16px 40px`
- Main padding: `32px 56px`
- Footer padding: `14px 40px`
- Main section gap: `28px`
- Content row gap: `44px`
- Text col gap: `16px`
- Support row gap: `18px`
- Divider gap: `12px`
- Disclaimer top margin: `12px`

### Typography scale
- Base font-family: `"Meiryo", "メイリオ", -apple-system, BlinkMacSystemFont, "Helvetica Neue", Arial, sans-serif`
- Site name: 22 / 600 / letter-spacing 0.5px
- Download label: 16 / 500 / line-height 1
- GitHub button: 16 / 500 / line-height 1
- Project lead: 20 / regular / line-height 2.0
- Project body: 18 / regular / line-height 2.0
- Support button: 16 / 500 / line-height 1
- Disclaimer: 12 / regular / line-height 1.75
  (font-family は system sans に切替: `-apple-system, BlinkMacSystemFont, "Helvetica Neue", Arial, sans-serif`)
- Footer: 12 / regular / color `#9a9488`
- Divider diamond: 14 / color `#a86a4b`

### Border-radius
- Buttons (GitHub / Support): `5px`
- Screenshot frame: `5px`

### Shadows
- Screenshot frame: `0 6px 18px rgba(0,0,0,0.15)`

### Layout dimensions
- Design base: **1280 × 720**
- Page container: `width: 1280px`, `min-height: 720px`
- Screenshot frame: `width: 100%`, `max-width: 480px`, `aspect-ratio: 512 / 452`
- GitHub button: height `38px`, padding `0 24px`
- Support button: height `42px`, padding `0 28px`, `min-width: 200px`
- Disclaimer: `max-width: 960px`

### Transitions
- GitHub button background: `background 0.2s`
- Support buttons: `opacity 0.2s`

## Assets

以下のバイナリアセットは本ハンドオフパッケージには含めていません。
本番実装側で別途用意してください。

| ファイル | 用途 | 備考 |
|---|---|---|
| `MAZE_0000 (Screenshot-2).png` | 左カラムのプロジェクトスクリーンショット | 元プロジェクト内で管理。実装時は縦横比 512:452 (≒ 1.133:1) の PNG/WEBP を推奨。ファイル名は英数のみに正規化することを推奨 (例: `maze-screenshot.png`) |

外部リンク (画像アセットではない、ボタン / テキストリンクの遷移先):

- `https://github.com/rICT6F/MAZE-12345h`
- `https://sourceforge.net/projects/bluemsx/`
- `https://webmsx.org/`
- `https://www.mozilla.org/`
- `https://www.libreoffice.org/`
- `https://www.gimp.org/`

アイコン / SVG は使用していません (`●` / `◆` はテキスト文字で表現)。

## Files

本ハンドオフフォルダに含まれるファイル:

- `README.md` — 本ドキュメント
- `index.html` — メインのデザインリファレンス (単一ページで全構造・全スタイル・
  レスポンシブ fit スクリプトを含む)
- `image_slot.js` — Genspark Designer 側で使用しているユーザードロップ用
  プレースホルダーコンポーネント。**本番実装では不要** (`<script src>` の参照ごと
  削除して構いません)。参考のためだけに同梱しています。

### 実装時のチェックリスト

- [ ] 1280 × 720 基準のレスポンシブ fit を JS (or ResizeObserver) で実装
- [ ] `overflow-x: hidden` + `overflow-y: auto` を body に適用
- [ ] 黒レターボックス (`background: #000000`) を最外枠に適用
- [ ] Design Tokens セクションのカラー / スペーシング / タイポを反映
- [ ] 全外部リンクを `target="_blank"` + `rel="noopener noreferrer"` に
- [ ] スクリーンショット画像を差し替え (`aspect-ratio: 512 / 452` を維持)
- [ ] `image_slot.js` の script タグは削除
