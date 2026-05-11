# 実装計画 v2：Day 1 ページ 知識吸収完結化アップデート

> **対象ファイル:** `vol01-1.html`（既存ファイルの追記・修正のみ。新規ファイル不要）
> **方針:** 動画を一切視聴しなくても、このページを読むだけで全6本分の知識が吸収・復習・確認できる状態にする。
> **作業:** Cursor が本ファイルを読んで `vol01-1.html` に反映する。

---

## 追加要素一覧（優先度順）

| # | 要素 | 挿入場所 | 優先度 |
|---|---|---|---|
| A | AGI 5段階レベル表 | 動画①カードの `lc-body` 末尾 | P1 必須 |
| B | 「AIに代替されない5つの理由」箇条書き | 動画②カードの `lc-body`（Instructor's Note内） | P1 必須 |
| C | 簿記の一巡フロー図（CSS矢印図） | 動画④カードの `lc-body` 末尾 | P1 必須 |
| D | B/S・P/L T字構造ビジュアルカード | 動画⑤カードの `lc-body` 末尾 | P1 必須 |
| E | サイト内4択クイズ（JS・5問） | まとめタブの `check-panel` 直後 | P1 必須 |
| F | 用語解説の拡充（6語→12語） | まとめタブの `term-grid` を差し替え | P2 重要 |
| G | NotebookLM ソース追加手順 | 前半タブ 実習セクションの `practice-area` 先頭 | P2 重要 |

---

## A. AGI 5段階レベル表

**挿入場所:** 動画①カードの `<ul class="lc-list">` の直後（`</div><!-- lc-body end -->` の手前）

```html
<!-- AGI 5段階レベル表 -->
<div class="level-table" style="margin-top: 16px;">
    <div class="level-row level-current">
        <div class="level-badge">Lv.1</div>
        <div class="level-info">
            <strong>チャットボット</strong>
            <span>自然な会話ができる。単語予測ベース。（例: 初期のChatGPT）</span>
        </div>
    </div>
    <div class="level-row level-current">
        <div class="level-badge">Lv.2</div>
        <div class="level-info">
            <strong>推論AI <span class="now-badge">← 現在</span></strong>
            <span>複雑な論理パズルや数学的推論が可能。（例: o1, o3）</span>
        </div>
    </div>
    <div class="level-row">
        <div class="level-badge lv-future">Lv.3</div>
        <div class="level-info">
            <strong>エージェント</strong>
            <span>人間の代わりに数日間のタスクを自律実行する。メール送信・予約も行動する。</span>
        </div>
    </div>
    <div class="level-row">
        <div class="level-badge lv-future">Lv.4</div>
        <div class="level-info">
            <strong>イノベーター</strong>
            <span>既存知識の組み合わせではなく、新しい科学的知識・技術を自ら「発明」する。</span>
        </div>
    </div>
    <div class="level-row">
        <div class="level-badge lv-future">Lv.5</div>
        <div class="level-info">
            <strong>組織（AGI完成形）</strong>
            <span>経営判断・人事・部門調整まで組織運営全体を全自動で行う。2030年以降。</span>
        </div>
    </div>
</div>
```

**追加CSS（`<style>` 内末尾に追加）:**
```css
/* === AGI レベル表 === */
.level-table { display: flex; flex-direction: column; gap: 6px; }
.level-row {
    display: flex; align-items: center; gap: 12px;
    background: #f8fafc; border-radius: 10px; padding: 10px 14px;
    border: 1px solid #e2e8f0;
}
.level-row.level-current { background: #ecfdf5; border-color: rgba(5,150,105,0.3); }
.level-badge {
    flex-shrink: 0; width: 44px; height: 44px; border-radius: 50%;
    background: var(--accent-green); color: #fff;
    font-family: 'Roboto', sans-serif; font-weight: 700; font-size: 0.8rem;
    display: flex; align-items: center; justify-content: center;
}
.level-badge.lv-future { background: #94a3b8; }
.level-info { display: flex; flex-direction: column; gap: 2px; }
.level-info strong { font-size: 0.92rem; color: var(--text-main); display: flex; align-items: center; gap: 8px; }
.level-info span { font-size: 0.82rem; color: var(--text-sub); line-height: 1.5; }
.now-badge {
    background: var(--accent-gold); color: #fff;
    font-size: 0.65rem; padding: 1px 7px; border-radius: 10px; font-weight: 700;
}
@media (max-width: 768px) { .level-row { flex-direction: row; } .level-badge { width: 36px; height: 36px; font-size: 0.72rem; } }
```

---

## B. 「AIに代替されない5つの理由」箇条書き

**挿入場所:** 動画②の `<div class="instructor-note" ...>` 内、`</div>` 閉じタグの手前

現在の Instructor's Note の末尾 `</div>` 直前に以下を追加：

```html
<p style="margin-bottom: 8px; font-weight: 700; color: #92400e;">📌 AIに代替されない5つの理由</p>
<ol style="margin: 0; padding-left: 1.4rem; color: #78350f; font-size: 0.95rem; line-height: 1.8;">
    <li><strong>柔軟な判断力</strong> — 取引の背景・会社の実情に応じた個別判断はAIに苦手</li>
    <li><strong>法律・会計基準への対応</strong> — 頻繁に改正される税法・会計基準の微妙なニュアンスを人間が判断</li>
    <li><strong>経営者とのコミュニケーション</strong> — 数字をもとに状況に応じたアドバイスを提供するのは人間の役割</li>
    <li><strong>データの正確性チェック</strong> — AIの処理ミス・エラーを検知し、財務データの整合性を保つ監視役</li>
    <li><strong>倫理的・社会的判断</strong> — 不正会計の発見、架空伝票への疑念など「常識的おかしい」を察知する力</li>
</ol>
```

---

## C. 簿記の一巡フロー図

**挿入場所:** 動画④カードの `<ul class="lc-list">` の直後

```html
<!-- 簿記の一巡フロー図 -->
<div class="boki-flow" style="margin-top: 16px;">
    <div class="boki-flow-inner">
        <div class="bf-step"><i class="fa-solid fa-handshake"></i><span>取引</span></div>
        <div class="bf-arrow"><i class="fa-solid fa-chevron-right"></i></div>
        <div class="bf-step"><i class="fa-solid fa-pen"></i><span>仕訳</span></div>
        <div class="bf-arrow"><i class="fa-solid fa-chevron-right"></i></div>
        <div class="bf-step"><i class="fa-solid fa-book"></i><span>総勘定元帳</span></div>
        <div class="bf-arrow"><i class="fa-solid fa-chevron-right"></i></div>
        <div class="bf-step"><i class="fa-solid fa-table"></i><span>試算表</span></div>
        <div class="bf-arrow"><i class="fa-solid fa-chevron-right"></i></div>
        <div class="bf-step"><i class="fa-solid fa-calculator"></i><span>決算</span></div>
        <div class="bf-arrow"><i class="fa-solid fa-chevron-right"></i></div>
        <div class="bf-step bf-step--goal"><i class="fa-solid fa-chart-pie"></i><span>財務諸表</span></div>
    </div>
    <p style="text-align:center; font-size:0.78rem; color:var(--text-sub); margin: 8px 0 0;">
        ↑ この流れが「簿記の一巡の手続き」。Day2以降で各ステップを順番に学ぶ。
    </p>
</div>
```

**追加CSS:**
```css
/* === 簿記一巡フロー図 === */
.boki-flow { background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 12px; padding: 16px; }
.boki-flow-inner { display: flex; align-items: center; justify-content: center; flex-wrap: wrap; gap: 4px; }
.bf-step {
    display: flex; flex-direction: column; align-items: center; gap: 4px;
    background: #fff; border: 1px solid #e2e8f0; border-radius: 10px;
    padding: 10px 12px; min-width: 60px; text-align: center;
}
.bf-step i { font-size: 1.1rem; color: var(--accent-green); }
.bf-step span { font-size: 0.75rem; font-weight: 700; color: var(--text-main); white-space: nowrap; }
.bf-step--goal { background: var(--accent-light); border-color: var(--accent-green); }
.bf-step--goal i, .bf-step--goal span { color: var(--accent-green); }
.bf-arrow { color: #cbd5e1; font-size: 0.8rem; }
@media (max-width: 768px) {
    .boki-flow-inner { gap: 3px; }
    .bf-step { padding: 8px; min-width: 50px; }
    .bf-step span { font-size: 0.65rem; }
}
```

---

## D. B/S・P/L T字構造ビジュアルカード

**挿入場所:** 動画⑤カードの `<ul class="lc-list">` の直後

```html
<!-- B/S・P/L 構造ビジュアル -->
<div class="fs-cards" style="margin-top: 16px; display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
    <!-- B/S -->
    <div class="fs-card">
        <div class="fs-card-title"><i class="fa-solid fa-landmark"></i> 貸借対照表（B/S）<small>ある時点の財産状態</small></div>
        <div class="fs-tshape">
            <div class="fs-left">
                <div class="fs-cell fs-asset">資産<small>（持っているもの）</small></div>
            </div>
            <div class="fs-right">
                <div class="fs-cell fs-liability">負債<small>（借りているもの）</small></div>
                <div class="fs-cell fs-equity">純資産<small>（自己資本）</small></div>
            </div>
        </div>
    </div>
    <!-- P/L -->
    <div class="fs-card">
        <div class="fs-card-title"><i class="fa-solid fa-chart-line"></i> 損益計算書（P/L）<small>ある期間の経営成績</small></div>
        <div class="fs-tshape">
            <div class="fs-left">
                <div class="fs-cell fs-expense">費用<small>（コスト）</small></div>
                <div class="fs-cell fs-profit">当期純利益</div>
            </div>
            <div class="fs-right">
                <div class="fs-cell fs-revenue">収益<small>（売上など）</small></div>
            </div>
        </div>
    </div>
</div>
<p style="font-size:0.78rem; color:var(--text-sub); margin: 8px 0 0; text-align:center;">
    ※ 簿記3級で扱うのは主にB/SとP/L。3つ目のC/F（キャッシュフロー計算書）は1〜2級の範囲。
</p>
```

**追加CSS:**
```css
/* === B/S・P/L 構造カード === */
.fs-card { background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 12px; overflow: hidden; }
.fs-card-title {
    background: #1e293b; color: #fff; font-size: 0.8rem; font-weight: 700;
    padding: 8px 12px; display: flex; flex-direction: column; gap: 2px;
}
.fs-card-title small { font-weight: 400; color: #94a3b8; font-size: 0.72rem; }
.fs-tshape { display: flex; border-top: 2px solid #334155; }
.fs-left, .fs-right { flex: 1; display: flex; flex-direction: column; }
.fs-left { border-right: 2px solid #334155; }
.fs-cell {
    flex: 1; padding: 10px 8px; font-size: 0.78rem; font-weight: 700;
    display: flex; flex-direction: column; align-items: center; justify-content: center;
    text-align: center; gap: 2px;
}
.fs-cell small { font-size: 0.65rem; font-weight: 400; color: var(--text-sub); }
.fs-cell + .fs-cell { border-top: 1px solid #e2e8f0; }
.fs-asset { color: #1d4ed8; background: #eff6ff; }
.fs-liability { color: #b91c1c; background: #fef2f2; }
.fs-equity { color: #15803d; background: #f0fdf4; }
.fs-expense { color: #b45309; background: #fffbeb; }
.fs-revenue { color: #1d4ed8; background: #eff6ff; }
.fs-profit { color: #15803d; background: #f0fdf4; font-size: 0.72rem; }
@media (max-width: 768px) { .fs-cards { grid-template-columns: 1fr; } }
```

---

## E. サイト内4択クイズ（JS・5問）

**挿入場所:** まとめタブの `check-panel` 閉じタグ `</div>` の直後（`next-work` の前）

**HTML:**
```html
<!-- サイト内4択クイズ -->
<div class="quiz-panel" id="quizPanel">
    <h3><i class="fa-solid fa-circle-question"></i> 確認クイズ（全5問）</h3>
    <p style="color:var(--text-sub); font-size:0.9rem; margin-bottom:20px;">動画を見た人も見ていない人も、理解度を確認してみましょう。</p>

    <div id="quiz-container"></div>

    <div id="quiz-result" style="display:none; margin-top:24px; padding:20px; border-radius:12px; text-align:center;">
        <p id="quiz-score" style="font-size:1.6rem; font-weight:900; margin-bottom:8px;"></p>
        <p id="quiz-msg" style="font-size:0.95rem; color:var(--text-sub);"></p>
        <button onclick="resetQuiz()" class="tool-link-btn" style="margin-top:16px; background:var(--accent-green);">
            <i class="fa-solid fa-rotate-right"></i> もう一度
        </button>
    </div>
</div>
```

**追加CSS（`<style>` 内末尾に追加）:**
```css
/* === 確認クイズ === */
.quiz-panel {
    background: #fafafa; border: 1px solid #e2e8f0; border-radius: var(--radius-medium);
    padding: 28px; margin: 28px 0;
}
.quiz-q {
    background: #fff; border: 1px solid #e2e8f0; border-radius: 12px;
    padding: 20px; margin-bottom: 20px;
}
.quiz-q-text { font-size: 1rem; font-weight: 700; color: var(--text-main); margin-bottom: 14px; }
.quiz-q-num { font-size: 0.78rem; color: var(--accent-gold); font-weight: 700; margin-bottom: 6px; }
.quiz-options { display: flex; flex-direction: column; gap: 8px; }
.quiz-opt {
    display: flex; align-items: center; gap: 12px;
    background: #f8fafc; border: 2px solid #e2e8f0; border-radius: 10px;
    padding: 12px 16px; cursor: pointer; transition: 0.2s;
    font-size: 0.95rem; color: var(--text-main); text-align: left;
}
.quiz-opt:hover { background: var(--accent-light); border-color: var(--accent-green); }
.quiz-opt.correct { background: #ecfdf5; border-color: #059669; color: #065f46; }
.quiz-opt.wrong { background: #fef2f2; border-color: #ef4444; color: #991b1b; }
.quiz-opt.disabled { cursor: default; }
.quiz-opt-icon { width: 24px; height: 24px; border-radius: 50%; border: 2px solid #cbd5e1; display: flex; align-items: center; justify-content: center; font-size: 0.75rem; font-weight: 700; flex-shrink: 0; }
.quiz-feedback { margin-top: 12px; padding: 10px 14px; border-radius: 8px; font-size: 0.88rem; line-height: 1.6; display: none; }
.quiz-feedback.show { display: block; }
.quiz-feedback.correct-fb { background: #ecfdf5; color: #065f46; border-left: 4px solid #059669; }
.quiz-feedback.wrong-fb { background: #fef2f2; color: #991b1b; border-left: 4px solid #ef4444; }
#quiz-result.pass { background: #ecfdf5; border: 2px solid #059669; }
#quiz-result.fail { background: #fff7ed; border: 2px solid #d97706; }
```

**追加JavaScript（既存の `<script>` タグ内末尾に追加）:**
```javascript
// ===== 確認クイズ =====
const quizData = [
    {
        q: "OpenAIが定義するAI進化の「5段階」のうち、2026年現在 最も近いと言われているレベルはどれ？",
        opts: ["Lv.1 チャットボット", "Lv.2 推論AI", "Lv.3 エージェント", "Lv.5 組織"],
        ans: 1,
        fb: "正解は「Lv.2 推論AI」。o1・o3などがこのレベルに該当します。Lv.3（エージェント）への移行期とも言われています。"
    },
    {
        q: "「AIが発達しても簿記が不要にならない理由」として、動画で挙げられて**いない**ものはどれ？",
        opts: ["業務ごとの柔軟な判断力", "法律・会計基準の変更への対応", "AIより計算が速い能力", "経営者とのコミュニケーション"],
        ans: 2,
        fb: "「AIより計算が速い能力」は不要になる理由です。正しい理由は①柔軟な判断 ②法律対応 ③コミュニケーション ④データ検証 ⑤倫理的判断の5つです。"
    },
    {
        q: "簿記の「一巡の手続き」の正しい順番はどれ？",
        opts: [
            "仕訳 → 取引 → 試算表 → 元帳 → 決算 → 財務諸表",
            "取引 → 仕訳 → 総勘定元帳 → 試算表 → 決算 → 財務諸表",
            "取引 → 試算表 → 仕訳 → 元帳 → 財務諸表 → 決算",
            "仕訳 → 元帳 → 取引 → 決算 → 試算表 → 財務諸表"
        ],
        ans: 1,
        fb: "取引が発生 → 仕訳で記録 → 総勘定元帳へ転記 → 試算表で確認 → 決算 → 財務諸表の完成、が正しい流れです。"
    },
    {
        q: "貸借対照表（B/S）と損益計算書（P/L）の違いとして正しいのはどれ？",
        opts: [
            "B/Sはある期間の成績、P/Lはある時点の財産状態を示す",
            "B/Sはある時点の財産状態、P/Lはある期間の経営成績を示す",
            "B/SもP/Lも同じ期間の情報をそれぞれ別の角度で示す",
            "B/Sは収益・費用、P/Lは資産・負債を示す"
        ],
        ans: 1,
        fb: "B/S（貸借対照表）＝財産の「静止画」（ある時点）、P/L（損益計算書）＝経営の「動画」（ある期間）と覚えましょう。"
    },
    {
        q: "GeminiとNotebookLMの使い分けとして最も適切なのはどれ？",
        opts: [
            "Geminiは計算専用、NotebookLMは文章作成専用",
            "どちらも同じ用途で使えるため使い分け不要",
            "Geminiは汎用対話AI、NotebookLMは読み込んだ資料に限定した正確な回答ができる",
            "NotebookLMはインターネット全体を検索できる万能ツール"
        ],
        ans: 2,
        fb: "Geminiは幅広い用途の対話型AI。NotebookLMは自分がソースとして追加した資料（PDFや動画URLなど）に限定して回答するため、ハルシネーションを抑えて学習に使えます。"
    }
];

let answered = [];

function buildQuiz() {
    const container = document.getElementById('quiz-container');
    container.innerHTML = '';
    answered = new Array(quizData.length).fill(false);
    quizData.forEach((q, qi) => {
        const letters = ['A', 'B', 'C', 'D'];
        const optsHtml = q.opts.map((o, oi) => `
            <button class="quiz-opt" onclick="answerQuiz(${qi},${oi})" id="opt-${qi}-${oi}">
                <span class="quiz-opt-icon">${letters[oi]}</span>${o}
            </button>`).join('');
        container.innerHTML += `
            <div class="quiz-q" id="qq-${qi}">
                <div class="quiz-q-num">問${qi+1} / ${quizData.length}</div>
                <div class="quiz-q-text">${q.q}</div>
                <div class="quiz-options">${optsHtml}</div>
                <div class="quiz-feedback" id="fb-${qi}"></div>
            </div>`;
    });
    document.getElementById('quiz-result').style.display = 'none';
}

function answerQuiz(qi, oi) {
    if (answered[qi]) return;
    answered[qi] = true;
    const q = quizData[qi];
    const isCorrect = oi === q.ans;
    // 全選択肢を無効化・色付け
    q.opts.forEach((_, i) => {
        const btn = document.getElementById(`opt-${qi}-${i}`);
        btn.classList.add('disabled');
        if (i === q.ans) btn.classList.add('correct');
        else if (i === oi && !isCorrect) btn.classList.add('wrong');
    });
    // フィードバック表示
    const fb = document.getElementById(`fb-${qi}`);
    fb.textContent = (isCorrect ? '✅ 正解！ ' : '❌ 不正解。') + q.fb;
    fb.className = `quiz-feedback show ${isCorrect ? 'correct-fb' : 'wrong-fb'}`;
    // 全問回答済みか確認
    if (answered.every(Boolean)) {
        const correctCount = quizData.reduce((acc, q, i) => {
            const opts = document.querySelectorAll(`#qq-${i} .quiz-opt`);
            return acc + (Array.from(opts).findIndex(o => o.classList.contains('correct') && !o.classList.contains('wrong')) === q.ans ? 1 : 0);
        }, 0);
        showResult(correctCount);
    }
}

function showResult(score) {
    const result = document.getElementById('quiz-result');
    const isPassed = score >= 4;
    result.style.display = 'block';
    result.className = isPassed ? 'pass' : 'fail';
    document.getElementById('quiz-score').textContent = `${score} / ${quizData.length} 正解`;
    document.getElementById('quiz-msg').textContent = isPassed
        ? '素晴らしい！Day1の内容をしっかり理解できています。Day2へ進みましょう。'
        : 'もう一度ページを読み直してから再挑戦しましょう。NotebookLMに解説を頼むのもOKです。';
    result.scrollIntoView({ behavior: 'smooth', block: 'center' });
}

function resetQuiz() { buildQuiz(); document.getElementById('quizPanel').scrollIntoView({ behavior: 'smooth' }); }

// 初期化
buildQuiz();
```

---

## F. 用語解説の拡充（6語→12語）

**対象:** まとめタブの `<div class="term-grid">` 全体を以下で差し替える。

```html
<h2>今日覚える最重要用語</h2>
<div class="term-grid" style="grid-template-columns: repeat(3, minmax(0, 1fr));">
    <div class="term-card">
        <strong>取引</strong>
        <span>会社のお金や財産が動く出来事。仕訳の出発点です。</span>
    </div>
    <div class="term-card">
        <strong>仕訳</strong>
        <span>取引を「借方（左）・貸方（右）」に分けて記録する簿記の基本動作。</span>
    </div>
    <div class="term-card">
        <strong>総勘定元帳</strong>
        <span>仕訳を勘定科目ごとに集計する帳簿。すべての取引の集大成です。</span>
    </div>
    <div class="term-card">
        <strong>試算表</strong>
        <span>記録の合計が左右一致するか確認する表。ズレがあれば記入ミスのサイン。</span>
    </div>
    <div class="term-card">
        <strong>B/S（貸借対照表）</strong>
        <span>ある時点の財産状態。左に資産、右に負債＋純資産が並ぶ。</span>
    </div>
    <div class="term-card">
        <strong>P/L（損益計算書）</strong>
        <span>ある期間の経営成績。収益－費用＝利益の構造を示す。</span>
    </div>
    <div class="term-card">
        <strong>AGI（汎用人工知能）</strong>
        <span>人間があらゆる知的作業をこなせるAI。特化型AI（ANI）と対比される。</span>
    </div>
    <div class="term-card">
        <strong>借方・貸方</strong>
        <span>仕訳の左側が「借方」、右側が「貸方」。どの勘定科目がどちらに来るかがルール。</span>
    </div>
    <div class="term-card">
        <strong>勘定科目</strong>
        <span>取引の内容を分類するラベル名。現金、売上、仕入れ、給料など多数存在する。</span>
    </div>
    <div class="term-card">
        <strong>C/F（キャッシュフロー計算書）</strong>
        <span>財務三表の3つ目。実際の現金の動きを示す。簿記3級では範囲外。</span>
    </div>
    <div class="term-card">
        <strong>OCR（光学文字認識）</strong>
        <span>画像や写真から文字を自動で読み取るAI技術。領収書の自動入力に使われる。</span>
    </div>
    <div class="term-card">
        <strong>インボイス（適格請求書）</strong>
        <span>消費税の計算に使う法定の請求書形式。2023年10月から義務化された。</span>
    </div>
</div>
```

---

## G. NotebookLM ソース追加の手順ガイド

**挿入場所:** 前半タブの実習セクション（`<div class="practice-area">`）の最初の `<h3>` 直前

```html
<!-- NotebookLM ソース追加手順 -->
<div style="background:#fffbeb; border:1px solid #fde68a; border-radius:12px; padding:20px; margin-bottom:24px;">
    <h4 style="color:#b45309; margin:0 0 12px; display:flex; align-items:center; gap:8px;">
        <i class="fa-solid fa-book"></i> NotebookLM へのソース追加手順
    </h4>
    <ol style="margin:0; padding-left:1.4rem; font-size:0.92rem; line-height:1.9; color:#78350f;">
        <li><strong>notebooklm.google.com</strong> にアクセスし「新しいノートブック」を作成</li>
        <li>画面左の「ソースを追加」をクリック →「YouTube」または「URL」を選択</li>
        <li>動画①のURL <code style="background:#fff; padding:1px 6px; border-radius:4px;">https://www.youtube.com/watch?v=xfwiwOtNn-I</code> を貼り付けて追加</li>
        <li>同様に動画②のURL <code style="background:#fff; padding:1px 6px; border-radius:4px;">https://www.youtube.com/watch?v=PSpM8snrOz4</code> も追加</li>
        <li>追加完了後、右側のチャット欄に下のプロンプトをコピーして送信する</li>
    </ol>
</div>
```

---

## Cursor への作業指示まとめ

| # | 作業 | 挿入場所の特定方法 |
|---|---|---|
| A | AGI5段階レベル表を追加 | 動画①の `<ul class="lc-list">` 直後 |
| B | AIに代替されない5つの理由リストを追加 | 動画②の `.instructor-note` 内、最後の `</div>` 手前 |
| C | 簿記一巡フロー図を追加 | 動画④の `<ul class="lc-list">` 直後 |
| D | B/S・P/L T字構造ビジュアルを追加 | 動画⑤の `<ul class="lc-list">` 直後 |
| E | 4択クイズ（HTML+CSS+JS）を追加 | まとめタブの `.check-panel` 閉じタグ直後 |
| F | term-grid を12語版に差し替え | まとめタブの `<div class="term-grid">` 全体 |
| G | NotebookLM 手順ガイドを追加 | 前半タブの `.practice-area` 内最初の `<h3>` 直前 |

**完成後のチェックリスト:**
- [ ] AGIレベル表でLv.1〜5が表示され、Lv.2に「現在」バッジがついているか
- [ ] 動画②のInstructor's Note内に「5つの理由」の番号付きリストが表示されるか
- [ ] 簿記一巡フロー図の矢印が取引→仕訳→元帳→試算表→決算→財務諸表の順に並ぶか
- [ ] B/SカードにT字（左：資産 / 右：負債+純資産）が表示されるか
- [ ] P/Lカードにカード（左：費用+利益 / 右：収益）が表示されるか
- [ ] クイズが5問表示され、選択肢クリックで正誤フィードバックが出るか
- [ ] 全5問回答後に「X / 5 正解」の結果が表示されるか
- [ ] まとめタブの用語カードが12個並ぶか
- [ ] NotebookLM手順ガイドが前半タブの実習セクションに表示されるか
- [ ] モバイル（幅360px想定）で各要素が崩れないか
