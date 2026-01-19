GPT token embedding:-


<img width="512" height="94" alt="image" src="https://github.com/user-attachments/assets/05779768-4dd7-473d-94a6-42e60de3abd1" />

---

# Token Embeddings

**Token embeddings** are numeric representations of words.  
If we simply assign random numbers to words, their semantic meaning or relationships are lost:

```
cat = 13
kitten = 32
```

Here, there’s no way to measure similarity between *cat* and *kitten*.


---

# Word Representations

There are several ways to represent words numerically so that machines can process them:

1. **Random Assignment**  
   - Each word is mapped to a random number.  
   - Problem: No semantic meaning is preserved.  
   - Example:  
     ```
     cat = 13
     kitten = 32
     ```
     → No way to measure similarity between *cat* and *kitten*.

2. **One-Hot Encoding (OHE)**  
   - Build a dictionary of all words in the vocabulary.  
   - Each word is represented as a vector of length = vocabulary size.  
   - All positions are `0` except one unique index which is `1`.  
   - Example (vocab = [dog, puppy, cat, kitten]):  
     ```
     dog    = [1,0,0,0]
     puppy  = [0,1,0,0]
     cat    = [0,0,1,0]
     kitten = [0,0,0,1]
     ```
   - **How it works:**  
     - Each word has a unique slot in the vector.  
     - Vectors are orthogonal (no overlap).  
     - Limitation: *dog* and *puppy* are just as far apart as *dog* and *banana*.  
     - No semantic similarity is captured.

3. **Dense Embeddings (Word2Vec, GloVe, Transformer embeddings)**  
   - Words are mapped to dense vectors (e.g., 300 dimensions).  
   - Vectors are trained so that semantically similar words are close in space.
     <img width="442" height="234" alt="image" src="https://github.com/user-attachments/assets/48dff7d5-83db-4c32-8f2a-b9a7d24fcb43" />

   - Example:  
     - *cat* and *kitten* vectors will be near each other.  
     - Similarity can be measured using cosine similarity.

---

## Key Takeaway

- **Random numbers** → meaningless.  
- **One-Hot Encoding** → unique identifiers, but no similarity.  
- **Dense Embeddings** → capture semantic meaning, enabling similarity comparisons.

---


## 🔧 How Dense Vectors (Embeddings) Are Created

Dense vectors aren’t assigned randomly — they’re **learned** during training. Here’s the process:

### 1. **Training Objective**
- Models are trained on huge text corpora.
- The goal is to predict context:  
  - **Word2Vec (Skip-gram/CBOW):** Predict nearby words given a target word (or vice versa).  
  - **GloVe:** Factorize word co-occurrence statistics.  
  - **Transformers (BERT, GPT):** Learn contextual embeddings by predicting masked words or next tokens.

### 2. **Initialization**
- Each word starts with a random vector (say 300 dimensions).
- These vectors are parameters of the model.

### 3. **Optimization**
- During training, the model adjusts vectors so that:
  - Words appearing in similar contexts move closer in vector space.
  - Example: *cat* and *kitten* often appear near “pet,” “animal,” “cute.”  
    → Their embeddings shift to be close together.

### 4. **Result**
- After training, each word has a dense vector (e.g., `[0.12, -0.45, 0.88, …]`).
- Similar words cluster together in this high-dimensional space.
- Similarity can be measured with **cosine similarity** or **dot product**.

---

## 🧩 Example

Imagine training on sentences:

```
The cat is sleeping.
The kitten is playful.
```

- The model sees *cat* and *kitten* in similar contexts.  
- Their vectors gradually move closer.  
- Later, when you compute similarity:  
  ```
  cosine(cat, kitten) ≈ high value
  cosine(cat, banana) ≈ low value
  ```

---





<img width="1600" height="800" alt="image" src="https://github.com/user-attachments/assets/283a3f9c-70f1-4b6e-8b14-e7d4d29f5730" />



---

## 🐾 Word2Vec (Skip-Gram Model)

**Idea:**  
Train a neural network to predict the *context words* given a *target word*.  
The embeddings are the hidden layer weights learned during training.

### How it works:
1. Take a sentence:  
   ```
   The cat sat on the mat
   ```
2. Choose a target word: **cat**  
3. Define a window size (say 2). Context words around "cat" are: **the, sat**  
4. Training objective:  
   - Input: "cat"  
   - Output: predict "the" and "sat"  
5. The network adjusts the vector for "cat" so that it becomes useful for predicting its neighbors.  
6. Over millions of sentences, words that appear in similar contexts (like *cat* and *kitten*) end up with similar vectors.

👉 Example:  
- "cat" → vector `[0.12, -0.45, 0.88, …]`  
- "kitten" → vector `[0.10, -0.40, 0.85, …]`  
- Cosine similarity(cat, kitten) ≈ high.

---

## 📊 GloVe (Global Vectors)

**Idea:**  
Instead of predicting neighbors, GloVe uses **global co-occurrence statistics** of words across the entire corpus.

### How it works:
1. Build a co-occurrence matrix:  
   - Rows = target words  
   - Columns = context words  
   - Values = how often they appear together.  
   Example (simplified):  

   |       | cat | kitten | dog | banana |
   |-------|-----|--------|-----|--------|
   | cat   |  —  |  50    | 40  |   1    |
   | kitten| 50  |   —    | 30  |   0    |
   | dog   | 40  |  30    | —   |   2    |
   | banana|  1  |   0    |  2  |   —    |

2. GloVe trains embeddings so that the **dot product of two word vectors** approximates the log of their co-occurrence probability.  
   - If *cat* and *kitten* co-occur often, their vectors will be close.  
   - If *cat* and *banana* rarely co-occur, their vectors will be far apart.

👉 Example:  
- "cat" vector and "kitten" vector → high similarity because they co-occur in contexts like "cute," "pet," "animal."  
- "cat" and "banana" → low similarity.

---

## ⚖️ Key Difference

| Aspect              | Word2Vec (Skip-Gram) | GloVe |
|---------------------|----------------------|-------|
| Training objective  | Predict context words given a target word | Factorize co-occurrence matrix |
| Focus               | Local context (window around words) | Global statistics (entire corpus) |
| Example intuition   | "cat" helps predict "sat" | "cat" and "kitten" co-occur often → similar vectors |

---

✅ **Takeaway:**  
- **Word2Vec Skip-Gram** learns embeddings by predicting neighbors (local context).  
- **GloVe** learns embeddings by compressing global co-occurrence statistics.  
- Both produce dense vectors where semantic similarity is captured.

---






