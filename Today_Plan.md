# ClaudeCode 向け申し送り（デプロイ運用・Day11/12 実装状況）

作成日: 2026-05-27  
作成者: Cursor（H.K 報告用）

---

## 1. 最重要：本番反映の正しい手順（必読）

このプロジェクトの Cloudflare Pages（`training-summary-2605`）は、**Git push だけでは本番サイトは更新されない**。

### 正式なデプロイ手順（`GEMINI.md` §4 と同じ）

```powershell
# 1. 資材を .deploy_tmp に集約
copy index.html .deploy_tmp\
copy vol*.html .deploy_tmp\

# 2. wrangler で本番反映
npx wrangler pages deploy .deploy_tmp --project-name=training-summary-2605
```

- **本番URL**: https://training-summary-2605.pages.dev
- **ブランチ**: `master` に push（`main` は不可）
- **`.deploy_tmp/`** も git に含め、ルートの HTML と同期しておく

### やってはいけない／誤解しやすいこと

| 誤解 | 実際 |
|---|---|
| `git push` すれば自動で本番更新 | **されない**（このプロジェクトは Git 連携の自動ビルド型ではない） |
| Cloudflare ダッシュボードの「Create deployment」が標準 | **wrangler が標準**。ダッシュボード手動は緊急時のみ |
| Deployments に `Source: master` と出る＝Git自動 | **wrangler / Direct Upload でも同様の表示になり得る** |
| `Today_Plan.md` の「手動アップロードで統一」 | **誤り**。wrangler が正。2026-05-27 に H.K と確認済み |

### 実装完了時のチェックリスト（ClaudeCode → Cursor 指示に必ず含める）

1. 対象 HTML を編集
2. `.deploy_tmp/` に `index.html` + 変更した `vol*.html` をコピー
3. `git add` → `git commit` → `git push origin master`
4. **`npx wrangler pages deploy .deploy_tmp --project-name=training-summary-2605` を実行**
5. https://training-summary-2605.pages.dev で表示確認

---

## 2. 今回のトラブル経緯（H.K とのやり取り）

- Day12 実装後、`git push` のみ実施 → Cloudflare の Deployments が「9時間前で止まった」ように見えた
- H.K が Cloudflare ダッシュボードで **手動アップロード**して初めて反映
- 調査の結果、**数日間うまくいっていたのは Cursor が `wrangler pages deploy` まで実行していたから**と判明
- H.K は以前から「wrangler でデプロイしてほしい」と依頼済み。今回 Cursor が push のみで止めた際、**wrangler 未実行である旨の言及が不足**していた（H.K 指摘）

**2026-05-27 時点で Cursor が wrangler 再実行済み** → 本番は最新想定。

---

## 3. 実装完了状況（Cursor 側）

| 内容 | 状態 | コミット例 |
|---|---|---|
| vol11-1 仕訳15問 入力採点型 | ✅ | `733c540` 等 |
| vol11-2 Tab1〜3 入力採点型（Tab4精算表は既存） | ✅ | 同上 |
| vol12-1.html 新規 | ✅ | `f24d6cd` |
| index.html Day12 更新 | ✅ | 同上 |
| wrangler deploy | ✅ 2026-05-27 再実行 | — |

---

## 4. ClaudeCode への指示テンプレ（今後の実装仕様書用）

実装仕様書（`Today_Plan.md`）の末尾に、**必ず**以下を記載すること：

```markdown
## デプロイ（必須・Cursor実装後）

git push のみでは本番に反映されない。以下を Cursor が実行すること。

1. `copy index.html .deploy_tmp\` および `copy vol*.html .deploy_tmp\`
2. `git add` / `commit` / `push origin master`
3. `npx wrangler pages deploy .deploy_tmp --project-name=training-summary-2605`

※ Cloudflare ダッシュボードの手動アップロードは不要。
※ `npx wrangler` は使用しない（手動アップロードで統一）という記載は誤り。
```

---

## 5. その他プロジェクトルール（再掲）

- **`CONTEXT.md` / `GEMINI.md` / `CLAUDE.md` 必読**
- **cache-bust**: `<!-- cache-bust: YYYY-MM-DDTHH:MM:SS -->`（ISO 8601、ページ固有文字列を混ぜない）
- **`.git/config` は変更禁止**（`worktreeConfig` 等）
- 一時スクリプトは `scratch/`、完了後削除
- 引き継ぎは `Today_Plan.md` に統合（`handoff_*.txt` 禁止）

---

## 6. Day 12 仕様書についての訂正

以前の `Today_Plan.md`（Day12前半仕様）にあった以下は **訂正**：

- ~~「Cloudflare Pages ダッシュボードで手動アップロード」~~ → **wrangler deploy**
- vol12-1 のデザイン：実装時は vol11-1 ベースで作成済み（ピンク系を使った版）。今後 vol11-1 完全踏襲で揃える指示が出た場合は別タスク

---

## 7. H.K が ClaudeCode に伝える一言（コピペ用）

> 本番反映は `git push` だけでは足りない。`GEMINI.md` 通り `npx wrangler pages deploy .deploy_tmp --project-name=training-summary-2605` まで Cursor が実行する運用。実装指示書に「ダッシュボード手動アップロード」や「wrangler 使わない」と書かないでほしい。Day12 で push のみで止まり、手動アップロードが必要になった。以降は実装完了＝wrangler deploy まで含めること。
