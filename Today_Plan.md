# Day 7 実装計画書 — 試算表の作成と活用

作成日: 2026-05-15
実装者: Antigravity / Cursor

---

## 0. Day 7 の位置づけ

| 項目 | 値 |
|---|---|
| ファイル名 | vol07-1.html |
| ページタイトル | Day 7 \| 試算表の作成と活用 |
| プログレスバー | **54%**（7/13 ≈ 53.8%） |
| cache-bust | `2026-05-15T15:00:00` |
| 前半動画（3本） | TqX3L-HbPEU / ibMfbGE8F-w / O0QkZ6yHkgQ |
| 後半動画（2本） | Wqkafa3IxqY / WQ2fMu3K81s |

> **注意**：今回は動画が計5本（後半2本）。後半タブの学習カードは2枚構成にすること。

**前半の役割**：試算表の目的・3種類の区別・T勘定からの集計手順・二重仕訳の消し込み・売掛買掛明細表を習得する  
**後半の役割**：試算表を「経営判断ツール」として活用する視点（売上・利益・キャッシュの読み方、月次試算表の鮮度管理、発生主義）を学ぶ

---

## 1. テンプレート複製手順

1. `vol06-1.html` を `vol07-1.html` にコピーして作業開始
2. 以下の固定値を一括置換する

| 変更箇所 | 変更前 | 変更後 |
|---|---|---|
| `<title>` | Day 6 \| 固定資産と減価償却 | Day 7 \| 試算表の作成と活用 |
| ヘッダータグ | DAY 06 | DAY 07 |
| `<h1>` | 固定資産と減価償却 | 試算表の作成と活用 |
| プログレスバー width | 46% | 54% |
| cache-bust | `2026-05-15T09:00:00` | `2026-05-15T15:00:00` |
| Day 7 ナビボタン onclick | `fetch('./vol07-1.html'...)` → alert "Day 7 は準備中" | `fetch('./vol08-1.html'...)` → alert "Day 8 は準備中" |

3. `.deploy_tmp/vol07-1.html` にも**同じ内容で**コピーし、vol07-1.html と完全同期させること

---

## 2. タブ構成概要

```
タブ1: 今日の目標 (id="goal")
  アイコン: fa-table-list
  ラベル: 今日の目標

タブ2: 前半：試算表の作成と検証 (id="first")
  アイコン: fa-magnifying-glass-chart
  ラベル: 前半：試算表の作成と検証

タブ3: 後半：試算表を経営判断に活かす (id="second")
  アイコン: fa-chart-line
  ラベル: 後半：経営判断ツールとしての試算表

タブ4: まとめ (id="summary")
  アイコン: fa-list-check
  ラベル: まとめ
```

---

## 3. goalタブ 完全HTML

```html
<div id="goal" class="tab-content active">

    <!-- Bloomナラティブ：7日目・朝 -->
    <div class="bloom-story">
        <span class="bloom-story-label">📍 Bloom 7日目・朝</span>
        <p>
            月末を迎えたBloom。Harukaさんは1ヶ月分の仕訳帳と元帳を前に、<br>
            「この数字、どこか間違えてないかな…」と不安になっています。<br><br>
            そこで使うのが<strong>試算表（TB）</strong>——帳簿の「健康診断」のような存在です。<br>
            転記ミスを早期発見し、月次の経営状態を一枚で把握する力を身につけましょう。
        </p>
    </div>

    <!-- Whyボックス -->
    <div class="why-box">
        <div class="why-q">なぜわざわざ「試算表」を作るの？決算でまとめればよくない？</div>
        <div class="why-a">
            1年分ためてから間違いを探すのは、1年分の針の山から1本の針を探すようなもの。<br>
            <strong>毎月1回試算表を作れば、ミスを1ヶ月以内に発見</strong>できます。<br>
            さらに経営者への月次レポートとして、<strong>売上・利益・現金の今を一目で伝えられます。</strong>
        </div>
    </div>

    <!-- talk-scene：あるある疑問 -->
    <div class="talk-scene">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">試験問題に「TB」って書いてあったんですが、これって何ですか？</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">Trial Balance Sheet の略です。試算表のことですよ。<br>試験の下書き用紙でもよく使われる略称なので、<strong>TB = 試算表</strong>と即反応できるようにしておきましょう。</div>
        </div>
    </div>

    <!-- goal-box -->
    <div class="goal-box">
        <i class="fa-solid fa-table-list"></i>
        <h3>「試算表の3種類を作成し、ミスの検証と経営判断に活かせる」</h3>
        <p>合計・残高・合計残高の3つの試算表を作成し、二重仕訳の消し込みや明細表作成まで対応できる力と、<br>試算表から経営の異常値を読み取る実務力を身につけます。</p>
    </div>

    <h2>本日の学習マップ</h2>
    <p>Day 7は「帳簿を集計・検証し、経営判断に活かす」を学びます。簿記の一連の流れのうち「決算整理前の確認」にあたる重要ステップです。</p>

    <!-- フローマップ（D：ビジュアルフロー） -->
    <div class="flow-map">
        <div class="flow-step">
            <i class="fa-solid fa-pen-to-square"></i>
            <strong>日々の仕訳</strong>
            <span>仕訳帳へ記録</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-right-left"></i>
            <strong>転記</strong>
            <span>総勘定元帳（T勘定）</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-table-list"></i>
            <strong>試算表の作成</strong>
            <span>合計・残高・合計残高</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-check-double"></i>
            <strong>貸借の検証</strong>
            <span>借方＝貸方で確認</span>
        </div>
        <div class="flow-step">
            <i class="fa-solid fa-chart-line"></i>
            <strong>経営判断へ</strong>
            <span>売上・利益・現金を読む</span>
        </div>
    </div>

    <div style="background:#fffbeb; padding:25px 30px; border-radius:12px; margin:35px 0 20px; border-left:6px solid var(--accent-gold);">
        <h4 style="color:#b45309; margin:0 0 10px; font-size:1.15rem;"><i class="fa-solid fa-lightbulb"></i> AI Director's Eye：会計ソフトとTBの自動生成</h4>
        <p style="margin:0; font-size:0.95rem; color:#333; line-height:1.7;">freeeやマネーフォワードでは、仕訳を入力するだけで残高試算表が自動生成されます。ただしAIが生成した試算表でも「現金残高が異常に多い」「利益率が急変している」などの異常値を発見して意味を説明できるのは人間の仕事。今日学ぶ「試算表の読み方」はAI時代の経理の核心スキルです。</p>
    </div>

    <div style="text-align:center; margin-top:30px;">
        <button class="tool-link-btn" onclick="openTab('first')" style="background:var(--accent-green);">
            前半（試算表の作成と検証）へ進む <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>
</div>
```

---

## 4. firstタブ 完全HTML

```html
<div id="first" class="tab-content">

    <!-- Bloomナラティブ：7日目・午前 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 7日目・午前</span>
        <p>
            Harukaさんは6月分のT勘定を広げ、試算表を作り始めました。<br>
            「合計を書くの？差額を書くの？どっちが正しいんだろう…」<br><br>
            実は試算表には3種類あって、どれを作っても<strong>借方合計と貸方合計は必ず一致</strong>します。<br>
            そして問題文の資料が「項目別」か「日付別」かで、解き方がガラリと変わります。
        </p>
    </div>

    <h2>セクション1：試算表の3種類と作成手順</h2>

    <!-- 3種類比較ビジュアル -->
    <div style="background:#f8fafc; border:1px solid #e2e8f0; border-radius:12px; padding:24px; margin-bottom:28px;">
        <h4 style="margin:0 0 16px; color:var(--accent-green);"><i class="fa-solid fa-table-list"></i> 試算表3種類の比較</h4>
        <div style="display:flex; gap:12px; align-items:stretch; flex-wrap:wrap; justify-content:center;">
            <div style="background:#ecfdf5; border:2px solid var(--accent-green); border-radius:10px; padding:16px 18px; text-align:center; min-width:160px; flex:1;">
                <div style="font-size:0.85rem; color:var(--accent-green); font-weight:700; margin-bottom:8px;">① 合計試算表</div>
                <div style="font-size:0.9rem; font-weight:700; margin-bottom:6px;">借方合計 ／ 貸方合計</div>
                <div style="font-size:0.78rem; color:var(--text-sub); line-height:1.6;">各T勘定の合計をそのまま転記。転記ミスを見つけやすい。</div>
            </div>
            <div style="background:#fef3c7; border:2px solid var(--accent-gold); border-radius:10px; padding:16px 18px; text-align:center; min-width:160px; flex:1;">
                <div style="font-size:0.85rem; color:var(--accent-gold); font-weight:700; margin-bottom:8px;">② 残高試算表</div>
                <div style="font-size:0.9rem; font-weight:700; margin-bottom:6px;">差額（残高）のみ記入</div>
                <div style="font-size:0.78rem; color:var(--text-sub); line-height:1.6;">B/S・P/Lに近い形。現在の各科目の残高が一目でわかる。</div>
            </div>
            <div style="background:#eff6ff; border:2px solid #3b82f6; border-radius:10px; padding:16px 18px; text-align:center; min-width:160px; flex:1;">
                <div style="font-size:0.85rem; color:#3b82f6; font-weight:700; margin-bottom:8px;">③ 合計残高試算表</div>
                <div style="font-size:0.9rem; font-weight:700; margin-bottom:6px;">合計 ＋ 残高の両方</div>
                <div style="font-size:0.78rem; color:var(--text-sub); line-height:1.6;">情報量が最多。①②のメリットを兼ね備えた実務で最も使われる形式。</div>
            </div>
        </div>
        <div style="margin-top:14px; background:#fff; border:1px solid #d1fae5; border-radius:8px; padding:12px 16px; font-size:0.9rem; color:#065f46;">
            <strong>貸借平均の原理</strong>：仕訳と転記が正しければ、どの試算表でも借方合計＝貸方合計は必ず一致する。一致しない場合はどこかにミスあり。
        </div>
    </div>

    <!-- Learning Card 1: 試算表の基本概念と3種類 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="TqX3L-HbPEU" style="background-image: url('https://img.youtube.com/vi/TqX3L-HbPEU/maxresdefault.jpg'), url('https://img.youtube.com/vi/TqX3L-HbPEU/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag">基礎概念</span>
                <h3 class="lc-title">試算表とは何か：目的と3種類の使い分け</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-table-list"></i> 試算表（TB）の役割</h4>
                <ul class="lc-list">
                    <li><strong>目的</strong>：仕訳帳→総勘定元帳への転記ミスを検証するための集計表。</li>
                    <li><strong>作成時期</strong>：通常は月次（1ヶ月ごと）または期末決算時。</li>
                    <li><strong>TB = Trial Balance Sheet</strong>（試験の下書きでよく使われる略称）。</li>
                    <li>どの種類を作っても<strong>借方合計＝貸方合計</strong>が成立すれば転記は正しい（貸借平均の原理）。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 2: 集計実務の基礎演習 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="ibMfbGE8F-w" style="background-image: url('https://img.youtube.com/vi/ibMfbGE8F-w/maxresdefault.jpg'), url('https://img.youtube.com/vi/ibMfbGE8F-w/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag gold">最重要</span>
                <h3 class="lc-title">T勘定から試算表へ：集計作業の手順</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-calculator"></i> 集計の3ステップ</h4>
                <ul class="lc-list">
                    <li><strong>Step1</strong>：各T勘定の借方合計・貸方合計を計算する（合計試算表の数値）。</li>
                    <li><strong>Step2</strong>：差額（残高）を計算し、大きい側の欄に記入（残高試算表の数値）。</li>
                    <li><strong>Step3</strong>：合計と残高の両方を記入（合計残高試算表）→最後に貸借合計が一致するか確認。</li>
                    <li>前月末の試算表残高は、次月のT勘定の<strong>スタート金額として最初に書き写す</strong>こと（忘れがち！）。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 3: 二重仕訳消し込み＆明細表 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="O0QkZ6yHkgQ" style="background-image: url('https://img.youtube.com/vi/O0QkZ6yHkgQ/maxresdefault.jpg'), url('https://img.youtube.com/vi/O0QkZ6yHkgQ/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag blue">試験頻出</span>
                <h3 class="lc-title">二重仕訳の消し込みと売掛・買掛明細表の作成</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-xmark"></i> 2パターンの対策を分けて覚える</h4>
                <ul class="lc-list">
                    <li><strong>【項目別パターン】</strong>→ 重複取引（二重仕訳）に注意。金額が一致する取引を両グループから探してバツで消してから仕訳。</li>
                    <li><strong>【日付別パターン】</strong>→ 二重仕訳は発生しない。その代わり<strong>売掛金・買掛金明細表の同時作成</strong>が必要。仕訳に「会社名」を書き添えるのが必須。</li>
                    <li>最終確認：「残高試算表の売掛金残高＝明細表の各社合計」が一致すれば正しい。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- talk-scene：二重仕訳あるある -->
    <div class="talk-scene" style="margin: 24px 0;">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">項目別の問題で、同じ取引が複数グループに出てきます。どれを消せばいいか迷います…</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">まず<strong>金額が一致するもの</strong>をペアで探します。「現金による仕入れ300円」と「仕入れ代金の現金支払い300円」は同じ取引なので、どちらか一方を消します。<br>消す前に「これは同じ取引か？」と必ず自問する癖をつけましょう。</div>
        </div>
    </div>

    <!-- NotebookLM 実習：前半 -->
    <div class="practice-area">
        <h3><i class="fa-solid fa-robot"></i> 実習：試算表の作成を問題で確認</h3>
        <p>3本の動画をNotebookLMに読み込ませ、T勘定からの集計・二重仕訳の消し込み・明細表作成の手順を整理しましょう。</p>
        <div style="background:#fffbeb; border:1px solid #fde68a; border-radius:12px; padding:20px; margin-bottom:24px;">
            <h4 style="color:#b45309; margin:0 0 12px; display:flex; align-items:center; gap:8px;">
                <i class="fa-solid fa-book"></i> NotebookLM へのソース追加手順
            </h4>
            <ol style="margin:0; padding-left:1.4rem; font-size:0.92rem; line-height:1.9; color:#78350f;">
                <li><strong>notebooklm.google.com</strong> にアクセスし「新しいノートブック」を作成</li>
                <li>画面左の「ソースを追加」をクリックし「YouTube」または「URL」を選択</li>
                <li>動画① <code>https://www.youtube.com/watch?v=TqX3L-HbPEU</code> を追加</li>
                <li>動画② <code>https://www.youtube.com/watch?v=ibMfbGE8F-w</code> を追加</li>
                <li>動画③ <code>https://www.youtube.com/watch?v=O0QkZ6yHkgQ</code> を追加</li>
                <li>追加完了後、下のプロンプトをコピーして選択式テストを作成する</li>
            </ol>
        </div>
        <div class="chat-prompt-container">
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">読み込んだ動画①〜③の内容をもとに、Day7前半（試算表の目的・合計/残高/合計残高試算表の作成方法・貸借平均の原理・T勘定からの集計手順・項目別パターンの二重仕訳消し込み・日付別パターンの売掛金買掛金明細表の作成）の理解度を確認する選択式テストを20問作ってください。各問は4択にし、解答は最後にまとめて表示してください。</span></div>
                <button class="copy-btn" title="プロンプトをコピー"><i class="fa-regular fa-copy"></i> コピー</button>
            </div>
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">「項目別に取引が与えられた場合の試算表作成」に特化した演習問題を3問作ってください。各問に重複取引（二重仕訳になる部分）を含む資料を設定し、消し込みの手順・仕訳・T勘定転記・試算表集計の解説を示してください。</span></div>
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
            後半（経営判断ツールとしての試算表）へ進む <i class="fa-solid fa-arrow-right"></i>
        </button>
    </div>
</div>
```

---

## 5. secondタブ 完全HTML

```html
<div id="second" class="tab-content">

    <!-- Bloomナラティブ：7日目・午後 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 7日目・午後</span>
        <p>
            Harukaさんが完成した試算表を眺めています。<br>
            「数字は合ったけど……これ、どう読めばいいんだろう？」<br><br>
            試算表は「作ること」だけがゴールではありません。<br>
            <strong>売上・利益・現金の3点から異常を読み取る力</strong>が、経理の本当の価値です。
        </p>
    </div>

    <h2>セクション2：試算表を経営判断に活かす</h2>

    <!-- 試算表で見るべき3つのポイント ビジュアルフロー -->
    <div style="background:#f8fafc; border:1px solid #e2e8f0; border-radius:12px; padding:24px; margin-bottom:28px;">
        <h4 style="margin:0 0 16px; color:var(--accent-green);"><i class="fa-solid fa-magnifying-glass-chart"></i> 試算表チェックの3ポイント</h4>
        <div style="display:flex; gap:12px; align-items:stretch; flex-wrap:wrap; justify-content:center;">
            <div style="background:#ecfdf5; border:2px solid var(--accent-green); border-radius:10px; padding:16px 18px; text-align:center; min-width:150px; flex:1;">
                <div style="font-size:1.5rem; margin-bottom:8px;">📈</div>
                <div style="font-size:0.85rem; color:var(--accent-green); font-weight:700; margin-bottom:6px;">① 売上高</div>
                <div style="font-size:0.82rem; color:var(--text-sub); line-height:1.6;">前月・前年同月と比較。変動の理由を説明できるか？</div>
            </div>
            <div style="background:#fef3c7; border:2px solid var(--accent-gold); border-radius:10px; padding:16px 18px; text-align:center; min-width:150px; flex:1;">
                <div style="font-size:1.5rem; margin-bottom:8px;">💰</div>
                <div style="font-size:0.85rem; color:var(--accent-gold); font-weight:700; margin-bottom:6px;">② 利益（率）</div>
                <div style="font-size:0.82rem; color:var(--text-sub); line-height:1.6;">利益率が急変していないか。原価の発生タイミングにズレはないか？</div>
            </div>
            <div style="background:#eff6ff; border:2px solid #3b82f6; border-radius:10px; padding:16px 18px; text-align:center; min-width:150px; flex:1;">
                <div style="font-size:1.5rem; margin-bottom:8px;">🏦</div>
                <div style="font-size:0.85rem; color:#3b82f6; font-weight:700; margin-bottom:6px;">③ 現預金</div>
                <div style="font-size:0.82rem; color:var(--text-sub); line-height:1.6;">現金残高が異常に多い/少ない場合は要注意。売掛金の滞留を確認。</div>
            </div>
        </div>
        <div style="margin-top:14px; background:#fff; border:1px solid #fde68a; border-radius:8px; padding:12px 16px; font-size:0.88rem; color:#78350f;">
            <strong>黒字倒産に注意</strong>：利益が出ていても現預金がゼロになれば会社は倒産する。キャッシュの残高チェックは最重要。
        </div>
    </div>

    <!-- Learning Card 4: 試算表で経営分析 -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="Wqkafa3IxqY" style="background-image: url('https://img.youtube.com/vi/Wqkafa3IxqY/maxresdefault.jpg'), url('https://img.youtube.com/vi/Wqkafa3IxqY/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag">実務応用</span>
                <h3 class="lc-title">試算表から読み解く経営分析と異常値の発見</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-chart-line"></i> 経理の「読み解く力」</h4>
                <ul class="lc-list">
                    <li><strong>売上</strong>：単月だけでなく前月・前年同月と比較し、変動の理由を説明できるようにする。</li>
                    <li><strong>利益率</strong>：業種が変わらなければ利益率は安定。急変した場合は原価の発生タイミングのズレを疑う。</li>
                    <li><strong>キャッシュ</strong>：現金残高の異常な増加は「管理できていない証拠」。売掛金の滞留や不適切な資金流出も確認する。</li>
                    <li>※ この動画は試験範囲外の実務知識も含みます。経理職を目指す方向けのプラスアルファ学習です。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- Learning Card 5: ダメな試算表パターン -->
    <div class="learning-card">
        <div class="lc-video">
            <div class="vc-thumb" data-video-id="WQ2fMu3K81s" style="background-image: url('https://img.youtube.com/vi/WQ2fMu3K81s/maxresdefault.jpg'), url('https://img.youtube.com/vi/WQ2fMu3K81s/hqdefault.jpg');">
                <i class="fa-brands fa-youtube vc-thumb-play"></i>
            </div>
        </div>
        <div class="lc-content">
            <div class="lc-header">
                <span class="vc-tag gold">実務の落とし穴</span>
                <h3 class="lc-title">脱どんぶり！ダメな試算表のパターンから学ぶ</h3>
            </div>
            <div class="lc-body">
                <h4><i class="fa-solid fa-triangle-exclamation"></i> よくある2大NG</h4>
                <ul class="lc-list">
                    <li><strong>NG①：試算表が古い</strong>→ 理想は翌月7日まで。15日を過ぎると銀行融資審査で「管理がずさん」と判断される。</li>
                    <li><strong>NG②：現金主義で計上</strong>→ 支払ったタイミングではなく<strong>使った月に費用を計上する「発生主義」</strong>が正しい月次損益を生む。</li>
                    <li>減価償却費は年1回ではなく年間見積もりを<strong>毎月1/12ずつ前倒し計上</strong>するのが実務のベストプラクティス。</li>
                    <li>※ この動画は試験範囲外の実務知識も含みます。</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- talk-scene：試算表の活用あるある -->
    <div class="talk-scene" style="margin: 24px 0;">
        <div class="talk-bubble owner">
            <div class="talk-icon">🧑</div>
            <div class="talk-text">試算表って「正しいかどうかチェックするだけ」のものじゃないんですね。</div>
        </div>
        <div class="talk-bubble teacher">
            <div class="talk-icon">👩‍🏫</div>
            <div class="talk-text">そうです。作れることは最低条件。<br>「なぜこの数字になったのか」を説明できる経理が、<strong>経営者から信頼されるパーソナルトレーナー</strong>になれます。AIが集計を自動化する時代だからこそ、読み解く力が経理の差別化ポイントですよ。</div>
        </div>
    </div>

    <!-- 本日の結論コミック枠 -->
    <div class="conclusion-comic">
        <span class="comic-label">✍️ DAY 7 の核心</span>
        <p>試算表は「作ること」がゴールではない——<br>
        数字の異常に気づき、理由を説明できる力が、経理の本当の価値。</p>
    </div>

    <!-- NotebookLM 実習：後半 -->
    <div class="practice-area">
        <h3><i class="fa-solid fa-clipboard-check"></i> 実習：試算表の活用と経営判断を深める</h3>
        <p>動画④⑤をNotebookLMに追加し、試算表から読み取るべき経営指標と、実務でよくある試算表の落とし穴を整理しましょう。</p>
        <div style="background:#fffbeb; border:1px solid #fde68a; border-radius:12px; padding:20px; margin-bottom:24px;">
            <h4 style="color:#b45309; margin:0 0 12px; display:flex; align-items:center; gap:8px;">
                <i class="fa-solid fa-book"></i> NotebookLM へのソース追加手順
            </h4>
            <ol style="margin:0; padding-left:1.4rem; font-size:0.92rem; line-height:1.9; color:#78350f;">
                <li><strong>notebooklm.google.com</strong> にアクセスし、前半と同じノートブックを開く</li>
                <li>画面左の「ソースを追加」をクリックし「YouTube」または「URL」を選択</li>
                <li>動画④ <code>https://www.youtube.com/watch?v=Wqkafa3IxqY</code> を追加</li>
                <li>動画⑤ <code>https://www.youtube.com/watch?v=WQ2fMu3K81s</code> を追加</li>
                <li>追加完了後、下のプロンプトをコピーして問いかけ問題を作成する</li>
            </ol>
        </div>
        <div class="chat-prompt-container">
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">読み込んだ動画④⑤の内容をもとに、「試算表から読み取るべき3つのチェックポイント（売上・利益・キャッシュ）」「現金主義と発生主義の違い」「試算表の鮮度（作成タイミング）の重要性」「月次で減価償却を前倒し計上する理由」について、それぞれ3問ずつ合計12問の選択式テストを作ってください。各問は4択にし、解答は最後にまとめて表示してください。</span></div>
                <button class="copy-btn" title="プロンプトをコピー"><i class="fa-regular fa-copy"></i> コピー</button>
            </div>
            <div class="chat-bubble">
                <div class="chat-text"><span class="prompt-text">「Bloomの試算表を見たら、現金残高が先月の3倍になっていた」「売上は先月より増えているのに利益率が大きく下がっていた」という2つのシナリオについて、考えられる原因と経理としての対応策を各シナリオ200字程度で説明してください。</span></div>
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

    <!-- Bloomナラティブ：7日目・夜 -->
    <div class="bloom-story" style="margin-bottom: 24px;">
        <span class="bloom-story-label">📍 Bloom 7日目・夜</span>
        <p>
            完成した試算表をHarukaさんが眺めています。<br>
            借方と貸方がぴったり一致したとき、小さな達成感が生まれました。<br><br>
            <strong>ミスをゼロにする地道な作業が、経営判断の土台を作る。</strong><br>
            Bloomの帳簿は今日も正直に、1ヶ月の軌跡を刻んでいます。
        </p>
    </div>

    <h2>DAY 7 まとめ</h2>
    <div class="summary-grid">
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-table-list"></i></div>
            <h3>試算表は「転記の健康診断」</h3>
            <p>仕訳→元帳への転記ミスを月次で発見するための集計表。合計・残高・合計残高の3種類がある。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-check-double"></i></div>
            <h3>貸借平均の原理</h3>
            <p>どの試算表を作っても、転記が正しければ借方合計＝貸方合計が成立する。一致しなければどこかにミスあり。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-xmark"></i></div>
            <h3>二重仕訳と明細表：2パターン対策</h3>
            <p>項目別→重複取引を消し込んでから仕訳。日付別→二重仕訳なし、売掛・買掛明細表を同時作成。</p>
        </div>
        <div class="summary-card">
            <div class="summary-icon"><i class="fa-solid fa-chart-line"></i></div>
            <h3>試算表で読む経営の3点</h3>
            <p>売上（比較）・利益率（変動確認）・現預金（異常残高チェック）。数字の異常に気づき説明できることが経理の価値。</p>
        </div>
    </div>

    <h2>重要用語チェック</h2>
    <div class="term-grid">
        <div class="term-card"><strong>試算表（TB）</strong><span>Trial Balance Sheet の略。仕訳帳から元帳への転記ミスを検証するために作成する集計表。</span></div>
        <div class="term-card"><strong>合計試算表</strong><span>各勘定科目の借方合計・貸方合計をそのまま集計した試算表。転記ミスを見つけやすい。</span></div>
        <div class="term-card"><strong>残高試算表</strong><span>各勘定科目の借方残高または貸方残高（差額）を集計した試算表。各科目の現在残高が一目でわかる。</span></div>
        <div class="term-card"><strong>合計残高試算表</strong><span>合計と残高の両方を記入する試算表。情報量が最多で実務で広く使われる。</span></div>
        <div class="term-card"><strong>貸借平均の原理</strong><span>複式簿記において、正しく仕訳・転記されていれば借方合計と貸方合計は必ず一致するという原理。</span></div>
        <div class="term-card"><strong>総勘定元帳（T勘定）</strong><span>勘定科目ごとに仕訳帳の内容を整理する帳簿。T字型の形式で借方・貸方を記録する。</span></div>
        <div class="term-card"><strong>二重仕訳</strong><span>項目別の取引資料で同一取引が複数グループに重複記載されること。消し込み処理で排除する。</span></div>
        <div class="term-card"><strong>消し込み</strong><span>二重仕訳になる重複取引を発見し、片方をバツで取り消して重複を排除する作業。</span></div>
        <div class="term-card"><strong>売掛金・買掛金明細表</strong><span>得意先・仕入先ごとの個別残高を管理する表。残高試算表の総額と明細合計が一致することで自己検証になる。</span></div>
        <div class="term-card"><strong>発生主義</strong><span>支払いタイミングにかかわらず、費用が発生した月に計上する考え方。正確な月次損益の基礎。</span></div>
        <div class="term-card"><strong>現金主義</strong><span>実際に現金が動いたタイミングで収益・費用を記録する方法。月次損益が歪むため実務では不適切。</span></div>
        <div class="term-card"><strong>月次試算表</strong><span>1ヶ月ごとに作成する試算表。理想は翌月7日まで完成。経営者への月次レポートとして機能する。</span></div>
    </div>

    <div class="check-panel">
        <h3><i class="fa-solid fa-clipboard-check"></i> セルフチェック</h3>
        <ol class="check-list">
            <li>合計試算表・残高試算表・合計残高試算表の違いを、「何を記入するか」の観点で説明できますか？</li>
            <li>「現金による仕入れ500円」が仕入グループと現金グループに両方記載されていた場合、これが二重仕訳と判断し、どちらかを消し込んでから仕訳できますか？</li>
            <li>試算表の現預金残高が先月の3倍に膨らんでいた場合、何を確認しますか？（社長による現金の個人的な持ち出し増・売掛金回収遅延・記録ミスなどを疑う）</li>
        </ol>
    </div>

    <!-- 確認クイズ -->
    <div class="quiz-panel" id="quizPanel">
        <h3><i class="fa-solid fa-circle-question"></i> 確認クイズ（全5問）</h3>
        <p style="color:var(--text-sub); font-size:0.9rem; margin-bottom:20px;">今日の試算表・二重仕訳・経営判断の理解度を確認しましょう。</p>
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
        <h4 style="color:#047857; margin:0 0 10px; font-size:1.15rem;"><i class="fa-solid fa-forward"></i> 次回予告：Day 8</h4>
        <p style="margin:0; font-size:0.95rem; color:#333; line-height:1.6;">（Day 8 の内容確定後に記入）</p>
    </div>
</div>
```

---

## 7. ページ末尾ナビボタン（Day 8 async fetch）

```html
<div style="text-align:center; padding: 3rem 0 2rem; display: flex; flex-direction: column; gap: 1rem; align-items: center;">
    <button type="button" class="tool-link-btn" style="padding: 1.2rem 4rem; font-size:1.25rem; background:linear-gradient(135deg, #34d399, #059669); border:none; box-shadow: 0 10px 30px rgba(5,150,105, 0.25); cursor:pointer;" onclick="(async function(){try{const r=await fetch('./vol08-1.html',{method:'HEAD',cache:'no-store'});if(r.ok){window.location.href='./vol08-1.html';return;}}catch(e){}alert('Day 8 は準備中です。もうしばらくお待ちください。');})()">
        Day 8 へ進む <i class="fa-solid fa-arrow-right"></i>
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
        q: "試算表（TB）を作成する主な目的はどれ？",
        opts: [
            "決算書を作るための最終手続き",
            "仕訳帳から総勘定元帳への転記ミスを検証するため",
            "売上と利益を計算するため",
            "消費税の申告準備のため"
        ],
        ans: 1,
        fb: "試算表は「転記ミスを早期に発見するための検算表」です。月次で作成することで、1年分ためてから探すコストを大幅に削減できます。決算書の作成は試算表の後の手続きです。"
    },
    {
        q: "残高試算表に記入する金額はどれ？",
        opts: [
            "各勘定の借方合計と貸方合計の両方",
            "各勘定の借方残高と貸方残高の差額（残高のある側のみ）",
            "各勘定の借方合計のみ",
            "各勘定の期末残高と期首残高の差額"
        ],
        ans: 1,
        fb: "残高試算表は借方合計と貸方合計の「差額（残高）」を、金額が大きい方の欄にのみ記入します。両方記入するのは合計残高試算表です。"
    },
    {
        q: "取引が「項目別」に与えられた試算表問題で最も注意すべきことは？",
        opts: [
            "売掛金・買掛金の明細表を同時に作成すること",
            "前月末の試算表残高をゼロから再計算すること",
            "重複している取引（二重仕訳）を消し込んでから仕訳すること",
            "合計試算表ではなく残高試算表を選ぶこと"
        ],
        ans: 2,
        fb: "項目別に資料が与えられると、同一の取引が複数グループに重複して記載されます。そのまま仕訳すると金額が2倍になる「二重仕訳」が発生します。まず金額が一致する重複取引を見つけて消し込み、残った取引だけを仕訳します。"
    },
    {
        q: "取引が「日付順」に与えられた試算表問題で追加作成を求められることが多いものは？",
        opts: [
            "固定資産台帳",
            "合計試算表と残高試算表の両方",
            "売掛金・買掛金明細表",
            "月次損益計算書"
        ],
        ans: 2,
        fb: "日付順の問題では二重仕訳は発生しませんが、得意先・仕入先ごとの個別残高を管理する「売掛金・買掛金明細表」の作成が求められます。仕訳の際に「売掛金（A社）」のように会社名を書き添えておくことが必須です。"
    },
    {
        q: "Bloomの試算表で「現金残高が先月の4倍」になっていた。経理として最初に疑うべきことは？",
        opts: [
            "売上が急増した証拠なので問題ない",
            "減価償却費の計上漏れ",
            "現金の管理ができていない・個人的な持ち出しの増加など、異常値の可能性",
            "消費税の申告が必要になった"
        ],
        ans: 2,
        fb: "現金残高が異常に増加している場合、現金管理ができていないか、社長等が個人的に使い込んでいる危険なシグナルです。銀行融資審査でも「帳簿だけ膨らんでいる」と判断されて不利になります。異常値に気づき理由を確認することが経理の重要な役割です。"
    }
];
```

---

## 9. 実装チェックリスト

### テンプレート変更
- [ ] `<title>` が「Day 7 | 試算表の作成と活用」になっている
- [ ] ヘッダータグが「DAY 07」になっている
- [ ] `<h1>` が「試算表の作成と活用」になっている
- [ ] プログレスバー width が `54%` になっている
- [ ] cache-bust が `2026-05-15T15:00:00` になっている

### 動画IDの確認（全5本）
- [ ] 動画① `TqX3L-HbPEU`（試算表の基本概念と3種類）
- [ ] 動画② `ibMfbGE8F-w`（集計実務の基礎演習）
- [ ] 動画③ `O0QkZ6yHkgQ`（二重仕訳消し込み＆明細表）
- [ ] 動画④ `Wqkafa3IxqY`（経営分析・異常値の発見）
- [ ] 動画⑤ `WQ2fMu3K81s`（ダメパターン・発生主義）

### コンテンツ確認
- [ ] goalタブ：bloom-story・why-box・talk-scene・goal-box・flow-map（5ステップ）がすべて実装されている
- [ ] firstタブ：3種類比較ビジュアル・learning-card 3枚（動画①②③）・talk-scene・practice-area がある
- [ ] secondタブ：3点チェックビジュアル・learning-card **2枚**（動画④⑤）・talk-scene・conclusion-comic・practice-area がある（学習カードは2枚！）
- [ ] summaryタブ：summary-grid 4枚・term-grid 12語・check-panel・quiz-panel・次回予告プレースホルダーがある

### ナビゲーション
- [ ] Day 8ボタンのonclickに `./vol08-1.html` が含まれている
- [ ] アラートメッセージが「Day 8 は準備中です。」になっている

### quizData（5問確認）
- [ ] 問1：試算表の目的 → 転記ミスの検証（答えB）
- [ ] 問2：残高試算表に記入するもの → 差額（残高）のある側のみ（答えB）
- [ ] 問3：項目別パターンの注意点 → 二重仕訳の消し込み（答えC）
- [ ] 問4：日付順パターンで追加作成するもの → 売掛・買掛明細表（答えC）
- [ ] 問5：現金残高4倍の異常値 → 管理できていない危険シグナル（答えC）

### デプロイ
- [ ] `vol07-1.html` と `.deploy_tmp/vol07-1.html` の内容が完全に一致している
- [ ] cache-bust コメントが両ファイルともに `<!-- cache-bust: 2026-05-15T15:00:00 -->` になっている
