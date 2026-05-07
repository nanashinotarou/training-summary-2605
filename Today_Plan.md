# 実装計画: Day 1 ページ `vol01-1.html`

> **作成日:** 2026-05-07　**担当:** Cursor（このファイルをベースに実装すること）

---

## 0. 作業概要

新規ファイル `vol01-1.html` を作成する。既存 `index.html` のカラー変数・フォントを踏襲した「Fresh Green」テーマで構築する。

**必須遵守ルール:**
- YouTube は必ず **Facadeパターン**（遅延読み込み）で実装すること。`<iframe>` を直接HTMLに書くことは禁止。
- CSS末尾に **モバイル向けメディアクエリ** `@media (max-width: 768px)` を必ず含めること。
- ページ内テキストは `Today_Research.md` セクション2の台本・要約テキストを忠実に反映すること。ハルシネーション厳禁。

---

## 1. デザイン仕様

### カラー変数（CSS :root）
```css
:root {
    --bg-body: #f0fdf4;
    --bg-card: #ffffff;
    --text-main: #1e293b;
    --text-sub: #64748b;
    --accent-green: #059669;
    --accent-gold: #d97706;
    --accent-green-light: #d1fae5;
    --shadow: 0 20px 50px rgba(0, 0, 0, 0.10);
}
```

### フォント（index.html と同一）
```html
<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@500;700;900&family=Noto+Sans+JP:wght@400;500;700&family=Teko:wght@600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
```

---

## 2. ページ全体構成

```
[固定ヘッダー]
    - 左: "簿記コース May 2026"
    - 右: "Day 1 / 20" のプログレスバー（緑）

[ヒーローセクション]
    - タイトル: "Day 1 | 簿記の基本概念"
    - サブ: "AIと簿記の未来・Gemini/NotebookLMの基礎を掴もう"
    - 本日の目標ボックス（3項目箇条書き）

[前半パートセクション] #part1
    - セクションラベル: "前半パート"
    - 動画カード × 3本
    - 実習ブロック（Gemini/NotebookLM基礎）
    - 「後半パートへ進む」ボタン → #part2 へスクロール

[後半パートセクション] #part2
    - セクションラベル: "後半パート"
    - 動画カード × 3本
    - 実習ブロック（NotebookLMでテスト生成）
    - 「今日のまとめへ」ボタン → #summary へスクロール

[まとめセクション] #summary
    - "今日のまとめ" タイトル
    - まとめカード（3〜4項目）
    - 「ホームへ戻る」ボタン → index.html

[フッター]
    - コピーライト表記
```

---

## 3. 固定ヘッダー

```html
<header class="fixed-header">
    <div class="header-left">
        <i class="fa-solid fa-book-open-reader" style="color: var(--accent-green);"></i>
        <span class="header-title">簿記コース May 2026</span>
    </div>
    <div class="header-right">
        <span class="day-label">Day 1 / 20</span>
        <div class="progress-bar-wrap">
            <div class="progress-bar-fill" style="width: 5%;"></div>
        </div>
    </div>
</header>
```

**CSS:**
```css
.fixed-header {
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    background: rgba(255,255,255,0.92); backdrop-filter: blur(10px);
    border-bottom: 1px solid #d1fae5;
    display: flex; align-items: center; justify-content: space-between;
    padding: 0 40px; height: 64px;
}
.header-title { font-family: "Roboto", sans-serif; font-weight: 700; font-size: 1rem; color: var(--text-main); margin-left: 10px; }
.day-label { font-size: 0.85rem; color: var(--text-sub); font-family: "Roboto", sans-serif; margin-right: 12px; }
.progress-bar-wrap { width: 120px; height: 6px; background: #e2e8f0; border-radius: 3px; overflow: hidden; }
.progress-bar-fill { height: 100%; background: var(--accent-green); border-radius: 3px; }
```

---

## 4. ヒーローセクション

```html
<section class="hero">
    <div class="container">
        <div class="hero-label">DAY 1</div>
        <h1 class="hero-title">簿記の基本概念</h1>
        <p class="hero-sub">AIと簿記の未来・Gemini / NotebookLMの基礎を掴もう</p>
        <div class="goal-box">
            <h2 class="goal-title"><i class="fa-solid fa-bullseye"></i> 本日の目標</h2>
            <ul class="goal-list">
                <li>AGI・AIの進化が「簿記・経理」に与える影響を理解する</li>
                <li>Gemini と NotebookLM の基本的な使い方を体験する</li>
                <li>簿記3級の「一巡の手続き」と「財務三表」の概念を把握する</li>
            </ul>
        </div>
    </div>
</section>
```

**CSS（ヒーロー）:**
```css
.hero {
    background: linear-gradient(135deg, #059669, #34d399);
    padding: 100px 0 60px;
    text-align: center; color: #fff;
}
.hero-label {
    font-family: "Teko", sans-serif; font-size: 1.4rem; letter-spacing: 6px;
    opacity: 0.8; margin-bottom: 10px;
}
.hero-title {
    font-family: "Teko", sans-serif; font-size: 3.5rem; margin: 0 0 10px;
    letter-spacing: 2px;
}
.hero-sub { font-size: 1.05rem; opacity: 0.9; margin-bottom: 40px; }
.goal-box {
    background: rgba(255,255,255,0.15); backdrop-filter: blur(6px);
    border-radius: 16px; padding: 30px 40px; max-width: 700px;
    margin: 0 auto; text-align: left; border: 1px solid rgba(255,255,255,0.3);
}
.goal-title { font-size: 1.1rem; margin: 0 0 15px; display: flex; align-items: center; gap: 10px; }
.goal-list { margin: 0; padding-left: 20px; }
.goal-list li { margin-bottom: 10px; font-size: 1rem; line-height: 1.6; }
.goal-list li:last-child { margin-bottom: 0; }
```

---

## 5. 前半パート（動画3本 + 実習）

### セクション外枠
```html
<section class="part-section" id="part1">
    <div class="container">
        <div class="part-header">
            <div class="part-badge">前半パート</div>
            <h2 class="part-title">AIと簿記の未来・AIツールの基礎</h2>
        </div>
        <!-- 動画カード群 -->
        <!-- 実習ブロック -->
        <div class="nav-btn-wrap">
            <button class="nav-btn" onclick="document.getElementById('part2').scrollIntoView({behavior:'smooth'})">
                <i class="fa-solid fa-arrow-down"></i> 後半パートへ進む
            </button>
        </div>
    </div>
</section>
```

### 動画カード3本（Facadeパターン）

**動画①: xfwiwOtNn-I**
```html
<div class="video-card">
    <div class="vc-thumb" data-video-id="xfwiwOtNn-I" style="background-image: url('https://img.youtube.com/vi/xfwiwOtNn-I/maxresdefault.jpg'), url('https://img.youtube.com/vi/xfwiwOtNn-I/hqdefault.jpg');">
        <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
    <div class="vc-info">
        <div class="vc-num">動画 ①</div>
        <div class="vc-title">【カウントダウン】人類最後の発明「AGI（汎用人工知能）」の正体とは？</div>
        <p class="vc-desc">AGI（汎用人工知能）とは、特定のタスクだけでなく、人間と同等かそれ以上の知的な作業をすべてこなせるAIのことです。OpenAIは5つのレベルでAIの進化を定義しており、現在はレベル2（推論するAI）に到達しつつあります。将来的にレベル5に達すれば、組織の運営までAIが行うようになるとされています。簿記や経理といった業務も、AIが代行する時代が近づいており、これからの時代は「作業をAIに任せ、人間がAIをどう使いこなすか」が重要になります。</p>
    </div>
</div>
```

**動画②: PSpM8snrOz4**
```html
<div class="video-card">
    <div class="vc-thumb" data-video-id="PSpM8snrOz4" style="background-image: url('https://img.youtube.com/vi/PSpM8snrOz4/maxresdefault.jpg'), url('https://img.youtube.com/vi/PSpM8snrOz4/hqdefault.jpg');">
        <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
    <div class="vc-info">
        <div class="vc-num">動画 ②</div>
        <div class="vc-title">【驚愕】AI（人工知能）の発達により簿記は不要になるか？</div>
        <p class="vc-desc">AIが発達すれば、単純な仕訳入力や帳簿の作成は間違いなく自動化されます。しかし「簿記が不要になるか」というと、そうではありません。AIが作成した財務データの正当性を判断し、その数字をもとに経営の意思決定を行うのは人間の役割だからです。つまり、単なる作業者としての簿記スキルではなく、財務状況を読み解きプロジェクトを動かす「管理者・ディレクター」としての簿記知識が、これまで以上に求められるようになるのです。</p>
    </div>
</div>
```

**動画③: 1INqlD-Hw78**
```html
<div class="video-card">
    <div class="vc-thumb" data-video-id="1INqlD-Hw78" style="background-image: url('https://img.youtube.com/vi/1INqlD-Hw78/maxresdefault.jpg'), url('https://img.youtube.com/vi/1INqlD-Hw78/hqdefault.jpg');">
        <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
    <div class="vc-info">
        <div class="vc-num">動画 ③</div>
        <div class="vc-title">【超進化‼️】Google 「Gemini」&「NotebookLM」に神アプデ！徹底解説</div>
        <p class="vc-desc">Googleの生成AI「Gemini」と、大量の資料を読み込ませて分析できる「NotebookLM」の基礎を解説します。Geminiは対話型AIとして日常的な疑問解決やアイデア出しに優れています。一方NotebookLMは、PDFやYouTube動画のURLなどをソースとして読み込ませ、その内容に限定して正確な回答を生成させることができる強力な学習ツールです。これらを活用することで、難解な簿記の概念も飛躍的に理解しやすくなります。</p>
    </div>
</div>
```

### 前半実習ブロック（Gemini / NotebookLM 基礎）

```html
<div class="practice-block">
    <div class="practice-header">
        <i class="fa-solid fa-flask practice-icon"></i>
        <h3 class="practice-title">実習① Gemini & NotebookLM を使ってみよう</h3>
    </div>
    <div class="practice-body">
        <div class="practice-section">
            <h4><i class="fa-solid fa-lightbulb"></i> 実習の目的</h4>
            <p>Gemini と NotebookLM の2つのAIツールに実際に触れ、「AIに質問する」「資料を読み込ませて学ぶ」という体験をします。今後の簿記学習で毎日活用するツールです。</p>
        </div>
        <div class="practice-section">
            <h4><i class="fa-solid fa-circle-check"></i> 事前準備</h4>
            <ul>
                <li>Googleアカウントにログインしていることを確認する</li>
                <li>ブラウザで <strong>gemini.google.com</strong> を開く</li>
                <li>ブラウザで <strong>notebooklm.google.com</strong> を開く</li>
            </ul>
        </div>
        <div class="practice-section">
            <h4><i class="fa-solid fa-list-ol"></i> 実習の流れ</h4>
            <ol>
                <li><strong>Geminiに聞いてみよう：</strong>下のプロンプトをコピーしてGeminiに貼り付け、回答を読む</li>
                <li><strong>NotebookLMにソースを追加：</strong>今日視聴した動画①②のURLをNotebookLMのソースとして追加する</li>
                <li><strong>NotebookLMに質問：</strong>下のプロンプトを使って、動画の内容を深掘りする</li>
            </ol>
        </div>
        <div class="prompt-box">
            <div class="prompt-label"><i class="fa-solid fa-robot"></i> Gemini 用プロンプト例</div>
            <pre class="prompt-text">簿記の仕事はAIに奪われますか？将来も価値のある簿記スキルとは何か、初心者にもわかるように教えてください。</pre>
        </div>
        <div class="prompt-box">
            <div class="prompt-label"><i class="fa-solid fa-book"></i> NotebookLM 用プロンプト例</div>
            <pre class="prompt-text">読み込んだ2つの動画の内容に基づいて、「AIが得意な経理業務」と「人間でなければできない経理業務」をそれぞれ3つずつ挙げてください。</pre>
        </div>
    </div>
</div>
```

---

## 6. 後半パート（動画3本 + 実習）

### セクション外枠
```html
<section class="part-section" id="part2">
    <div class="container">
        <div class="part-header">
            <div class="part-badge part-badge--gold">後半パート</div>
            <h2 class="part-title">簿記の基本概念・AI業務効率化</h2>
        </div>
        <!-- 動画カード群 -->
        <!-- 実習ブロック -->
        <div class="nav-btn-wrap">
            <button class="nav-btn" onclick="document.getElementById('summary').scrollIntoView({behavior:'smooth'})">
                <i class="fa-solid fa-arrow-down"></i> 今日のまとめへ
            </button>
        </div>
    </div>
</section>
```

**動画④: pQrxmzrDR1c**
```html
<div class="video-card">
    <div class="vc-thumb" data-video-id="pQrxmzrDR1c" style="background-image: url('https://img.youtube.com/vi/pQrxmzrDR1c/maxresdefault.jpg'), url('https://img.youtube.com/vi/pQrxmzrDR1c/hqdefault.jpg');">
        <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
    <div class="vc-info">
        <div class="vc-num">動画 ④</div>
        <div class="vc-title">【簿記3級】簿記の目的と全体の流れを15分で解説</div>
        <p class="vc-desc">簿記の最終目的は、企業に「いくら財産があるか（貸借対照表）」と「いくら儲かったか（損益計算書）」を明らかにし、利害関係者に報告することです。日々の取引をルールに従って「仕訳」し、「総勘定元帳」に転記します。そして月末や期末に「試算表」を作成し、最終的に「決算」を行って財務諸表を作ります。この一連の「簿記の一巡の手続き」をイメージすることが、簿記学習の第一歩となります。</p>
    </div>
</div>
```

**動画⑤: ydenauRwjS0**
```html
<div class="video-card">
    <div class="vc-thumb" data-video-id="ydenauRwjS0" style="background-image: url('https://img.youtube.com/vi/ydenauRwjS0/maxresdefault.jpg'), url('https://img.youtube.com/vi/ydenauRwjS0/hqdefault.jpg');">
        <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
    <div class="vc-info">
        <div class="vc-num">動画 ⑤</div>
        <div class="vc-title">【簿記3級】簿記シリーズ開始！財務三表の超基礎</div>
        <p class="vc-desc">財務三表とは、貸借対照表（B/S）、損益計算書（P/L）、キャッシュフロー計算書（C/F）のことです。簿記3級では主にB/SとP/Lを学びます。B/Sは「ある時点の財産状態（資産・負債・純資産）」を示し、P/Lは「ある期間の経営成績（収益・費用・利益）」を示します。この2つの表の構造と繋がりを理解することで、会社の健康状態を読み取ることができるようになります。</p>
    </div>
</div>
```

**動画⑥: ohBnhlh5vMY**
```html
<div class="video-card">
    <div class="vc-thumb" data-video-id="ohBnhlh5vMY" style="background-image: url('https://img.youtube.com/vi/ohBnhlh5vMY/maxresdefault.jpg'), url('https://img.youtube.com/vi/ohBnhlh5vMY/hqdefault.jpg');">
        <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
    <div class="vc-info">
        <div class="vc-num">動画 ⑥</div>
        <div class="vc-title">【AI業務効率化】まだ手で入力？スマホで領収書を撮るだけで会計ソフトへの入力を自動化する裏ワザ</div>
        <p class="vc-desc">実務におけるAI活用例として、領収書の自動入力があります。スマホで領収書の写真を撮るだけで、AIが日付、金額、取引先を自動で読み取り、会計ソフトに仕訳データとして取り込みます。手入力の手間が省けるだけでなく、入力ミスも防ぐことができます。簿記の知識があれば、AIが自動生成した仕訳が正しいかを一瞬でチェックするだけで、煩雑な経理業務が完了します。</p>
    </div>
</div>
```

### 後半実習ブロック（NotebookLMでテスト生成）

```html
<div class="practice-block practice-block--gold">
    <div class="practice-header">
        <i class="fa-solid fa-pen-to-square practice-icon practice-icon--gold"></i>
        <h3 class="practice-title">実習② NotebookLMで簿記テストを作ろう</h3>
    </div>
    <div class="practice-body">
        <div class="practice-section">
            <h4><i class="fa-solid fa-lightbulb"></i> 実習の目的</h4>
            <p>NotebookLMに今日視聴した簿記動画を読み込ませ、AIに確認テストを生成してもらいます。「自分でテスト問題を作らせる」というAI学習法の基本を体験します。</p>
        </div>
        <div class="practice-section">
            <h4><i class="fa-solid fa-circle-check"></i> 事前準備</h4>
            <ul>
                <li>NotebookLM に今日の簿記動画（④⑤）のURLをソースとして追加する</li>
                <li>動画④: <code>https://www.youtube.com/watch?v=pQrxmzrDR1c</code></li>
                <li>動画⑤: <code>https://www.youtube.com/watch?v=ydenauRwjS0</code></li>
            </ul>
        </div>
        <div class="practice-section">
            <h4><i class="fa-solid fa-list-ol"></i> 実習の流れ</h4>
            <ol>
                <li><strong>テスト生成：</strong>下のプロンプトをNotebookLMに送り、確認問題を作ってもらう</li>
                <li><strong>自己採点：</strong>問題を見て、自分で答えを考えてみる（まず自力で！）</li>
                <li><strong>答え合わせ：</strong>NotebookLMに「答えと解説を教えて」と聞く</li>
            </ol>
        </div>
        <div class="prompt-box prompt-box--gold">
            <div class="prompt-label"><i class="fa-solid fa-book"></i> NotebookLM 用プロンプト例</div>
            <pre class="prompt-text">読み込んだ動画の内容を元に、簿記3級の確認テスト問題を5問作ってください。選択肢ありの4択形式でお願いします。答えはすぐに表示せず、問題だけ先に出してください。</pre>
        </div>
    </div>
</div>
```

---

## 7. まとめセクション

```html
<section class="summary-section" id="summary">
    <div class="container">
        <h2 class="summary-title"><i class="fa-solid fa-flag-checkered"></i> 今日のまとめ</h2>
        <div class="summary-grid">
            <div class="summary-card">
                <div class="summary-icon"><i class="fa-solid fa-robot"></i></div>
                <h3>AIと簿記の関係</h3>
                <p>単純な仕訳作業はAIに代替されていく。しかし財務データを読み解き意思決定する「管理者」としての簿記知識は、これまで以上に価値を持つ。</p>
            </div>
            <div class="summary-card">
                <div class="summary-icon"><i class="fa-solid fa-wand-magic-sparkles"></i></div>
                <h3>Gemini & NotebookLM</h3>
                <p>Geminiは対話型AI、NotebookLMは資料ベースの学習ツール。今後の簿記学習でフル活用する。動画URLを読み込ませてテスト生成まで体験した。</p>
            </div>
            <div class="summary-card">
                <div class="summary-icon"><i class="fa-solid fa-scale-balanced"></i></div>
                <h3>簿記の一巡の手続き</h3>
                <p>取引 → 仕訳 → 総勘定元帳 → 試算表 → 決算 → 財務諸表。この流れが簿記学習の骨格。B/SとP/Lの役割を押さえておくことが最優先。</p>
            </div>
            <div class="summary-card">
                <div class="summary-icon"><i class="fa-solid fa-camera"></i></div>
                <h3>AIによる業務効率化</h3>
                <p>領収書の写真1枚でAIが仕訳データを生成。簿記知識がある人間がその正確性を秒でチェックする——それが「AI時代の経理スキル」の形。</p>
            </div>
        </div>
        <div class="nav-btn-wrap">
            <a href="index.html" class="nav-btn nav-btn--home">
                <i class="fa-solid fa-house"></i> ホーム（カレンダー）へ戻る
            </a>
        </div>
    </div>
</section>
```

---

## 8. 動画カード・実習ブロック 共通CSS

```css
/* === コンテナ === */
.container { max-width: 900px; margin: 0 auto; padding: 0 30px; }

/* === パートセクション === */
.part-section { padding: 80px 0; background: var(--bg-body); }
.part-section:nth-child(even) { background: #f8fafc; }

.part-header { margin-bottom: 40px; }
.part-badge {
    display: inline-block; background: var(--accent-green); color: #fff;
    font-size: 0.8rem; font-weight: 700; letter-spacing: 2px;
    padding: 4px 14px; border-radius: 20px; margin-bottom: 12px;
}
.part-badge--gold { background: var(--accent-gold); }
.part-title { font-size: 1.6rem; margin: 0; color: var(--text-main); font-family: "Noto Sans JP", sans-serif; font-weight: 700; }

/* === 動画カード === */
.video-card {
    background: var(--bg-card); border-radius: 16px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.07);
    overflow: hidden; margin-bottom: 30px;
    display: grid; grid-template-columns: 1fr 1.2fr;
}
.vc-thumb {
    width: 100%; aspect-ratio: 16/9;
    background-color: #000; background-size: cover; background-position: center;
    position: relative; cursor: pointer;
}
.vc-thumb::before {
    content: ''; position: absolute; inset: 0;
    background: rgba(0,0,0,0.1); transition: background 0.3s;
    pointer-events: none;
}
.vc-thumb:hover::before { background: rgba(0,0,0,0.25); }
.vc-thumb-play {
    position: absolute; top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    font-size: 3rem; color: rgba(255,255,255,0.9);
    text-shadow: 0 2px 10px rgba(0,0,0,0.4);
    transition: transform 0.3s, color 0.3s; pointer-events: none;
}
.vc-thumb:hover .vc-thumb-play { transform: translate(-50%, -50%) scale(1.15); color: #ff0000; }
.vc-info { padding: 24px 28px; }
.vc-num { font-size: 0.75rem; color: var(--accent-green); font-weight: 700; letter-spacing: 1px; margin-bottom: 6px; }
.vc-title { font-size: 1rem; font-weight: 700; color: var(--text-main); margin-bottom: 12px; line-height: 1.5; }
.vc-desc { font-size: 0.88rem; color: var(--text-sub); line-height: 1.75; margin: 0; }

/* === 実習ブロック === */
.practice-block {
    background: #ecfdf5; border-left: 4px solid var(--accent-green);
    border-radius: 12px; padding: 32px; margin: 40px 0;
}
.practice-block--gold { background: #fffbeb; border-left-color: var(--accent-gold); }
.practice-header { display: flex; align-items: center; gap: 14px; margin-bottom: 24px; }
.practice-icon { font-size: 1.8rem; color: var(--accent-green); }
.practice-icon--gold { color: var(--accent-gold); }
.practice-title { font-size: 1.15rem; font-weight: 700; margin: 0; color: var(--text-main); }
.practice-body { display: flex; flex-direction: column; gap: 20px; }
.practice-section h4 { font-size: 0.95rem; font-weight: 700; color: var(--text-main); margin: 0 0 10px; display: flex; align-items: center; gap: 8px; }
.practice-section p, .practice-section ul, .practice-section ol { font-size: 0.9rem; color: var(--text-sub); line-height: 1.75; margin: 0; }
.practice-section ul, .practice-section ol { padding-left: 20px; }
.practice-section li { margin-bottom: 6px; }

/* === プロンプトボックス === */
.prompt-box {
    background: #1e293b; border-radius: 10px; overflow: hidden;
    margin-top: 5px;
}
.prompt-box--gold .prompt-label { background: var(--accent-gold); }
.prompt-label {
    background: var(--accent-green); color: #fff;
    font-size: 0.8rem; font-weight: 700; padding: 8px 16px;
    display: flex; align-items: center; gap: 8px;
}
.prompt-text {
    color: #e2e8f0; font-size: 0.88rem; line-height: 1.75;
    padding: 16px 20px; margin: 0;
    white-space: pre-wrap; word-break: break-all;
    font-family: "Noto Sans JP", sans-serif;
}

/* === ナビゲーションボタン === */
.nav-btn-wrap { text-align: center; margin-top: 50px; }
.nav-btn {
    display: inline-flex; align-items: center; gap: 10px;
    background: var(--accent-green); color: #fff;
    border: none; border-radius: 50px; padding: 16px 40px;
    font-size: 1rem; font-weight: 700; cursor: pointer;
    font-family: "Noto Sans JP", sans-serif;
    transition: background 0.2s, transform 0.2s; text-decoration: none;
}
.nav-btn:hover { background: #047857; transform: translateY(-2px); }
.nav-btn--home { background: #334155; }
.nav-btn--home:hover { background: #1e293b; }

/* === まとめセクション === */
.summary-section { padding: 80px 0; background: linear-gradient(135deg, #1e293b, #334155); }
.summary-title {
    text-align: center; color: #fff; font-size: 2rem;
    margin-bottom: 40px; display: flex; align-items: center; justify-content: center; gap: 14px;
}
.summary-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 50px; }
.summary-card {
    background: rgba(255,255,255,0.08); border: 1px solid rgba(255,255,255,0.12);
    border-radius: 16px; padding: 28px;
}
.summary-icon { font-size: 2rem; color: #34d399; margin-bottom: 14px; }
.summary-card h3 { color: #f0fdf4; font-size: 1rem; margin: 0 0 10px; }
.summary-card p { color: #94a3b8; font-size: 0.88rem; line-height: 1.75; margin: 0; }

/* === フッター === */
footer { text-align: center; padding: 24px; background: #1e293b; color: #64748b; font-size: 0.8rem; }
```

---

## 9. JavaScript（YouTube Facade）

```javascript
document.querySelectorAll('.vc-thumb[data-video-id]').forEach(thumb => {
    thumb.addEventListener('click', function() {
        const videoId = this.dataset.videoId;
        this.innerHTML = `<iframe src="https://www.youtube.com/embed/${videoId}?autoplay=1" allow="autoplay; encrypted-media" allowfullscreen style="width:100%;height:100%;border:none;"></iframe>`;
        this.style.backgroundImage = 'none';
        this.style.cursor = 'default';
        this.onclick = null;
    }, { once: true });
});
```

---

## 10. モバイル向けメディアクエリ（CSS末尾に必ず追加）

```css
/* === レスポンシブ対応（モバイル最適化） === */
@media (max-width: 768px) {
    .container { padding: 0 15px; }
    .fixed-header { padding: 0 15px; }
    .progress-bar-wrap { width: 80px; }
    .hero { padding: 90px 0 50px; }
    .hero-title { font-size: 2.2rem; }
    .goal-box { padding: 20px; }
    .part-section { padding: 50px 0; }
    .part-title { font-size: 1.3rem; }
    .video-card { grid-template-columns: 1fr; }
    .vc-info { padding: 16px 20px; }
    .practice-block { padding: 20px; }
    .summary-grid { grid-template-columns: 1fr; }
    .summary-section { padding: 50px 0; }
    .nav-btn { padding: 14px 28px; font-size: 0.95rem; }
}
```

---

## 11. index.html への Day 1 エントリ追加

`index.html` の `.calendar-grid` 内、5月7日（木）に対応するセルを以下のように更新する（既存の `.cal-cell` を `.has-event` に変更し、ポップアップ・リンクを追加）：

```html
<div class="cal-cell has-event" onclick="location.href='vol01-1.html'">
    <span class="date-num">7</span>
    <span class="ev-pill">Day 1</span>
    <div class="cal-popup">Day 1<br>簿記の基本概念</div>
</div>
```

また、右側の `.curriculum-list` に Day 1 のエントリを追加する：

```html
<div class="c-item" onclick="location.href='vol01-1.html'">
    <div class="c-date">5/7 (Thu)</div>
    <div class="c-content">
        <div class="c-title">Day 1 — 簿記の基本概念</div>
        <div class="c-tags">
            <span class="tag">AGI・AIの未来</span>
            <span class="tag">Gemini</span>
            <span class="tag">NotebookLM</span>
            <span class="tag">財務三表</span>
        </div>
    </div>
    <i class="fa-solid fa-chevron-right c-arrow"></i>
</div>
```

**`.c-item` / `.c-date` / `.c-title` / `.c-tags` / `.tag` のCSSは index.html の既存スタイルに揃える。**

---

## 12. Cursor への作業指示まとめ

| # | タスク | 対象ファイル |
|---|---|---|
| 1 | 新規作成: `vol01-1.html`（セクション3〜10の内容で完全実装） | vol01-1.html（新規） |
| 2 | 5月7日カレンダーセルを `has-event` に更新し Day 1 リンクを追加 | index.html |
| 3 | カリキュラムリストに Day 1 エントリを追加 | index.html |

**完成後のチェックリスト:**
- [ ] 6本の動画サムネイルがすべて表示されるか
- [ ] サムネイルをクリックすると動画がインライン再生されるか（別タブ遷移しないこと）
- [ ] 「後半パートへ進む」「今日のまとめへ」「ホームへ戻る」ボタンが正常に動作するか
- [ ] スマホ表示で動画カードが縦1列になり、余白が適切か
- [ ] index.html の 5/7 セルをクリックすると vol01-1.html に遷移するか
