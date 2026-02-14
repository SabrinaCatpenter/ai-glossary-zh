# ai-glossary-zh

English | [中文](README_CN.md)

A bilingual (Chinese-English) glossary for AI/ML terminology, designed for:
- Chinese word segmentation (jieba)
- Term normalization (merge synonyms)
- Translation reference

## Quick Start

### Use with jieba

```python
import jieba

# Load the dictionary
jieba.load_userdict("dict.txt")

# Now jieba recognizes AI terms correctly
text = "OpenAI发布了新的embedding模型，支持RAG检索增强生成"
print(list(jieba.cut(text)))
# ['OpenAI', '发布', '了', '新的', 'embedding', '模型', '，', '支持', 'RAG', '检索增强生成']
```

### Use for term normalization

```python
import json

with open("glossary.json", "r", encoding="utf-8") as f:
    glossary = json.load(f)

# Build lookup: alias -> canonical term
lookup = {}
for entry in glossary:
    canonical = entry["en"]
    lookup[canonical.lower()] = canonical
    if entry.get("zh"):
        lookup[entry["zh"]] = canonical
    for alias in entry.get("aliases", []):
        lookup[alias.lower()] = canonical

# Normalize terms
print(lookup.get("嵌入"))  # "embedding"
print(lookup.get("大语言模型"))  # "LLM"
```

## Files

| File | Description |
|------|-------------|
| `dict.txt` | jieba dictionary format (`term frequency pos`) |
| `glossary.json` | Full glossary with metadata |

## Glossary Format

```json
{
  "en": "embedding",
  "zh": "嵌入",
  "aliases": ["embeddings", "向量嵌入"],
  "type": "concept"
}
```

**Types:**
- `concept` - AI/ML concepts (embedding, transformer, RAG)
- `company` - Companies (OpenAI, Anthropic, NVIDIA)
- `product` - Products/tools (ChatGPT, Claude, Docker)
- `framework` - Libraries/frameworks (PyTorch, LangChain, pandas)
- `platform` - Platforms (AWS, Azure, GitHub)

## Stats

- Total entries: 202
- Concepts: 140+
- Frameworks: 15+
- Products: 15+
- Platforms/Companies: 20+

## Contributing

PRs welcome! To add new terms:

1. Edit `glossary.json`
2. Run `python scripts/generate_dict.py` to regenerate `dict.txt`
3. Submit PR

### Term format

```json
{
  "en": "your-term",
  "zh": "中文翻译",
  "aliases": ["alias1", "alias2"],
  "type": "concept|company|product|framework|platform"
}
```

## Source

Initial terms extracted from 924 AI-related articles using:
1. jieba word segmentation
2. LLM filtering (GPT-4o-mini)
3. LLM bilingual pairing

## License

MIT
