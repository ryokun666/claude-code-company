# Agent Communication System

## エージェント構成
- **PRESIDENT** (別セッション): 統括責任者
- **boss1** (multiagent:0.0): チームリーダー
- **worker1,2,3** (multiagent:0.1-3): 実行担当

## あなたの役割
- **PRESIDENT**: @instructions/president.md
- **boss1**: @instructions/boss.md
- **worker1,2,3**: @instructions/worker.md

## メッセージ送信
```bash
./agent-send.sh [相手] "[メッセージ]"
```

## 基本フロー
PRESIDENT → boss1 → workers → boss1 → PRESIDENT 

## Next.js 開発におけるベストプラクティス
以下の点に注意してください:

/(admin)/page.tsx 等のルートグループをネストルーティングとして認識してしまっている
➡ ルートグループはルーティングとしては認識しません。トップページの page.tsx と競合するのでエラーになります。

useEffect() を多用してしまう
➡ ベストプラクティスは「サーバーコンポーネントでのデータフェッチ」です。DAL 等でデータフェッチ用のファイルを作成しておき、呼び出すときはサーバーコンポーネントで行うようにしましょう。

ストリーミングデータフェッチングしてくれない
➡ サーバーコンポーネントの利用に加えて、各コンポーネントでデータ取得時にスケルトン表示したい場合は「ストリーミングデータフェッチング」を Suspense で行うように指示しましょう。

Server Actions を使ってくれない
➡ 「Server Actions で実装して」と言わないとイベントハンドラで実装するのがデフォルトになっている。可能であれば Server Actions を積極的に利用してください。

useSearchParams や動的ルーティングの /blog/[id] 等で params を受け取る際は、非同期で受け取る
➡ async/await で受け取らないとエラーになります。

Supabase をサーバー側で実行する場合は createServerClient() を利用する
➡ クライアント側で実行する場合は createClient()、サーバー側で実行する場合は createServerClient() を使い分けるのが正解です。使用モジュールは @supabase/supabase-js と @supabase/ssr です。