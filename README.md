# 1. Web-Archive-with-AI

To integrate AI tools with web-archive preservation, discovery and understanding.


# 2. The Basics of LLM

## 2.1. Tokenization of Text

Text tokenization strategies fall into three primary paradigms: **Word-level (Vocabulary-based)**, **Character-level (Alphabet-based)**, and **Subword-level**. Selecting among them involves balancing vocabulary size, sequence length, and computational efficiency across different natural languages.

---

## 1. Paradigm Comparison Matrix

| Property | Vocabulary-Based (Word Level) | Alphabet-Based (Character Level) | Subword-Based (BPE, WordPiece, Unigram) |
|---|---|---|---|
| **Vocabulary Size (V)** | **Very Large** (50,000 to 1,000,000+) | **Very Small** (100 - 500) | **Moderate / Configurable** (30,000 - 200,000) |
| **Number of Tokens** | **Lowest** (1 token per word) | **Highest** (1 token per character) | **Optimal** (Common words = 1 token, rare words = few tokens) |
| **Out-of-Vocabulary (OOV) Rate** | **High** (Replaces unseen words with `[UNK]`) | **Zero** (All text can be built from characters) | **Zero / Near-Zero** (Fallback to character/byte tokens) |
| **Embedding Layer Parameters** | **Massive** (V x D) | **Tiny** | **Balanced / Efficient** |
| **Transformer Context Cost** | Low context usage (Short sequence length) | Extreme context usage (Very long sequences) | Low to Moderate context usage |
| **Primary Industry Use** | Classical NLP / Rule-based systems | Specific character-level tasks | **Modern LLMs** (GPT-4o, Llama 3, Claude) |



