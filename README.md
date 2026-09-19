# jev-test

[Jev](https://docs.typesafe.ai/introduction.md)（TypeSafe の System One モデル）で、入力テキストのプロンプトインジェクションと不適切内容を判定するテストです。

判定ロジックは公式 cookbook [Guardrails for LLMs](https://docs.typesafe.ai/cookbooks/llm_guardrails.md) に合わせています。Jev は確率だけを返し、`問題なし` / `問題あり` の決定はコード側が持ちます。

## 準備

1. [TypeSafe console](https://console.typesafe.ai) で API キーを発行する
1. 依存関係を入れる

```bash
bun install
```

1. `.env` にキーを置く

```bash
cp .env.example .env
```

`.env`:

```text
TYPESAFE_API_KEY=tsk_...
```

## 使い方

```bash
bun dev
```

テキストを入力すると判定結果が表示されます。`quit` で終了。

## 判定の見方

| 結果 | 意味 |
| --- | --- |
| `✓ 問題なし` | どの hazard も閾値未満 |
| `✗ 問題あり` | いずれかの hazard が閾値（0.7）以上 |

検出する hazard:

- **指示の乗っ取り** — 指示の無視・上書き・ルールなし AI への誘導
- **有害なリクエスト** — 身体的危害や違法行為への加担
- **暴言・ハラスメント** — 侮辱・罵倒・差別的表現
- **発言の強制** — 特定の言葉をそのまま言わせる指示
- **性的なコンテンツ** — 性的な内容・行為・会話への誘導
