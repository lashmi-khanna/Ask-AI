---
layout: default
title: Machine Learning 1
nav_order: 5
---

<script>
  window.MathJax = {
    tex: {
      inlineMath: [['$', '$'], ['\\(', '\\)']],
      displayMath: [['$$', '$$'], ['\\[', '\\]']]
    }
  };
</script>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

# Introduction to Machine Learning and Supervised Learning

Machine learning (ML) is the science of getting computers to learn without being explicitly programmed. For complex tasks like speech recognition or autonomous driving, writing explicit rules is impossible; instead, systems must learn from data and experience.

ML already powers everyday tools like web search, spam filters, and content recommendations, while transforming industries through medical diagnostics, automated manufacturing, and optimized power generation.

## Supervised Learning

The most widely used ML approach is supervised learning, which learns an "X to Y" (input-to-output) mapping. The algorithm is trained on datasets containing the "right answers" (labels) so it can accurately predict outputs for brand-new, unseen inputs. Supervised learning is divided into two main categories:

### 1. Regression

Regression algorithms predict a continuous number from an infinite set of possibilities. For example, a regression model can analyze historical real estate data to predict the specific dollar value of a house based on its square footage.

### 2. Classification

Classification algorithms predict a finite, discrete set of categories. For instance, a medical diagnostic tool might analyze patient age and tumor size to classify a tumor as either "benign" or "malignant." Unlike regression, the outputs are strictly limited to predefined categories.

---

## Unsupervised Learning

Unlike its supervised counterpart, unsupervised learning operates on data devoid of output labels ($y$). The system is provided only with input features ($x$) and must independently discover latent structures, underlying patterns, or correlations within the dataset without human guidance.

### 1. Clustering

Clustering is the primary unsupervised method, which organizes similar data points into groups automatically. Notable applications include:

* **News Aggregation:** Categorizing thousands of daily news stories into specific topics based on linguistic similarities.


* **Market Segmentation:** Partitioning a customer database into distinct demographic or behavioral segments to personalize services.



### 2. Other Unsupervised Techniques

* **Anomaly Detection:** Identifying rare events or statistical outliers, which is a vital component of financial fraud prevention.


* **Dimensionality Reduction:** Distilling complex, high-dimensional datasets into simpler representations while retaining core informational value.



---

## The Supervised Learning Process & Linear Regression

Supervised learning utilizes a training set comprised of features ($x$) and known targets ($y$). The algorithm constructs a mathematical model, or function ($f$), which takes new inputs ($x$) to produce an estimated output ($\hat{y}$).

### Linear Regression with One Variable

Commonly referred to as univariate linear regression, this fundamental model generates predictions by fitting a linear function to the data:

$$f(x)=wx+b$$

In this equation, $w$ and $b$ represent parameters, numerical weights that define the slope and intercept. The model aims to optimize these values for the best possible fit. To evaluate accuracy, we utilize a cost function, which quantifies the variance between the true target and the prediction.

### Standard Machine Learning Notation

Precision in notation is vital for technical communication in ML:

* $x$: The independent input variable or feature.


* $y$: The ground truth or target variable.


* $\hat{y}$: The value predicted by the model.


* $m$: The total count of training examples.


* $(x^{(i)}, y^{(i)})$: A single data entry, where $i$ denotes its index within the set.



---

## The Cost Function

To determine how well a linear regression model fits the training data, machine learning utilizes a cost function (denoted as $J$). The cost function measures the difference between the model's predictions ($\hat{y}$) and the actual true values ($y$).

In linear regression, the most commonly used cost function is the Squared Error Cost Function. Here is how it is calculated:

1. Find the error for a single data point by subtracting the true value from the predicted value.


2. Square that error.


3. Sum up the squared errors for all training examples in the dataset.


4. Divide by the total number of training examples ($m$) to get the average (by mathematical convention in ML, it is divided by $2m$ to make later calculus calculations cleaner).



$$J(w,b) = \frac{1}{2m} \sum_{i=1}^{m} \left( f_{w,b}(x^{(i)}) - y^{(i)} \right)^2$$

* $m$: The total number of training examples.


* $f_{w,b}(x^{(i)})$: The model's prediction for the $i$-th training example.


* $y^{(i)}$: The actual true target value for the $i$-th training example.


* $\frac{1}{2m}$: We divide by $m$ to find the average error, and divide by an extra $2$ by convention to make the calculus easier later on.



The goal of linear regression is to find the specific values for parameters $w$ (the slope) and $b$ (the y-intercept) that make the cost function $J$ as small as possible.

---

## Visualizing the Cost Function

Visualizing the cost function helps build an intuition for how machine learning models improve.

* **One Parameter ($w$ only):** If you simplify the model by temporarily removing $b$, the cost function $J(w)$ can be graphed as a simple U-shaped curve. The bottom of the "U" represents the lowest possible error (the optimal value for $w$).


* **Two Parameters ($w$ and $b$):** When both parameters are used, the cost function $J(w, b)$ becomes a 3D surface plot. For linear regression, this 3D plot always takes the shape of a convex, stretched-out "bowl" or "hammock."


* **Contour Plots:** To easily view this 3D bowl in 2D, data scientists use contour plots, concentric ovals or ellipses (similar to a topographical map). Any point on the same oval has the exact same cost. The very center of the smallest oval represents the absolute minimum cost, meaning it is the best possible straight-line fit for the data.



---

## Gradient Descent

Manually guessing values for $w$ and $b$ by looking at contour plots is highly inefficient, especially as models get more complex. Instead, machine learning relies on an automated algorithm called Gradient Descent to find the minimum cost. This algorithm is universally important and is used to train everything from basic linear regression to the most advanced Deep Learning neural networks.

**How it works (The Hill Analogy):**
Imagine you are standing at the top of a hilly landscape, and your goal is to reach the bottom of the lowest valley as efficiently as possible. Gradient descent works by:

1. Starting at a random point (an initial guess for $w$ and $b$).


2. Looking around 360 degrees to find the direction of steepest descent.


3. Taking a small step downhill in that exact direction.


4. Repeating this process over and over until you reach the bottom of the valley (the minimum cost).



In more complex models like neural networks, the cost function landscape might have multiple different valleys, known as local minima. In these cases, depending on where your initial random guess places you, gradient descent might guide you down into entirely different valleys. However, for linear regression, the squared error cost function always creates a single, uniform bowl shape, guaranteeing there is only one true minimum.

## Gradient Descent Implementation and Intuition

### The Mathematical Update Rule
The gradient descent algorithm iteratively updates its parameters until it reaches convergence—the point where the parameters stop changing because the cost function has reached its absolute minimum. The algorithm relies on the following mathematical update rules:

$$w = w - \alpha \frac{\partial}{\partial w} J(w,b)$$

$$b = b - \alpha \frac{\partial}{\partial b} J(w,b)$$

*   **$\alpha$ (Alpha):** This is the **learning rate**. It is a small positive number (such as 0.01) that determines the size of the "step" the algorithm takes downhill.
*   **$\frac{\partial}{\partial w} J(w,b)$ and $\frac{\partial}{\partial b} J(w,b)$:** These are the **partial derivatives** (or slopes) of the cost function at the current point. They act as a compass, dictating the *direction* of the step.

### The Simultaneous Update Requirement
When programming this algorithm in Python (or any language), $w$ and $b$ must be updated **simultaneously**. You must calculate the new values for both parameters using the *current* values, store them in temporary variables, and then reassign both variables at the exact same time. If you update $w$ first, and then immediately use that new $w$ to calculate the update for $b$, the mathematical execution will be incorrect.

### Understanding the Derivative and Learning Rate
The derivative naturally steers the parameter toward the minimum:
*   If the current weight is on the right side of the minimum (a positive slope), subtracting a positive derivative causes the parameter to decrease, moving it left toward the center.
*   If the current weight is on the left side of the minimum (a negative slope), subtracting a negative derivative causes the parameter to increase, moving it right toward the center.

The learning rate ($\alpha$) dictates how fast you move along that slope:
*   **Too small:** The steps are tiny, making the algorithm incredibly slow to converge.
*   **Too large:** The steps are too massive, causing the algorithm to overshoot the minimum. The cost might bounce back and forth or completely diverge (where the cost grows larger instead of shrinking). 

As the algorithm gets closer to the minimum, the slope naturally becomes flatter. Because the derivative gets closer to zero, the resulting step size automatically shrinks. Therefore, you do not need to manually decrease $\alpha$ over time; the algorithm naturally takes smaller baby steps as it zeroes in on the valley floor.

---

## Gradient Descent for Linear Regression

When the gradient descent algorithm is combined with the Squared Error Cost Function used in linear regression, the partial derivatives are mathematically derived as follows:

$$\frac{\partial}{\partial w} J(w,b) = \frac{1}{m} \sum_{i=1}^{m} \left( f_{w,b}(x^{(i)}) - y^{(i)} \right) x^{(i)}$$

$$\frac{\partial}{\partial b} J(w,b) = \frac{1}{m} \sum_{i=1}^{m} \left( f_{w,b}(x^{(i)}) - y^{(i)} \right)$$

### Batch Gradient Descent
This specific implementation is known as **Batch Gradient Descent**. The term "batch" means that at every single step of the descent, the algorithm looks at the entire batch of all $m$ training examples to compute the gradients. 

Because the squared error cost function always forms a perfectly convex, uniform "bowl" shape, linear regression has no competing local minima—it only has one global minimum. Provided the learning rate is chosen correctly, batch gradient descent is mathematically guaranteed to eventually converge to the best possible straight-line fit for the data.
## Multiple Linear Regression Model Representation

Multiple linear regression generalizes linear regression to accommodate multiple input features instead of just one.

### Notation Setup

* $n$: The total number of features (variables) available per training example.
* $x_j$: The $j$-th feature (where $j$ ranges from $1$ to $n$).
* $x^{(i)}$: A row vector containing all the feature values for the $i$-th training example.
* $x_j^{(i)}$: The value of feature $j$ in the $i$-th training example.

### Mathematical Formulation

Instead of a simple univariate setup, the model prediction $\hat{y}$ scales linearly with $n$ parameters:

$$f_{w,b}(x) = w_1x_1 + w_2x_2 + \dots + w_nx_n + b$$

---

## Vectorization

Vectorization is the process of grouping multiple features and parameters into vectors or matrices to replace explicit `for` loops with optimized linear algebra operations.

### Parameter and Feature Vectors

We define our weights and features as mathematical vectors:

$$\vec{w} = \begin{bmatrix} w_1 & w_2 & \dots & w_n \end{bmatrix}, \quad \vec{x} = \begin{bmatrix} x_1 & x_2 & \dots & x_n \end{bmatrix}$$

### Compact Vectorized Model Function

Using the vector dot product ($\vec{w} \cdot \vec{x}$ or transpose matrix multiplication $w^T x$), the hypothesis simplifies to:

$$f_{\vec{w},b}(\vec{x}) = \vec{w} \cdot \vec{x} + b$$

### Computational Benefits

* **Un-vectorized implementation (Slow):** Requires writing a loop (`for j in range(0, n): f += w[j] * x[j]`), which handles elements one-by-one sequentially in CPU threads.
* **Vectorized implementation (Fast):** Uses parallel computing hardware (like SIMD instructions or GPUs) to execute the vector dot product in a single step using optimized linear algebra backends like NumPy (`np.dot(w, x) + b`).

---

## Gradient Descent for Multiple Linear Regression

When scaling up parameters, the cost function $J(\vec{w},b)$ now calculates error across the vectorized weights:

$$J(\vec{w},b) = \frac{1}{2m} \sum_{i=1}^{m} \left( f_{\vec{w},b}(\vec{x}^{(i)}) - y^{(i)} \right)^2$$

### Vectorized Update Rules

Gradient descent minimizes the cost by simultaneously updating every weight parameter $w_j$ alongside the bias $b$:

$$\begin{aligned}
&\text{Repeat until convergence \{} \\
&\quad w_j = w_j - \alpha \frac{\partial}{\partial w_j} J(\vec{w},b) \quad \text{(simultaneously for all } j \text{ from } 1 \text{ to } n\text{)} \\
&\quad b = b - \alpha \frac{\partial}{\partial b} J(\vec{w},b) \\
&\text{\}}
\end{aligned}$$

### Explicit Gradient Equations

Evaluating the partial derivatives gives:

$$\frac{\partial}{\partial w_j} J(\vec{w},b) = \frac{1}{m} \sum_{i=1}^{m} \left( f_{\vec{w},b}(\vec{x}^{(i)}) - y^{(i)} \right) x_j^{(i)}$$

$$\frac{\partial}{\partial b} J(\vec{w},b) = \frac{1}{m} \sum_{i=1}^{m} \left( f_{\vec{w},b}(\vec{x}^{(i)}) - y^{(i)} \right)$$

### An Alternative to Gradient Descent: The Normal Equation

The Normal Equation solves for the parameters analytically in a single step without iterations.

* **Key Characteristics:** It is used exclusively for linear regression to compute parameters without loop dependencies.
* **Disadvantages:**
  * **Lack of Generalization:** It does not generalize to other machine learning algorithms (like logistic regression or neural networks).
  * **Scalability Issues:** It becomes exceptionally slow when the number of features is very large ($n > 10,000$) due to the computational cost of calculating a matrix inverse ($O(n^3)$).
* **What You Need to Know:** While the Normal Equation method may be utilized behind the scenes in certain machine learning libraries for small-scale linear regression, gradient descent remains the recommended method for finding parameters $\vec{w}$ and $b$ at scale.

---

## Feature Scaling

When different features have vastly different ranges of scale (e.g., house size spanning $300 \text{ to } 5000 \text{ sq ft}$ vs. number of bedrooms spanning $1 \text{ to } 5$), the cost function contours become extremely elongated, forming narrow ellipses.

### The Problem

Gradient descent will oscillate wildly back and forth across the narrow valley, taking a long time to find the minimum.

### The Solution

Rescale features so they take on a similar range of values (ideally between $-1$ and $+1$), making the cost contours spherical. This allows gradient descent to path directly to the global minimum without wasteful oscillations.

### Method 1: Min-Max Scaling (Normalization)

Rescales the values to fall strictly within the range $[0, 1]$:

$$x_j = \frac{x_j - x_{\min}}{x_{\max} - x_{\min}}$$

### Method 2: Mean Normalization

Centers the feature around zero with a range roughly spanning $[-1, 1]$:

$$x_j = \frac{x_j - \mu_j}{x_{\max} - x_{\min}}$$

Where $\mu_j$ is the average/mean value of feature $j$ in the training set.

### Method 3: Z-score Standardization

Transforms features to have a mean ($\mu$) of $0$ and a standard deviation ($\sigma$) of $1$:

$$x_j = \frac{x_j - \mu_j}{\sigma_j}$$

Where the standard deviation is calculated as:

$$\sigma_j = \sqrt{\frac{1}{m}\sum_{i=1}^{m}(x_j^{(i)} - \mu_j)^2}$$

---

## Checking Gradient Descent for Convergence

To guarantee that gradient descent is working correctly, you must monitor the cost $J(\vec{w},b)$ over the course of iterations.

### Learning Curves

A learning curve plots the number of iterations on the x-axis and the cost value $J(\vec{w},b)$ on the y-axis.

* If gradient descent is functioning properly, $J(\vec{w},b)$ must strictly decrease after every single iteration.
* If the curve goes up or fluctuates up and down, your learning rate $\alpha$ is too large or there is a bug in your vectorized implementation code.

### Automatic Convergence Test

Instead of looking at a graph manually, an algorithm can use an automated epsilon ($\epsilon$) threshold check:

* Let $\epsilon = 10^{-3}$ (or another tiny value chosen based on application needs).
* If $J(\vec{w},b)$ decreases by less than or equal to $\epsilon$ in a single iteration, declare convergence and stop running the training loop.

---

## Choosing the Learning Rate $\alpha$

Finding the optimal step size requires a methodical trial-and-error approach based on analyzing the learning curve:

* **Debugging a rising cost:** If $J(\vec{w},b)$ increases, use a smaller $\alpha$.
* **Testing Strategy:** To find the functional boundaries, test learning rates exponentially spaced apart (e.g., $0.001, 0.003, 0.01, 0.03, 0.1, 0.3, 1.0$).
* **Selection:** Identify the highest value that causes a consistent decrease, and then pick a value slightly smaller or safer than that limit.

---

## Feature Engineering & Polynomial Regression

Linear models cannot fit non-linear patterns. Feature engineering allows us to transform data to fit non-linear curves using the exact same linear regression framework.

### Feature Engineering Definition

The process of combining or transforming existing features to create entirely new ones that capture better insights (e.g., given features $x_1 = \text{width}$ and $x_2 = \text{depth}$, creating an engineered feature $x_3 = \text{width} \times \text{depth}$ to represent area).

### Polynomial Regression Models

If data displays non-linear trends (like a curve or acceleration), you can introduce powers of your features:

* **Quadratic Model:** $f_{\vec{w},b}(x) = w_1x + w_2x^2 + b$
* **Cubic Model:** $f_{\vec{w},b}(x) = w_1x + w_2x^2 + w_3x^3 + b$

### Critical Multi-Feature Conversion

When applying polynomial terms, the exponents act as individual features in multiple linear regression:

$$x_1 = x, \quad x_2 = x^2, \quad x_3 = x^3$$

> **Crucial Note:** Because squaring or cubing numbers dramatically magnifies their scales (e.g., if $x=100$, then $x^3=1,000,000$), applying **Feature Scaling** is absolutely mandatory when executing polynomial regression.

---

## Classification vs. Regression

While regression predicts a continuous numeric value, classification predicts a categorical output value where the target variable $y$ can only take on a small set of discrete values.

### Binary Classification

The simplest form of classification where the output variable $y$ can take on only two values: $0$ or $1$.

* $0$: Negative Class (e.g., "Not Spam", "Benign Tumor")
* $1$: Positive Class (e.g., "Spam", "Malignant Tumor")

### Why Linear Regression Fails for Classification

* Introducing an extreme outlier data point far to the right shifts the linear decision boundary line significantly, destroying accuracy for previously well-classified points.
* Linear regression can yield predictions way above $1$ or well below $0$, which makes no sense for classification probabilities.

---

## The Logistic Regression Model

Logistic regression addresses this by passing the linear model output through a function that restricts the final output values strictly between $0$ and $1$.

### The Sigmoid (Logistic) Function

The mathematical function used to constrain output is the Sigmoid function, denoted as $g(z)$:

$$g(z) = \frac{1}{1 + e^{-z}}$$

* If $z$ is a very large positive number, $g(z) \approx 1$.
* If $z$ is a very large negative number, $g(z) \approx 0$.
* If $z = 0$, $g(z) = 0.5$.

### Composite Hypothesis Formulation

To build the logistic regression model, we pass our linear equation vector ($\vec{w} \cdot \vec{x} + b$) as the input parameter $z$ into the sigmoid function:

$$f_{\vec{w},b}(\vec{x}) = g(\vec{w} \cdot \vec{x} + b) = \frac{1}{1 + e^{-(\vec{w} \cdot \vec{x} + b)}}$$

### Interpreting the Output

The output $f_{\vec{w},b}(\vec{x})$ is interpreted as the probability that the target variable $y$ is equal to $1$, given the input features $\vec{x}$:

$$f_{\vec{w},b}(\vec{x}) = P(y=1 \mid \vec{x}; \vec{w},b)$$

> **Example:** If $f_{\vec{w},b}(\vec{x}) = 0.70$, it means there is a 70% chance the example is positive ($y=1$), and conversely, a 30% chance it is negative ($y=0$).

---

## Decision Boundary

The decision boundary is the line (or surface) that separates the regions where the model predicts $\hat{y}=1$ from where it predicts $\hat{y}=0$.

### Defining the Threshold

A standard threshold choice is predicting $\hat{y}=1$ when the probability is greater than or equal to $0.5$:

$$\begin{aligned}
f_{\vec{w},b}(\vec{x}) \ge 0.5 &\implies \text{Predict } \hat{y}=1 \\
f_{\vec{w},b}(\vec{x}) < 0.5 &\implies \text{Predict } \hat{y}=0
\end{aligned}$$

Looking back at the Sigmoid function math:
* $g(z) \ge 0.5$ happens precisely when $z \ge 0$.

Therefore, the model predicts $\hat{y}=1$ whenever:

$$\vec{w} \cdot \vec{x} + b \ge 0$$

### Linear Decision Boundary

For a model with two features ($x_1, x_2$), setting the parameter equation to exactly zero forms a line:

$$w_1x_1 + w_2x_2 + b = 0$$

This literal line drawn across your feature space acts as the boundary split.

### Non-Linear Decision Boundary

Just like polynomial regression, we can add higher-order terms to create non-linear shapes. For example, the parameter equation:

$$w_1x_1^2 + w_2x_2^2 + b = 0$$

creates a circular decision boundary where everything outside the circle is classified as $1$ and everything inside is classified as $0$.

---

## Cost Function for Logistic Regression

We cannot reuse the Squared Error cost function from linear regression here. If we plug the non-linear Sigmoid function into a squared error equation, the resulting cost function surface becomes "non-convex" (filled with many local minima), meaning gradient descent will get stuck and fail to reach the global minimum.

### The Convex Loss Function

Instead, we compute the loss for a single training example using logarithms to ensure the overall cost space remains perfectly convex (bowl-shaped):

$$L\left(f_{\vec{w},b}(\vec{x}^{(i)}), y^{(i)}\right) = \begin{cases} -\log\left(f_{\vec{w},b}(\vec{x}^{(i)})\right) & \text{if } y^{(i)} = 1 \\ -\log\left(1 - f_{\vec{w},b}(\vec{x}^{(i)})\right) & \text{if } y^{(i)} = 0 \end{cases}$$

### Loss Intuition

* **If $y^{(i)} = 1$:** As the model prediction approaches $1$, the loss approaches $0$. If the model predicts $0$, the loss shoots up to infinity ($L \to \infty$), heavily penalizing the wrong prediction.
* **If $y^{(i)} = 0$:** As the model prediction approaches $0$, the loss matches $0$. If the model predicts $1$, the loss shoots up to infinity.

---

## Simplified Cost Function for Logistic Regression

Instead of writing a conditional piecewise equation for the loss function based on whether $y=1$ or $y=0$, we can combine both cases into a single, elegant algebraic expression.

### The Unified Loss Equation

For a single training example, the unified loss is written as:

$$L\left(f_{\vec{w},b}(\vec{x}^{(i)}), y^{(i)}\right) = -y^{(i)} \log\left(f_{\vec{w},b}(\vec{x}^{(i)})\right) - (1 - y^{(i)}) \log\left(1 - f_{\vec{w},b}(\vec{x}^{(i)})\right)$$

* **Case $y^{(i)} = 1$:** The second term multiplying by $(1 - 1)$ completely drops out, leaving only $-\log(f(x))$.
* **Case $y^{(i)} = 0$:** The first term multiplying by $(0)$ completely drops out, leaving only $-\log(1 - f(x))$.

### The Total Cost Function $J(\vec{w},b)$

By taking the average of this unified loss over all $m$ training examples, we get the complete convex cost function for logistic regression:

$$J(\vec{w},b) = -\frac{1}{m} \sum_{i=1}^{m} \left[ y^{(i)} \log\left(f_{\vec{w},b}(\vec{x}^{(i)})\right) + (1 - y^{(i)}) \log\left(1 - f_{\vec{w},b}(\vec{x}^{(i)})\right) \right]$$

---

## Gradient Descent for Logistic Regression

To minimize the cost function $J(\vec{w},b)$, we apply the exact same iterative gradient descent update format.

### The Update Rules

$$\begin{aligned}
&\text{Repeat until convergence \{} \\
&\quad w_j = w_j - \alpha \frac{1}{m} \sum_{i=1}^{m} \left( f_{\vec{w},b}(\vec{x}^{(i)}) - y^{(i)} \right) x_j^{(i)} \\
&\quad b = b - \alpha \frac{1}{m} \sum_{i=1}^{m} \left( f_{\vec{w},b}(\vec{x}^{(i)}) - y^{(i)} \right) \\
&\text{\}}
\end{aligned}$$

> **Crucial Conceptual Note:** While this derivative looks mathematically identical to the linear regression gradient descent rule, it is not the same model. The definition of the hypothesis function $f_{\vec{w},b}(\vec{x})$ here is the non-linear Sigmoid function $g(\vec{w} \cdot \vec{x} + b)$, whereas in linear regression it is simply $(\vec{w} \cdot \vec{x} + b)$.

---

## The Problem of Overfitting

When training a model, it can fall into one of three structural categories depending on its generalization capabilities:

* **Underfitting (High Bias):** The model is too simple to capture the underlying pattern of the data (e.g., trying to fit a straight line to completely circular data). It performs poorly on both training and new data.
* **Generalization (Just Right):** The model fits the training patterns cleanly and generalizes effectively to accurately predict completely unseen test examples.
* **Overfitting (High Variance):** The model is overly complex (e.g., has too many polynomial features) and fits the training data perfectly—including its random noise. The training error is close to $0$, but it fails drastically when attempting to make predictions on new data points.

### Solutions to Overfitting

1. Collect more training data.
2. Select or eliminate features (Feature Selection).
3. **Regularization:** Keep all features, but reduce the magnitude/values of the parameters $\vec{w}$.

---

## Regularization Cost Function

Regularization works by adding a penalty term to the cost function to discourage the weights $\vec{w}$ from becoming too large.

### Intuition

If you have a model with a massive polynomial term like $1000x^4$, adding a heavy penalty that forces $w_4 \to 0$ effectively cancels out the wavy complexity of that polynomial term, smoothing out the decision curve.

### L2 Regularization Formulation

We modify our standard cost function by appending an extra regularization penalty term at the end:

$$\text{Penalty} = \frac{\lambda}{2m} \sum_{j=1}^{n} w_j^2$$

When applied directly to Linear Regression, the complete cost formula becomes:

$$J(\vec{w},b) = \frac{1}{2m} \sum_{i=1}^{m} \left( f_{\vec{w},b}(\vec{x}^{(i)}) - y^{(i)} \right)^2 + \frac{\lambda}{2m} \sum_{j=1}^{n} w_j^2$$

### Key Parameters

* **$\lambda$ (Lambda):** The Regularization Parameter. It controls the trade-off between trying to fit the training data well versus keeping the parameter weights small.
* **Why the bias $b$ is omitted:** Conventionally, we only penalize the weights $\vec{w}$ and leave the bias scalar $b$ unregularized, as adjusting $b$ merely shifts the function vertically and doesn't introduce complex curve fitting.

### The Effect of Choosing $\lambda$

* **If $\lambda = 0$:** The model acts with no regularization (highly prone to overfitting).
* **If $\lambda$ is set extremely large (e.g., $\lambda = 10^{10}$):** The optimization forces all weights $w_j \approx 0$. The model drops down to a flat horizontal line ($f(x) \approx b$), causing extreme underfitting.