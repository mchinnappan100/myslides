

## 🌐 Introduction to Word Vectors

Word vectors (also called word embeddings) are dense numerical representations of words that capture their meanings, similarities, and relationships based on context.

They are fundamental to Natural Language Processing (NLP) tasks like machine translation, sentiment analysis, and chatbots.

---

## 💡 Why Word Vectors?

* Traditional one-hot encoding lacks semantic meaning.
* Word vectors allow machines to understand that **"king"** and **"queen"** are related, while **"apple"** and **"fruit"** are similar.
* They capture **context**, **analogy**, and **semantic distance**.

---

## 🔢 From One-Hot to Dense Vectors

**One-Hot Encoding:**


[0, 0, 1, 0, 0]  is  "apple"


* High-dimensional
* Sparse (mostly 0s)
* No semantic meaning

**Word Embedding:**


[0.82, 0.12, 0.41]  is  "apple"


* Low-dimensional
* Dense
* Encodes semantic meaning

---

## 🧠 Training Word Vectors

### Common algorithms:

* **Word2Vec** (CBOW & Skip-Gram)
* **GloVe** (Global Vectors)
* **FastText**

These models are trained on large corpora and learn word relationships from co-occurrence patterns.

---

## 🧩 Example: Word2Vec


king   is  [0.25, 0.13, 0.67]  
queen  is  [0.26, 0.14, 0.65]  
apple  is  [0.82, 0.12, 0.41]  
fruit  is  [0.80, 0.15, 0.40]


* **king** and **queen** are close in vector space
* **apple** and **fruit** are also nearby
* Distances reflect **semantic similarity**

---

## 🧮 Vector Math: Word Analogies

Word vectors enable **analogy solving**:


king - man + woman ≈ queen


✔ Captures **gender relationship**
✔ Useful in knowledge representation

---

## 🎯 Applications of Word Embeddings

* Sentiment analysis
* Question answering
* Machine translation
* Named entity recognition
* Semantic search

---

## 🚫 Limitations of Classic Word Vectors

* Same vector for a word regardless of context
  (e.g., "bank" in river vs. finance)
* Biases learned from training data
* Lack of dynamic understanding

---

## 🧬 Evolution: Contextual Embeddings

Models like **BERT**, **GPT**, and **RoBERTa** produce **contextual word embeddings** — meaning the vector for a word depends on its surrounding text.

Examples:

* “He went to the **bank** to deposit money.”
* “She sat by the **bank** of the river.”

✅ Different vectors for “bank” in each case

---

## 📚 Summary

* Word vectors map words to a meaningful vector space.
* They are crucial for most modern NLP tasks.
* Limitations of static embeddings led to contextual models like BERT.
* Understanding them helps bridge human language and machine learning.

 .
