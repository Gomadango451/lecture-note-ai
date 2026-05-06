# PROMPTS.md — AIに投げた重要プロンプト記録（DAY5）

> この課題でAIに投げた重要な指示と、採用した回答をまとめたファイルです。
> 「なぜそうしたか」の根拠が残るので、振り返りや横展開に役立ちます。

\---

## P-01：仕様書（SPEC.md）の作成

**投げたAI：** Claude

**プロンプト：**

```
DAY5の課題内容をもとに、SPEC.mdを作成してください。
以下を含めること：
- 画面の流れ（ユーザー導線）
- 入力・出力の仕様
- 技術スタック
- エラー処理のパターン
- APIキーの取り扱い方針
```

**採用した内容：**

* 技術スタック：HTML/CSS/JS（単一ファイル）+ GitHub Pages
* APIキー管理：localStorage（リポジトリに含めない）
* エラーケース：APIキーなし・ファイルなし・API失敗（400/401）

\---

## P-02：Gemini APIの最新情報収集（API\_NOTES.md）

**投げたAI：** Claude

**プロンプト：**

```
以下の公式ドキュメントを参照して、API\_NOTES.mdを作成してください。
- モデル一覧：https://ai.google.dev/gemini-api/docs/models?hl=ja
- 音声理解：https://ai.google.dev/gemini-api/docs/audio

含めること：
- 使用するモデル名（最新の安定版）
- エンドポイントURL
- 音声データの渡し方（inline / Files API）
- 対応フォーマット・サイズ制限
- JavaScriptでの実装パターン
```

**採用した内容：**

* モデル：`gemini-2.5-flash`（安定版・音声対応）

  * 根拠：https://ai.google.dev/gemini-api/docs/models?hl=ja
* 音声の渡し方：Base64 inline（20MB以下）

  * 根拠：https://ai.google.dev/gemini-api/docs/audio
* `gemini-2.0-flash` は非推奨のため不使用

**重要な気づき：**
AIが最初に提案してきた `gemini-2.0-flash` は公式で非推奨になっていた。
公式ドキュメントを先に確認したことで、古いモデルを使うミスを防げた。

\---

## P-03：アプリ本体（index.html）の作成

**投げたAI：** Claude

**プロンプト：**

```
SPEC.mdとAPI\_NOTES.mdをもとに、講義音声ノートAIのindex.htmlを作成してください。

条件：
- HTML/CSS/JS 単一ファイル
- スマホ対応（レスポンシブ）
- APIキーはlocalStorageで管理（リポジトリに含めない）
- 使用モデル：gemini-2.5-flash
- 音声：Base64 inlineで送信
- 処理の流れ：文字起こし → 要約・要点（2回APIを呼ぶ）
- 各結果にコピーボタンを設置
- エラーはユーザーにわかりやすく表示
```

**採用した内容：**

* 文字起こしと要約を2ステップに分けて実装（精度が上がるため）
* ローディング中はボタンを非活性化
* 20MB超のファイルはクライアント側で事前にエラー表示

\---

## P-04：文字起こし用プロンプト（アプリ内）

**用途：** アプリがGemini APIに送るプロンプト

**採用したプロンプト：**

```
これは「{講義名}」の講義音声です。この音声を日本語でそのまま文字起こしして、全文を出力してください。
```

（講義名が未入力の場合は「この音声を日本語でそのまま文字起こしして、全文を出力してください。」）

**採用理由：**

* 「そのまま」を入れることで要約せず全文を出力させる
* 講義名を入れることでコンテキストを与え、専門用語の認識精度が上がる

\---

## P-05：要約・要点用プロンプト（アプリ内）

**用途：** アプリがGemini APIに送るプロンプト

**採用したプロンプト：**

```
以下は講義の文字起こしです。

{文字起こし全文}

以下の形式で出力してください：

【要約】
3行以内で内容を簡潔にまとめる

【要点】
・（重要ポイント1）
・（重要ポイント2）
・（重要ポイント3）
（最大5個まで）
```

**採用理由：**

* 出力形式を明示することで、毎回安定したフォーマットで返ってくる
* 文字起こしを一度挟んでから要約させることで精度が上がる

\---

## 参照した公式ドキュメント

|ドキュメント|URL|
|-|-|
|Gemini モデル一覧|https://ai.google.dev/gemini-api/docs/models?hl=ja|
|音声理解（Audio understanding）|https://ai.google.dev/gemini-api/docs/audio|
|Files API|https://ai.google.dev/gemini-api/docs/files|
|ファイル入力方法|https://ai.google.dev/gemini-api/docs/file-input-methods|



