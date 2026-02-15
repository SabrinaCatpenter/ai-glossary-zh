# ai-glossary-zh

[English](README.md) | 中文

AI/ML 领域中英双语术语词典，适用于：
- 中文分词（jieba 自定义词典）
- 术语归一化（合并同义词）
- 翻译参考

## 项目背景

这个词典是我在搭建个人 AI 知识库时顺手整理的。

我的方向偏 **AI Engineer + NLP 实战**，所以词汇覆盖会有这方面的倾向，包括：
- LLM 相关：prompt engineering、RAG、fine-tuning、hallucination...
- 工具链：LangChain、Hugging Face、Ollama、n8n...
- 公司/产品：OpenAI、Anthropic、Claude、ChatGPT...

后续完善知识库时会持续更新。如果项目有人用的话，会开放让大家一起贡献。

## 快速使用

### 配合 jieba 分词

```python
import jieba

jieba.load_userdict("dict.txt")

text = "OpenAI发布了新的embedding模型，支持RAG检索增强生成"
print(list(jieba.cut(text)))
# ['OpenAI', '发布', '了', '新的', 'embedding', '模型', '，', '支持', 'RAG', '检索增强生成']
```

### 术语归一化

```python
import json

with open("glossary.json", "r", encoding="utf-8") as f:
    glossary = json.load(f)

# 构建查找表：别名 -> 标准词
lookup = {}
for entry in glossary:
    canonical = entry["en"]
    lookup[canonical.lower()] = canonical
    if entry.get("zh"):
        lookup[entry["zh"]] = canonical
    for alias in entry.get("aliases", []):
        lookup[alias.lower()] = canonical

# 使用
print(lookup.get("嵌入"))  # "embedding"
print(lookup.get("大语言模型"))  # "LLM"
print(lookup.get("克劳德"))  # "Claude"
```

## 文件说明

| 文件 | 说明 |
|------|------|
| `dict.txt` | jieba 词典格式（`词 词频 词性`） |
| `glossary.json` | 完整词汇表（含元数据） |

## 词条格式

```json
{
  "en": "embedding",
  "zh": "嵌入",
  "aliases": ["embeddings", "向量嵌入"],
  "type": "concept"
}
```

**类型说明：**
- `concept` - 概念术语（embedding、transformer、RAG）
- `company` - 公司（OpenAI、Anthropic、NVIDIA）
- `product` - 产品/工具（ChatGPT、Claude、Docker）
- `framework` - 框架/库（PyTorch、LangChain、pandas）
- `platform` - 平台（AWS、Azure、GitHub）

## 统计

- 词条总数：273
- 概念：200+
- 框架/库：20+
- 产品：20+
- 平台/公司：25+

## 贡献

目前项目刚起步，暂时由我个人维护。

如果你觉得有用，欢迎：
- Star 支持一下
- 提 Issue 反馈缺失的术语
- 等项目稳定后会开放 PR 贡献

## License

MIT
