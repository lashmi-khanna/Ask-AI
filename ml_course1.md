---
layout: default
title: Machine Learning 1
nav_order: 5
---
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

Unlike its supervised counterpart, unsupervised learning operates on data devoid of output labels. The system is provided only with input features and must independently discover latent structures, underlying patterns, or correlations within the dataset without human guidance.

### 1. Clustering

Clustering is the primary unsupervised method, which organizes similar data points into groups automatically. Notable applications include:

* **News Aggregation:** Categorizing thousands of daily news stories into specific topics based on linguistic similarities.

* **Market Segmentation:** Partitioning a customer database into distinct demographic or behavioral segments to personalize services.

### 2. Other Unsupervised Techniques

* **Anomaly Detection:** Identifying rare events or statistical outliers, which is a vital component of financial fraud prevention.

* **Dimensionality Reduction:** Distilling complex, high-dimensional datasets into simpler representations while retaining core informational value.

---

## The Supervised Learning Process & Linear Regression

Supervised learning utilizes a training set comprised of features (x) and known targets (y). The algorithm constructs a mathematical model, or function (f), which takes new inputs (x) to produce an estimated output (y-hat).

### Linear Regression with One Variable

Commonly referred to as univariate linear regression, this fundamental model generates predictions by fitting a linear function to the data:

f(x) = w * x + b

In this equation, w and b represent parameters, numerical weights that define the slope and intercept. The model aims to optimize these values for the best possible fit. To evaluate accuracy, we utilize a cost function, which quantifies the variance between the true target and the prediction.

### Standard Machine Learning Notation

Precision in notation is vital for technical communication in ML:

* x: The independent input variable or feature.

* y: The ground truth or target variable.

* y-hat: The value predicted by the model.

* m: The total count of training examples.

* (x_i, y_i): A single data entry, where i denotes its index within the set.

---

## The Cost Function

To determine how well a linear regression model fits the training data, machine learning utilizes a cost function (denoted as J). The cost function measures the difference between the model's predictions (y-hat) and the actual true values (y).

In linear regression, the most commonly used cost function is the Squared Error Cost Function. Here is how it is calculated:

1. Find the error for a single data point by subtracting the true value from the predicted value.
2. Square that error.
3. Sum up the squared errors for all training examples in the dataset.
4. Divide by the total number of training examples (m) to get the average (by mathematical convention in ML, it is divided by 2m to make later calculus calculations cleaner).

J(w,b) = (1 / 2m) * Σ (f_w,b(x_i) - y_i)^2

* m: The total number of training examples.
* f_w,b(x_i): The model's prediction for the i-th training example.
* y_i: The actual true target value for the i-th training example.

The goal of linear regression is to find the specific values for parameters w (the slope) and b (the y-intercept) that make the cost function J as small as possible.

---

## Gradient Descent

Manually guessing values for w and b by looking at contour plots is highly inefficient. Instead, machine learning relies on an automated algorithm called Gradient Descent to find the minimum cost.

**How it works (The Hill Analogy):**
1. Starting at a random point (an initial guess for w and b).
2. Looking around 360 degrees to find the direction of steepest descent.
3. Taking a small step downhill in that exact direction.
4. Repeating this process over and over until you reach the bottom of the valley (the minimum cost).

## Gradient Descent Implementation

### The Mathematical Update Rule

w = w - α * (∂/∂w) J(w,b)

b = b - α * (∂/∂b) J(w,b)

* α (Alpha): This is the learning rate. It is a small positive number (such as 0.01) that determines the size of the "step" the algorithm takes downhill.

* (∂/∂w) J(w,b) and (∂/∂b) J(w,b): These are the partial derivatives (or slopes) of the cost function at the current point. They act as a compass, dictating the direction of the step.

### Batch Gradient Descent
This implementation looks at the entire batch of all m training examples to compute the gradients at every step.
## Multiple Linear Regression Model Representation

Multiple linear regression generalizes linear regression to accommodate multiple input features instead of just one ($x$).

### Notation Setup
* $n$: The total number of features (variables) available per training example.
* $x_{j}$: The $j$-th feature (where $j$ ranges from $1$ to $n$).
* $x^{(i)}$: A row vector containing all the feature values for the $i$-th training example.
* $x_{j}^{(i)}$: The value of feature $j$ in the $i$-th training example.

### Mathematical Formulation
Instead of a simple $wx+b$ setup, the model prediction $f_{w,b}(x)$ scales linearly with $n$ parameters:

$$f_{w,b}(x)=w_{1}x_{1}+w_{2}x_{2}+\dots+w_{n}x_{n}+b$$

---

## Vectorization

Vectorization is the process of grouping multiple features and parameters into vectors or matrices to replace explicit for loops with optimized linear algebra operations.

### Parameter and Feature Vectors
We define our weights and features as mathematical vectors:

$$w=\begin{bmatrix}w_{1}\\ w_{2}\\ \vdots\\ w_{n}\end{bmatrix}, x=\begin{bmatrix}x_{1}\\ x_{2}\\ \vdots\\ x_{n}\end{bmatrix}$$

### Compact Vectorized Model Function
Using the matrix dot product ($w \cdot x$ or transpose matrix multiplication $W^{T}x$), the hypothesis simplifies to:

$$f_{w,b}(x)=w\cdot x+b$$

### Computational Benefits
* **Un-vectorized implementation (Slow):** Requires writing a loop (`for j in range(0, n): F += w[j] * x[j]`), which handles elements one-by-one.
* **Vectorized implementation (Fast):** Uses parallel computing hardware (like SIMD or GPUs) to execute the vector dot product in a single step using optimized backends like NumPy (`np.dot(w,x)+b`).

---

## Gradient Descent for Multiple Linear Regression

When scaling up parameters, the cost function $J(w,b)$ now calculates error across the vectorized weights:

$$J(w,b)=\frac{1}{2m}\sum_{i=1}^{m}(f_{w,b}(x^{(i)})-y^{(i)})^{2}$$

### Vectorized Update Rules
Gradient descent minimizes the cost by simultaneously updating every weight parameter $w_{j}$ alongside the bias $b$:

Repeat until convergence:
$$w_{j}=w_{j}-\alpha\frac{\partial}{\partial w_{j}}J(w,b)$$
(simultaneously for all $j=1 \dots n$)

$$b=b-\alpha\frac{\partial}{\partial b}J(w,b)$$

### Explicit Gradient Equations
Evaluating the partial derivatives gives:

$$w_{j}=w_{j}-\alpha\left[\frac{1}{m}\sum_{i=1}^{m}(f_{w,b}(x^{(i)})-y^{(i)})\cdot x_{j}^{(i)}\right]$$
$$b=b-\alpha\left[\frac{1}{m}\sum_{i=1}^{m}(f_{w,b}(x^{(i)})-y^{(i)})\right]$$

---

## An Alternative to Gradient Descent: The Normal Equation

### Key Characteristics:
* It is used only for linear regression.
* It solves for the parameters $w$ and $b$ analytically without iterations (no loops required).

### Disadvantages:
* **Lack of Generalization:** It does not generalize to other machine learning algorithms.
* **Scalability Issues:** It becomes exceptionally slow when the number of features is very large ($n>10,000$).

### What You Need to Know:
* The Normal Equation method may be utilized behind the scenes in certain machine learning libraries that implement linear regression.
* Despite this alternative, gradient descent remains the recommended method for finding parameters $w$ and $b$.

---

## Feature Scaling

When different features have vastly different ranges of scale (e.g., $x_{1} =$ size in sq ft $[300,5000]$ and $x_{2} =$ number of bedrooms $[1,5]$), the cost function contours become extremely elongated (narrow ellipses).

### The Problem
Gradient descent will oscillate wildly back and forth, taking a long time to find the minimum.

### The Solution
Rescale features so they take on a similar range of values (ideally between $-1$ and $+1$), making the cost contours spherical and allowing gradient descent to path directly to the global minimum.

### Method 1: Min-Max Scaling (Normalization)
Rescales the values to fall strictly within the range $[0,1]$:

$$x_{j}=\frac{x_{j}-\min(x_{j})}{\max(x_{j})-\min(x_{j})}$$

### Method 2: Mean Normalization
Centers the feature around zero with a range roughly spanning $[-0.5,0.5]$:

$$x_{j}=\frac{x_{j}-\mu_{j}}{\max(x_{j})-\min(x_{j})}$$

Where $\mu_{j}$ is the average/mean value of feature $j$ in the training set.

### Method 3: Z-score Standardization
Transforms features to have a mean ($\mu$) of $0$ and a standard deviation ($\sigma$) of $1$:

$$x_{j}=\frac{x_{j}-\mu_{j}}{\sigma_{j}}$$

Where the standard deviation is calculated as:

$$\sigma_{j}=\sqrt{\frac{1}{m}\sum_{i=1}^{m}(x_{j}^{(i)}-\mu_{j})^{2}}$$

---

## Checking Gradient Descent for Convergence

To guarantee that gradient descent is working correctly, you must monitor the cost $J(w,b)$ over the course of iterations.

### Learning Curves
Plotting a graph where the x-axis is the **Number of Iterations** and the y-axis is $J(w,b)$.
* If gradient descent is functioning properly, $J(w,b)$ must strictly decrease after every single iteration.
* If the curve goes up or fluctuates up and down, your learning rate $\alpha$ is too large or there is a bug in the code.

### Automatic Convergence Test
Instead of looking at a graph, an algorithm can use an automated epsilon ($\epsilon$) threshold check:
* Let $\epsilon=10^{-3}$ (or another tiny value).
* If $J(w,b)$ decreases by less than or equal to $\epsilon$ in a single iteration, declare convergence and stop running the loop.

---

## Choosing the Learning Rate $\alpha$

Finding the optimal step size requires a methodical trial-and-error approach based on analyzing the learning curve.

* **Debugging a rising cost:** If $J(w,b)$ increases, use a smaller $\alpha$.
* **Testing Strategy:** To find the boundaries, test learning rates exponentially spaced apart (e.g., $0.001, 0.003, 0.01, 0.03, 0.1, 0.3, 1.0$).
* **Selection:** Identify the highest value that causes a consistent decrease, and then pick a value slightly smaller or safer than that limit.

---

## Feature Engineering & Polynomial Regression

Linear models cannot fit non-linear patterns. Feature engineering allows us to transform data to fit non-linear curves using the exact same linear regression framework.

### Feature Engineering Definition
The process of combining or transforming existing features to create entirely new ones that capture better insights (e.g., given width and depth, creating an engineered feature $\text{area} = \text{width} \times \text{depth}$).

### Polynomial Regression Models
If data displays non-linear trends (like a curve or acceleration), you can introduce powers of your features:

* **Quadratic Model:** $f_{w,b}(x)=w_{1}x+w_{2}x^{2}+b$
* **Cubic Model:** $f_{w,b}(x)=w_{1}x+w_{2}x^{2}+w_{3}x^{3}+b$

### Critical Multi-Feature Conversion
When applying polynomial terms, the exponents act as individual features in multiple linear regression:

$$x_{1}=x, x_{2}=x^{2}, x_{3}=x^{3}$$

> **Crucial Note:** Because squaring or cubing numbers dramatically magnifies their scales (e.g., if $x=10$, then $x^{3}=1000$), applying Feature Scaling is absolutely mandatory when executing polynomial regression.

---

## Classification vs. Regression

While regression predicts a continuous numeric value, classification predicts a categorical output value where the target variable $y$ can only take on a small set of discrete values.

### Binary Classification
The simplest form of classification where the output variable $y$ can take on only two values: $0$ or $1$.
* $y=0$: Negative Class (e.g., "Not Spam", "Benign Tumor")
* $y=1$: Positive Class (e.g., "Spam", "Malignant Tumor")

### Why Linear Regression Fails for Classification
If you attempt to map a linear regression line ($f(x)=wx+b$) to fit classification data:
* Introducing an extreme outlier data point far to the right shifts the decision boundary line, destroying accuracy.
* Linear regression can yield predictions way above $1$ or well below $0$, which makes no physical sense for classification probabilities.

---

## The Logistic Regression Model

Logistic regression addresses this by passing the linear model output through a function that restricts the final output values strictly between $0$ and $1$.

### The Sigmoid (Logistic) Function
The mathematical function used to constrain output is the Sigmoid function, denoted as $g(z)$:

$$g(z)=\frac{1}{1+e^{-z}}$$

* If $z$ is a very large positive number, $e^{-z}\rightarrow0\Rightarrow g(z)\rightarrow1$.
* If $z$ is a very large negative number, $e^{-z}\rightarrow\infty\Rightarrow g(z)\rightarrow0$.
* If $z=0$, $e^{0} = 1 \Rightarrow g(z) = 0.5$.

### Composite Hypothesis Formulation
To build the logistic regression model, we pass our linear equation vector ($w\cdot x+b$) as the input parameter $z$ into the sigmoid function:

$$f_{w,b}(x)=g(w\cdot x+b)=\frac{1}{1+e^{-(w\cdot x+b)}}$$

### Interpreting the Output
The output $f_{w,b}(x)$ is interpreted as the probability that the target variable $y$ is equal to $1$, given the input features $x$:

$$f_{w,b}(x)=P(y=1|x;w,b)$$

* **Example:** If $f_{w,b}(x)=0.7$, it means there is a $70\%$ chance the example is positive ($y=1$), and conversely, a $30\%$ chance it is negative ($P(y=0)=1-0.7=0.3$).

---

## Decision Boundary

The decision boundary is the line (or surface) that separates the regions where the model predicts $y=1$ from where it predicts $y=0$.

### Defining the Threshold
A standard threshold choice is predicting $\hat{y}=1$ when the probability is $\ge0.5$:
* $\hat{y}=1$ if $f_{w,b}(x)\ge0.5$
* $\hat{y}=0$ if $f_{w,b}(x)<0.5$

Looking back at the Sigmoid function math, $g(z)\ge0.5$ happens precisely when $z\ge0$. Therefore, the model predicts $\hat{y}=1$ whenever:

$$w\cdot x+b\ge0$$

### Linear Decision Boundary
For a model with two features $(x_{1},x_{2})$, setting the parameter equation to exactly zero forms a line:

$$w_{1}x_{1}+w_{2}x_{2}+b=0$$

This literal line drawn across your feature space acts as the boundary split.

### Non-Linear Decision Boundary
Just like polynomial regression, we can add higher-order terms $(x_{1}^{2},x_{2}^{2})$ to create non-linear shapes. For example, the parameter equation:

$$x_{1}^{2}+x_{2}^{2}-1=0$$

creates a circular decision boundary where everything outside the circle is classified as $1$ and everything inside is classified as $0$.

---

## Cost Function for Logistic Regression

We cannot reuse the Squared Error cost function from linear regression here. If we plug the non-linear Sigmoid function into a squared error equation, the resulting cost function surface becomes "non-convex" (filled with many local minima), meaning gradient descent will get stuck and fail to optimize.

### The Convex Loss Function
Instead, we compute the loss for a single training example using logarithms to ensure the overall cost space remains perfectly convex (bowl-shaped):

$$\text{Loss}(f_{w,b}(x^{(i)}),y^{(i)})=\begin{cases}-\log(f_{w,b}(x^{(i)})) & \text{if } y^{(i)}=1\\ -\log(1-f_{w,b}(x^{(i)})) & \text{if } y^{(i)}=0\end{cases}$$

### Loss Intuition
* **If $y^{(i)}=1$:** As the model prediction approaches $1$, the loss approaches $0$. If the model predicts $0$, the loss shoots up to infinity ($-\log(0)\rightarrow\infty$), heavily penalizing the wrong prediction.
* **If $y^{(i)}=0$:** As the model prediction approaches $0$, the loss matches $0$. If the model predicts $1$, the loss shoots up to infinity.

### Simplified Unified Loss Equation
Instead of writing a conditional piecewise equation, we can combine both cases into a single, elegant algebraic expression for a single training example ($i$):

$$\text{Loss}(f_{w,b}(x^{(i)}),y^{(i)})=-y^{(i)}\log(f_{w,b}(x^{(i)}))-(1-y^{(i)})\log(1-f_{w,b}(x^{(i)}))$$

* **Case $y^{(i)}=1$:** The second term multiplying by $(1-1)=0$ completely drops out, leaving only $-1\cdot \log(f(x))$.
* **Case $y^{(i)}=0$:** The first term multiplying by $0$ completely drops out, leaving only $-1\cdot \log(1-f(x))$.

### The Total Cost Function $J(w,b)$
By taking the average of this unified loss over all $m$ training examples, we get the complete convex cost function for logistic regression:

$$J(w,b)=-\frac{1}{m}\sum_{i=1}^{m}\left[y^{(i)}\log(f_{w,b}(x^{(i)}))+(1-y^{(i)})\log(1-f_{w,b}(x^{(i)}))\right]$$

---

## Gradient Descent for Logistic Regression

To minimize the cost function $J(w,b)$ we apply the exact same iterative gradient descent update format.

Repeat until convergence:
$$w_{j}=w_{j}-\alpha\left[\frac{1}{m}\sum_{i=1}^{m}(f_{w,b}(x^{(i)})-y^{(i)})\cdot x_{j}^{(i)}\right] \text{ (simultaneously for all } j=1\dots n)$$
$$b=b-\alpha\left[\frac{1}{m}\sum_{i=1}^{m}(f_{w,b}(x^{(i)})-y^{(i)})\right]$$

> **Crucial Conceptual Note:** While this derivative looks mathematically identical to the linear regression gradient descent rule, it is not the same model. The definition of the hypothesis function $f_{w,b}(x)$ here is the non-linear Sigmoid function $\frac{1}{1+e^{-(w\cdot x+b)}}$, whereas in linear regression it is simply $(w\cdot x+b)$.

---

## The Problem of Overfitting

When training a model, it can fall into one of three structural categories depending on its generalization capabilities:

* **Underfitting (High Bias):** The model is too simple to capture the underlying pattern of the data (e.g., trying to fit a straight line to completely circular data). It performs poorly on both training and new data.
* **Generalization (Just Right):** The model fits the training patterns cleanly and generalizes effectively to accurately predict completely unseen test examples.
* **Overfitting (High Variance):** The model is overly complex (e.g., has too many polynomial features) and fits the training data perfectly—including its random noise. Result: The training error is $0$, but it fails drastically when attempting to make predictions on new data points.

### Solutions to Overfitting
1. Collect more training data.
2. Select or eliminate features (Feature Selection).
3. **Regularization:** Keep all features, but reduce the magnitude/values of the parameters $w$.

---

## Regularization Cost Function

Regularization works by adding a penalty term to the cost function to discourage the weights $w$ from becoming too large.

### Intuition
If you have a model with a massive polynomial term like $w_{4}x^{4}$, adding a heavy penalty that forces $w_{4}\approx0$ effectively cancels out the wavy complexity of that polynomial term, smoothing out the decision curve.

### L2 Regularization Formulation
We modify our standard cost function by appending an extra regularization penalty term at the end:

$$J(w,b) = \text{Original Cost Function} + \frac{\lambda}{2m}\sum_{j=1}^{n}w_{j}^{2}$$

When applied directly to **Linear Regression**, the complete cost formula becomes:

$$J(w,b)=\frac{1}{2m}\sum_{i=1}^{m}(f_{w,b}(x^{(i)})-y^{(i)})^{2}+\frac{\lambda}{2m}\sum_{j=1}^{n}w_{j}^{2}$$

### Key Parameters
* **$\lambda$ (Lambda):** The Regularization Parameter. It controls the trade-off between trying to fit the training data well versus keeping the parameter weights small.
* **Why the bias $b$ is omitted:** Conventionally, we only penalize the weights $w$ and leave the bias scalar $b$ unregularized, as adjusting $b$ merely shifts the function vertically and doesn't introduce complex curve fitting.

### The Effect of Choosing $\lambda$
* If $\lambda=0$: The model acts with no regularization (prone to overfitting).
* If $\lambda$ is set extremely large $(\lambda\rightarrow\infty)$: The optimization forces all weights $w_{j}\approx 0$. The model drops down to a flat horizontal line $(f(x)=b)$, causing extreme underfitting.