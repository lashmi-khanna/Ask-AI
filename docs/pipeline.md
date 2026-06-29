---
layout: default
title: NLP Pipeline
nav_order: 2
---

# NLP Pipeline

Natural Language Processing (NLP) serves as the data translation layer of the ASK AI assistant. Because computers cannot natively comprehend raw text, NLP acts as a bidirectional bridge, converting unstructured research papers into vectors for semantic searching, and decoding calculated vector answers back into fluid human language.

The production system runs across 4 sequential stages:

![Core Architecture Flowchart](core_architecture.jpeg)

<br>

## Stage 1: Text Preprocessing & Chunking
**Core Objective:** Cleans background noise from raw PDF text and partitions massive documents into small, manageable text sections.

**Key Operations:**
* **Lowercasing:** Standardizes text so variations like "PPO", "Ppo", and "ppo" map to the exact same word.
* **Punctuation Removal:** Strips structural characters (!, ?, @) that do not add semantic value.
* **spaCy Lemmatization:** Resolves words to their structural dictionary roots (e.g., "clipping" and "clipped" both become "clip").
* **Section-Based Chunking:** Slices lengthy research papers into focused blocks of 300–500 tokens while keeping logical document sections intact(e.g. Abstract).
* **Sliding Window Overlap:** Maintains a 50-token overlap between consecutive blocks to guarantee that contextual meaning is not split or lost at hard chunk boundaries.

<br>

## Stage 2: Embedding Generation
**Core Objective:** Converts normalized text blocks into a matrix of dense numbers (vectors) that mathematically capture human meaning.

**Key Operations:**
* **Model Selection:** Implements the localized all-MiniLM-L6-v2 SentenceTransformer model.
* **Vector Output:** Compresses every text chunk into a continuous numeric array of exactly 384 dimensions.
* **Semantic Proximity:** Operates on spatial geometry[cite: 2]. Words or paragraphs sharing same meaning sit near one another. Themes like "clipping mechanism" and "training stability" are automatically grouped close together.

<br>

## Stage 3: Vector Database (ChromaDB)
**Core Objective:** To index, store, and provide ultra-fast geometric proximity matching (Cosine Similarity) for high-dimensional vector arrays.

**Key Operations:**
* **Cosine Similarity Metric:** Uses cosine formula or inbuilt method to find angle between two vectors, to find similarity between them.

```text
Cosine Similarity = (A · B) / (|A| * |B|)
```

* **Result is between -1 and 1:**
  * **1.0** → identical direction (same meaning)
  * **0.0** → completely unrelated (90° apart)
  * **-1.0** → opposite meanings
* **HNSW Indexing:** Leverages a Hierarchical Navigable Small World (HNSW) multi-layered graph. It acts like an express highway system, skipping broad landmark clusters at upper layers before dropping into lower layers to find matching paragraphs instantly.
* **Metadata Binding:** Permanently attaches the source text and tracking keys (e.g., file_name: PPO.pdf, page: 4) to the back of the vector array as Metadata for instant post-search retrieval.

<br>

## Stage 4: Large Language Model (LLM) Inference
**Core Objective:** To read the question and the retrieved database facts together, then write out a smooth, accurate answer.

**Key Operations:**
* **Prompt Setup:** Glues the database text and user question together into a clear template: `Context: [Paper Text] | Question: [User Query] | Answer:`.
* **Self-Attention Engine:** 
  1. **The Matrix Match:** The model multiplies the Query vector of a token against the Key vectors of all other tokens in the sentence (a scaled dot-product operation). This generates a raw relevance score for every pair.
  2. **The Softmax Weights:** These raw scores are turned into a clean percentage map. For instance, in the phrase "The algorithm failed because it was buggy," the token "it" will assign a 90% attention weight to "algorithm".
  3. **The Value Extraction:** The model multiplies that 90% weight against the Value vector of "algorithm". This floods the vector for the word "it" with the semantic traits and meaning of "algorithm".
* **Why is it called "Multi-Head"?** The model runs this (Q, K, V) calculation multiple times in parallel (usually 8 to 16 "heads"). One head focuses on grammar, another on pronoun tracking and another on academic concepts. Localized Feed-Forward Networks (FFN) then isolate and process each token's updated context.
* **Logits & Softmax:** The top layer generates raw popularity votes (Logits) to every word it knows. The Softmax function turns those scores into clean percentages that add up to 100%. Mathematically, it does two things to the logits:

  * **It eliminates negatives:** It raises Euler's number to the power of each logit. This forces every negative score to become a positive decimal, and it heavily amplifies the highest scores to widen the gap between the smart choices and the bad choices.

  * **It normalizes them:** It adds up all those new positive scores and divides each individual score by that total sum.

```text
Logits from Output Layer          Vocabulary Softmax
  ["dataset":  12.4]              ["dataset": 89.1%] (Highest probability)
  ["code":      8.1]   ───►       ["code":    10.8%]
  ["banana":   -4.5]              ["banana":   0.01%]
                                 ──────────────────
                                 Total Sum = 100%
 ```

  * **Zero Temperature (0.0):** Turns off all AI creativity. The model must strictly choose the word with the highest percentage, which completely stops the AI from making up fake facts (hallucinations).
  * **Word-by-Word Loop:** Writes out the answer left-to-right, predicting one word at a time and looking back at what it just wrote until the sentence is complete.

![LLM Inference Workflow](inference_workflow.jpeg)

### NLP WORKFLOW:

![Full NLP Offline and Online Workflow](full_workflow.jpeg)