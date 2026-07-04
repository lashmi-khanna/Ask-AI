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

*   **$\alpha$ (Alpha):** This is the **learning rate**. It is a small positive number (such as $0.01$) that determines the size of the "step" the algorithm takes downhill.
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
