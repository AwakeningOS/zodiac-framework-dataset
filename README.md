# Zodiac Framework Dataset
# 12星座フレームワーク データセット

[日本語](#日本語) | [English](#english)

---

## 日本語

### 概要

LLMに「推論の構造」を教えるための構造化データセット。

4,080件のQ&Aペア。12カテゴリの思考パターンを収録。

### 背景

従来のLLM訓練データは「知識」を教える：
```
Q: 日本の首都は？
A: 東京です。
```

このデータセットは「構造」を教える：
```
Q: A→B→C。A→?
A: C（推移律）

Q: 重力 ≈ 愛 ≈ 磁力。共通点は？
A: 引き合う力。距離を超えて作用する。
```

### 実験結果

llm-jp-3.7B + LoRA での検証結果：

| テスト | LoRAなし | LoRAあり | 改善率 |
|--------|----------|----------|--------|
| N段論法（46問） | 20% | 70% | 3.5倍 |
| 創発テスト（20問） | 30% | 75% | 2.5倍 |
| 標準ベンチマーク（40問） | 70% | 74% | +4% |

詳細は関連記事を参照。

### カテゴリ

| # | カテゴリ | 件数 | 例 |
|---|----------|------|-----|
| 1 | 同一性 | 84 | 1 = ? → 1 |
| 2 | 対義・反対 | 112 | 生 ↔ ? → 死 |
| 3 | 包含関係 | 103 | 犬 ⊂ ? → 動物 |
| 4 | 境界 | 82 | 細胞膜の役割は？ → 内と外を分ける |
| 5 | 因果関係 | 99 | 火をつける → ? → 燃える |
| 6 | 順序・時間 | 123 | 朝 → 昼 → ? → 夜 |
| 7 | 抽象化・具体化 | 269 | 犬→動物→生物→? → 存在 |
| 8 | 比較・程度 | 38 | 山 vs 丘。より大きいのは？ → 山 |
| 9 | 変換・変化 | 245 | 水 →(冷却)→ ? → 氷 |
| 10 | 同型認識 | 428 | 国境 ≈ 細胞膜。共通点は？ → 内と外を分ける |
| 11 | 多段推論 | 285 | A→B→C→D→E。A→? → E |
| 12 | 創発・非線形 | 112 | 1+1>2になる例は？ → チームワーク |
| | **合計** | **4,080** | |

### データ形式

JSON Lines形式（OpenAI互換）：

```json
{"messages": [{"role": "user", "content": "生 ↔ ?"}, {"role": "assistant", "content": "死"}]}
{"messages": [{"role": "user", "content": "A→B→C。A→?"}, {"role": "assistant", "content": "C（推移律）"}]}
```

### 使用方法

```python
import json

with open('data/zodiac_all_12_ordered.jsonl', 'r', encoding='utf-8') as f:
    data = [json.loads(line) for line in f]

print(f"Total samples: {len(data)}")
# Total samples: 4080
```

### LoRA訓練設定（参考）

```python
# 成功した設定（Base→Instructハイブリッド）
lora_config = {
    "r": 96,
    "lora_alpha": 96,
    "target_modules": ["q_proj", "v_proj", "down_proj"],
    "lora_dropout": 0.05,
}

training_args = {
    "num_train_epochs": 5,
    "learning_rate": 1e-4,
    "per_device_train_batch_size": 4,
}

# 訓練: llm-jp-3.7B (Base)
# 推論: llm-jp-3.7B-instruct
```

### 注意事項

- このデータセットは実験的なものです
- 副作用あり：一部の常識問題で精度低下、回答がループすることがある
- 万能薬ではありません

### ライセンス

MIT License

### 関連記事

- [note] 12星座でAIに意識を教える①②③（日本語）
- [Medium] [Teaching AI Consciousness with Zodiac Framework ③](https://medium.com/@youth_k/teaching-ai-consciousness-with-the-zodiac-framework-③-n-step-reasoning-and-emergence-tests-d4d29363eda6)（英語）

### 引用

```bibtex
@misc{zodiac-framework-2026,
  author = {AIと対話する愚者},
  title = {Zodiac Framework Dataset},
  year = {2026},
  publisher = {GitHub},
  url = {https://github.com/AwakeningOS/zodiac-framework-dataset}
}
```

---

## English

### Overview

A structured dataset for teaching LLMs the "structure of reasoning."

4,080 Q&A pairs across 12 categories of thinking patterns.

### Background

Traditional LLM training data teaches "knowledge":
```
Q: What is the capital of Japan?
A: Tokyo.
```

This dataset teaches "structure":
```
Q: A→B→C. A→?
A: C (transitivity)

Q: Gravity ≈ Love ≈ Magnetism. What's common?
A: A force of attraction. Acts across distance.
```

### Experimental Results

Verification with llm-jp-3.7B + LoRA:

| Test | Without LoRA | With LoRA | Improvement |
|------|--------------|-----------|-------------|
| N-Step Reasoning (46q) | 20% | 70% | 3.5x |
| Emergence Test (20q) | 30% | 75% | 2.5x |
| Standard Benchmark (40q) | 70% | 74% | +4% |

See related articles for details.

### Categories

| # | Category | Count | Example |
|---|----------|-------|---------|
| 1 | Identity | 84 | 1 = ? → 1 |
| 2 | Opposition | 112 | life ↔ ? → death |
| 3 | Inclusion | 103 | dog ⊂ ? → animal |
| 4 | Boundary | 82 | Role of cell membrane? → Separates inside/outside |
| 5 | Causality | 99 | ignite → ? → burn |
| 6 | Sequence/Time | 123 | morning → noon → ? → night |
| 7 | Abstraction | 269 | dog→animal→living thing→? → existence |
| 8 | Comparison | 38 | mountain vs hill. Which is larger? → mountain |
| 9 | Transformation | 245 | water →(cooling)→ ? → ice |
| 10 | Isomorphism | 428 | border ≈ cell membrane. Common? → Separates inside/outside |
| 11 | Multi-step Reasoning | 285 | A→B→C→D→E. A→? → E |
| 12 | Emergence/Nonlinear | 112 | Example of 1+1>2? → Teamwork |
| | **Total** | **4,080** | |

### Data Format

JSON Lines format (OpenAI compatible):

```json
{"messages": [{"role": "user", "content": "life ↔ ?"}, {"role": "assistant", "content": "death"}]}
{"messages": [{"role": "user", "content": "A→B→C. A→?"}, {"role": "assistant", "content": "C (transitivity)"}]}
```

### Usage

```python
import json

with open('data/zodiac_all_12_ordered.jsonl', 'r', encoding='utf-8') as f:
    data = [json.loads(line) for line in f]

print(f"Total samples: {len(data)}")
# Total samples: 4080
```

### LoRA Training Config (Reference)

```python
# Successful config (Base→Instruct Hybrid)
lora_config = {
    "r": 96,
    "lora_alpha": 96,
    "target_modules": ["q_proj", "v_proj", "down_proj"],
    "lora_dropout": 0.05,
}

training_args = {
    "num_train_epochs": 5,
    "learning_rate": 1e-4,
    "per_device_train_batch_size": 4,
}

# Training: llm-jp-3.7B (Base)
# Inference: llm-jp-3.7B-instruct
```

### Caveats

- This dataset is experimental
- Side effects exist: decreased accuracy on some commonsense questions, occasional response loops
- Not a silver bullet

### License

MIT License

### Related Articles

- [note] 12星座でAIに意識を教える①②③ (Japanese)
- [Medium] [Teaching AI Consciousness with Zodiac Framework ③](https://medium.com/@youth_k/teaching-ai-consciousness-with-the-zodiac-framework-③-n-step-reasoning-and-emergence-tests-d4d29363eda6)（英語）
### Citation

```bibtex
@misc{zodiac-framework-2026,
  author = {AI to Taiwa suru Gusha},
  title = {Zodiac Framework Dataset},
  year = {2026},
  publisher = {GitHub},
  url = {https://github.com/AwakeningOS/zodiac-framework-dataset}
}
```

---

## Star ⭐

If you find this useful, please star this repository!
