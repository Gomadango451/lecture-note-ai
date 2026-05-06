# API\_NOTES.md — Gemini API 仕様メモ（DAY5）

> ⚠️ このファイルは公式ドキュメントを根拠にしています。
> AIが古いモデル名を提案してきた場合は、このファイルの情報を優先してください。

\---

## 1\. 使用モデル

|項目|内容|
|-|-|
|**モデル名**|`gemini-2.5-flash`|
|**理由**|安定版かつ音声対応・高速・低コスト。本番利用に適している|
|**注意**|`gemini-2.0-flash` は非推奨（2026年時点）。AIが提案してきても使わない|

**📌 公式モデル一覧（必ず参照）：**
https://ai.google.dev/gemini-api/docs/models?hl=ja

\---

## 2\. エンドポイント

```
POST https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=YOUR\\\_API\\\_KEY
```

* 認証はクエリパラメータ `?key=` にAPIキーを付与する
* ヘッダーに `Content-Type: application/json` が必要

\---

## 3\. 音声データの渡し方（2種類）

### ✅ 方法A：inline（推奨・今回はこちら）

音声をBase64エンコードしてリクエストに直接含める方法。

**条件：リクエスト全体が 20MB 以下であること**

```json
{
  "contents": \\\[
    {
      "parts": \\\[
        {
          "text": "この音声を日本語で文字起こしし、要約と要点も作成してください。"
        },
        {
          "inline\\\_data": {
            "mime\\\_type": "audio/mp3",
            "data": "（Base64エンコードされた音声データ）"
          }
        }
      ]
    }
  ]
}
```

### 方法B：Files API（20MB超の場合）

大きいファイルは先にFiles APIでアップロードし、URIを使う。
→ 今回のMVPでは短い音声（30秒〜2分）を使うため、方法Aで十分。

**📌 音声入力の公式ドキュメント：**
https://ai.google.dev/gemini-api/docs/audio

\---

## 4\. 対応している音声フォーマット

|フォーマット|MIMEタイプ|
|-|-|
|MP3|`audio/mp3`|
|WAV|`audio/wav`|
|AAC|`audio/aac`|
|OGG Vorbis|`audio/ogg`|
|FLAC|`audio/flac`|
|AIFF|`audio/aiff`|

\---

## 5\. 音声に関する技術的な制限

|項目|内容|
|-|-|
|inlineの最大リクエストサイズ|**20MB**（音声＋テキスト合計）|
|1プロンプトの最大音声長|9.5時間|
|トークン換算|音声1秒 = 32トークン（1分 ≈ 1,920トークン）|
|サンプリングレート|16Kbpsにダウンサンプリングされる|
|チャンネル数|複数チャンネルは自動でモノラルに結合|

**→ 今回は30秒〜2分の音声でテストすること（長すぎると詰まりやすい）**

\---

## 6\. JavaScriptでのリクエスト実装パターン

```javascript
// 音声ファイルをBase64に変換
async function fileToBase64(file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onload = () => resolve(reader.result.split(',')\\\[1]); // Base64部分のみ
    reader.onerror = reject;
    reader.readAsDataURL(file);
  });
}

// Gemini APIへリクエスト
async function callGeminiAPI(apiKey, audioFile, prompt) {
  const base64Audio = await fileToBase64(audioFile);
  const mimeType = audioFile.type; // 例: "audio/mp3"

  const response = await fetch(
    `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${apiKey}`,
    {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        contents: \\\[{
          parts: \\\[
            { text: prompt },
            { inline\\\_data: { mime\\\_type: mimeType, data: base64Audio } }
          ]
        }]
      })
    }
  );

  const data = await response.json();
  return data.candidates\\\[0].content.parts\\\[0].text;
}
```

\---

## 7\. APIキーの管理

```javascript
// 保存
localStorage.setItem('gemini\\\_api\\\_key', apiKey);

// 読み込み
const apiKey = localStorage.getItem('gemini\\\_api\\\_key');
```

**⚠️ GitHubリポジトリには絶対に含めない！**

\---

## 8\. エラーコードと対処法

|HTTPステータス|意味|対処法|
|-|-|-|
|400|リクエスト形式エラー|JSON構造・MIMEタイプを確認|
|401|APIキー無効|キーのスペースや文字を確認|
|403|権限なし|Google AI Studioでキーを確認|
|429|レート制限|少し待ってから再試行|
|500|サーバーエラー|再試行。続くなら公式ステータスを確認|

\---

## 9\. 参照ドキュメント一覧

|ドキュメント|URL|
|-|-|
|モデル一覧|https://ai.google.dev/gemini-api/docs/models?hl=ja|
|音声理解|https://ai.google.dev/gemini-api/docs/audio|
|Files API|https://ai.google.dev/gemini-api/docs/files|
|ファイル入力方法|https://ai.google.dev/gemini-api/docs/file-input-methods|
|レート制限|https://ai.google.dev/gemini-api/docs/rate-limits|



