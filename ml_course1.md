---
layout: default
title: Machine Learning 1
nav_order: 5
---

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
