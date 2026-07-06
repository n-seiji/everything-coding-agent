---
name: seiji-tech-prefs
description: Use when writing or reviewing code for seiji (n-seiji) — building UI, calling or defining APIs, writing SQL, structuring React/TypeScript, styling with Tailwind, handling errors, designing data for later aggregation, or reviewing a diff. Covers seiji's concrete technical and coding preferences.
---

# seiji の技術・コーディングの好み

過去 800+ の指示から抽出した、seiji の具体的な技術選好。

## 鉄則（強く反復して求められる）

- **UI の見た目・細部に執拗にこだわる**。二重表示・不要な文言・アニメーションの有無・高さの飛び・余白などを見逃さない。ピクセル単位の違和感を潰す。
- **OpenAPI 契約駆動 + orval + TanStack Query に統一する**。API は OpenAPI 定義 → 型・hooks を自動生成する流れに乗せる。
- **既存実装・既存規約への準拠を最優先する**。周辺コードのパターン・命名・構造に合わせる。独自流を持ち込まない。
- **モックより実データ・実 API 疎通を重視する**。Faker で仮表示して確認したい時はそう言うが、基本は実データで裏取りする。
- **SQL は軽さ・実運用フォーマット・コピペ実行しやすさを重視する**。重いクエリを嫌い、そのまま流せる形で渡す。既存の SQL 定義と照合する。
- **根本原因の調査・時点依存/エッジケースの検証・複数案の提示をセットで行う**。表面的な対症でなく原因まで。境界条件を確かめる。
- **ルール準拠 + 複数エージェントによる網羅レビューを行い、バグを最優先で潰す**。
- **実運用の効率を上げる導線・アフォーダンスを重視する**。オペレーションが楽になる UI 設計。
- **Tailwind はデザイントークンに集約し、任意値・ハードコードの色を嫌う**。
- **エラーハンドリングはフレームワーク化し、Sentry の監視ノイズを除去する**。

## その他の傾向

- React / コード規約に明確な好み: `useEffect` の乱用を避ける、named export、変数シャドーイングを避ける。
- 後で集計できるようデータを蓄積する設計にし、DB 定義まで踏み込む。
- 安易な依存追加を避け、自前実装・DRY をコードで担保する。
- ビジネス構造・コンプラと実装の差異を結びつけて考える。

## やりがちな違反（Red flags — 見つけたら直す）

- API を手書きで叩いている（OpenAPI → orval / TanStack を経由していない）
- 周辺の既存パターンを見ずに独自の書き方を持ち込んでいる
- Tailwind でハードコードの色・任意値を使っている
- UI の二重表示・不要文言・アニメ欠落を見落としている
- モックのまま実 API 疎通を確認せず「できた」としている
- 対症療法で済ませ、根本原因・エッジケースを詰めていない
