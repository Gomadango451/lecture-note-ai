# 講義ノートAI 🎙

音声ファイルを入れると、**文字起こし・要約・要点**を自動生成するWebアプリです。

---

## 機能

- 🎵 音声ファイルをアップロード（mp3 / wav / m4a / ogg / flac 対応）
- 📝 Gemini APIで文字起こし（全文）
- 💡 要約（3行）＆ 要点（箇条書き 3〜5個）を自動生成
- 📋 各結果をワンタップでコピー
- 📱 スマホ対応（レスポンシブデザイン）

---

## デモ

👉 [GitHub Pages URL]（※公開後に記入）

---

## セットアップ

### 必要なもの

- Gemini APIキー（[Google AI Studio](https://aistudio.google.com/apikey) で無料取得できます）
- モダンブラウザ（Chrome / Safari / Firefox）

### 使い方

1. アプリを開く
2. 「Gemini APIキーを設定する」を開いてAPIキーを入力 → 「保存」
3. 講義名を入力（任意）
4. 音声ファイルを選択（推奨：30秒〜2分）
5. 「文字起こし＆要約する」ボタンを押す
6. 結果が表示されたら「コピー」ボタンで内容をコピー

### ローカルで動かす場合

インストール不要です。`index.html` をブラウザで開くだけで動作します。

```bash
# リポジトリをクローン
git clone https://github.com/（あなたのユーザー名）/（リポジトリ名）.git

# index.html をブラウザで開く
open index.html
```

---

## ファイル構成

```
/
├── index.html         # アプリ本体（UI + Gemini API連携）
├── README.md          # 本ファイル
└── docs/
    ├── SPEC.md        # 仕様書
    ├── API_NOTES.md   # Gemini API仕様メモ
    ├── TESTCASES.md   # テストケース・動作証跡
    ├── STATUS.md      # 進捗記録
    └── PROMPTS.md     # AIに投げた重要プロンプト記録
└── screenshots/       # 動作証跡スクリーンショット
```

---

## 技術スタック

| 項目 | 内容 |
|------|------|
| フロントエンド | HTML / CSS / JavaScript（単一ファイル） |
| AI API | Gemini API（`gemini-2.5-flash`） |
| 公開 | GitHub Pages |
| APIキー管理 | localStorage（リポジトリには含めない） |

---

## セキュリティについて

- APIキーはブラウザのlocalStorageに保存されます
- GitHubリポジトリにはAPIキーを含めていません
- 個人情報・機密情報を含む音声のアップロードはしないでください

---

## 注意事項

- 音声ファイルは **20MB以下** を使用してください
- 長い音声（10分超）は処理に時間がかかる場合があります
- Gemini APIの無料枠にはレート制限があります（連続実行時は少し間を置いてください）

---

## 参考ドキュメント

- [Gemini API モデル一覧](https://ai.google.dev/gemini-api/docs/models?hl=ja)
- [Gemini API 音声理解](https://ai.google.dev/gemini-api/docs/audio)
