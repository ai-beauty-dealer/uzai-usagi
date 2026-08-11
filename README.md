# ウザイうさぎ まとめサイト

ウザイうさぎシリーズの LINEスタンプ・絵文字を、毎回作っている紹介POPと販売URLで一覧にする静的サイト。

## 正本

- `catalog.json` … 掲載する作品のデータ（**ここが唯一の正本**）
- `build_site.py` … サイト生成スクリプト（`tools/build_promo_pop.py` と対）
- `dist/` … 生成物。公開ディレクトリ（毎回作り直されるので直接編集しない）

## 新作を出した時の手順

1. 紹介POPを作る（`python3 tools/build_promo_pop.py <案件>/06_promo/promo.json`）
2. LINEストアの作者ページで販売URLを拾う（Creators Marketの管理番号とは別体系なので変換不可）
3. `catalog.json` の `products` に1ブロック足す

   ```json
   {
     "id": "英数字のスラッグ",
     "title": "LINEストアと同じ商品名",
     "type": "stamp | animated | emoji",
     "count": 16,
     "new": true,
     "store_url": "https://store.line.me/...",
     "desc": "1〜2行の紹介",
     "scenes": ["使う場面", "使う場面"],
     "pop": "line_stamp/<案件フォルダ>/06_promo/out/sns_pop_1080.png",
     "project": "line_stamp/<案件フォルダ>"
   }
   ```

   `scenes` セクションに載せたい場合は `catalog.json` の `scenes` にも id を足す。
   `site.updated` の日付も更新する。
4. ビルド

   ```bash
   cd 99_Sbox/mendokusai_stamp
   python3 site/build_site.py
   ```
5. `site/dist/index.html` をブラウザで開いて確認 → 公開（下記）

## 公開

GitHub Pages（`dist/` の中身をリポジトリのルートに置く）。
`.nojekyll` を含めているので `_` 始まりのファイルも配信される。

## メモ

- `pop` のパスは `99_Sbox/mendokusai_stamp/` からの相対
- 顔芸リアクション絵文字だけ案件フォルダがVaultに無いため、POPは
  `site/assets_src/pop_emoji32.png`（`ai-beauty-dealer/mendokusai-stamp-assets` から取得）を使っている
- ヒーローのうさぎは `line_stamp/2026-08-08_uzai-usagi-keigo/03_processed/final/14.png` の
  フレーズ帯を落としたもの。変える時は `build_site.py` の `HERO_SRC`
- POPは1080/640のWebPに変換して載せている（全9作品で1.5MBくらい）
