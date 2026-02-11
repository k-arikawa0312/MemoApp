# MemoApp

Expo + React Native + Firebase で作成した、シンプルなメモアプリです。  
学習をきっかけに作り始めたプロジェクトですが、認証やアカウント管理など実運用で必要になる要素も含めて継続的に改善しています。

## 概要

MemoApp は、ユーザーごとにメモを管理できるモバイルアプリです。

- メール / パスワードでのログイン・サインアップ
- Google ログイン（設定で有効化可能）
- メモの作成 / 一覧 / 詳細 / 編集 / 削除
- パスワードリセット
- ログアウト / アカウント削除

データは Firestore に保存し、`users/{uid}/memos` でユーザー単位に分離しています。

---

## 主な機能

### 認証
- Email / Password サインアップ
- Email / Password ログイン
- Google ログイン（要 clientId 設定）
- パスワード再設定メール送信

### メモ管理
- メモ作成
- メモ一覧（更新日時順）
- メモ詳細
- メモ編集
- メモ削除

### アカウント管理
- ログアウト
- アカウント削除

---

## 使用技術

- React Native
- Expo（Expo Router）
- TypeScript
- Firebase Authentication
- Cloud Firestore
- AsyncStorage（認証状態の永続化）

---

## アーキテクチャメモ

### 認証
- Firebase Auth を利用。
- `initializeAuth + getReactNativePersistence(AsyncStorage)` でログイン状態を永続化。

### データ構造
- `users/{uid}/memos/{memoId}` に保存。
- 一覧は `onSnapshot` で購読し、更新をリアルタイム反映。
- `updatedAt` で並び順を管理。

### ルーティング
- Expo Router のファイルベースルーティングを使用。
- 認証状態に応じて初期遷移先を切り替え。

---

## セットアップ

### 前提
- Node.js 18+
- npm
- Firebase プロジェクト

### 1. 依存関係インストール

```bash
npm install
```

### 2. 環境変数設定

`.env.sample` をコピーして `.env` を作成し、Firebase の値を設定してください。

```bash
cp .env.sample .env
```

必要な環境変数:

- `EXPO_PUBLIC_FB_API_KEY`
- `EXPO_PUBLIC_FB_AUTH_DOMAIN`
- `EXPO_PUBLIC_FB_PROJECT_ID`
- `EXPO_PUBLIC_FB_STORAGE_BUCKET`
- `EXPO_PUBLIC_FB_MESSAGING_SENDER_ID`
- `EXPO_PUBLIC_FB_APP_ID`

Google ログインを利用する場合は、`src/components/GoogleLoginButton.tsx` の `clientId` を設定してください。

### 3. 起動

```bash
npm run start
```

必要に応じて:

```bash
npm run android
npm run ios
npm run web
```

---

## ディレクトリ構成（抜粋）

```text
src/
  app/
    auth/
      log_in.tsx
      sign_up.tsx
    memo/
      list.tsx
      create.tsx
      detail.tsx
      edit.tsx
  components/
    GoogleLoginButton.tsx
    LogOutButton.tsx
    DeleteAccountButton.tsx
  config.ts

public/
  privacy-policy/
  delete-account/

functions/
  index.js
```

---

## 今後の改善予定

- 入力バリデーションの強化
- エラーハンドリングの共通化
- テスト（単体 / E2E）の追加
- Google ログイン設定の環境変数化
- CI で lint / typecheck を自動実行

---

## License

個人学習・ポートフォリオ用途で作成。
必要に応じてライセンスを追記してください。
