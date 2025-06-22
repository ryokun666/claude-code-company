# 🤖 Claude Codeを用いたAI組織開発デモ（Next.js編）

このプロジェクトは、[@nishimoto265/Claude-Code-Communication](https://github.com/nishimoto265/Claude-Code-Communication) を元に、Next.jsを用いたWebサービス開発に特化する形で改良したものです。

複数のAIエージェントが協力して、Next.jsアプリケーションを開発するデモ環境です。


## 🎯 デモ概要

**President** → **Boss (PM)** → **Workers (Developers)** の階層型指示システムを通じて、仕様書に基づいたWebアプリケーション開発のプロセスをシミュレートします。

### 👥 エージェントの役割

-   **👑 PRESIDENT**: 開発したいアプリケーションの概要を指示します。基本的に人間はPRESIDENTと会話します。
-   **🎯 BOSS (Project Manager)**: 仕様書を読み解き、開発タスクを分解して各Workerに指示します。
-   **👷 WORKERS (Developers)**: 担当するコード（UI、ページ、APIなど）を実装します。

📊 PRESIDENT セッション (1ペイン)
└── PRESIDENT: プロジェクトのオーナー

📊 multiagent セッション (4ペイン)  
├── boss1: プロジェクトマネージャー
├── worker1: 開発者 (UI担当)
├── worker2: 開発者 (ページ担当)
└── worker3: 開発者 (API担当)


## 🚀 使い方

### 1. 仕様書の準備

`docs/specifications.md` に、開発したいWebアプリケーションの仕様を記述します。
（例：顧客管理システムの要件、画面レイアウト、APIエンドポイントの定義など）

わからない場合は以下のプロンプトをChatGPTなどに投げて仕様書を作成してもらい、`docs/specifications.md`にコピペしてください。

```
Webサービスの仕様書作成を支援してください。まず「システムの目的・背景・成功指標」を確認する質問を行い、一問一答形式で「ユーザーペルソナ・利用シナリオ」「MVP機能と拡張機能の優先順位」「機能要件／非機能要件（性能・セキュリティ・可用性）」「技術スタックと連携制約」「APIエンドポイントとデータモデル」「UIフロー」「受け入れ基準」を順にヒアリングし、各回答後に要約を提示、最終的にMarkdownで章立てされた仕様書（概要・要件一覧・技術構成・データモデル・API仕様・UIフロー・受け入れ基準）を出力してください。
```

### 2. 環境構築

⚠️ **注意**: 既存の `multiagent` と `president` tmuxセッションがある場合は自動的に削除されます。

```bash
./setup.sh
```

### 3. エージェントの起動

各ターミナルで `claude` コマンドを実行し、エージェントを起動・認証します。

**Presidentの起動:**
```bash
# President用ターミナルで実行
tmux attach-session -t president
claude --dangerously-skip-permissions
```

**BossとWorkerの起動:**
```bash
# multiagent用ターミナルで実行
tmux attach-session -t multiagent
for i in {0..3}; do tmux send-keys -t multiagent:0.$i 'claude --dangerously-skip-permissions' C-m; done
```

### 4. 開発開始の指示

**President**セッションで、開発開始の指示を出します。

```
あなたはpresidentです。指示書に従ってください。
```

これにより、`instructions/president.md` が読み込まれ、開発プロセスが開始されます。

## 📜 指示書と仕様書

各エージェントは、以下のドキュメントに従って行動します。

-   **全体方針**: `instructions/president.md`
-   **開発管理**: `instructions/boss.md`
-   **実装作業**: `instructions/worker.md`
-   **開発要件**: `docs/specifications.md`

## 🎬 期待される動作フロー

1.  **PRESIDENT** → **boss1**: 「顧客管理システムを開発せよ」
2.  **boss1**: `docs/specifications.md`を読み込み、タスクを計画。
3.  **boss1**: Next.jsプロジェクトをセットアップ。
4.  **boss1** → **workers**: 「UIを作れ」「ページを作れ」「APIを作れ」
5.  **workers** → (コーディング) → **boss1**: 「UIできました」「ページできました」「APIできました」
6.  **boss1** → **PRESIDENT**: 「開発が完了し、[URL]にデプロイしました」

## 🔧 手動操作

### agent-send.shを使った送信

```bash
# 基本送信
./agent-send.sh [エージェント名] [メッセージ]

# 例
./agent-send.sh boss1 "緊急タスクです"
./agent-send.sh worker1 "作業完了しました"
./agent-send.sh president "最終報告です"

# エージェント一覧確認
./agent-send.sh --list
```

## 🧪 確認・デバッグ

### ログ確認

```bash
# 送信ログ確認
cat logs/send_log.txt

# 特定エージェントのログ
grep "boss1" logs/send_log.txt

# 完了ファイル確認
ls -la ./tmp/worker*_done.txt
```

### セッション状態確認

```bash
# セッション一覧
tmux list-sessions

# ペイン一覧
tmux list-panes -t multiagent
tmux list-panes -t president
```

## 🔄 環境リセット

```bash
# セッション削除
tmux kill-session -t multiagent
tmux kill-session -t president

# 完了ファイル削除
rm -f ./tmp/worker*_done.txt

# 再構築（自動クリア付き）
./setup.sh
```

---

## 📄 ライセンス

このプロジェクトは[MIT License](LICENSE)の下で公開されています。

## 🤝 コントリビューション

プルリクエストやIssueでのコントリビューションを歓迎いたします！

---

🚀 **Agent Communication を体感してください！** 🤖✨ 