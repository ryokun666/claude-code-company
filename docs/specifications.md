# Webサービス仕様書: [プロジェクト名]

## 1. プロジェクト概要 (Project Overview)

### 1.1 プロダクトの目的 (Purpose)
このWebサービスが解決する課題、目指すゴールを記述します。

### 1.2 ターゲットユーザー (Target Audience)
このサービスの主な利用者層を記述します。ペルソナを設定するとより明確になります。

### 1.3 主要機能一覧 (High-level Features)
このサービスが提供する主要な機能を箇条書きで記述します。
- (例) ユーザー認証機能
- (例) 投稿機能
- (例) 投稿へのコメント機能

## 2. 技術スタック (Technology Stack)

使用する技術を記述します。
- **フロントエンド**:
- **バックエンド**:
- **データベース**:
- **インフラ**:
- **その他利用サービス**:

## 3. データベース設計 (Database Schema)

ER図やテーブル定義を記述します。

### 3.1 ER図 (ER Diagram)
(Mermaid.jsなどで記述するか、画像を貼り付け)

### 3.2 テーブル定義 (Table Definitions)

#### Usersテーブル
| カラム名 | データ型 | 説明 | 制約 |
|---|---|---|---|
| id | INTEGER | 主キー | PRIMARY KEY |
| name | VARCHAR(255) | ユーザー名 | NOT NULL |
| email | VARCHAR(255) | メールアドレス | NOT NULL, UNIQUE |
| password_hash | VARCHAR(255) | ハッシュ化されたパスワード | NOT NULL |
| created_at | TIMESTAMP | 作成日時 | |
| updated_at | TIMESTAMP | 更新日時 | |

#### Postsテーブル
(同様に定義)


## 4. API仕様 (API Specification)

APIエンドポイントの詳細を記述します。[Swagger/OpenAPI](https://swagger.io/)などの利用も検討します。

### 4.1 ユーザー認証 (Authentication)
- `POST /api/v1/signup`: 新規ユーザー登録
- `POST /api/v1/login`: ログイン
- `POST /api/v1/logout`: ログアウト

#### POST /api/v1/signup
- **説明**: 新規ユーザーを作成する
- **リクエストボディ**:
  ```json
  {
    "name": "username",
    "email": "user@example.com",
    "password": "password123"
  }
  ```
- **レスポンス (201 CREATED)**:
  ```json
  {
    "id": 1,
    "name": "username",
    "email": "user@example.com"
  }
  ```
- **レスポンス (400 BAD REQUEST)**:
  ```json
  {
    "error": "Invalid input"
  }
  ```

(以下、他のエンドポイントも同様に記述)

## 5. 画面仕様 (Screen Specification)

各画面のデザインや動作について記述します。Figmaなどのデザインツールへのリンクを貼ることが望ましいです。

### 5.1 画面一覧
- トップページ (非ログイン時)
- ダッシュボード (ログイン後トップ)
- ログインページ
- 新規登録ページ
- 投稿一覧ページ
- 投稿詳細ページ
- 投稿作成/編集ページ

### 5.2 各画面の詳細

#### 5.2.1 トップページ (非ログイン時)
- **URL**: `/`
- **ワイヤーフレーム/デザイン**: [Figma Link]
- **画面概要**: サービスの紹介、ログイン/新規登録への導線を表示する。
- **コンポーネント**:
  - ヘッダー
    - ロゴ
    - ログインボタン
    - 新規登録ボタン
  - ヒーローセクション
    - キャッチコピー
    - サービス概要
    - CTAボタン (新規登録へ)
  - フッター

(以下、他の画面も同様に記述)


## 6. 非機能要件 (Non-functional Requirements)

- **パフォーマンス**:
  - (例) 各ページの表示速度は3秒以内とする。
- **セキュリティ**:
  - (例) パスワードはハッシュ化して保存する。
  - (例) 全ての通信はHTTPSで行う。
- **SEO**:
  - (例) 各ページに適切な `title`, `meta description` を設定する。 