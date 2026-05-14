# Day 5 実装計画：`vol05-1.html`（完全版）

作成日: 2026-05-14（Claude Code によるレビュー補完版）  
方針: Bloom設定（A）＋Whyファースト（B）＋あるある疑問（C）＋ビジュアルフロー（D）  
担当: Cursor で実装・Claude Code でレビュー

---

## 0. Day 5 の位置づけ

```
雑貨カフェ「Bloom」
Day 5：5日目。取引が「現金」から「ツケ（掛取引）」や「カード」に広がる日。
        商品を売買する際の様々なパターン（返品・諸掛）を学び、
        在庫管理の基本である「商品有高帳」をマスターする。
前半：分記法（基礎概念）、売掛金・買掛金、仕訳の3ステップ
後半：返品と諸掛、商品有高帳（先入先出法）、クレジット売掛金
```

### 使用動画（6本・全ての transcript 取得済み）
- 前半①: `BboEGZR2XV8`（分記法：資産と利益を分ける）
- 前半②: `qHPXHWv8xws`（売掛金・買掛金の基本：権利と義務）
- 前半③: `NCw9voGKR74`（仕訳の3ステップ：迷わないためのルール）
- 後半①: `8YeucmRVqbw`（返品と諸掛：仕入原価に含めるルール）
- 後半②: `fai9jxaXIho`（商品有高帳：先入先出法のロジック）
- 後半③: `wmihafdKezA`（クレジット売掛金：キャッシュレス対応）

---

## 1. テンプレートの複製

**最新の `vol04-1.html` を `vol05-1.html` としてコピーして作業を開始する。**  
（Bloom CSS コンポーネント・レスポンシブ設定を継承するため）

変更が必要な固定値：
- `<title>Day 4 | 現金・預金の管理</title>` → `<title>Day 5 | 商品売買と掛取引</title>`
- `<span class="header-tag">DAY 04</span>` → `<span class="header-tag">DAY 05</span>`
- `<h1>現金・預金の管理</h1>` → `<h1>商品売買と掛取引</h1>`
- プログレスバー: `width: 31%` → `width: 38%`（5/13コース進捗）
- cache-bust: `<!-- cache-bust: 2026-05-14T09:00:00 -->`

---

## 2. タブ構成

```
今日の目標（goal）
前半：掛取引と仕訳の基本（first）
後半：返品・在庫・カード決済（second）
まとめ（summary）
```

---

## 3. goalタブ

```html
<div id="goal" class="tab-content active">

    <!-- Bloomナラティブ：5日目・朝 -->
    <div class="bloom-story">
        <span class="bloom-story-label">📍 Bloom 5日目・朝</span>
        <p>
            Bloomもオープンして5日。常連さんが増えてきました。<br>
            「あ、今日財布忘れちゃった。ツケにしといて！」なんて会話や、<br>
            「カードで払えますか？」というお客さんも。<br><br>
            <strong>「その場でお金が動かない取引」</strong>をどう記録するか——それが今日のテーマです。
        </p>
    </div>

    <!-- Whyボックス -->
    <div class="why-box">
        <div class="why-q">なぜ「売掛金」と「売上」を分けるの？</div>
        <div class="why-a">
            「売上」は今日いくら売ったかという <strong>成果（収益）</strong> です。<br>
            「売掛金」は後でいくらもらえるかという <strong>権利（資産）</strong> です。<br>
            「儲け」と「手元の権利」を分けて記録することで、将来の資金繰りが予測できるようになります。
        </div>
    </div>

    <!-- talk-scene -->
    <div class="talk-scene">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">「諸掛（しょがかり）」って言葉、難しそうですね。何のことですか？</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">簡単に言えば「送料や保険料」のことです。<br>商品を仕入れる時にかかった送料は、<strong>商品の原価の一部</strong>として扱います。これが簿記の面白いルールの一つですよ。</div>
        </div>
    </div>

    <!-- 既存goal-box -->
    <div class="goal-box">
        <i class="fa-solid fa-truck-fast"></i>
        <h3>「掛取引・返品・諸掛を理解し、在庫管理とカード決済をマスターする」</h3>
        <p>売掛金・買掛金の基本から、返品時の逆仕訳、送料の処理（諸掛）、<br>そして現代に欠かせないクレジット決済の仕組みまでを網羅します。</p>
    </div>

    <h2>本日の学習マップ</h2>
    <p>Day 5は「商売の広がり」を学びます。現金以外のやり取りを正確に記録する力を身につけましょう。</p>

    <!-- フローマップ（D：ビジュアルフロー） -->
    <div class="flow-map">
        <div class="flow-step">
            <i class="fa-solid fa-handshake"></i>
            <strong>掛取引の発生</strong>
            <span>売掛金・買掛金</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-rotate-left"></i>
            <strong>返品・諸掛</strong>
            <span>逆仕訳と原価算入</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-boxes-stacked"></i>
            <strong>在庫管理</strong>
            <span>商品有高帳（FIFO）</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-credit-card"></i>
            <strong>カード決済</strong>
            <span>支払手数料の処理</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-file-invoice-dollar"></i>
            <strong>代金回収・支払</strong>
            <span>掛の消し込み</span>
        </div>
    </div>

    <div style="background:#fffbeb; padding:25px 30px; border-radius:12px; margin:35px 0 20px; border-left:6px solid var(--accent-gold);">
        <h4 style="color:#b45309; margin:0 0 10px; font-size:1.15rem;"><i class="fa-solid fa-lightbulb"></i> AI Director's Eye：在庫管理の自動化</h4>
        <p style="margin:0; font-size:0.95rem; color:#333; line-height:1.7;">POSシステムやAIによる在庫管理の裏側では、今日学ぶ「先入先出法（FIFO）」などのアルゴリズムが動いています。基本のロジックを知ることで、システムの異常に気づいたり、より効率的な発注予測ができるようになります。</p>
    </div>

    <div style="text-align:center; margin-top:30px;">
        <button class="tool-link-btn" onclick="openTab('first')" style="background:var(--accent-green);">
            前半（掛取引と仕訳）へ進む <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>
</div>
```

---

## 4. 前半タブ（掛取引と仕訳）

```html
<div id="first" class="tab-content">

    <!-- Bloomナラティブ：5日目・午前 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 5日目・午前</span>
        <p>
            卸売業者さんから新しい雑貨が届きました。「代金は月末にまとめて払うね」という約束。<br>
            これが<strong>買掛金（かいかけきん）</strong>です。<br><br>
            まずは「分記法」という基礎的な考え方で利益の構造を理解し、
            その後に実務で必須の「売掛金・買掛金」の仕訳を学びます。
        </p>
    </div>

    <h2>セクション1：商品売買と掛取引の基礎</h2>

    <!-- Learning Card 1: 分記法 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="BboEGZR2XV8" style="background-image: url('https://img.youtube.com/vi/BboEGZR2XV8/maxresdefault.jpg'), url('https://img.youtube.com/vi/BboEGZR2XV8/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag">基礎概念</span>
                <h3 class="lc-title">分記法：商品と利益を分けて考える</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-layer-group"></i> 分記法（ぶんきほう）とは？</h4>
                <ul class="lc-list">
                    <li><strong>仕入時</strong>：(借) 商品 100 ／ (貸) 現金 100 （資産の増加として記録）</li>
                    <li><strong>売上時</strong>：(借) 現金 150 ／ (貸) 商品 100, <strong>商品売買益 50</strong></li>
                    <li><strong>メリット</strong>：取引のたびに「いくら儲かったか」が明確になる。</li>
                    <li><strong>デメリット</strong>：実務では商品ごとの原価計算が煩雑なため、通常は「三分法」を使う。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 2: 売掛金・買掛金 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="qHPXHWv8xws" style="background-image: url('https://img.youtube.com/vi/qHPXHWv8xws/maxresdefault.jpg'), url('https://img.youtube.com/vi/qHPXHWv8xws/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag">重要科目</span>
                <h3 class="lc-title">売掛金と買掛金：掛取引の仕訳</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-file-contract"></i> 資産と負債の区別</h4>
                <ul class="lc-list">
                    <li><strong>売掛金（資産）</strong>：代金を後でもらえる「権利」。商品を売った時に発生。</li>
                    <li><strong>買掛金（負債）</strong>：代金を後で払う「義務」。商品を仕入れた時に発生。</li>
                    <li><strong>回収時</strong>：(借) 現金 100 ／ (貸) 売掛金 100（資産である権利を消す）</li>
                    <li><strong>支払時</strong>：(借) 買掛金 100 ／ (貸) 現金 100（負債である義務を消す）</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 3: 仕訳3ステップ -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="NCw9voGKR74" style="background-image: url('https://img.youtube.com/vi/NCw9voGKR74/maxresdefault.jpg'), url('https://img.youtube.com/vi/NCw9voGKR74/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag gold">鉄則</span>
                <h3 class="lc-title">迷わない！仕訳の解き方3ステップ</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-list-check"></i> 手順をルーチン化する</h4>
                <ul class="lc-list">
                    <li><strong>Step 1</strong>：取引を読んで、何が増えたか・何が減ったかを探す。</li>
                    <li><strong>Step 2</strong>：それが「5つの要素（資産・負債・純資産・収益・費用）」のどれかを判断。</li>
                    <li><strong>Step 3</strong>：増減に応じて左右（借方・貸方）を決める。資産増加＝借方、負債増加＝貸方など。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- talk-scene：掛取引あるある -->
    <div class="talk-scene" style="margin: 24px 0;">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">売掛金と売上、どちらも「売った時」に出てくるので混乱します…</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">「売上」は<strong>今日儲けたという事実（収益）</strong>、「売掛金」は<strong>まだもらっていないお金の権利（資産）</strong>です。<br>掛取引では、この2つが同時に生まれます。商売の成果と、まだ手元にないお金——これを分けて記録するのが複式簿記の面白さですよ。</div>
        </div>
    </div>

    <!-- NotebookLM 実習：前半 -->
    <div class="practice-area">
        <h3><i class="fa-solid fa-robot"></i> 実習：掛取引の理解を深める</h3>
        <p>3本の動画をNotebookLMに読み込ませ、分記法・掛取引の仕訳を自分の言葉で整理しましょう。</p>
        <div style="background:#fffbeb; border:1px solid #fde68a; border-radius:12px; padding:20px; margin-bottom:24px;">
            <h4 style="color:#b45309; margin:0 0 12px; display:flex; align-items:center; gap:8px;">
                <i class="fa-solid fa-book"></i> NotebookLM へのソース追加手順
            </h4>
            <ol style="margin:0; padding-left:1.4rem; font-size:0.92rem; line-height:1.9; color:#78350f;">
                <li><strong>notebooklm.google.com</strong> にアクセスし「新しいノートブック」を作成</li>
                <li>画面左の「ソースを追加」をクリックし「YouTube」または「URL」を選択</li>
                <li>動画① <code>https://www.youtube.com/watch?v=BboEGZR2XV8</code> を追加</li>
                <li>動画② <code>https://www.youtube.com/watch?v=qHPXHWv8xws</code> を追加</li>
                <li>動画③ <code>https://www.youtube.com/watch?v=NCw9voGKR74</code> を追加</li>
                <li>追加完了後、下のプロンプトをコピーして選択式テストを作成する</li>
            </ol>
        </div>
        <div class="chat-prompt-container">
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">読み込んだ動画①〜③の内容をもとに、Day5前半（分記法・売掛金・買掛金・掛取引の発生と回収・支払・仕訳の3ステップ）の理解度を確認する選択式テストを20問作ってください。各問は4択にし、解答は最後にまとめて表示してください。</span></div>
                <button class="copy-btn" title="プロンプトをコピー"><i class="fa-regular fa-copy"></i> コピー</button>
            </div>
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">上の20問テストの中から特に「売掛金か買掛金か」「資産か負債か」「借方か貸方か」の判断を問う問題だけを10問に絞り、各問に「なぜその答えになるのか」を2文で添えてください。</span></div>
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
            後半（返品・在庫・カード）へ進む <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>
</div>
```

---

## 5. 後半タブ（返品・在庫・カード決済）

```html
<div id="second" class="tab-content">

    <!-- Bloomナラティブ：5日目・午後 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 5日目・午後</span>
        <p>
            「あ、このお皿、角が欠けてる…」仕入れた商品に不備が見つかり、業者さんに返品することに。<br>
            さらに、ネットショップでの販売分を発送するための送料も発生しました。<br><br>
            <strong>返品や送料（諸掛）</strong>、そして在庫の数え方など、
            商売を続ける上で避けて通れない「実務のルール」を学びましょう。
        </p>
    </div>

    <h2>セクション2：返品・諸掛と在庫管理</h2>

    <!-- 先入先出法フロービジュアル（D：ビジュアルフロー） -->
    <div style="background:#f8fafc; border:1px solid #e2e8f0; border-radius:12px; padding:24px; margin-bottom:28px;">
        <h4 style="margin:0 0 16px; color:var(--accent-green);"><i class="fa-solid fa-route"></i> 先入先出法（FIFO）の払い出し計算ロジック</h4>
        <div style="display:flex; gap:12px; align-items:center; flex-wrap:wrap; justify-content:center;">
            <div style="background:#ecfdf5; border:2px solid var(--accent-green); border-radius:10px; padding:14px 18px; text-align:center; min-width:140px;">
                <div style="font-size:0.78rem; color:var(--accent-green); font-weight:700; margin-bottom:6px;">STEP 1：古い在庫から</div>
                <div style="font-size:0.9rem; font-weight:700;">先に仕入れた分を<br>先に払い出す</div>
                <div style="font-size:0.78rem; color:var(--text-sub); margin-top:4px;">古い単価 × 個数</div>
            </div>
            <div style="font-size:1.5rem; color:#94a3b8;">→</div>
            <div style="background:#fef3c7; border:2px solid var(--accent-gold); border-radius:10px; padding:14px 18px; text-align:center; min-width:140px;">
                <div style="font-size:0.78rem; color:var(--accent-gold); font-weight:700; margin-bottom:6px;">STEP 2：足りなければ</div>
                <div style="font-size:0.9rem; font-weight:700;">次の仕入れ分で<br>補充する</div>
                <div style="font-size:0.78rem; color:var(--text-sub); margin-top:4px;">新しい単価 × 残り個数</div>
            </div>
            <div style="font-size:1.5rem; color:#94a3b8;">→</div>
            <div style="background:#eff6ff; border:2px solid #3b82f6; border-radius:10px; padding:14px 18px; text-align:center; min-width:140px;">
                <div style="font-size:0.78rem; color:#3b82f6; font-weight:700; margin-bottom:6px;">払い出し欄の記入は</div>
                <div style="font-size:0.9rem; font-weight:700;">必ず「原価」で！<br>売価では❌</div>
                <div style="font-size:0.78rem; color:var(--text-sub); margin-top:4px;">残高も同様に原価で</div>
            </div>
        </div>
    </div>

    <!-- Learning Card 4: 返品と諸掛 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="8YeucmRVqbw" style="background-image: url('https://img.youtube.com/vi/8YeucmRVqbw/maxresdefault.jpg'), url('https://img.youtube.com/vi/8YeucmRVqbw/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag gold">重要</span>
                <h3 class="lc-title">返品と諸掛（送料）の処理ルール</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-truck-ramp-box"></i> ここがテストに出る！</h4>
                <ul class="lc-list">
                    <li><strong>返品（仕入戻し）</strong>：仕入時の仕訳をそのまま逆にするだけ。</li>
                    <li><strong>仕入諸掛（運賃等）</strong>：原則として <strong>仕入原価に含める</strong>（仕入勘定に加算）。</li>
                    <li><strong>売上諸掛</strong>：当社負担の場合は「発送費」等の費用科目で別処理。</li>
                    <li>「仕入は原価にプラス、売上は別科目」——この対比を覚える。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 5: 商品有高帳 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="fai9jxaXIho" style="background-image: url('https://img.youtube.com/vi/fai9jxaXIho/maxresdefault.jpg'), url('https://img.youtube.com/vi/fai9jxaXIho/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag">帳簿</span>
                <h3 class="lc-title">商品有高帳：先入先出法のマスター</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-boxes-packing"></i> 在庫計算のロジック</h4>
                <ul class="lc-list">
                    <li><strong>商品有高帳</strong>：商品の在庫が「いつ・いくらで・何個」動いたかを管理する補助簿。</li>
                    <li><strong>先入先出法（FIFO）</strong>：古い仕入れ分から先に売れたとみなして計算。</li>
                    <li><strong>最重要注意</strong>：払い出し欄は「原価」で記入。問題文に売価が書いてあっても惑わされない。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 6: クレジット売掛金 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="wmihafdKezA" style="background-image: url('https://img.youtube.com/vi/wmihafdKezA/maxresdefault.jpg'), url('https://img.youtube.com/vi/wmihafdKezA/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag blue">現代実務</span>
                <h3 class="lc-title">クレジット売掛金と支払手数料</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-credit-card"></i> カード決済の流れ</h4>
                <ul class="lc-list">
                    <li><strong>クレジット売掛金（資産）</strong>：信販会社から後日受け取れるお金の権利。</li>
                    <li><strong>支払手数料（費用）</strong>：カード利用時に信販会社へ払う手数料。</li>
                    <li><strong>売上時（手数料3%の場合）</strong>：(借) クレジット売掛金 9,700、支払手数料 300 ／ (貸) 売上 10,000</li>
                    <li><strong>入金時</strong>：(借) 当座預金 9,700 ／ (貸) クレジット売掛金 9,700</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- talk-scene：諸掛あるある -->
    <div class="talk-scene" style="margin: 24px 0;">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">仕入れた時の送料を「仕入」に含めるのが、どうしても慣れません。</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">「その商品を使える状態にするまでにかかったコスト」を原価と考えるからです。<br>100円のお皿を10円の送料で仕入れたなら、そのお皿の仕入原価は自分にとって110円ですよね？そう考えると納得しやすいですよ。</div>
        </div>
    </div>

    <!-- 本日の結論コミック枠 -->
    <div class="conclusion-comic">
        <span class="comic-label">✍️ DAY 5 の核心</span>
        <p>掛取引は「いつかお金が動く約束」——<br>
        権利（売掛金）と義務（買掛金）を正確に記録することが、健全な商売の基礎。</p>
    </div>

    <!-- NotebookLM 実習：後半 -->
    <div class="practice-area">
        <h3><i class="fa-solid fa-clipboard-check"></i> 実習：返品・在庫管理・カード決済を問題で確認</h3>
        <p>動画④〜⑥をNotebookLMに追加し、諸掛の処理ルールと先入先出法の計算力を鍛えましょう。</p>
        <div style="background:#fffbeb; border:1px solid #fde68a; border-radius:12px; padding:20px; margin-bottom:24px;">
            <h4 style="color:#b45309; margin:0 0 12px; display:flex; align-items:center; gap:8px;">
                <i class="fa-solid fa-book"></i> NotebookLM へのソース追加手順
            </h4>
            <ol style="margin:0; padding-left:1.4rem; font-size:0.92rem; line-height:1.9; color:#78350f;">
                <li><strong>notebooklm.google.com</strong> にアクセスし、前半と同じノートブックを開く</li>
                <li>画面左の「ソースを追加」をクリックし「YouTube」または「URL」を選択</li>
                <li>動画④ <code>https://www.youtube.com/watch?v=8YeucmRVqbw</code> を追加</li>
                <li>動画⑤ <code>https://www.youtube.com/watch?v=fai9jxaXIho</code> を追加</li>
                <li>動画⑥ <code>https://www.youtube.com/watch?v=wmihafdKezA</code> を追加</li>
                <li>追加完了後、下のプロンプトをコピーして選択式テストを作成する</li>
            </ol>
        </div>
        <div class="chat-prompt-container">
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">読み込んだ動画④〜⑥の内容をもとに、Day5後半（商品の返品・仕入諸掛と売上諸掛の違い・商品有高帳の先入先出法・クレジット売掛金と支払手数料）の理解度を確認する選択式テストを20問作ってください。各問は4択にし、解答は最後にまとめて表示してください。</span></div>
                <button class="copy-btn" title="プロンプトをコピー"><i class="fa-regular fa-copy"></i> コピー</button>
            </div>
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">先入先出法（FIFO）の計算に特化した問題を5問作ってください。各問に「仕入日・単価・数量」の具体的な数字を設定し、払い出し時の計算過程と残高の求め方を解説付きで示してください。</span></div>
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

## 6. まとめタブ（完全版）

```html
<div id="summary" class="tab-content">

    <!-- Bloomナラティブ：5日目・夜 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 5日目・夜</span>
        <p>
            「ツケで売った」「返品があった」「カードで払ってもらった」——<br>
            今日だけでこんなに多様な取引が生まれました。<br><br>
            <strong>お金が動く瞬間だけじゃなく、「約束」も記録する。</strong><br>
            それが複式簿記の誠実さです。
        </p>
    </div>

    <h2>DAY 5 まとめ</h2>
    <div class="summary-grid">
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-handshake"></i></div>
            <h3>掛取引は「権利」と「義務」</h3>
            <p>売掛金は資産、買掛金は負債。お金を後でもらう約束、払う約束を明確に区別して仕訳します。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-rotate-left"></i></div>
            <h3>返品は「逆再生」</h3>
            <p>返品が起きたら、仕入・売上の仕訳をそのままひっくり返して反対側に書くことで打ち消します。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-truck"></i></div>
            <h3>仕入諸掛は「本体価格」に</h3>
            <p>仕入れ時の送料は「仕入」に含める。売り上げ時の送料は「発送費」など別科目。この区別が最重要です。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-boxes-stacked"></i></div>
            <h3>商品有高帳は「原価」で書く</h3>
            <p>在庫管理ノートには、売れた時の値段（売価）ではなく、仕入れた時の値段（原価）を記録します。</p>
        </div>
    </div>

    <h2>重要用語チェック</h2>
    <div class="term-grid">
        <div class="term-card"><strong>掛取引（かけとりひき）</strong><span>商品の売買代金をその場ではなく後日まとめて受け払いする取引形態。</span></div>
        <div class="term-card"><strong>売掛金</strong><span>商品を掛けで売った際に発生する「後日受け取る権利」。資産のグループに属する。</span></div>
        <div class="term-card"><strong>買掛金</strong><span>商品を掛けで仕入れた際に発生する「後日払う義務」。負債のグループに属する。</span></div>
        <div class="term-card"><strong>分記法（ぶんきほう）</strong><span>商品売買を「商品（資産）」と「商品売買益（収益）」に分けて記録する方法。</span></div>
        <div class="term-card"><strong>商品売買益</strong><span>分記法で、売値と原価の差額として計上する収益科目。</span></div>
        <div class="term-card"><strong>仕入戻し・売上戻り</strong><span>返品の簿記用語。仕入の返品が「仕入戻し」、売上の返品が「売上戻り」。逆仕訳で処理する。</span></div>
        <div class="term-card"><strong>仕入諸掛（しいれしょがかり）</strong><span>商品仕入れ時にかかる運賃・保険料等。原則として仕入原価に含める。</span></div>
        <div class="term-card"><strong>売上諸掛（発送費）</strong><span>商品売上時に当社が負担する発送費用。仕入とは別に費用科目で処理する。</span></div>
        <div class="term-card"><strong>商品有高帳</strong><span>商品の在庫の入出庫を「数量・単価・金額」で管理する補助簿。原価で記録する。</span></div>
        <div class="term-card"><strong>先入先出法（FIFO）</strong><span>古く仕入れた商品から順に売れたとみなして在庫計算を行う方法。</span></div>
        <div class="term-card"><strong>クレジット売掛金</strong><span>クレジットカード決済時に信販会社から後日受け取る金額の権利。資産に属する。</span></div>
        <div class="term-card"><strong>支払手数料</strong><span>クレジット決済等で信販会社に支払う手数料。費用科目として処理する。</span></div>
    </div>

    <div class="check-panel">
        <h3><i class="fa-solid fa-clipboard-check"></i> セルフチェック</h3>
        <ol class="check-list">
            <li>掛取引の仕訳を「発生時・回収時（または支払時）」の2段階それぞれで、正しく書けますか？</li>
            <li>仕入諸掛（送料）と売上諸掛（発送費）がなぜ別々の処理になるのか、理由とともに説明できますか？</li>
            <li>先入先出法の問題で「80個売れた（古い在庫50個@100円、新しい在庫@110円）」という設定で、払い出し金額と残高を正しく計算できますか？</li>
        </ol>
    </div>

    <!-- 確認クイズ -->
    <div class="quiz-panel" id="quizPanel">
        <h3><i class="fa-solid fa-circle-question"></i> 確認クイズ（全5問）</h3>
        <p style="color:var(--text-sub); font-size:0.9rem; margin-bottom:20px;">今日の掛取引・諸掛・在庫管理・クレジット決済の理解度を確認しましょう。</p>
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
        <h4 style="color:#047857; margin:0 0 10px; font-size:1.15rem;"><i class="fa-solid fa-forward"></i> 次回予告：Day 6</h4>
        <p style="margin:0; font-size:0.95rem; color:#333; line-height:1.6;">次回は「手形と電子記録債権」へ。Bloomが取引先と約束手形でやり取りするようになり、受取手形・支払手形・電子記録債権の仕訳を学びます。</p>
    </div>
</div>
```

---

## 7. ページ末尾ナビボタン（summaryタブの外・scriptより前）

```html
<div style="text-align:center; padding: 3rem 0 2rem; display: flex; flex-direction: column; gap: 1rem; align-items: center;">
    <button type="button" class="tool-link-btn" style="padding: 1.2rem 4rem; font-size:1.25rem; background:linear-gradient(135deg, #34d399, #059669); border:none; box-shadow: 0 10px 30px rgba(5,150,105, 0.25); cursor:pointer;" onclick="(async function(){try{const r=await fetch('./vol06-1.html',{method:'HEAD',cache:'no-store'});if(r.ok){window.location.href='./vol06-1.html';return;}}catch(e){}alert('Day 6 は準備中です。もうしばらくお待ちください。');})()">
        Day 6 へ進む <i class="fa-solid fa-arrow-right"></i>
    </button>
    <button type="button" class="tool-link-btn" style="padding: 1.2rem 4rem; font-size:1.25rem; background:#fff; color:#4a4a4a; border:2px solid #e2e8f0; box-shadow:none; cursor:pointer;" onclick="window.location.href='./index.html'">
        <i class="fa-solid fa-house"></i> 学習記録をつけて Home へ戻る
    </button>
</div>
```

---

## 8. quizData（ページ末尾の `<script>` ブロック内に記述）

> ⚠️ 注意：quizData は `<div id="summary">` の中ではなく、  
> ページ末尾の `<script>` ブロック（`openTab()`・`buildQuiz()` 等の関数定義と同じ場所）に記述すること。

```javascript
const quizData = [
    {
        q: "Bloomが取引先に雑貨を掛けで仕入れた。正しい仕訳はどれ？",
        opts: [
            "(借) 仕入 100 ／ (貸) 現金 100",
            "(借) 仕入 100 ／ (貸) 買掛金 100",
            "(借) 売掛金 100 ／ (貸) 売上 100",
            "(借) 買掛金 100 ／ (貸) 仕入 100"
        ],
        ans: 1,
        fb: "掛けで仕入れると「後で払う義務（負債）」が生まれます。これが買掛金です。現金はまだ動いていないので貸方に現金は入りません。"
    },
    {
        q: "Bloomが10,000円の商品を仕入れ、送料500円を現金で支払った。仕入勘定の金額は？",
        opts: ["10,000円", "500円", "10,500円", "9,500円"],
        ans: 2,
        fb: "仕入諸掛（送料）は仕入原価に含めます。Bloomにとってその商品の原価は「10,000 + 500 = 10,500円」となります。"
    },
    {
        q: "商品有高帳（先入先出法）で「80個売れた」時、まず@100円の在庫50個を払い出した後、残り30個をどう処理する？",
        opts: [
            "売価の単価で計算する",
            "50個と同じ@100円を使う",
            "次に仕入れた単価（@110円など）で計算する",
            "払い出しをいったん止めて在庫確認する"
        ],
        ans: 2,
        fb: "先入先出法は「古い在庫から先に使う」ルールです。@100円の50個が尽きたら、次に仕入れた単価の在庫から残り30個を払い出します。"
    },
    {
        q: "Bloomでお客さんがカード払いで10,000円の商品を購入。手数料3%の場合、売上時の仕訳はどれ？",
        opts: [
            "(借) 現金 10,000 ／ (貸) 売上 10,000",
            "(借) クレジット売掛金 10,000 ／ (貸) 売上 10,000",
            "(借) クレジット売掛金 9,700, 支払手数料 300 ／ (貸) 売上 10,000",
            "(借) 売掛金 9,700 ／ (貸) 売上 9,700"
        ],
        ans: 2,
        fb: "カード決済では現金はもらえず、信販会社への請求権（クレジット売掛金）が生まれます。手数料は支払手数料として費用計上し、売上は全額10,000円のまま計上するのがポイントです。"
    },
    {
        q: "Bloomが仕入れた商品を一部返品（仕入戻し）した。買掛金の処理はどうなる？",
        opts: [
            "買掛金を借方に記入して、負債を減らす",
            "買掛金を貸方に記入して、負債を増やす",
            "買掛金の仕訳は不要",
            "買掛金を資産として計上する"
        ],
        ans: 0,
        fb: "返品は仕入時の逆仕訳です。仕入時は「(借)仕入 ／ (貸)買掛金」だったので、逆にすると「(借)買掛金 ／ (貸)仕入」となります。借方に書くことで負債が減ります。"
    }
];
```

---

## 9. 実装チェックリスト

- [ ] vol04-1.html を vol05-1.html としてコピーして開始
- [ ] タイトル・ヘッダー・プログレスバー（38%）・DAY番号（05）を変更
- [ ] goalタブ：Bloom 5日目・朝ナラティブ、Whyボックス、Talk-scene、goal-box、フローマップ（5ステップ）、AI Director's Eye
- [ ] 前半タブ：Bloom午前ナラティブ、分記法・売掛買掛・仕訳3ステップの学習カード3枚、talk-scene（売掛金vs売上）、NotebookLM実習（URL手順書＋プロンプト2個）
- [ ] 後半タブ：Bloom午後ナラティブ、FIFOフロービジュアル（3ステップ）、学習カード3枚、talk-scene（諸掛）、conclusion-comic、NotebookLM実習（URL手順書＋プロンプト2個）
- [ ] まとめタブ：Bloom夜ナラティブ、summary-grid（4枚）、term-grid（12用語）、check-panel（3項目）、quiz-panel（UI部分のみ・quizDataはscript内）、次回予告ボックス（Day 6）
- [ ] ページ末尾ナビ：Day 6 async fetchボタン＋Home へ戻るボタン（vol04と同じスタイル）
- [ ] ページ末尾 `<script>`：quizData（5問）・buildQuiz()・answerQuiz()・showResult()・resetQuiz()・buildQuiz()呼び出しを記述
- [ ] cache-bust: `<!-- cache-bust: 2026-05-14T09:00:00 -->`（ファイル末尾）
- [ ] .deploy_tmp/vol05-1.html に同じファイルをコピー（内容を必ず同期）
- [ ] index.html の Day 5 リンクが vol05-1.html を正しく指しているか確認
