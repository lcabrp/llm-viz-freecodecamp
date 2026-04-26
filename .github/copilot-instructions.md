# LLM Visualization Tutorial — Copilot Instructions

This repo is a **learning project** following the freeCodeCamp tutorial on Large Language Model (LLM) internals and visualization. Primary focus: understanding embeddings, attention mechanisms, and transformer architecture through hands-on visualization.

**Tutorial Source:** https://www.freecodecamp.org/news/how-llms-work-under-the-hood/  
**Tech Stack:** Python 3.13, numpy, matplotlib, Jupyter notebooks  
**Package Manager:** uv (fast Python package management)

---

## Project Structure

```
llm-viz-freecodecamp/
├── llm-viz.ipynb              # Main Jupyter notebook (tutorial walkthrough)
├── main.py                    # Standalone Python script version
├── pyproject.toml             # Dependencies
├── *.png                      # Generated visualizations
│   ├── positive_sentiment.png
│   ├── negative_sentiment.png
│   ├── word_analogies.png
│   └── sentence_comparison.png
└── README.md
```

---

## Project Purpose

### Learning Objectives

This project helps understand:

1. **Word Embeddings:** How words are represented as vectors in high-dimensional space
2. **Semantic Similarity:** Computing cosine similarity between word vectors
3. **Word Analogies:** Vector arithmetic (e.g., "king" - "man" + "woman" ≈ "queen")
4. **Sentence Embeddings:** Aggregating word vectors to represent sentences
5. **Attention Mechanisms:** How transformers focus on relevant parts of input
6. **Visualization:** Plotting embeddings in 2D/3D space

### Not Production Code

**Important:** This is a **tutorial/learning project**, not production-grade software:
- Focus on clarity and educational value over performance
- Simplified implementations for demonstration
- Limited error handling
- No comprehensive testing

---

## Python Conventions

### Jupyter Notebook Workflow

**Primary interface:** `llm-viz.ipynb`

```bash
# Launch Jupyter notebook
uv run jupyter notebook llm-viz.ipynb

# Or if uv not in PATH
python -m uv run jupyter notebook llm-viz.ipynb
```

**Notebook structure:**
- Markdown cells: Explanations and theory
- Code cells: Implementations and visualizations
- Output cells: Generated plots and results

### Standalone Script

**Alternative execution:** `main.py`

```bash
uv run python main.py
```

Contains same code as notebook, organized as linear script.

### Visualization Patterns

```python
import matplotlib.pyplot as plt
import numpy as np

# Standard figure sizing for clarity
plt.figure(figsize=(10, 6))

# Clear labels and titles
plt.title('Word Embeddings Visualization')
plt.xlabel('Dimension 1')
plt.ylabel('Dimension 2')

# Save visualizations
plt.savefig('output_name.png', dpi=300, bbox_inches='tight')
plt.show()
```

### Vector Operations

```python
# Cosine similarity (key concept)
def cosine_similarity(vec1, vec2):
    """
    Calculate cosine similarity between two vectors.
    Returns: float in range [-1, 1]
    - 1: vectors point in same direction (very similar)
    - 0: vectors are orthogonal (unrelated)
    - -1: vectors point in opposite directions (opposite meaning)
    """
    dot_product = np.dot(vec1, vec2)
    norm_product = np.linalg.norm(vec1) * np.linalg.norm(vec2)
    return dot_product / norm_product

# Word analogy (vector arithmetic)
def word_analogy(word_vectors, word1, word2, word3):
    """
    Solve analogy: word1 is to word2 as word3 is to ?
    Example: "king" - "man" + "woman" ≈ "queen"
    """
    vec = word_vectors[word1] - word_vectors[word2] + word_vectors[word3]
    # Find most similar word to result vector
    # (excluding input words)
```

---

## Setup & Dependencies

### Using uv (Recommended)

**Fast package manager that simplifies setup:**

```bash
# Install uv (if not already installed)
pip install uv

# Or use official installer
# Windows (PowerShell):
irm https://astral.sh/uv/install.ps1 | iex

# macOS/Linux:
curl -LsSf https://astral.sh/uv/install.sh | sh

# Sync dependencies (creates .venv and installs packages)
uv sync

# Run notebook
uv run jupyter notebook llm-viz.ipynb
```

### Traditional pip (Alternative)

```bash
# Create virtual environment
python -m venv .venv

# Activate
.venv\Scripts\activate  # Windows
source .venv/bin/activate  # macOS/Linux

# Install dependencies
pip install -r requirements.txt  # If requirements.txt exists
# Or install from pyproject.toml
pip install -e .
```

---

## Key Concepts & Patterns

### Embeddings Representation

```python
# Word embeddings as numpy arrays
word_embeddings = {
    'cat': np.array([0.2, 0.8, 0.5, ...]),  # 300-dim vector
    'dog': np.array([0.3, 0.7, 0.4, ...]),
    'car': np.array([0.9, 0.1, 0.2, ...])
}

# Semantic similarity
similarity = cosine_similarity(
    word_embeddings['cat'], 
    word_embeddings['dog']
)
# Result: high similarity (~0.8) because both are animals
```

### Sentence Embeddings

**Simple aggregation approach:**

```python
def sentence_embedding(words, word_vectors):
    """
    Create sentence embedding by averaging word vectors.
    This is a simplified approach; production models use more sophisticated methods.
    """
    vectors = [word_vectors[word] for word in words if word in word_vectors]
    return np.mean(vectors, axis=0)

# Example
sentence1 = "the cat sat on the mat"
emb1 = sentence_embedding(sentence1.split(), word_embeddings)

sentence2 = "the dog sat on the rug"
emb2 = sentence_embedding(sentence2.split(), word_embeddings)

similarity = cosine_similarity(emb1, emb2)
# High similarity because sentences have similar structure and meaning
```

### Dimensionality Reduction for Visualization

```python
from sklearn.manifold import TSNE

# Reduce 300D embeddings to 2D for plotting
embeddings_matrix = np.array(list(word_embeddings.values()))
tsne = TSNE(n_components=2, random_state=42)
embeddings_2d = tsne.fit_transform(embeddings_matrix)

# Plot
plt.scatter(embeddings_2d[:, 0], embeddings_2d[:, 1])
for i, word in enumerate(word_embeddings.keys()):
    plt.annotate(word, (embeddings_2d[i, 0], embeddings_2d[i, 1]))
```

---

## Visualization Outputs

### Generated Images

1. **positive_sentiment.png:** Words with positive connotations clustered together
2. **negative_sentiment.png:** Words with negative connotations clustered together
3. **word_analogies.png:** Visual demonstration of vector arithmetic
4. **sentence_comparison.png:** Sentence embeddings in 2D space

**Usage:**
- Include in documentation
- Present in learning materials
- Compare different embedding techniques

---

## Common Workflows

### Working Through Tutorial

1. **Open notebook:** `uv run jupyter notebook llm-viz.ipynb`
2. **Read markdown cells:** Understand theory and concepts
3. **Run code cells:** Execute implementations step-by-step
4. **Experiment:** Modify parameters, try different words/sentences
5. **Generate visualizations:** Save outputs as PNG files

### Experimenting with Embeddings

1. **Load or generate word vectors**
2. **Test similarity:** Compare different word pairs
3. **Try analogies:** Experiment with vector arithmetic
4. **Visualize:** Plot embeddings in 2D/3D
5. **Document findings:** Add markdown cells with observations

### Creating Standalone Version

1. **Extract notebook code:** Copy from notebook cells
2. **Organize in main.py:** Structure as functions
3. **Add command-line args:** Parse inputs for flexibility
4. **Test execution:** `uv run python main.py`

---

## Learning Resources

### Tutorial Source

**freeCodeCamp Article:**
https://www.freecodecamp.org/news/how-llms-work-under-the-hood/

Topics covered:
- Tokenization
- Embeddings
- Attention mechanisms
- Transformer architecture
- Training process

### Related Concepts

- **Pre-trained embeddings:** Word2Vec, GloVe, FastText
- **Contextual embeddings:** BERT, GPT
- **Transformer architecture:** Self-attention, positional encoding
- **Fine-tuning:** Adapting models to specific tasks

### Further Reading

- **Papers:** "Attention Is All You Need" (Vaswani et al.)
- **Courses:** Stanford CS224N (NLP with Deep Learning)
- **Tools:** Hugging Face Transformers, PyTorch, TensorFlow

---

## Code Style Preferences

### Simplicity Over Optimization

```python
# ✓ PREFERRED - Clear and educational
def calculate_similarity(vec1, vec2):
    """Calculate cosine similarity between two vectors"""
    dot_product = np.dot(vec1, vec2)
    norm_product = np.linalg.norm(vec1) * np.linalg.norm(vec2)
    return dot_product / norm_product

# ✗ AVOID - Production-grade but harder to learn from
def calculate_similarity_optimized(vec1, vec2):
    """Optimized similarity with edge cases, caching, and vectorization"""
    # Complex optimizations...
```

### Descriptive Variable Names

```python
# ✓ GOOD - Clear purpose
word_embeddings = {...}
sentence_similarity = cosine_similarity(emb1, emb2)

# ✗ AVOID - Cryptic names
we = {...}
sim = cos_sim(e1, e2)
```

### Inline Comments for Learning

```python
# Calculate dot product (measures alignment of vectors)
dot_product = np.dot(vec1, vec2)

# Calculate magnitude product (normalizes by vector lengths)
norm_product = np.linalg.norm(vec1) * np.linalg.norm(vec2)

# Divide to get cosine similarity (range: -1 to 1)
similarity = dot_product / norm_product
```

---

## Project Scope

### What This Project Is

- **Educational:** Learn LLM fundamentals through hands-on coding
- **Visual:** Generate plots to understand concepts
- **Experimental:** Safe space to try different approaches
- **Tutorial-based:** Follow structured learning path

### What This Project Is Not

- **Production system:** No error handling, scaling, or optimization
- **Complete LLM:** Simplified implementations for learning
- **Research code:** Not exploring novel techniques
- **Library:** Not meant to be imported by other projects

---

## References

- **Tutorial:** https://www.freecodecamp.org/news/how-llms-work-under-the-hood/
- **uv Documentation:** https://github.com/astral-sh/uv
- **NumPy Docs:** https://numpy.org/doc/
- **Matplotlib Gallery:** https://matplotlib.org/stable/gallery/
