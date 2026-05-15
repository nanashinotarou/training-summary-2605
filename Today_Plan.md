# Day 6 実装計画書 — 固定資産と減価償却

作成日: 2026-05-15  
実装者: Antigravity / Cursor

---

## 0. Day 6 の位置づけ

| 項目 | 値 |
|---|---|
| ファイル名 | vol06-1.html |
| ページタイトル | Day 6 \| 固定資産と減価償却 |
| プログレスバー | **46%** （6/13 ≈ 46.2%） |
| cache-bust | `2026-05-15T09:00:00` |
| 前半動画 | MOR8WF5UHqM / C1tXaGiIyS4 / pPvP1uFf8Uw |
| 後半動画 | pbAig18gqpo / 95gTu-gtf7Y / GsCpbBK1BM0 |

**前半の役割**: 固定資産の取得・減価償却（定額法・間接法）・資本的支出と収益的支出を理解する  
**後半の役割**: 月割り計算・減価償却累計額の正体（評価勘定）・売却損益の3パターンを理解する

---

## 1. テンプレート複製手順

1. `vol05-1.html` を `vol06-1.html` にコピーして作業開始
2. 以下の固定値を一括置換する

| 変更箇所 | 変更前 | 変更後 |
|---|---|---|
| `<title>` | Day 5 \| 商品売買と掛取引 | Day 6 \| 固定資産と減価償却 |
| ヘッダータグ | DAY 05 | DAY 06 |
| `<h1>` | 商品売買と掛取引 | 固定資産と減価償却 |
| プログレスバー width | 38% | 46% |
| cache-bust | （末尾のコメント） | `<!-- cache-bust: 2026-05-15T09:00:00 -->` |
| Day 7 ナビボタン | `fetch('./vol06-1.html'...)` → alert "Day 6 は準備中" | `fetch('./vol07-1.html'...)` → alert "Day 7 は準備中" |

3. `.deploy_tmp/vol06-1.html` にも**同じ内容で**コピーし、vol06-1.html と完全同期させること

---

## 2. タブ構成概要

```
タブ1: 今日の目標 (id="goal")
  アイコン: fa-building
  ラベル: 今日の目標

タブ2: 前半：取得と減価償却の基本 (id="first")
  アイコン: fa-arrow-trend-down
  ラベル: 前半：取得と減価償却の基本

タブ3: 後半：売却と評価勘定 (id="second")
  アイコン: fa-money-bill-trend-up
  ラベル: 後半：売却と評価勘定

タブ4: まとめ (id="summary")
  アイコン: fa-list-check
  ラベル: まとめ
```

---

## 3. goalタブ 完全HTML

```html
<div id="goal" class="tab-content active">

    <!-- Bloomナラティブ：6日目・朝 -->
    <div class="bloom-story">
        <span class="bloom-story-label">📍 Bloom 6日目・朝</span>
        <p>
            Bloomがオープンして1週間。Harukaさんが念願のエスプレッソマシンを購入しました。<br>
            「150,000円か……大きな買い物だったな」<br><br>
            でも簿記では、この出費を「今日全部の費用」にはしません。<br>
            <strong>長く使うモノの代金は、使う年数に分けて記録する</strong>——それが今日のテーマです。
        </p>
    </div>

    <!-- Whyボックス -->
    <div class="why-box">
        <div class="why-q">なぜ「減価償却累計額」という別勘定を使うの？</div>
        <div class="why-a">
            資産を直接減らしてしまうと、<strong>「もともといくらで買ったか」が帳簿から消えてしまいます。</strong><br>
            別勘定で「今まで何円分価値が減ったか」をメモしておくことで、<br>
            取得原価・累計の減少額・現在の帳簿価額を<strong>同時に見える化</strong>できます。
        </div>
    </div>

    <!-- talk-scene：あるある疑問 -->
    <div class="talk-scene">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">機械を買った時、なぜ「費用」じゃなくて「資産」で記録するんですか？</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">買った瞬間に全額費用にすると、その年だけ大赤字になりますよね。<br>5年使うなら、<strong>5年に分けて費用化する</strong>のが正直な記録です。これが「費用収益対応の原則」です。</div>
        </div>
    </div>

    <!-- goal-box -->
    <div class="goal-box">
        <i class="fa-solid fa-building"></i>
        <h3>「固定資産の取得から売却まで、一連の流れを仕訳できる」</h3>
        <p>付随費用の取扱い、定額法による減価償却、資本的支出vs収益的支出の区別、<br>そして売却時の損益計算まで——固定資産の全サイクルをBloomで体験します。</p>
    </div>

    <h2>本日の学習マップ</h2>
    <p>Day 6は「設備投資の記録」を学びます。長期にわたる資産の管理が、いかに企業の実態を正確に伝えるかを理解しましょう。</p>

    <!-- フローマップ（D：ビジュアルフロー） -->
    <div class="flow-map">
        <div class="flow-step">
            <i class="fa-solid fa-cart-shopping"></i>
            <strong>固定資産の取得</strong>
            <span>購入代価＋付随費用</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-arrow-trend-down"></i>
            <strong>減価償却</strong>
            <span>定額法・間接法</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-screwdriver-wrench"></i>
            <strong>改良・修繕</strong>
            <span>資本的支出 vs 収益的支出</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-scale-balanced"></i>
            <strong>評価勘定の理解</strong>
            <span>累計額は資産のマイナス</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-money-bill-trend-up"></i>
            <strong>固定資産の売却</strong>
            <span>売却益・売却損の計算</span>
        </div>
    </div>

    <div style="background:#fffbeb; padding:25px 30px; border-radius:12px; margin:35px 0 20px; border-left:6px solid var(--accent-gold);">
        <h4 style="color:#b45309; margin:0 0 10px; font-size:1.15rem;"><i class="fa-solid fa-lightbulb"></i> AI Director's Eye：資産管理の自動化</h4>
        <p style="margin:0; font-size:0.95rem; color:#333; line-height:1.7;">ERPやクラウド会計ソフトでは、固定資産台帳が自動で減価償却を計算します。「定額法で5年・残存価額0」と入力するだけで毎期の仕訳が自動生成される仕組みの裏側には、今日学ぶ計算式がそのまま動いています。</p>
    </div>

    <div style="text-align:center; margin-top:30px;">
        <button class="tool-link-btn" onclick="openTab('first')" style="background:var(--accent-green);">
            前半（取得と減価償却）へ進む <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>
</div>
```

---

## 4. firstタブ 完全HTML

```html
<div id="first" class="tab-content">

    <!-- Bloomナラティブ：6日目・午前 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 6日目・午前</span>
        <p>
            エスプレッソマシン150,000円に加え、設置業者への工事費10,000円もかかりました。<br>
            「この工事費も、機械の原価に含めるんですか？」<br><br>
            そう——<strong>使える状態にするためのコストは、すべて取得原価</strong>です。<br>
            そして代金は月末払い。この時「買掛金」ではなく「未払金」を使います。
        </p>
    </div>

    <h2>セクション1：固定資産の取得と減価償却</h2>

    <!-- Learning Card 1: 固定資産の購入と付随費用 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="MOR8WF5UHqM" style="background-image: url('https://img.youtube.com/vi/MOR8WF5UHqM/maxresdefault.jpg'), url('https://img.youtube.com/vi/MOR8WF5UHqM/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag">取得原価</span>
                <h3 class="lc-title">固定資産の購入と付随費用の処理</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-cart-shopping"></i> 取得原価の計算ルール</h4>
                <ul class="lc-list">
                    <li><strong>取得原価 ＝ 購入代価 ＋ 付随費用</strong>（設置費・仲介手数料・登記料など）</li>
                    <li>付随費用を「支払手数料」等の費用にするのは<strong>NG</strong>——資産の原価に含める。</li>
                    <li><strong>代金後払い → 「未払金」</strong>（商品以外の負債）。「買掛金」は商品仕入れ専用。</li>
                    <li>例：機械150,000円 ＋ 設置費10,000円 → (借) 備品 160,000 ／ (貸) 未払金 160,000</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 2: 定額法と間接法 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="C1tXaGiIyS4" style="background-image: url('https://img.youtube.com/vi/C1tXaGiIyS4/maxresdefault.jpg'), url('https://img.youtube.com/vi/C1tXaGiIyS4/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag gold">最重要</span>
                <h3 class="lc-title">定額法と間接法：減価償却の計算と仕訳</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-calculator"></i> 計算式と仕訳を丸ごと覚える</h4>
                <ul class="lc-list">
                    <li><strong>定額法</strong>：(取得原価 − 残存価額) ÷ 耐用年数 ＝ 年間減価償却費</li>
                    <li><strong>間接法の仕訳</strong>：(借) 減価償却費 XXX ／ (貸) 減価償却累計額 XXX</li>
                    <li>資産（備品など）は直接減らさない——<strong>取得原価を帳簿上に残す</strong>のが間接法の目的。</li>
                    <li><strong>月割り計算</strong>：期中取得の場合 → 年額 × 使用月数 ÷ 12</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 3: 資本的支出・収益的支出 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="pPvP1uFf8Uw" style="background-image: url('https://img.youtube.com/vi/pPvP1uFf8Uw/maxresdefault.jpg'), url('https://img.youtube.com/vi/pPvP1uFf8Uw/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag blue">判断力</span>
                <h3 class="lc-title">資本的支出 vs 収益的支出：改良か修繕か</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-screwdriver-wrench"></i> 「価値UP」か「現状維持」かで判断する</h4>
                <ul class="lc-list">
                    <li><strong>資本的支出</strong>：資産の価値向上・耐用年数の延長（非常階段の設置、耐震工事）→ <strong>資産（建物など）に加算</strong></li>
                    <li><strong>収益的支出</strong>：現状維持・故障箇所を元に戻す（雨漏り修繕、窓ガラス交換）→ <strong>修繕費（費用）</strong></li>
                    <li>試験では「価値を高める」「耐用年数を延ばす」というキーワードが資本的支出のヒント。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- talk-scene：定額法あるある -->
    <div class="talk-scene" style="margin: 24px 0;">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">「残存価額ゼロ」って試験でよく見ますが、毎回ゼロとは限らないですよね？</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">その通り。「残存価額 = 取得原価の10%」と指定される問題もあります。<br>まず<strong>問題文の条件を確認してから計算式に代入</strong>する癖をつけましょう。条件が変わっても計算式の構造は同じです。</div>
        </div>
    </div>

    <!-- NotebookLM 実習：前半 -->
    <div class="practice-area">
        <h3><i class="fa-solid fa-robot"></i> 実習：固定資産の取得・減価償却を問題で確認</h3>
        <p>3本の動画をNotebookLMに読み込ませ、取得原価の計算・定額法・資本的支出の区別を自分の言葉で整理しましょう。</p>
        <div style="background:#fffbeb; border:1px solid #fde68a; border-radius:12px; padding:20px; margin-bottom:24px;">
            <h4 style="color:#b45309; margin:0 0 12px; display:flex; align-items:center; gap:8px;">
                <i class="fa-solid fa-book"></i> NotebookLM へのソース追加手順
            </h4>
            <ol style="margin:0; padding-left:1.4rem; font-size:0.92rem; line-height:1.9; color:#78350f;">
                <li><strong>notebooklm.google.com</strong> にアクセスし「新しいノートブック」を作成</li>
                <li>画面左の「ソースを追加」をクリックし「YouTube」または「URL」を選択</li>
                <li>動画① <code>https://www.youtube.com/watch?v=MOR8WF5UHqM</code> を追加</li>
                <li>動画② <code>https://www.youtube.com/watch?v=C1tXaGiIyS4</code> を追加</li>
                <li>動画③ <code>https://www.youtube.com/watch?v=pPvP1uFf8Uw</code> を追加</li>
                <li>追加完了後、下のプロンプトをコピーして選択式テストを作成する</li>
            </ol>
        </div>
        <div class="chat-prompt-container">
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">読み込んだ動画①〜③の内容をもとに、Day6前半（固定資産の取得原価・付随費用・未払金・定額法による減価償却費の計算・間接法の仕訳・資本的支出と収益的支出の区別）の理解度を確認する選択式テストを20問作ってください。各問は4択にし、解答は最後にまとめて表示してください。</span></div>
                <button class="copy-btn" title="プロンプトをコピー"><i class="fa-regular fa-copy"></i> コピー</button>
            </div>
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">定額法の計算に特化した問題を5問作ってください。各問に「取得原価・残存価額・耐用年数・取得日」の具体的な数字を設定し、年間減価償却費と月割り計算の過程を解説付きで示してください。</span></div>
                <button class="copy-btn" title="プロンプトをコピー"><i class="fa-regular fa-copy"></i> コピー</button>
            </div>
        </div>
        <div style="text-align:center; margin-top:20px;">
            <a href="https://notebooklm.google.com/" target="_blank" class="tool-link-btn" style="background:var(--accent-gold);">
                <i class="fa-solid fa-book"></i> NotebookLM を開く
            </a>
        </div>
    </div>

    <div style="text-align:center; margin-top:30px;">
        <button class="tool-link-btn" onclick="openTab('second')" style="background:var(--accent-green);">
            後半（売却と評価勘定）へ進む <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>
</div>
```

---

## 5. secondタブ 完全HTML

```html
<div id="second" class="tab-content">

    <!-- Bloomナラティブ：6日目・午後 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 6日目・午後</span>
        <p>
            3年前に買ったショーケースを、ついに売ることになりました。<br>
            「帳簿上の価値は残っているのに、売れた金額は違う……これって損？得？」<br><br>
            <strong>「帳簿価額と売値の比較」</strong>が固定資産売却のポイント。<br>
            月割り計算・評価勘定の正体・売却3パターンを一気に理解しましょう。
        </p>
    </div>

    <h2>セクション2：売却と評価勘定の深化</h2>

    <!-- 売却損益判定フロービジュアル（D：ビジュアルフロー） -->
    <div style="background:#f8fafc; border:1px solid #e2e8f0; border-radius:12px; padding:24px; margin-bottom:28px;">
        <h4 style="margin:0 0 16px; color:var(--accent-green);"><i class="fa-solid fa-route"></i> 固定資産売却の損益判定フロー</h4>
        <div style="display:flex; gap:12px; align-items:center; flex-wrap:wrap; justify-content:center;">
            <div style="background:#ecfdf5; border:2px solid var(--accent-green); border-radius:10px; padding:14px 18px; text-align:center; min-width:150px;">
                <div style="font-size:0.78rem; color:var(--accent-green); font-weight:700; margin-bottom:6px;">STEP 1：帳簿価額を計算</div>
                <div style="font-size:0.9rem; font-weight:700;">取得原価<br>− 減価償却累計額</div>
                <div style="font-size:0.78rem; color:var(--text-sub); margin-top:4px;">= 帳簿上の現在価値</div>
            </div>
            <div style="font-size:1.5rem; color:#94a3b8;">→</div>
            <div style="background:#fef3c7; border:2px solid var(--accent-gold); border-radius:10px; padding:14px 18px; text-align:center; min-width:150px;">
                <div style="font-size:0.78rem; color:var(--accent-gold); font-weight:700; margin-bottom:6px;">STEP 2：売値と比較</div>
                <div style="font-size:0.9rem; font-weight:700;">売却価格<br>vs 帳簿価額</div>
                <div style="font-size:0.78rem; color:var(--text-sub); margin-top:4px;">購入価格との比較ではない！</div>
            </div>
            <div style="font-size:1.5rem; color:#94a3b8;">→</div>
            <div style="background:#eff6ff; border:2px solid #3b82f6; border-radius:10px; padding:14px 18px; text-align:center; min-width:150px;">
                <div style="font-size:0.78rem; color:#3b82f6; font-weight:700; margin-bottom:6px;">STEP 3：損益を記録</div>
                <div style="font-size:0.9rem; font-weight:700;">売値 ＞ 帳簿価額<br>→ <strong style="color:#059669;">売却益（収益）</strong></div>
                <div style="font-size:0.78rem; color:#555; margin-top:4px;">売値 ＜ 帳簿価額<br>→ <strong style="color:#dc2626;">売却損（費用）</strong></div>
            </div>
        </div>
    </div>

    <!-- Learning Card 4: 月割り計算と間接法 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="pbAig18gqpo" style="background-image: url('https://img.youtube.com/vi/pbAig18gqpo/maxresdefault.jpg'), url('https://img.youtube.com/vi/pbAig18gqpo/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag">月割り計算</span>
                <h3 class="lc-title">月割り計算と間接法の仕組みを深める</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-calendar-days"></i> 期中取得・期中売却の月割り</h4>
                <ul class="lc-list">
                    <li><strong>月割り公式</strong>：年間減価償却費 × 使用月数 ÷ 12</li>
                    <li>取得月を含めてカウント（例：10月取得→期末3月まで = 6ヶ月）</li>
                    <li>間接法では減価償却累計額が「これまで総額いくら減ったか」を示す。</li>
                    <li>売却時は取得原価と累計額を<strong>セットで消す</strong>のがルール。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 5: 評価勘定の正体 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="95gTu-gtf7Y" style="background-image: url('https://img.youtube.com/vi/95gTu-gtf7Y/maxresdefault.jpg'), url('https://img.youtube.com/vi/95gTu-gtf7Y/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag gold">よく出る疑問</span>
                <h3 class="lc-title">減価償却累計額は負債なの？評価勘定の正体</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-scale-balanced"></i> 「資産のマイナス」という特別な役割</h4>
                <ul class="lc-list">
                    <li>減価償却累計額は<strong>負債ではなく「資産の評価勘定」</strong>。</li>
                    <li>通常の資産と逆で、ホームポジションは<strong>貸方（右側）</strong>。</li>
                    <li>B/S上では固定資産の<strong>すぐ下にマイナス表示</strong>され、現在の帳簿価額を示す。</li>
                    <li>メリット：取得原価が一目でわかり、どれだけ使ったかも把握できる。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 6: 固定資産の売却3パターン -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="GsCpbBK1BM0" style="background-image: url('https://img.youtube.com/vi/GsCpbBK1BM0/maxresdefault.jpg'), url('https://img.youtube.com/vi/GsCpbBK1BM0/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag blue">売却3パターン</span>
                <h3 class="lc-title">固定資産の売却：期首・期中・期末の違い</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-money-bill-trend-up"></i> タイミングで仕訳が変わる！</h4>
                <ul class="lc-list">
                    <li><strong>期首売却</strong>：前期末に償却済み → 当期の減価償却費計上は不要。</li>
                    <li><strong>期中・期末売却</strong>：当期使用分を月割りで先に計上してから売却処理。</li>
                    <li><strong>代金後払い → 「未収入金」</strong>（商品以外）。「売掛金」は商品販売専用。</li>
                    <li><strong>土地は減価償却しない</strong>（使っても価値が減らないため）。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- talk-scene：売却あるある -->
    <div class="talk-scene" style="margin: 24px 0;">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">期中に売った時、なぜ売却の前に減価償却費を計上するんですか？順番が逆では？</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">売却日までの間も資産を使っていたからです。<br>「4月〜6月の3ヶ月分は使った」という事実を<strong>先に記録してから</strong>、その累計額を反映した状態で売却損益を計算します。</div>
        </div>
    </div>

    <!-- 本日の結論コミック枠 -->
    <div class="conclusion-comic">
        <span class="comic-label">✍️ DAY 6 の核心</span>
        <p>固定資産は「買った瞬間」ではなく「使い切った後まで」記録し続ける——<br>
        その誠実な記録が、企業の真の実力を映し出します。</p>
    </div>

    <!-- NotebookLM 実習：後半 -->
    <div class="practice-area">
        <h3><i class="fa-solid fa-clipboard-check"></i> 実習：売却損益の計算と評価勘定を問題で確認</h3>
        <p>動画④〜⑥をNotebookLMに追加し、月割り計算・評価勘定・売却3パターンの計算力を鍛えましょう。</p>
        <div style="background:#fffbeb; border:1px solid #fde68a; border-radius:12px; padding:20px; margin-bottom:24px;">
            <h4 style="color:#b45309; margin:0 0 12px; display:flex; align-items:center; gap:8px;">
                <i class="fa-solid fa-book"></i> NotebookLM へのソース追加手順
            </h4>
            <ol style="margin:0; padding-left:1.4rem; font-size:0.92rem; line-height:1.9; color:#78350f;">
                <li><strong>notebooklm.google.com</strong> にアクセスし、前半と同じノートブックを開く</li>
                <li>画面左の「ソースを追加」をクリックし「YouTube」または「URL」を選択</li>
                <li>動画④ <code>https://www.youtube.com/watch?v=pbAig18gqpo</code> を追加</li>
                <li>動画⑤ <code>https://www.youtube.com/watch?v=95gTu-gtf7Y</code> を追加</li>
                <li>動画⑥ <code>https://www.youtube.com/watch?v=GsCpbBK1BM0</code> を追加</li>
                <li>追加完了後、下のプロンプトをコピーして選択式テストを作成する</li>
            </ol>
        </div>
        <div class="chat-prompt-container">
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">読み込んだ動画④〜⑥の内容をもとに、Day6後半（月割り計算・減価償却累計額は評価勘定であること・B/S上の表示・期首/期中/期末別の売却仕訳・未収入金と売掛金の違い・土地は減価償却しない）の理解度を確認する選択式テストを20問作ってください。各問は4択にし、解答は最後にまとめて表示してください。</span></div>
                <button class="copy-btn" title="プロンプトをコピー"><i class="fa-regular fa-copy"></i> コピー</button>
            </div>
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">固定資産の売却に特化した計算問題を5問作ってください。各問に「取得原価・耐用年数・残存価額・経過年数（または累計額）・売却価格・売却時期（期首/期中/期末）」の具体的な数字を設定し、売却損益の計算過程と正しい仕訳を解説付きで示してください。</span></div>
                <button class="copy-btn" title="プロンプトをコピー"><i class="fa-regular fa-copy"></i> コピー</button>
            </div>
        </div>
        <div style="text-align:center; margin-top:20px;">
            <a href="https://notebooklm.google.com/" target="_blank" class="tool-link-btn" style="background:var(--accent-gold);">
                <i class="fa-solid fa-book"></i> NotebookLM を開く
            </a>
        </div>
    </div>

    <div style="text-align:center; margin-top:30px;">
        <button class="tool-link-btn" onclick="openTab('summary')" style="background:var(--accent-green);">
            まとめへ進む <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>
</div>
```

---

## 6. summaryタブ 完全HTML

```html
<div id="summary" class="tab-content">

    <!-- Bloomナラティブ：6日目・夜 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 6日目・夜</span>
        <p>
            「設備を買って、少しずつ費用にして、最後に売る」——<br>
            固定資産は長い時間をかけて帳簿に刻まれていく存在でした。<br><br>
            <strong>毎年の減価償却が、企業の「正直さ」を守っている。</strong><br>
            Harukaさんも今日、その意味をじわじわ理解し始めました。
        </p>
    </div>

    <h2>DAY 6 まとめ</h2>
    <div class="summary-grid">
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-cart-shopping"></i></div>
            <h3>取得原価は「使える状態になるまで」</h3>
            <p>付随費用（設置費・仲介料）は取得原価に含める。後払いは「未払金」（買掛金ではない）。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-arrow-trend-down"></i></div>
            <h3>定額法 × 間接法が基本セット</h3>
            <p>(取得原価 − 残存価額) ÷ 耐用年数 ＝ 年間償却費。仕訳は借方に「減価償却費」、貸方に「減価償却累計額」。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-screwdriver-wrench"></i></div>
            <h3>改良か修繕か：目的で仕訳が変わる</h3>
            <p>価値UP・耐用年数延長 → 資産（資本的支出）。現状回復・維持 → 修繕費（収益的支出）。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-money-bill-trend-up"></i></div>
            <h3>売却損益は「帳簿価額」との比較</h3>
            <p>売価 ＞ 帳簿価額 → 固定資産売却益（収益）。売価 ＜ 帳簿価額 → 固定資産売却損（費用）。</p>
        </div>
    </div>

    <h2>重要用語チェック</h2>
    <div class="term-grid">
        <div class="term-card"><strong>固定資産</strong><span>1年を超えて使用する有形の資産（建物・備品・車両運搬具など）。</span></div>
        <div class="term-card"><strong>取得原価</strong><span>購入代価に付随費用（設置費・登記料など）を加えた、資産取得のための総コスト。</span></div>
        <div class="term-card"><strong>未払金</strong><span>商品以外のもの（固定資産など）を後払いで購入した際に生じる負債科目。</span></div>
        <div class="term-card"><strong>減価償却（げんかしょうきゃく）</strong><span>固定資産の価値の減少分を、使用期間にわたって費用として配分する手続き。</span></div>
        <div class="term-card"><strong>定額法（ていがくほう）</strong><span>毎年同額ずつ減価償却する計算方法。(取得原価 − 残存価額) ÷ 耐用年数。</span></div>
        <div class="term-card"><strong>耐用年数（たいようねんすう）</strong><span>固定資産が使用できると見込まれる期間。税法上の年数が試験では使われる。</span></div>
        <div class="term-card"><strong>間接法（かんせつほう）</strong><span>資産を直接減らさず「減価償却累計額」という別勘定で累積の減少額を管理する方法。</span></div>
        <div class="term-card"><strong>減価償却累計額</strong><span>これまでの減価償却の合計額を示す評価勘定。資産のマイナスで貸方残高。</span></div>
        <div class="term-card"><strong>評価勘定（ひょうかかんじょう）</strong><span>本体資産の価値をマイナスして評価するための特別な勘定科目。</span></div>
        <div class="term-card"><strong>資本的支出</strong><span>固定資産の価値向上・耐用年数延長を目的とした支出。取得原価に加算する。</span></div>
        <div class="term-card"><strong>収益的支出（修繕費）</strong><span>固定資産の現状維持・原状回復のための支出。費用（修繕費）として計上。</span></div>
        <div class="term-card"><strong>未収入金</strong><span>商品以外のもの（固定資産など）を後払いで売却した際に生じる資産科目。</span></div>
    </div>

    <div class="check-panel">
        <h3><i class="fa-solid fa-clipboard-check"></i> セルフチェック</h3>
        <ol class="check-list">
            <li>備品300,000円・残存価額0・耐用年数5年を定額法・間接法で記録する場合、1年目の仕訳を正しく書けますか？（減価償却費60,000円・減価償却累計額60,000円）</li>
            <li>「雨漏りの修繕工事30,000円」と「耐震補強工事200,000円」のうち、どちらが資本的支出でどちらが収益的支出か、理由とともに説明できますか？</li>
            <li>取得原価300,000円・減価償却累計額90,000円の備品を180,000円で期首に売却した場合の仕訳を書けますか？（売却損30,000円が発生します）</li>
        </ol>
    </div>

    <!-- 確認クイズ -->
    <div class="quiz-panel" id="quizPanel">
        <h3><i class="fa-solid fa-circle-question"></i> 確認クイズ（全5問）</h3>
        <p style="color:var(--text-sub); font-size:0.9rem; margin-bottom:20px;">今日の固定資産・減価償却・売却損益の理解度を確認しましょう。</p>
        <div id="quiz-container"></div>
        <div id="quiz-result" style="display:none; margin-top:24px; padding:20px; border-radius:12px; text-align:center;">
            <p id="quiz-score" style="font-size:1.6rem; font-weight:900; margin-bottom:8px;"></p>
            <p id="quiz-msg" style="font-size:0.95rem; color:var(--text-sub);"></p>
            <button onclick="resetQuiz()" class="tool-link-btn" style="margin-top:16px; background:var(--accent-green);">
                <i class="fa-solid fa-rotate-right"></i> もう一度
            </button>
        </div>
    </div>

    <div style="background:#f0fdf4; padding:25px 30px; border-radius:12px; margin:35px 0 20px; border-left:6px solid #059669;">
        <h4 style="color:#047857; margin:0 0 10px; font-size:1.15rem;"><i class="fa-solid fa-forward"></i> 次回予告：Day 7</h4>
        <p style="margin:0; font-size:0.95rem; color:#333; line-height:1.6;">（Day 7 の内容確定後に記入）</p>
    </div>
</div>
```

---

## 7. ページ末尾ナビボタン（Day 7 async fetch）

```html
<div style="text-align:center; padding: 3rem 0 2rem; display: flex; flex-direction: column; gap: 1rem; align-items: center;">
    <button type="button" class="tool-link-btn" style="padding: 1.2rem 4rem; font-size:1.25rem; background:linear-gradient(135deg, #34d399, #059669); border:none; box-shadow: 0 10px 30px rgba(5,150,105, 0.25); cursor:pointer;" onclick="(async function(){try{const r=await fetch('./vol07-1.html',{method:'HEAD',cache:'no-store'});if(r.ok){window.location.href='./vol07-1.html';return;}}catch(e){}alert('Day 7 は準備中です。もうしばらくお待ちください。');})()">
        Day 7 へ進む <i class="fa-solid fa-arrow-right"></i>
    </button>
    <button type="button" class="tool-link-btn" style="padding: 1.2rem 4rem; font-size:1.25rem; background:#fff; color:#4a4a4a; border:2px solid #e2e8f0; box-shadow:none; cursor:pointer;" onclick="window.location.href='./index.html'">
        <i class="fa-solid fa-house"></i> 学習記録をつけて Home へ戻る
    </button>
</div>
```

---

## 8. quizData（ページ末尾 `<script>` 内）

```javascript
const quizData = [
    {
        q: "Bloomがショーケース150,000円を購入し、設置費10,000円を現金で支払った。備品勘定に計上する取得原価は？",
        opts: ["150,000円", "10,000円", "160,000円", "140,000円"],
        ans: 2,
        fb: "付随費用（設置費）は取得原価に含めます。Bloomにとってショーケースの原価は「150,000 + 10,000 = 160,000円」です。費用（支払手数料等）にするのは誤りです。"
    },
    {
        q: "取得原価90,000円・残存価額0・耐用年数5年の備品を定額法で減価償却する場合、1年間の減価償却費は？",
        opts: ["9,000円", "18,000円", "15,000円", "4,500円"],
        ans: 1,
        fb: "(90,000 − 0) ÷ 5 = 18,000円。定額法は残存価額を引いてから耐用年数で割ります。残存価額がゼロの場合は取得原価をそのまま年数で割ればOKです。"
    },
    {
        q: "Bloomのカフェで雨漏りが発生し、修繕工事費30,000円を現金で支払った。正しい仕訳の借方科目は？",
        opts: ["建物", "修繕費", "資本的支出", "減価償却費"],
        ans: 1,
        fb: "雨漏りの修繕は「現状回復」なので収益的支出です。費用科目「修繕費」を借方に計上します。資産価値を高める工事（資本的支出）なら建物に加算しますが、今回は違います。"
    },
    {
        q: "取得原価300,000円・減価償却累計額90,000円の備品を180,000円で売却した（代金現金）。売却損益はいくら？",
        opts: ["売却益90,000円", "売却損30,000円", "売却益30,000円", "売却損90,000円"],
        ans: 1,
        fb: "帳簿価額 = 300,000 − 90,000 = 210,000円。売却価格180,000円 ＜ 帳簿価額210,000円なので、差額30,000円が「固定資産売却損（費用）」です。"
    },
    {
        q: "減価償却累計額は、簿記の5大要素（資産・負債・純資産・収益・費用）のどれに属する？",
        opts: ["負債", "費用", "純資産", "資産（の評価勘定）"],
        ans: 3,
        fb: "減価償却累計額は「資産の評価勘定」です。資産のマイナスを表す特別な科目で、負債ではありません。貸借対照表では固定資産の下にマイナス表示され、現在の帳簿価額を示します。"
    }
];
```

---

## 9. 実装チェックリスト

実装完了後、以下を確認すること。

### テンプレート変更
- [ ] `<title>` が「Day 6 | 固定資産と減価償却」になっている
- [ ] ヘッダータグが「DAY 06」になっている
- [ ] `<h1>` が「固定資産と減価償却」になっている
- [ ] プログレスバー width が `46%` になっている
- [ ] cache-bust が `2026-05-15T09:00:00` になっている

### 動画IDの確認（全6本）
- [ ] 動画① `MOR8WF5UHqM`（固定資産の購入・付随費用）
- [ ] 動画② `C1tXaGiIyS4`（定額法・間接法）
- [ ] 動画③ `pPvP1uFf8Uw`（資本的支出・収益的支出）
- [ ] 動画④ `pbAig18gqpo`（月割り計算・間接法の深化）
- [ ] 動画⑤ `95gTu-gtf7Y`（評価勘定の正体）
- [ ] 動画⑥ `GsCpbBK1BM0`（売却3パターン）

### コンテンツ確認
- [ ] goalタブ：bloom-story・why-box・talk-scene・goal-box・flow-map がすべて実装されている
- [ ] firstタブ：learning-card 3枚（動画①②③）・talk-scene・practice-area がある
- [ ] secondタブ：売却フロービジュアル・learning-card 3枚（動画④⑤⑥）・talk-scene・conclusion-comic・practice-area がある
- [ ] summaryタブ：summary-grid 4枚・term-grid 12語・check-panel・quiz-panel・次回予告がある

### ナビゲーション
- [ ] Day 7ボタンのonclickに `./vol07-1.html` が含まれている
- [ ] アラートメッセージが「Day 7 は準備中です。」になっている

### quizData（5問確認）
- [ ] 問1：取得原価 = 150,000 + 10,000 = 160,000円（答えC）
- [ ] 問2：(90,000 − 0) ÷ 5 = 18,000円（答えB）
- [ ] 問3：雨漏り修繕 → 修繕費（答えB）
- [ ] 問4：帳簿価額210,000 vs 売価180,000 → 売却損30,000円（答えB）
- [ ] 問5：減価償却累計額は資産の評価勘定（答えD）

### デプロイ
- [ ] `vol06-1.html` と `.deploy_tmp/vol06-1.html` の内容が完全に一致している
- [ ] cache-bust コメントが両ファイルともに `<!-- cache-bust: 2026-05-15T09:00:00 -->` になっている
