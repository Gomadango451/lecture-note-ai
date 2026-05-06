# STATUS.md — 現在地・進捗記録（DAY5）

> AIに相談するときは、このファイルをそのまま貼ると精度が上がります。
> 状況が変わったら都度更新してください。

---

## 現在の状況

**更新日：** 　　　　年　　月　　日

**フェーズ：** フェーズ5（提出物整備）

**自己評価：** 最小機能 ✅ 達成

---

## 完了済み

- [x] SPEC.md 作成（仕様書）
- [x] API_NOTES.md 作成（Gemini API仕様メモ）
- [x] index.html 作成（UI + 文字起こし + 要約）
- [x] 動作確認（音声ファイルで文字起こし・要約の生成を確認）
- [x] TESTCASES.md 作成
- [x] STATUS.md 作成

---

## 残り作業

- [ ] PROMPTS.md 作成
- [ ] README.md 作成
- [ ] GitHubリポジトリ作成・ファイルアップロード
- [ ] GitHub Pages で公開
- [ ] スマホで動作確認（TC-08）
- [ ] TESTCASES.md に結果を記入・スクショ撮影
- [ ] 提出

---

## 現在の構成

```
/
├── index.html         # アプリ本体（UI + Gemini API連携）
├── README.md          # セットアップ・使い方（作成予定）
└── docs/
    ├── SPEC.md        # 仕様書
    ├── API_NOTES.md   # Gemini API仕様メモ
    ├── TESTCASES.md   # テストケース
    ├── STATUS.md      # 本ファイル
    ├── PROMPTS.md     # 使用プロンプト記録（作成予定）
└── screenshots/       # 動作証跡（撮影予定）
```

---

## 技術スタック

| 項目 | 内容 |
|------|------|
| フロントエンド | HTML / CSS / JavaScript（単一ファイル） |
| AI API | Gemini API（gemini-2.5-flash） |
| 公開方法 | GitHub Pages（予定） |
| APIキー管理 | localStorage（リポジトリに含めない） |

---

## 詰まったこと・解決したこと

| 日付 | 詰まった内容 | 解決方法 |
|------|------------|---------|
| | （例：400エラーが出た） | （例：MIMEタイプを audio/mp3 に修正） |

---

## メモ・気づき

（開発中に気づいたことや工夫した点をここに書く）
