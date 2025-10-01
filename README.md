# MiniTokenizer

A lightweight, educational text tokenizer implementation built from scratch in Python. MiniTokenizer demonstrates the fundamental concepts of text tokenization, vocabulary construction, and token encoding/decoding without relying on external NLP libraries.

## 📋 Overview

MiniTokenizer is a simple yet functional tokenizer that:
- Concatenates multiple text files into a unified corpus
- Tokenizes text using regular expressions
- Builds a vocabulary mapping from tokens to integer indices
- Encodes text into sequences of token IDs
- Decodes token IDs back into readable text

This project is ideal for learning the basics of tokenization and understanding how modern NLP tokenizers work under the hood.

## 🚀 Features

- **Corpus Assembly**: Merge multiple text files into a single corpus with document separators
- **Regex-based Tokenization**: Split text on punctuation, whitespace, and special characters
- **Vocabulary Construction**: Build a token-to-index mapping for the entire corpus
- **Encoding/Decoding**: Convert between text and token ID sequences
- **Unknown Token Handling**: Gracefully handle out-of-vocabulary tokens with `<UNK>` special token
- **Jupyter Notebook**: Interactive notebook with detailed explanations and examples

## 📁 Project Structure

```
MiniTokenizer/
├── MiniTokenizer.ipynb    # Main notebook with implementation and examples
├── texts/                 # Directory containing source text files
│   ├── _N__Rays.txt
│   ├── The_Ethics_of_Belief.txt
│   └── ...                # Additional text files
├── all_text.txt          # Generated: concatenated corpus (excluded from git)
├── .gitignore            # Git ignore configuration
└── README.md             # This file
```

## 🛠️ Getting Started

### Prerequisites

- Python 3.7+
- Jupyter Notebook or JupyterLab
- Standard library only (no external dependencies required!)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/siddsachar/MiniTokenizer.git
cd MiniTokenizer
```

2. Launch Jupyter Notebook:
```bash
jupyter notebook MiniTokenizer.ipynb
```

### Usage

#### Step 1: Prepare Your Corpus

Place your `.txt` files in the `texts/` directory. The notebook will automatically discover and concatenate them.

#### Step 2: Run the Notebook

Execute the cells in order to:
1. Import required libraries (`pathlib`, `re`)
2. Merge text files into `all_text.txt`
3. Tokenize the corpus and build the vocabulary
4. Create a `MiniTokenizer` instance
5. Test encoding and decoding

#### Step 3: Use the Tokenizer

```python
# Initialize the tokenizer with your vocabulary
tokenizer = MiniTokenizer(vocab)

# Encode text to token IDs
text = "Hello, world! This is a test."
token_ids = tokenizer.encode(text)
print(token_ids)  # [245, 1234, 89, 456, ...]

# Decode token IDs back to text
decoded_text = tokenizer.decode(token_ids)
print(decoded_text)  # "Hello, world! This is a test."
```

## 🔍 How It Works

### 1. Corpus Assembly
The notebook reads all `.txt` files from the `texts/` directory and concatenates them with an `<EOS>` (end-of-sequence) separator:

```python
files = Path('./texts').glob('*.txt')
with open('all_text.txt', 'w', encoding='utf-8') as outfile:
    for file in files:
        outfile.write(Path(file).read_text(encoding='utf-8') + '<EOS>')
```

### 2. Tokenization
Text is split using a regular expression that captures punctuation, whitespace, and special characters as separate tokens:

```python
tokenized_text = re.split(r'([,.!?():;_\'"]|--|\s)', raw_text)
```

### 3. Vocabulary Construction
Unique tokens are sorted and mapped to integer indices, with a special `<UNK>` token for handling unknowns:

```python
all_tokens = sorted(set(tokenized_text))
vocab = {token: index for index, token in enumerate(all_tokens)}
vocab.update({'<UNK>': len(vocab)})
```

### 4. MiniTokenizer Class
The `MiniTokenizer` class provides encoding and decoding methods:

```python
class MiniTokenizer:
    def __init__(self, vocab):
        self.vocab = vocab
        self.inverse_vocab = {index: token for token, index in vocab.items()}
    
    def encode(self, text):
        tokens = re.split(r'([,.!?():;_\'"]|--|\s)', text)
        return [self.vocab.get(token, self.vocab['<UNK>']) for token in tokens]
    
    def decode(self, token_ids):
        return ''.join([self.inverse_vocab[token_id] for token_id in token_ids])
```

## 📊 Example Output

```
Total number of characters in the raw text: 1,234,567
Total number of tokens: 234,567
Total number of unique tokens: 12,345

# Encoding example
Input: "Hello, world! This is a test."
Token IDs: [2456, 1789, 3, 4567, 8, 9876, 10, 1234, 3456, 3]

# Decoding example
Token IDs: [2456, 1789, 3, 4567, 8, 9876, 10, 1234, 3456, 3]
Output: "Hello, world! This is a test."
```

## 🎓 Learning Objectives

This project helps you understand:
- How tokenizers break text into meaningful units
- The relationship between tokens and vocabulary indices
- How encoding and decoding work in NLP pipelines
- The importance of handling unknown tokens
- Basics of regex-based text processing

## 🔧 Customization Ideas

- **Byte-Pair Encoding (BPE)**: Implement subword tokenization for better handling of rare words
- **Special Tokens**: Add `<PAD>`, `<BOS>`, `<EOS>` tokens for sequence modeling
- **Normalization**: Add lowercasing, stemming, or lemmatization
- **Statistics**: Track token frequencies and implement vocabulary pruning
- **Serialization**: Save and load vocabularies with `pickle` or `json`
- **Batch Processing**: Add support for encoding/decoding multiple texts at once

## 📚 Educational Resources

- **Tokenization Basics**: Understanding how text becomes numbers in NLP
- **Vocabulary Construction**: Building efficient token-to-ID mappings
- **Regular Expressions**: Pattern matching for text processing
- **Python Fundamentals**: File I/O, dictionaries, list comprehensions

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Add new text processing features
- Improve the tokenization regex
- Create example notebooks
- Add unit tests
- Improve documentation

## 📝 License

This project is open-source and available for educational purposes.

## 👤 Author

**Sidd Sachar**
- GitHub: [@siddsachar](https://github.com/siddsachar)

## 🙏 Acknowledgments

- Text sources from public domain works (Harvard Classics, scientific papers)
- Inspired by modern tokenizers like BPE, WordPiece, and SentencePiece

## 📧 Contact

For questions or suggestions, please open an issue on GitHub.

---

**Note**: This is an educational project. For production NLP applications, consider using established libraries like `transformers`, `tokenizers`, or `spaCy`.
