# Day 3 実装計画：`vol03-1.html`

作成日: 2026-05-13  
担当AI: Cursor (Gemini Flash / Claude Sonnet)  
レビュー: Claude Code

---

## 0. 作業前チェック

- `CLAUDE.md` と `GEMINI.md` を必ず読み込むこと
- テンプレート: `vol02-1.html` を複製して差し替える
- 完成ファイルは 2 か所に保存: ① プロジェクトルート `vol03-1.html` ② `.deploy_tmp/vol03-1.html`
- デプロイ前に `.deploy_tmp/` に `index.html` + `vol01-1.html` + `vol02-1.html` + `vol03-1.html` が揃っていることを確認

---

## 1. ページ基本情報

| 項目 | 値 |
|------|-----|
| ファイル名 | `vol03-1.html` |
| `<title>` | `Day 3 | 複式簿記・帳簿体系の基礎` |
| ヘッダータグ | `DAY 3` |
| ヘッダーh1 | `複式簿記と帳簿体系の基礎` |
| ヘッダーサブ | `T字勘定・天記・総勘定元帳から帳簿の種類まで` |

---

## 2. 構造の変更点（vol02 との差分）

| 比較項目 | vol02 | vol03 |
|----------|-------|-------|
| 前半動画数 | 2本 (Canva+簿記) | **3本**（すべて簿記） |
| 後半動画数 | 3本 (簿記3本) | **2本**（簿記） |
| Canva実習 | 前半に1箇所 | **なし** |
| NotebookLM実習 | 後半のみ | **前半・後半の両方** |
| テスト問題数（NotebookLM） | 5問プロンプト | **20問プロンプト** |
| Canva注意書き | あり | **なし** |
| Day 2 に戻るリンク | なし | **あり**（`./vol02-1.html`） |

---

## 3. タブ構成

```
[前半（全体像と仕訳の流れ）] [後半（帳簿体系と補助簿）] [テスト]
```

- タブID: `first`, `second`, `quiz`
- デフォルト表示: `first`

---

## 4. 前半タブ（`id="first"`）

### セクションタイトル
```
<h2>セクション1：複式簿記の仕組みとT字勘定</h2>
<p>「なぜ左右に分けて書くのか？」「仕訳はどこへ行くのか？」今日は複式簿記の心臓部を学びます。</p>
```

---

### Learning Card 1（前半①）

```html
<!-- Learning Card 1: 簿記の5要素と全体像 -->
<div class="learning-card">
  <div class="lc-video">
    <div class="vc-thumb" data-video-id="HI2DXDOyx8Q"
      style="background-image: url('https://img.youtube.com/vi/HI2DXDOyx8Q/maxresdefault.jpg'),
                               url('https://img.youtube.com/vi/HI2DXDOyx8Q/hqdefault.jpg');">
      <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
  </div>
  <div class="lc-content">
    <div class="lc-header">
      <span class="vc-tag">全体像</span>
      <h3 class="lc-title">「資産・負債・純資産・収益・費用」5要素で簿記の世界を見る</h3>
    </div>
    <div class="lc-body">
      <h4><i class="fa-solid fa-layer-group"></i> 学習のポイント</h4>
      <ul class="lc-list">
        <li>5要素（資産・負債・純資産・収益・費用）はすべての仕訳の「材料」</li>
        <li>資産・費用は「借方（左）ホーム」、負債・純資産・収益は「貸方（右）ホーム」</li>
        <li>会計ソフト（弥生・freee など）も内部でこの仕訳ルールに従って動いている</li>
      </ul>
    </div>
  </div>
</div>
```

---

### Learning Card 2（前半②）

```html
<!-- Learning Card 2: T字勘定攻略 -->
<div class="learning-card">
  <div class="lc-video">
    <div class="vc-thumb" data-video-id="JGnQIxkZChI"
      style="background-image: url('https://img.youtube.com/vi/JGnQIxkZChI/maxresdefault.jpg'),
                               url('https://img.youtube.com/vi/JGnQIxkZChI/hqdefault.jpg');">
      <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
  </div>
  <div class="lc-content">
    <div class="lc-header">
      <span class="vc-tag">T字勘定</span>
      <h3 class="lc-title">T字勘定の書き方3ステップ：前払保険料で完全攻略</h3>
    </div>
    <div class="lc-body">
      <h4><i class="fa-solid fa-t"></i> 解き方の型</h4>
      <ul class="lc-list">
        <li>T字勘定は「総勘定元帳」の略式形式。仕訳のあとに「天記」する帳簿</li>
        <li>前払費用など経過勘定の問題は①タイムテーブルで整理→②仕訳→③T勘定の3ステップ</li>
        <li>資産・負債科目のT勘定には期首「前期繰越」・期末「次期繰越」が入る</li>
      </ul>
    </div>
  </div>
</div>
```

---

### Learning Card 3（前半③）

```html
<!-- Learning Card 3: 天記と総勘定元帳 -->
<div class="learning-card">
  <div class="lc-video">
    <div class="vc-thumb" data-video-id="EeDOxSb95ew"
      style="background-image: url('https://img.youtube.com/vi/EeDOxSb95ew/maxresdefault.jpg'),
                               url('https://img.youtube.com/vi/EeDOxSb95ew/hqdefault.jpg');">
      <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
  </div>
  <div class="lc-content">
    <div class="lc-header">
      <span class="vc-tag">天記・元帳</span>
      <h3 class="lc-title">仕訳 → 総勘定元帳への「天記」：帳簿づくりの流れをつかむ</h3>
    </div>
    <div class="lc-body">
      <h4><i class="fa-solid fa-right-left"></i> 帳簿づくりの流れ</h4>
      <ul class="lc-list">
        <li>天記とは「日付順の仕訳帳」の内容を、科目別の「総勘定元帳」に集計する手続き</li>
        <li>元帳で相手勘定を書くことで、複式簿記の「取引の2面性」が見える</li>
        <li>全体の流れ：取引 → 仕訳帳 → 天記 → 試算表 → 決算整理 → 財務諸表</li>
      </ul>
    </div>
  </div>
</div>
```

---

### 前半 NotebookLM 実習

```html
<div class="practice-area">
  <h3 style="color:var(--accent-green);">
    <i class="fa-solid fa-pencil"></i> 実習：NotebookLM で確認テストを自動生成しよう
  </h3>
  <p>今日学んだ3本の動画からNotebookLMがテストを自動生成します。アウトプットで記憶を定着させましょう。</p>

  <ul class="step-list">
    <li>NotebookLMにアクセスして新しいノートブックを作成し、「ソースを追加」から下記3本のYouTube動画URLを入力する。</li>
    <li>チャット欄に下のプロンプトを貼り付けて実行する。</li>
    <li>生成されたテストを解いて、間違えた問題は動画に戻って確認する。</li>
  </ul>

  <!-- プロンプトボックス -->
  <div class="prompt-box">
    <div class="prompt-header">
      <span><i class="fa-solid fa-robot"></i> NotebookLM プロンプト（コピーして貼り付けよう）</span>
    </div>
    <div class="prompt-urls" style="margin: 10px 0; font-size: 0.9rem; color: var(--text-sub);">
      ソースに追加するURL：<br>
      https://youtu.be/HI2DXDOyx8Q<br>
      https://youtu.be/JGnQIxkZChI<br>
      https://youtu.be/EeDOxSb95ew
    </div>
    <div class="chat-bubble">
      <p style="margin:0;">読み込んだ3つの動画の内容に基づいて、簿記の基礎知識に関する確認テストを20問作成してください。形式は選択式とし、最後に解答と解説も出力してください。</p>
      <button class="copy-btn" onclick="navigator.clipboard.writeText('読み込んだ3つの動画の内容に基づいて、簿記の基礎知識に関する確認テストを20問作成してください。形式は選択式とし、最後に解答と解説も出力してください。')">
        <i class="fa-solid fa-copy"></i> コピー
      </button>
    </div>
  </div>

  <div style="text-align: center; margin-top: 20px;">
    <a href="https://notebooklm.google.com/" target="_blank" class="tool-link-btn">
      <i class="fa-solid fa-book"></i> NotebookLM を開く
    </a>
  </div>
</div>
```

### 前半→後半ナビボタン

```html
<div style="text-align: center; margin-top: 30px;">
  <button class="tool-link-btn" onclick="openTab('second')" style="background:var(--accent-green);">
    後半（帳簿体系）へ進む <i class="fa-solid fa-arrow-right"></i>
  </button>
</div>
```

---

## 5. 後半タブ（`id="second"`）

### セクションタイトル

```
<h2>セクション2：帳簿体系と補助簿の活用</h2>
<p>日々の仕訳を集計・管理する「帳簿の種類」を学びます。試験で頻出の「どの補助簿に記入するか？」問題も攻略しましょう。</p>
```

---

### Learning Card 4（後半①）

```html
<!-- Learning Card 4: 主要簿と伝票会計 -->
<div class="learning-card">
  <div class="lc-video">
    <div class="vc-thumb" data-video-id="5yVU7wLuXP0"
      style="background-image: url('https://img.youtube.com/vi/5yVU7wLuXP0/maxresdefault.jpg'),
                               url('https://img.youtube.com/vi/5yVU7wLuXP0/hqdefault.jpg');">
      <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
  </div>
  <div class="lc-content">
    <div class="lc-header">
      <span class="vc-tag">帳簿の基本</span>
      <h3 class="lc-title">主要簿（仕訳帳・総勘定元帳）と3伝票制の仕組み</h3>
    </div>
    <div class="lc-body">
      <h4><i class="fa-solid fa-book-bookmark"></i> 学習のポイント</h4>
      <ul class="lc-list">
        <li>主要簿（仕訳帳・総勘定元帳）は作成が法定義務のマスト帳簿</li>
        <li>補助簿は管理したいニーズがある場合にのみ作成する任意の帳簿</li>
        <li>3伝票制：入金伝票・出金伝票・振替伝票で全取引を記録する仕組み</li>
      </ul>
    </div>
  </div>
</div>
```

---

### Learning Card 5（後半②）

```html
<!-- Learning Card 5: 補助簿の種類と活用 -->
<div class="learning-card">
  <div class="lc-video">
    <div class="vc-thumb" data-video-id="ViIAqPXtrAY"
      style="background-image: url('https://img.youtube.com/vi/ViIAqPXtrAY/maxresdefault.jpg'),
                               url('https://img.youtube.com/vi/ViIAqPXtrAY/hqdefault.jpg');">
      <i class="fa-brands fa-youtube vc-thumb-play"></i>
    </div>
  </div>
  <div class="lc-content">
    <div class="lc-header">
      <span class="vc-tag">補助簿</span>
      <h3 class="lc-title">各種補助簿の特徴と「どれに記入するか？」問題の攻略法</h3>
    </div>
    <div class="lc-body">
      <h4><i class="fa-solid fa-table-list"></i> 試験のツボ</h4>
      <ul class="lc-list">
        <li>主な補助簿：現金出納帳・当座預金出納帳・売上帳・仕入帳・商品有高帳など</li>
        <li>商品有高帳の単価計算：先入先出法（FIFO）と移動平均法の2方式</li>
        <li>補助簿の選択問題→「どの勘定科目が動くか」＝仕訳が分かれば解ける</li>
      </ul>
    </div>
  </div>
</div>
```

---

### 後半 NotebookLM 実習

```html
<div class="practice-area">
  <h3 style="color:var(--accent-green);">
    <i class="fa-solid fa-pencil"></i> 実習：NotebookLM で確認テストを自動生成しよう
  </h3>
  <p>帳簿体系の2本の動画からテストを生成して復習しましょう。</p>

  <ul class="step-list">
    <li>新しいNotebookLMのノートブックを作成し、下記2本のURLをソースとして追加する。</li>
    <li>チャット欄に下のプロンプトを貼り付けて実行する。</li>
    <li>生成されたテストを解いて、間違えた問題は動画に戻って確認する。</li>
  </ul>

  <!-- プロンプトボックス -->
  <div class="prompt-box">
    <div class="prompt-header">
      <span><i class="fa-solid fa-robot"></i> NotebookLM プロンプト（コピーして貼り付けよう）</span>
    </div>
    <div class="prompt-urls" style="margin: 10px 0; font-size: 0.9rem; color: var(--text-sub);">
      ソースに追加するURL：<br>
      https://youtu.be/5yVU7wLuXP0<br>
      https://youtu.be/ViIAqPXtrAY
    </div>
    <div class="chat-bubble">
      <p style="margin:0;">読み込んだ2つの動画の内容に基づいて、簿記の基礎知識に関する確認テストを20問作成してください。形式は選択式とし、最後に解答と解説も出力してください。</p>
      <button class="copy-btn" onclick="navigator.clipboard.writeText('読み込んだ2つの動画の内容に基づいて、簿記の基礎知識に関する確認テストを20問作成してください。形式は選択式とし、最後に解答と解説も出力してください。')">
        <i class="fa-solid fa-copy"></i> コピー
      </button>
    </div>
  </div>

  <div style="text-align: center; margin-top: 20px;">
    <a href="https://notebooklm.google.com/" target="_blank" class="tool-link-btn">
      <i class="fa-solid fa-book"></i> NotebookLM を開く
    </a>
  </div>
</div>
```

### 後半→テストナビボタン

```html
<div style="text-align: center; margin-top: 30px;">
  <button class="tool-link-btn" onclick="openTab('quiz')" style="background:var(--accent-gold);">
    理解度チェックテストへ <i class="fa-solid fa-arrow-right"></i>
  </button>
</div>
```

---

## 6. テストタブ（`id="quiz"`）

vol02-1.html の quizData を以下で**まるごと差し替える**。

```javascript
const quizData = [
  {
    q: "T字勘定（総勘定元帳の略式）において、期首（4月1日）の借方に「前期繰越」が記入されるのはどのような勘定科目か？",
    opts: [
      "売上・受取手数料など収益科目",
      "仕入・給料など費用科目",
      "現金・売掛金など資産科目",
      "すべての勘定科目に前期繰越が記入される"
    ],
    ans: 2,
    fb: "資産・負債・純資産の科目（貸借対照表に乗る科目）は毎期残高が繰り越されるため、期首（4月1日）に「前期繰越」が記入されます。費用・収益科目（損益計算書の科目）は毎期リセットされるため「前期繰越」は記入されません。"
  },
  {
    q: "11月1日に向こう1年分の保険料18万円を現金で支払った（会計期間4/1〜3/31、月割計算）。当期末（3/31）の決算整理仕訳で「前払保険料」として計上する金額はいくらか？",
    opts: [
      "7万5,000円",
      "10万5,000円",
      "18万円",
      "1万5,000円"
    ],
    ans: 1,
    fb: "1ヶ月分の保険料は18万円÷12ヶ月＝1万5,000円。11月〜3月の5ヶ月が当期費用（7万5,000円）。残り4月〜10月の7ヶ月分（1万5,000円×7＝10万5,000円）が来期に所属するため「前払保険料」に計上します。"
  },
  {
    q: "「天記」とはどのような手続きか？",
    opts: [
      "取引内容を仕訳帳に記録すること",
      "仕訳帳の仕訳を勘定科目ごとに総勘定元帳へ書き写すこと",
      "試算表から財務諸表を作成すること",
      "期末に決算整理仕訳を行うこと"
    ],
    ans: 1,
    fb: "天記（転記）は仕訳帳（日付順の記録）の内容を、勘定科目別の帳簿「総勘定元帳」に書き写す手続きです。これにより科目ごとの残高が把握できます。"
  },
  {
    q: "次のうち「主要簿」に含まれるものはどれか？",
    opts: [
      "現金出納帳・商品有高帳",
      "売掛金元帳・買掛金元帳",
      "仕訳帳・総勘定元帳",
      "売上帳・仕入帳"
    ],
    ans: 2,
    fb: "主要簿は「仕訳帳」と「総勘定元帳」の2つで、作成が法律で義務づけられています。現金出納帳・商品有高帳・売掛金元帳・売上帳などはすべて「補助簿」で、必要に応じて作成します。"
  },
  {
    q: "「商品を現金4,000円で売り上げた」取引がある場合、記入される補助簿の組み合わせとして正しいのはどれか？",
    opts: [
      "現金出納帳と売上帳のみ",
      "現金出納帳・売上帳・商品有高帳の3つ",
      "売上帳と商品有高帳のみ",
      "仕訳帳のみ（補助簿は不要）"
    ],
    ans: 1,
    fb: "現金売上の仕訳は（借）現金4,000 /（貸）売上4,000です。①現金が動く→現金出納帳、②売上が増える→売上帳、③商品の在庫が減る→商品有高帳の3つに記入します。補助簿の選択問題は「どの勘定科目が動くか」＝仕訳が分かれば解けます。"
  }
];
```

---

## 7. Day ナビゲーションリンク

タブエリアの**外側・下部**に配置（vol02-1.html と同じ位置）。

```html
<!-- Day ナビゲーション（タブ外に常時表示） -->
<div style="padding: 30px 40px 20px; text-align: center; border-top: 1px solid #eee; display: flex; justify-content: space-between; align-items: center;">
  <a href="./vol02-1.html" style="color: var(--text-sub); text-decoration: none; font-size: 0.95rem;">
    <i class="fa-solid fa-arrow-left"></i> Day 2 に戻る
  </a>
  <span id="day4-link-area"></span>
</div>
```

Day 4 ページの存在チェック（XHRによる存在確認）は vol01-1.html / vol02-1.html の実装を踏襲する。

```javascript
// Day4リンク表示チェック
var x = new XMLHttpRequest();
x.open('HEAD', './vol04-1.html', false);
try {
  x.send();
  if (x.status === 200) {
    document.getElementById('day4-link-area').innerHTML =
      '<a href="./vol04-1.html" style="color:var(--accent-green);text-decoration:none;font-size:0.95rem;">Day 4 へ進む <i class="fa-solid fa-arrow-right"></i></a>';
  }
} catch(e) {}
```

---

## 8. cache-bust コメント

HTML末尾（`</html>` の後）に追記：

```html
<!-- cache-bust: 2026-05-13T00:00:00 -->
```

---

## 9. CSS 差分（vol02 から追加・変更が必要な点）

### prompt-box スタイル（vol02 に存在しない場合は追加）

```css
.prompt-box {
  background: #f8fafc; border: 1px solid #e2e8f0;
  border-radius: var(--radius-medium); padding: 20px;
  margin: 20px 0;
}
.prompt-header {
  font-weight: 700; color: var(--accent-green);
  margin-bottom: 12px; font-size: 0.95rem;
}
.chat-bubble {
  background: white; border: 1px solid #e2e8f0;
  border-radius: 12px; padding: 16px;
  position: relative; font-size: 0.95rem; line-height: 1.7;
}
.copy-btn {
  position: absolute; top: 12px; right: 12px;
  background: var(--accent-green); color: white;
  border: none; border-radius: 8px; padding: 6px 12px;
  font-size: 0.8rem; cursor: pointer; font-weight: 700;
}
.copy-btn:hover { background: #047857; }
```

※ vol01-1.html には prompt-box / chat-bubble / copy-btn の実装があるので参照すること。

### step-list（NotebookLM用）

vol02 の `.step-list.canva` スタイルを流用可。Day3では `.step-list`（デフォルト緑）のみ使用。

---

## 10. 実装チェックリスト

実装後に以下を必ず確認すること：

- [ ] 前半タブに3つのLearning Card（HI2DXDOyx8Q / JGnQIxkZChI / EeDOxSb95ew）がある
- [ ] 後半タブに2つのLearning Card（5yVU7wLuXP0 / ViIAqPXtrAY）がある
- [ ] 各動画のFacadeがクリックでiframeに置き換わりautoplay=1で再生される
- [ ] 前半・後半ともにNotebookLM実習エリア（prompt-box + chat-bubble）がある
- [ ] 前半の「後半へ進む」ボタンが正しくopenTab('second')を呼ぶ
- [ ] 後半の「テストへ進む」ボタンが正しくopenTab('quiz')を呼ぶ
- [ ] quizData が5問ある（Day3内容で上書き済み）
- [ ] 「Day 2 に戻る」リンクが `./vol02-1.html` を参照している
- [ ] Day 4 存在チェックXHRが実装されている（`vol04-1.html` を参照）
- [ ] Canva関連のUI（注意書き・canvaクラスのstep-list等）が残っていない
- [ ] cache-bust コメントが末尾にある
- [ ] `.deploy_tmp/vol03-1.html` にもコピーされている
- [ ] ブラウザでタブ切り替えが正常動作する（openTab関数がquerySelectorベースの修正済み版）
- [ ] スマートフォン幅（375px）でレイアウト崩れがない

---

## 11. ハルシネーション防止ルール

GEMINI.mdの通り、以下を遵守すること：

- Learning Card の箇条書きは **Today_Research.md の内容のみ** に基づく
- 存在しないCanva機能名・簿記の説明は書かない
- 使用する動画IDは以下の5個のみ（他IDは使わない）：
  - 前半: `HI2DXDOyx8Q`, `JGnQIxkZChI`, `EeDOxSb95ew`
  - 後半: `5yVU7wLuXP0`, `ViIAqPXtrAY`
- VTTファイルを取得した場合は実装後に必ず削除する
