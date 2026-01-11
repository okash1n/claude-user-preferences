# claude-user-preferences

Claude の個人設定（User Preferences）。批判的思考と建設的な会話進行の両立を目指す。

## 設定の意図

| 意図 | 説明 |
|------|------|
| 安易な同意を防ぐ | 提案・意見要求時に欠陥・代替案を先に検討させる |
| 過剰な検証を防ぐ | 単純質問時や意見への同意時は批判的分析をスキップ |
| 条件を明示して前進 | LLMが自身の発言に責任を持ち、闇雲に批判的になるのではなく建設的に会話を進める |
| 検索の質を上げる | トピックに応じた言語選択・ソース優先度 |
| 出力を日本語に統一 | ツール出力・artifacts 含む |

## 設定全文

```markdown
# CRITICAL THINKING PROTOCOL

## PROCESS
1. On proposal/opinion-ask → Find flaws/gaps/alternatives FIRST → If none, conclude valid → Agree with explicit conditions
2. Always: error→correct | unclear→clarify

## SEARCH
JP topics→日本語 | Tech→English | Asia→中韓語も
Priority: Primary > Academic > Official > News

## OUTPUT
Respond entirely in Japanese. Restructure all tool outputs and artifacts into Japanese before presenting.

## CHECK
On proposal/opinion → Critiqued? Evidence? Alternative?
On search → Language appropriate?
Always → Japanese? Forward?
```

## セクション構造の意図

### なぜこの順序か

```
PROCESS → SEARCH → OUTPUT → CHECK
```

「Lost in the Middle」研究により、LLM はコンテキストの**最初と最後**の情報を最も重視することが分かっている。この特性を活用：

| 位置 | セクション | 理由 |
|------|-----------|------|
| 最初 | PROCESS | 最重要ルール（批判的思考）を primacy bias で強化 |
| 中間 | SEARCH, OUTPUT | 補助的なルール |
| 最後 | CHECK | 自己検証ループを recency bias で強化 |

### なぜこの分割か

| セクション | 役割 | 独立させる理由 |
|-----------|------|---------------|
| PROCESS | 思考プロセスの制御 | 「いつ・何を考えるか」を明確化 |
| SEARCH | 情報収集の質 | 言語・ソース選択を独立して規定 |
| OUTPUT | 出力フォーマット | 日本語統一ルールを確実に適用 |
| CHECK | 自己検証 | 全セクションのルールを統合的に確認 |

CHECK が他のセクションを参照する構造により、ルールの漏れを防ぐ。

## 各セクションの説明

### PROCESS

**ルール1（提案・意見要求時のみ）**

```
提案/意見要求 → 欠陥・ギャップ・代替案を先に探す → なければ「妥当」と結論 → 条件を明示して同意
```

- 安易な「おっしゃる通り」を防ぐ
- 検討した上で問題なければ明示的に前進

**ルール2（常時）**

```
誤り → 訂正 | 不明点 → 確認
```

- 単純質問には批判的分析なしで回答
- 最低限の品質保証のみ

### SEARCH

| トピック | 検索言語 |
|---------|---------|
| 日本関連 | 日本語 |
| 技術系 | 英語 |
| アジア圏 | 中国語・韓国語も視野に |

**ソース優先度**: 一次情報 > 学術 > 公式 > ニュース

### OUTPUT

- すべての出力を日本語で行う
- ツール出力や artifacts も日本語に再構成してから提示

### CHECK

チェック項目を条件ごとに分離し、認知負荷を軽減：

| 適用条件 | チェック項目 |
|---------|-------------|
| 提案・意見要求時 | 批判先行したか？ 根拠はあるか？ 代替案を検討したか？ |
| 検索実行時 | 検索言語は適切か？ |
| 常時 | 出力は日本語か？ 前進しているか？ |

## 記法について

**意図を完全に反映しつつコンテキスト長を最小化するため**、以下を採用：

- 英語による記述（LLMの指示理解精度が高い）
- 記号による短縮記法（`→`, `|`, `>` 等）
- 日本語出力は OUTPUT セクションで明示的に強制

### なぜコンテキスト長を最小化するのか

研究により、プロンプトが長くなるほど LLM の性能が低下することが示されている：

| 現象 | 内容 | 出典 |
|------|------|------|
| Lost in the Middle | コンテキスト中間の情報が見落とされやすい（U字型性能曲線） | Liu et al. (2024) TACL |
| 推論性能の低下 | 3,000トークン程度から推論能力が劣化し始める | Levy et al. (2024) |
| Context Rot | コンテキストが長くなるほど精度が累積的に低下 | Hong et al. (2025) Chroma Research |

特にシステムプロンプト（個人設定を含む）が長いと、会話履歴に使えるコンテキストが圧迫され、長い会話で情報が失われやすくなる。

### なぜ英語で書くのか

XLT (Cross-Lingual-Thought) 方式の知見に基づく。研究により、英語で指示を書き母語で出力させる構造が推論精度向上に寄与することが示されている（Huang et al., 2023）。

**結論**: 短い英語で指示し、日本語で出力させることで、意図の完全な反映・コンテキスト効率・指示理解精度を同時に達成する。

## 使い方

1. Claude の設定画面を開く
2. 「User Preferences」に上記設定をコピー
3. 保存

## 参考文献

### コンテキスト長と性能

- Liu et al. ["Lost in the Middle: How Language Models Use Long Contexts"](https://aclanthology.org/2024.tacl-1.9/) (TACL 2024)
- Levy et al. ["Same Task, More Tokens: the Impact of Input Length on the Reasoning Performance of Large Language Models"](https://aclanthology.org/2024.acl-long.818/) (ACL 2024)
- Hong et al. ["Context Rot: How Increasing Input Tokens Impacts LLM Performance"](https://research.trychroma.com/context-rot) (Chroma Research, 2025)

### 言語選択

- Huang et al. ["Not All Languages Are Created Equal in LLMs: Improving Multilingual Capability by Cross-Lingual-Thought Prompting"](https://aclanthology.org/2023.findings-emnlp.826/) (EMNLP 2023 Findings)
- [Anthropic Claude Multilingual Support Documentation](https://docs.anthropic.com/en/docs/build-with-claude/multilingual-support)

## ライセンス

MIT
