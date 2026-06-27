---
layout: default
title: Transformer
nav_order: 3
---

# Transformer

The Transformer is a model architecture where all the "learning" is stored in matrices. It processes all words simultaneously, rather than one by one, using functions composed together to refine the representation of text and output a probability distribution over all words in the vocabulary.

The pipeline runs across 7 sequential mathematical stages:


![Global information pipeline](globalinfo.png)

<br>

## Stage 1: Tokenization
**Core Objective:** Before doing any math, we have to slice raw text into small, manageable pieces that our computer can look up in a dictionary index.


**Key Operations:**

* **Vocabulary Matrix (V):** A fixed list of the most common word fragments found in a large body of text. Think of this as the model's master dictionary.
* **Token Isolation:** Each item in this vocabulary list is called a token. The total vocabulary size is typically 30,000–100,000 tokens. If a word isn't in here, the model can't read it.


<br>

## Stage 2: Word Embedding 
**Core Objective:** Computers don't understand letters, so we have to convert tokens into lists of numbers (vectors) where the numbers actually encode human meaning.


**Key Operations:**
* **One-Hot Vector Matrix:** The first attempt at representing a token gives it a unique position in a massive list filled with zeros and a single 1 at its index slot. For example, a 5-word vocabulary looks like this: `cat → [1, 0, 0, 0, 0]`, `dog → [0, 1, 0, 0, 0]`. These vectors are huge and completely useless for finding relationships because every single word sits at a perfect 90-degree angle from the others, meaning their similarity score is always 0.
* **Embedding Matrix Transformation (WE):** To fix this, we multiply the massive one-hot vector by a trainable matrix WE. This compresses the giant list (e.g., 50,000 dimensions) down into a compact, dense vector of numbers (typically 300 dimensions):

        word_vector = one_hot_vector × WE

* **Skip-gram Contextual Alignment:** The values inside this matrix are learned by forcing a target word vector (w_t) to predict its real-world neighboring context words (w_j) using a probability ratio:

        P(w_j | w_t) = e^(w_j · w_t) / Σ e^(w_l · w_t)

* **In Simple Words:** The dot product calculates a similarity score between vectors. The exponential forces everything to be positive, and the big sum on the bottom divides the score by all other words to turn it into a clean percentage. Because words like "king" and "man" frequently share the same neighbors, the math forces their vectors to point in a similar spatial direction.


<br>

## Stage 3: Positional Encoding
**Core Objective:** Because Transformers process an entire sentence at the exact same time instead of word-by-word, the network natively has no idea which word comes first, middle, or last. We use wave frequencies to inject word order.

**Key Operations:**
* **Sinusoidal Position Functions:** For each word position i in a sentence and each dimension channel j inside its vector, we add unique, static wave coordinates:

      P[i, 2j] = sin( i / 10000^(2j/d) ) | P[i, 2j+1] = cos( i / 10000^(2j/d) ) 

* **In Simple Words:** Even dimensions use a sine wave and odd dimensions use a cosine wave. Because waves oscillate smoothly between -1 and +1, every single slot in a sentence gets stamped with a unique mathematical ripple pattern that never blows up too large.
* **Linear Relative Rotations:** This wave approach has a clever property: the position vector at position k+l (an offset of l steps away from position k) can always be calculated by multiplying the position vector at k by a stable rotation matrix T(l)

                   p(k+l) = p(k) \times T(l)


* **In Simple Words:** The rotation matrix T(l) cares only about how far apart two words are (l), not where they are in the sentence. This allows the model to instantly recognize that two words are 3 slots apart without needing to worry about the absolute sentence length. Adding these coordinates to our embeddings yields our starting text matrix U(s).

<br>

## Stage 4: Attention Mechanism
**Core Objective:** A standalone word vector only has one fixed definition (e.g., "pool" can mean a swimming pool or a billiard game). Attention lets words "talk" to each other to update their meanings based on context.


**Key Operations:**
* **Functional Matrix Projections:** We take our initial word vector u and split it into three functional roles by multiplying it across three learned matrices (W^Q, W^K, W^V):
* **Query (q = u \times W^Q):** What questions is this word actively asking about the sentence?
* **Key (k = u \times W^K):** What clues or information does this word offer to other words?
* **Value (v = u \times W^V):** What is the actual structural text meaning this word carries?


* **Attention Score:** To find out how relevant word B is to word A, we calculate the dot product of A's query vector with B's key vector:

            AttentionScore(A, B) = q_A · k_B


* **Attention Pattern Matrix:**  We map these scores across all pairs of words into an N*N grid, then use a row-by-row softmax to turn the raw scores into clean percentages that add up to 1. This tells us exactly what percentage of focus word A should place on word B.
* **Updating Representations:**  We multiply those attention weight percentages (a[i,j]) by the value vectors (v) to compute a context-blended average vector for each word slot, creating our attention head matrix H(s):

      new_representation(word_i) = Σ a[i,j] × value(word_j) 

* **Residual Bypass Connection:**  We then take this fresh context matrix H(s) and add it straight back to our original input matrix U(s):

      Y(s) = U(s) + H(s)


* **In Simple Words:** This acts as an architectural shortcut. By adding the new context back to the original values, gradients can roll backwards smoothly through deep layers during training without shrinking to zero.

![Attention Routing Loop](attention.png)

## Stage 5: Position-Wise Neural Network

**Core Objective:** After attention updates word meanings using surrounding context, each word vector is sent through an identical small neural network to independently sharpen its individual features.


**Key Operations:**
* **Two-Layer Linear Expansion:** The vector is multiplied by matrix W_1 and given a bias shift b_1 to project it into a massive hidden dimension (d_F), then multiplied by matrix W_2 and bias b_2 to squeeze it back into its normal dimension (d):

      Output Layer 1 = x × W_1 + b_1 | Output Layer 2 = x × W_2 + b_2 

* **ReLU Activation Function:** Right in the middle of these layers, we pass values through the Rectified Linear Unit function:

        ReLU(x) = max(0, x) 


* **In Simple Words:** If an incoming number is negative, ReLU instantly clips it to 0. If it's positive, it passes through unchanged.
* **The Non-Linear Necessity:** Without this simple clipping step, stacking multiple neural layers would be mathematically pointless. Multiplying matrices over and over just collapses back into a single flat linear equation. This zero-clip creates a geometric bend, allowing the model to learn complex curved patterns and text logic rules. This step yields our enhanced feature matrix Z(s).


<br>

## Stage 6: Normalisation

**Core Objective:** As data moves deeper through multiple matrix multiplications, the values can easily drift out of control. Normalization forces everything back to a stable baseline.


**Key Operations:**
* **Variance Centering:** We take each vector row z and scale its internal values using its calculated average component mean and its standard deviation spread:

      z_normalized = (z - mean) / std_deviation 


* **In Simple Words:** This calculation forces the numbers in the vector to have a clean average mean of 0 and a steady standard deviation variance of 1. This acts as a safety shield, stopping numbers from inflating uncontrollably or shrinking to absolute zero as they travel through deep stacked layers.

<br>

## Stage 7: Output Distribution

**Core Objective:** Turns the deeply processed, context-rich vector representations back into human words by calculating a ranked guess of what word belongs next.


**Key Operations:**
* **Terminal Row Isolation:** The model isolates the enhanced, normalized vector sitting at the very last row of our sequence matrix(z_last) . Because of function composition, this final vector row successfully holds context from all previous words.
Softmax Vocabulary Projection: We multiply(z_last) against our vocabulary weights to generate raw score logits, then pass them into our final softmax exponent ratio:

       P(α_j | text) = e^(w_j · z_last) / Σ e^(w_l · z_last) 


* **In Simple Words:** The exponent e^(...) ensures all output numbers are strictly positive decimals between 0 and 1, and the bottom sum normalizes them so the entire vocabulary distribution adds up to exactly 1.0. It heavily accentuates the highest scores, widening the gap between smart, context-appropriate words and bad guesses. The word with the highest percentage wins.

<br>


### Neural Network:

![Feed forward neural network](neuralnetwork.png)