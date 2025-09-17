# Claris Click Dialer

VonageのVoice APIを使用したクリックツーコール（Click-to-Call）アプリケーションです。Webブラウザから電話番号をクリックするだけで電話をかけることができます。

## 機能

### 主な機能
- **クリックツーコール**: URLパラメータで電話番号を指定して即座に発信
- **着信対応**: ブラウザで着信を受けて通話可能
- **モバイル対応**: スマートフォンのブラウザでも利用可能
- **着信音通知**: 着信時に音声またはバイブレーションで通知

### 技術的特徴
- Vonage Client SDK/VCR SDKを使用したWebRTC通話
- Express.jsベースのサーバーサイド実装
- レスポンシブデザイン（Tailwind CSS）
- モバイルブラウザの音声制限に対応

## システム構成

```
claris-click-dialer/
├── index.js            # サーバーサイドアプリケーション
├── public/
│   ├── index.html      # クライアントUI
│   ├── ringtone.mp3    # 着信音ファイル
│   └── styles.css      # スタイルシート
├── vcr.yml            # VCR設定ファイル（本番用）
├── vcr-sample.yml     # VCR設定ファイルのサンプル
├── build.sh           # ビルドスクリプト
└── package.json       # Node.js依存関係

```

## 必要条件

- Node.js 18以上
- Vonageアカウント
- Vonageアプリケーション（Voice機能が有効）
- Vonage電話番号

## セットアップ

### 1. 依存関係のインストール
```bash
npm install
```

### 2. VCR設定ファイルの準備

`vcr-sample.yml`をコピーして`vcr.yml`を作成し、以下の値を設定：

```yaml
instance:
  application-id: YOUR_APPLICATION_ID  # VonageアプリケーションID
  environment:
    - name: VONAGE_NUMBER
      value: "YOUR_PHONE_NUMBER"      # Vonage電話番号（国際形式）
```

### 3. 環境変数の設定

VCRが自動的に以下の環境変数を設定します：
- `VCR_PORT`: サーバーのポート番号
- `API_APPLICATION_ID`: VonageアプリケーションID
- `PRIVATE_KEY`: アプリケーションの秘密鍵

## 起動方法

### ローカル開発環境
```bash
npm run debug
```

### 本番環境（VCR）
```bash
npm start
```

## 使用方法

### 発信（Click-to-Call）

ブラウザで以下のURLにアクセス：
```
http://localhost:PORT/?number=電話番号
```

例：
```
http://localhost:3000/?number=090XXXXYYYY
```

- 日本の番号（0で始まる）は自動的に国際形式（81）に変換されます
- ページを開くと自動的に指定番号への発信が準備されます
- 「発信」ボタンをクリックして通話を開始

### 着信

1. オペレーターがブラウザでアプリを開いている状態
2. 外部から着信があると画面に「着信中...」ボタンが表示
3. ボタンをクリックして通話開始

## API エンドポイント

### `GET /getToken`
クライアント認証用のJWTトークンを生成

**パラメータ:**
- `name`: オペレーター名（省略時は"Operator"）

### `POST /onCall`
着信/発信時のコールバックハンドラー

### `POST /onEvent`
通話イベントのコールバックハンドラー

## クライアントサイドの機能

### 通話状態管理
- **RINGING**: 呼び出し中
- **ANSWERED**: 通話中
- **COMPLETED**: 通話終了

### モバイル対応機能
- レスポンシブデザイン（画面サイズに応じたボタンサイズ）
- タッチ操作での音声有効化
- バイブレーション通知（対応デバイスのみ）

## トラブルシューティング

### スマートフォンで着信音が鳴らない
モバイルブラウザのセキュリティ制限により、ユーザー操作なしでは音声を再生できません。
一度画面をタップすることで音声が有効化されます。

### 発信できない
- URLパラメータで電話番号が正しく指定されているか確認
- Vonageアプリケーションの設定を確認
- ネットワーク接続を確認

### 着信を受けられない
- ブラウザがマイクへのアクセスを許可しているか確認
- VonageアプリケーションのWebhook URLが正しく設定されているか確認

## ライセンス

ISC

## 依存ライブラリ

- `@vonage/server-sdk`: Vonage Server SDK
- `@vonage/vcr-sdk`: Vonage VCR SDK
- `express`: Webアプリケーションフレームワーク
- `cors`: CORS対応ミドルウェア
- Tailwind CSS: CSSフレームワーク（CDN経由）