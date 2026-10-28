# 1. Web-Archive-with-AI

This project aims to apply AI tools to the field of web-archiving for the purposes of:

- automating routine preservation workflows
- facilitating seemless information discovery
- enhancing deep content understanding within and across collections. 


# 2. The Basics of LLM

## 2.1. Tokenization of Text

Text tokenization strategies fall into three primary paradigms: 

- **Word-level (Vocabulary-based)**
- **Character-level (Alphabet-based)**
- **Subword-level**

Which one to choose depends on the balance among different factors:

- vocabulary size
-  sequence length
-  computational efficiency


## 2.1.1. Paradigm Comparison Matrix

| Property | Vocabulary-Based (Word Level) | Alphabet-Based (Character Level) | Subword-Based (BPE, WordPiece, Unigram) |
|---|---|---|---|
| **Vocabulary Size (V)** | **Very Large** (50,000 to 1,000,000+) | **Very Small** (100 - 500) | **Moderate / Configurable** (30,000 - 200,000) |
| **Number of Tokens** | **Lowest** (1 token per word) | **Highest** (1 token per character) | **Optimal** (Common words = 1 token, rare words = few tokens) |
| **Out-of-Vocabulary (OOV) Rate** | **High** (Replaces unseen words with `[UNK]`) | **Zero** (All text can be built from characters) | **Zero / Near-Zero** (Fallback to character/byte tokens) |
| **Embedding Layer Parameters** | **Massive** (V x D) | **Tiny** | **Balanced / Efficient** |
| **Transformer Context Cost** | Low context usage (Short sequence length) | Extreme context usage (Very long sequences) | Low to Moderate context usage |
| **Primary Industry Use** | Classical NLP / Rule-based systems | Specific character-level tasks | **Modern LLMs** (GPT-4o, Llama 3, Claude) |

## 2.1.2. Text Examples

Taking the University of Alberta Library Vision statement as an example text,  and analyze its tokenization by different models.

"
We apply our expertise to help our community create, find, use, and analyze a wide array of information and data. We create a dynamic Library that is essential for every researcher, teacher, and learner at the University of Alberta.
"

```
1. WORD-LEVEL (Vocabulary-Based)
[We] [apply] [our] [expertise] [to] [help] [our] [community] [create,] [find,] [use,] 
[and] [analyze] [a] [wide] [array] [of] [information] [and] [data.] [We] [create] [a] 
[dynamic] [Library] [that] [is] [essential] [for] [every] [researcher,] [teacher,] [and] 
[learner] [at] [the] [University] [of] [Alberta.]

2. SUBWORD-LEVEL (BPE / WordPiece)
[We] [ apply] [ our] [ expertise] [ to] [ help] [ our] [ community] [ create] [,] [ find] [,] 
[ use] [,] [ and] [ analyze] [ a] [ wide] [ array] [ of] [ information] [ and] [ data] [.] 
[ We] [ create] [ a] [ dynamic] [ Library] [ that] [ is] [ essential] [ for] [ every] 
[ research] [er] [,] [ teacher] [,] [ and] [ learn] [er] [ at] [ the] [ University] [ of] 
[ Alberta] [.]

3. CHARACTER-LEVEL (Alphabet-Based)
[W] [e] [ ] [a] [p] [p] [l] [y] [ ] [o] [u] [r] [ ] [e] [x] [p] [e] [r] [t] [i] [s] [e] ... (238 tokens)
```

Comparative Matrix

| Metric | Vocabulary-Based (Word) | Subword-Based (BPE / WordPiece) | Alphabet-Based (Character) |
|---|---|---|---|
| **Total Token Count** | **39 tokens** | **48 tokens** | **238 tokens** |
| **Token-to-Word Ratio** | 1 token / word | 1.10 tokens / word | 6.10 tokens / word |
| **Sequence Length Impact** | Lowest sequence length | Near-optimal sequence length | 5.5 times longer** sequence than subword |
| **Transformer Context Efficiency** | Maximum space efficiency | **High efficiency** (0.91 words per token) | Extremely poor (5.5 times context consumption) |
| **OOV / Splitting Behavior** | Treats entire words as single tokens; punctuation stays attached (e.g., `[create,]`, `[data.]`). | Standard words remain 1 token. Punctuation is separated (`[,]`, `[.]`). Suffixes split logically (e.g., `[research] + [er]`). | Every single character and space is assigned an individual token. |

# 3. WARC-GPT

## 3.1. WARC-GPT Ingest

The pipeline of WARC-GPT can be presented as below:

- Parsing and Text Extraction: This entails reading warc and warc.gz files, and extracting meaningful text from HTML and PDF files
  contained inside warc/warc.gz files. This is I/O intensive. Faster storage media such as SSD, large RAM, and powerful CPU can help speedup the process, but GPU     doesn't help at this stage.

- Text Chunking: The extracted text is divided into chunks. CPU and RAM determine its performance. GPU is not necessary

- Generating Embeddings: large number of text chunks are processed to generate vector embeddings using local embedding models. GPU can provide massive performance    speedup. 
