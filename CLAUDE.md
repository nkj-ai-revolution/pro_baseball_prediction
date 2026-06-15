# pro_baseball_prediction — Claude Code ガイド

## リポジトリ概要
NPBプロ野球の試合予測 HTMLを毎日生成・更新するプロジェクト。
メインファイル: `index.html`（GitHub Pages で公開）

---

## ⚠️ Git プッシュの重要ルール

### 問題
このリモート実行環境では `git push` がGitHub本体ではなく
**ローカルプロキシ（`127.0.0.1:42599`）** に送られる。
別プロセスがGitHubを直接更新すると内容がずれる。

### 必須ワークフロー（毎回厳守）

```
1. ファイルを編集
2. git add & git commit（ローカル履歴の記録）
3. mcp__github__push_files でGitHub main ブランチへ直接プッシュ
4. mcp__github__get_file_contents で重要キーワードを検証
```

### 禁止事項
- `git push` **だけ** で完結させない（GitHub本体に届かない場合がある）
- claude/ ブランチの作成・ PR の作成（mainに直接コミット）

---

## 日次更新フロー

### 朝版（08:00 JST 目安）
1. Web検索で予告先発・前日結果を取得
2. 各試合の勝率を計算しindex.htmlを更新
3. ヘッダーバッジを `朝版` に設定
4. コミットメッセージ: `予測更新: YYYY-MM-DD 朝版`

### 最終分析版（12:30 JST 目安・デーゲーム開始前）
1. NPB公式 → baseball-freak.com → Yahoo!スポーツナビ の順で以下を取得
   - 当日の登録公示（選手の登録・抹消）
   - 先発変更の有無
   - 朝版で「未取得」だった投手の成績（ERA・勝敗など）
2. 変動した試合は `前回比±○○pt` と理由を reason-list に明記
3. ヘッダーバッジを `最終分析版` に設定（`.final` クラスで緑色表示）
4. コミットメッセージ: `予測更新: YYYY-MM-DD 最終分析版`
5. **必ず `mcp__github__push_files` でGitHubに直接プッシュ**
6. **`mcp__github__get_file_contents` で投手名・確率が反映されているか確認**

### デーゲームが開始済みの場合
- 未開始の試合のみ更新し、index.html内にその旨を記載

---

## グレード基準

| グレード | 勝率 | 説明 |
|----------|------|------|
| A        | 70%以上 | 厳選ピック推奨 |
| B        | 58～69% | 参考推奨 |
| C        | 50～57% | 見送り推奨 |

---

## index.html 構造メモ

- `.header-badge.final` — 最終分析版のときに緑色バッジ
- `.change-badge` — 朝版比アップ時の緑バッジ
- `.change-badge.down` — 朝版比ダウン時の赤バッジ
- `.grade-b-card` / `.grade-c-card` — カード上部ラインの色
- `.upd-pos` / `.upd-neg` / `.upd-neu` — reason-list内の変動表記色
- カード順: 確率の高い順（降順）に並べる
- `.about-note` — サイト趣旨・賭博利用禁止の説明ブロック（免責事項の直前）

> ⚠️ **`.about-note` ブロックは変更・削除しないこと。**
> 日次更新では試合予測・確率・投手名・順位表・免責末尾の変動記録のみを更新し、このブロックは常に原文のまま維持する。

---

## GitHub 情報

- リポジトリ: `nkj-ai-revolution/pro_baseball_prediction`
- ブランチ: `main`（直接コミット・PRなし）
