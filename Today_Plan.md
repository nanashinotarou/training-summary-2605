# 実装計画：Day 2 ページ 品質修正（vol02-1.html）

> **対象ファイル:** `vol02-1.html`（既存ファイルへの追記・修正のみ。新規ファイル不要）  
> **作業:** Cursor が本ファイルを読み込み、以下のバッチ順で `vol02-1.html` に反映する。  
> **注意:** バッチは必ず順番通りに実施し、1バッチ完了後に次へ進むこと。

---

## ▼ バッチ 1：小修正3点（A・B・C）

### Task A：Canva実習の注意書きボックスを追加

**場所:** `id="first"` の中にある `<div class="practice-area">` 内、  
`<ul class="step-list canva">` の **直前** に挿入する。

**挿入するHTML:**
```html
<div style="background: #fef3c7; border: 2px solid #f59e0b; border-radius: 12px; padding: 16px 20px; margin-bottom: 20px; display: flex; align-items: flex-start; gap: 12px;">
    <i class="fa-solid fa-triangle-exclamation" style="color: #d97706; font-size: 1.3rem; flex-shrink: 0; margin-top: 2px;"></i>
    <p style="margin: 0; font-size: 0.95rem; line-height: 1.7; color: #78350f; font-weight: 500;">
        <strong>保存の注意：</strong>作ったデザインは、必ずファイル名に自分の名前をつけてから、自分のフォルダの中に格納しましょう。
    </p>
</div>
```

---

### Task B：`openTab()` のバグ修正

**問題:** `<script>` 内の `openTab` 関数が `event.currentTarget` というグローバルイベント参照を使っているため、タブボタン以外（「前半へ進む」ボタン等）から呼ばれるとタブのアクティブ表示が壊れる。

**現在のコード（`<script>` 内）:**
```javascript
function openTab(tabId) {
    const tabs = document.querySelectorAll('.tab-content');
    const buttons = document.querySelectorAll('.tab-btn');
    tabs.forEach(tab => tab.classList.remove('active'));
    buttons.forEach(btn => btn.classList.remove('active'));
    document.getElementById(tabId).classList.add('active');
    event.currentTarget.classList.add('active');
    window.scrollTo({ top: 0, behavior: 'smooth' });
}
```

**修正後のコード（丸ごと置換）:**
```javascript
function openTab(tabId) {
    document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
    document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
    document.getElementById(tabId).classList.add('active');
    const targetBtn = document.querySelector(`.tab-btn[onclick*="openTab('${tabId}')"]`);
    if (targetBtn) targetBtn.classList.add('active');
    window.scrollTo({ top: 0, behavior: 'smooth' });
}
```

---

### Task C：Canva動画①の「スタイルコピー」をリサーチ内容に修正 ＋ Day 1 リンク追加

#### C-1: 学習カード①の箇条書き修正

**場所:** `data-video-id="nRds9qeaLiM"` の learning-card 内 `<ul class="lc-list">` の3番目の `<li>`

**現在:**
```html
<li>デザインに一貫性を出す「スタイルコピー」機能</li>
```

**修正後:**
```html
<li>ワンクリックで人物・背景を切り抜く「背景除去」機能</li>
```

#### C-2: Day 1 ページへのリンクをフッター付近に追加

**場所:** `</div><!-- .container 閉じタグ -->` の直前（まとめタブ内の次回予告ブロックの後）に追加する。

**挿入するHTML:**
```html
<div style="text-align: center; padding: 20px 40px 40px; background: var(--bg-card-solid);">
    <a href="./vol01-1.html" style="display: inline-flex; align-items: center; gap: 8px; color: var(--text-sub); font-size: 0.9rem; text-decoration: none; border: 1px solid #e2e8f0; padding: 10px 20px; border-radius: 20px; transition: 0.2s;" onmouseover="this.style.borderColor='var(--accent-green)';this.style.color='var(--accent-green)'" onmouseout="this.style.borderColor='#e2e8f0';this.style.color='var(--text-sub)'">
        <i class="fa-solid fa-arrow-left"></i> Day 1 に戻る
    </a>
</div>
```

---

## ▼ バッチ 2：インラインクイズの実装（Task D）

**バッチ1が完了してから着手すること。**

### 1. CSS を `<style>` タグ内の末尾（`</style>` の直前）に追加

```css
/* ===== 確認クイズ ===== */
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
.quiz-opt.disabled { cursor: default; pointer-events: none; }
.quiz-opt-icon { width: 24px; height: 24px; border-radius: 50%; border: 2px solid #cbd5e1; display: flex; align-items: center; justify-content: center; font-size: 0.75rem; font-weight: 700; flex-shrink: 0; }
.quiz-feedback { margin-top: 12px; padding: 10px 14px; border-radius: 8px; font-size: 0.88rem; line-height: 1.6; display: none; }
.quiz-feedback.show { display: block; }
.quiz-feedback.correct-fb { background: #ecfdf5; color: #065f46; border-left: 4px solid #059669; }
.quiz-feedback.wrong-fb { background: #fef2f2; color: #991b1b; border-left: 4px solid #ef4444; }
#quiz-result.pass { background: #ecfdf5; border: 2px solid #059669; border-radius: 12px; }
#quiz-result.fail { background: #fff7ed; border: 2px solid #d97706; border-radius: 12px; }
```

### 2. クイズのHTMLを「まとめ」タブに追加

**場所:** `id="summary"` タブ内の `<div class="term-grid">...</div>` の **直後** に挿入する。

```html
<!-- 確認クイズ -->
<div class="quiz-panel" id="quizPanel">
    <h3><i class="fa-solid fa-circle-question"></i> 確認クイズ（全5問）</h3>
    <p style="color:var(--text-sub); font-size:0.9rem; margin-bottom:20px;">動画を見た人も見ていない人も、今日の理解度を確認してみましょう。</p>
    <div id="quiz-container"></div>
    <div id="quiz-result" style="display:none; margin-top:24px; padding:20px; text-align:center;">
        <p id="quiz-score" style="font-size:1.6rem; font-weight:900; margin-bottom:8px;"></p>
        <p id="quiz-msg" style="font-size:0.95rem; color:var(--text-sub);"></p>
        <button onclick="resetQuiz()" class="tool-link-btn" style="margin-top:16px; background:var(--accent-green);">
            <i class="fa-solid fa-rotate-right"></i> もう一度
        </button>
    </div>
</div>
```

### 3. JavaScript を `<script>` タグ内の `buildQuiz();` の直前に追加

**場所:** `</script>` の直前、既存の `// YouTube Facade` ブロックの後に追加する。

```javascript
// ===== 確認クイズ =====
const quizData = [
    {
        q: "簿記の5要素のうち、借方（左）がホームポジション（増えた時に左に記入）になるものの組み合わせはどれ？",
        opts: ["負債・純資産・収益", "資産・費用", "資産・収益", "費用・負債"],
        ans: 1,
        fb: "正解は「資産・費用」。この2つが増えたら左（借方）へ。残りの負債・純資産・収益は増えたら右（貸方）がホームポジションです。"
    },
    {
        q: "「売掛金」はどのグループに分類されますか？",
        opts: ["費用", "負債", "資産", "収益"],
        ans: 2,
        fb: "売掛金は「まだ受け取っていない代金を受け取る権利」なので資産です。対になる「買掛金（支払う義務）」は負債になります。"
    },
    {
        q: "取引の「二面性」の説明として正しいのはどれ？",
        opts: [
            "取引は必ず現金の授受を伴う",
            "借方と貸方の合計が一致しない取引もある",
            "すべての取引は「原因」と「結果」の2つの側面で記録される",
            "二面性は掛け取引にのみ適用されるルールだ"
        ],
        ans: 2,
        fb: "どんな取引も「何かが増えた（原因）」「何かが減った（結果）」という2つの面を持ちます。借方と貸方の合計は必ず一致します（貸借平均の原則）。"
    },
    {
        q: "次のうち「負債」に分類される勘定科目はどれ？",
        opts: ["普通預金", "売掛金", "受取手数料", "未払家賃"],
        ans: 3,
        fb: "「未払〜」という名前の勘定科目はすべて負債です。「まだ払っていない義務」を表します。普通預金・売掛金は資産、受取手数料は収益です。"
    },
    {
        q: "商品を現金50,000円で仕入れた。この取引で借方に記入する勘定科目と金額は？",
        opts: ["現金 50,000", "仕入 50,000", "売上 50,000", "買掛金 50,000"],
        ans: 1,
        fb: "「仕入」は費用グループ。費用のホームポジションは借方（左）なので、増えたら借方に記入します。現金は資産で減るので貸方（右）へ移動します。"
    }
];

let selectedAnswers = [];

function buildQuiz() {
    const container = document.getElementById('quiz-container');
    if (!container) return;
    container.innerHTML = '';
    selectedAnswers = new Array(quizData.length).fill(null);
    quizData.forEach((q, qi) => {
        const letters = ['A', 'B', 'C', 'D'];
        const optsHtml = q.opts.map((o, oi) => `
            <button type="button" class="quiz-opt" onclick="answerQuiz(${qi},${oi})" id="opt-${qi}-${oi}">
                <span class="quiz-opt-icon">${letters[oi]}</span>${o}
            </button>`).join('');
        container.innerHTML += `
            <div class="quiz-q" id="qq-${qi}">
                <div class="quiz-q-num">問${qi + 1} / ${quizData.length}</div>
                <div class="quiz-q-text">${q.q}</div>
                <div class="quiz-options">${optsHtml}</div>
                <div class="quiz-feedback" id="fb-${qi}"></div>
            </div>`;
    });
    document.getElementById('quiz-result').style.display = 'none';
}

function answerQuiz(qi, oi) {
    if (selectedAnswers[qi] !== null) return;
    selectedAnswers[qi] = oi;
    const q = quizData[qi];
    const isCorrect = oi === q.ans;
    q.opts.forEach((_, i) => {
        const btn = document.getElementById(`opt-${qi}-${i}`);
        btn.classList.add('disabled');
        if (i === q.ans) btn.classList.add('correct');
        else if (i === oi && !isCorrect) btn.classList.add('wrong');
    });
    const fb = document.getElementById(`fb-${qi}`);
    fb.textContent = (isCorrect ? '正解！ ' : '不正解。') + q.fb;
    fb.className = `quiz-feedback show ${isCorrect ? 'correct-fb' : 'wrong-fb'}`;
    if (selectedAnswers.every(a => a !== null)) {
        const score = selectedAnswers.reduce((c, a, i) => c + (a === quizData[i].ans ? 1 : 0), 0);
        showResult(score);
    }
}

function showResult(score) {
    const result = document.getElementById('quiz-result');
    const isPassed = score >= 4;
    result.style.display = 'block';
    result.className = isPassed ? 'pass' : 'fail';
    document.getElementById('quiz-score').textContent = `${score} / ${quizData.length} 正解`;
    document.getElementById('quiz-msg').textContent = isPassed
        ? '素晴らしい！仕訳のルールと勘定科目をしっかり理解できています。Day 3へ進みましょう。'
        : 'もう一度ページを読み直してから再挑戦しましょう。NotebookLMに解説を頼むのもOKです。';
    result.scrollIntoView({ behavior: 'smooth', block: 'center' });
}

function resetQuiz() {
    buildQuiz();
    document.getElementById('quizPanel').scrollIntoView({ behavior: 'smooth' });
}

buildQuiz();
```

---

## 作業完了の確認チェックリスト

- [ ] 「保存の注意」ボックスが Canva実習エリアに表示される
- [ ] タブボタン以外のボタン（「前半へ進む」等）をクリックしてもタブのアクティブ状態が正しく切り替わる
- [ ] Canva動画①の3箇条目が「背景除去」になっている
- [ ] ページ下部に「← Day 1 に戻る」リンクが表示される
- [ ] まとめタブに5問クイズが表示され、回答すると正解/不正解とフィードバックが出る
- [ ] 全問回答後にスコアと合否メッセージが表示される
- [ ] 「もう一度」ボタンでリセット・再挑戦できる
