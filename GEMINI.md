# プロジェクト実装ルール (GEMINI.md)

このファイルは本プロジェクト固有の実装ルールや推奨パターンを記録したものです。AIエージェント（Antigravity, Gemini等）はプロジェクトを編集する際、必ずこのファイルの内容を遵守してください。

## 0. 必読：プロジェクト背景コンテキスト

**`CONTEXT.md`** を必ず読むこと。研修事業の背景・目的・ユーザー（H.K）のポジション・教材の設計思想が記載されている。

## 0.1 一時ファイルの運用ルール

- **リサーチメモ**: `Today_Research.md` を**上書き更新**して使う。新規ファイルを作らない
- **実装計画**: `Today_Plan.md` を**上書き更新**して使う。新規ファイルを作らない
- **YouTube文字起こし(VTT)とリサーチ手法**: 動画リサーチを依頼された場合、AI自身による過度な要約やタイトルからの推測はハルシネーション（後続AIへの伝言ゲーム）の原因となるため禁止。**必ず `yt-dlp` 等を用いて字幕の「ほぼ全文」を取得**し、`Today_Research.md` 等に展開可能な形式（`<details>`タグ等）で追記して、ClaudeCodeやCursorが一次情報を直接参照できるようにすること。なお、ダウンロードしたVTTファイル等の実体は実装完了後に必ず削除する。
- **スクリプト（.py / .js）**: 使い捨てスクリプトはプロジェクトルートに置かない。必要なら `scratch/` フォルダを作って格納し、不要になったら削除する
- **引き継ぎメモ**: ClaudeCode/Cursorへの引き継ぎは `Today_Plan.md` に統合する。別ファイル（handoff_*.txt等）を作らない

## 1. YouTube動画の最適化埋め込み（Facadeパターン）

YouTube動画を埋め込む際は、そのまま単純な `<iframe>` を記述するのではなく、**必ず「Facade（ファサード）パターン」を採用**してください。

### 採用理由
- **サムネイルの高画質化:** 通常の埋め込みでは表示枠が小さい場合に低解像度（480p相当等）のサムネが強制適用され、文字が粗くなる（ガビガビになる）のを防ぐため。
- **パフォーマンス向上:** 初期表示時に重いYouTubeのPlayer APIや複数のiframeを読み込まないようにするため。
- **UXの統一:** クリック時に別タブへ遷移させず、その場の枠内でいきなり再生を開始させるため。

### 実装の必須要件

1. **フォールバック付きの背景画像設定**
   `maxresdefault.jpg` (1080p相当) を第一候補とし、存在しない場合に備えてCSSの複数背景指定を利用して `hqdefault.jpg` をフォールバックとしてカンマ区切りで必ず記述すること。
2. **プレースホルダーのHTML構造**
   `<iframe>` ではなく、以下のような `<div>` の枠を用いること。
3. **Vanilla JSによる遅延読み込み**
   クリックイベントを検知してから `innerHTML` を `<iframe>` (引数に `?autoplay=1` を付与) で置換すること。

### 実装コードのテンプレート

**【HTML】**
```html
<div class="video-card">
    <div class="vc-thumb" data-video-id="[YouTube動画ID]" style="background-image: url('https://img.youtube.com/vi/[YouTube動画ID]/maxresdefault.jpg'), url('https://img.youtube.com/vi/[YouTube動画ID]/hqdefault.jpg');">
        <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
    <div class="vc-info">
        <div class="vc-title">動画タイトル</div>
    </div>
</div>
```

**【CSS】**
```css
/* サムネイル枠 */
.vc-thumb {
    width: 100%;
    aspect-ratio: 16/9;
    background-color: #000;
    background-size: cover;
    background-position: center;
    position: relative;
    cursor: pointer;
}
/* ホバー時の暗転（オプション） */
.vc-thumb::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0; bottom: 0;
    background: rgba(0, 0, 0, 0.1);
    transition: background 0.3s ease;
    pointer-events: none;
}
.vc-thumb:hover::before {
    background: rgba(0, 0, 0, 0.2);
}
/* 中央のYouTubeアイコン */
.vc-thumb-play {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 3rem;
    color: rgba(255, 255, 255, 0.9);
    text-shadow: 0 2px 10px rgba(0, 0, 0, 0.4);
    transition: transform 0.3s ease, color 0.3s ease;
    pointer-events: none;
}
.vc-thumb:hover .vc-thumb-play {
    transform: translate(-50%, -50%) scale(1.1);
    color: #ff0000;
}
```

**【JavaScript】**
```javascript
// YouTube Facadeの実装（クリック時にiframeへ変換してインライン再生）
document.querySelectorAll('.vc-thumb[data-video-id]').forEach(thumb => {
    thumb.addEventListener('click', function() {
        const videoId = this.dataset.videoId;
        // iframeに置き換え、自動再生（autoplay=1）
        this.innerHTML = `<iframe src="https://www.youtube.com/embed/${videoId}?autoplay=1" allow="autoplay; encrypted-media" allowfullscreen style="width:100%; height:100%; border:none;"></iframe>`;
        // 背景画像を消す
        this.style.backgroundImage = 'none';
        this.style.cursor = 'default';
        // 再度クリックされないようにする
        this.onclick = null;
    }, { once: true });
});
```

## 2. モバイルレイアウトの最適化（レスポンシブ対応）

各Dayのページ（`volXX-1.html` 等）を作成・修正する際は、スマートフォン閲覧時のテキスト表示領域やコンテナ幅が適切になるよう、**必ず以下のモバイル向けメディアクエリ（CSS）を `<style>` タグの末尾に含めてください**。

### 実装の必須要件
- `.container` 等の左右の余白（`padding`, `margin`）を削減し、スマートフォン画面の幅を有効活用すること。
- 画像要素は必ず画面幅に収まるよう `max-width: 100%; height: auto;` 等を適用すること。

### 実装コードのテンプレート (CSS末尾用)
```css
/* === レスポンシブ対応（モバイル最適化） === */
@media (max-width: 768px) {
    .container { padding: 0 10px; margin-top: 80px; }
    .hero { margin-bottom: 2rem; padding-top: 1rem; }
    .hero h1 { font-size: 2rem; }
    .glass-card { padding: 1.5rem; margin-bottom: 2rem; gap: 1rem; }
    .info-box, .highlight-box, .column-box { padding: 1.25rem; margin: 1.25rem 0; }
    .tab-btn { padding: 0.8rem 1rem; font-size: 0.95rem; flex: 1 1 100%; justify-content: center; }
    .mission-area { padding: 1.5rem; margin-top: 2rem; }
    .task-item { padding: 1.25rem; gap: 1rem; flex-direction: column; }
    .pub-card { padding: 1.5rem; }
    .bento-item { padding: 1.5rem; }
    .step-box { padding: 1.25rem 1.5rem; }
    .accordion summary { padding: 1.25rem 1.5rem; font-size: 1.05rem; }
    .accordion .acc-body { padding: 0 1.5rem 1.5rem 3rem; }
    .fixed-header { padding: 0 20px; }
    .progress-container { width: 130px; }
    .wax-seal { width: 100px; height: 100px; font-size: 2.5rem; right: -10px; top: -10px; border-width: 3px; }
    .wax-seal::after { border-width: 2px; inset: 8px; }
}
```

## 3. Webデザイナーガイド（`webdesigner_guide/`）のデザインシステム

研修サイト（桜テーマ）とは**独立したデザイン体系**を持つ。以下を遵守すること。

### 3.1 カラーパレット（Warm Professional テーマ）
```css
:root {
  --bg-body:   #faf8f5;   /* ウォームクリーム */
  --bg-card:   #ffffff;   /* 白 */
  --bg-dark:   #3d4f6f;   /* ソフトネイビー（ヘッダー・フッター） */
  --text-main: #2c3440;   /* ウォームダーク */
  --text-sub:  #7a8494;   /* ミュートグレー */
  --accent:    #e07050;   /* ウォームコーラル */
  --accent-2:  #3bb4a0;   /* ティール */
  --border:    #e8e4df;   /* ウォームボーダー */
}
```

- **ヒーロー背景**: `linear-gradient(135deg, #3d4f6f, #566d94)` — ソフトネイビーグラデーション
- **メイキングページのアクセント**: `--accent-making: #c47a20`（落ち着いたアンバー）
- **旧ダーク色 `#1a1f36` は廃止**。使用禁止

### 3.2 ライティングトーン
- **文体**: です/ます調。「〜だ」「〜である」は使わない
- **トーン**: 優しさと親しみやすさ。カジュアル層・ライト層が主な読者
- **ただし**: 誠実さ・実直さ・真剣さは維持。ふざけすぎない
- テキスト壁を避ける。アイコン・カード・ビジュアルで視覚的に休憩ポイントを作る
