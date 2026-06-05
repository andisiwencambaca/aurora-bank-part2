# Isolation Forest: Rationale, Method, and Limitations
## Motivation for Using Isolation Forest

In this project, the original dataset did not contain a verified fraud label which meant that supervised learning methods could not be directly trained on ground-truth fraud outcomes.

To address this, the problem is reframed as an anomaly detection task, where the objective is to identify transactions that deviate significantly from typical behavioural patterns.

Isolation Forest is selected because it is specifically designed for:

* Detecting rare and unusual observations
* Operating without labelled data
* Scaling efficiently to large transactional datasets

The core assumption is that fraudulent transactions are statistically rare and exhibit behavioural patterns that differ from legitimate activity.

## Conceptual Approach

Isolation Forest is based on a simple but effective principle:

* Anomalies are easier to isolate than normal observations.

Instead of modelling normal behaviour explicitly, the algorithm:

1. Randomly selects a feature
2. Randomly selects a split value
3. Repeats this process to partition the data

Observations that require fewer splits to isolate are considered more anomalous.

This is particularly suitable for fraud detection because fraudulent transactions often differ along multiple dimensions such as:

* Transaction amount
* Transaction frequency
* Temporal spacing between events
* Behavioural consistency within a user profile

## Why Isolation Forest Fits This Problem

Isolation Forest is appropriate for this context due to several characteristics of the dataset:

### 1. Absence of labels

The dataset lacks a binary fraud indicator. Isolation Forest does not require labelled data, making it suitable for unsupervised learning.

### 2. High imbalance expectation

Fraud is assumed to be rare. Isolation Forest performs well under extreme class imbalance because it does not rely on class distributions.

### 3. Heterogeneous behavioural patterns

Transactions vary across multiple behavioural dimensions (amount, timing, frequency). Isolation Forest can handle mixed behavioural signals without explicit assumptions about their distribution.

### 4. Minimal preprocessing requirements

Unlike many statistical models, Isolation Forest does not require:

* Feature scaling
* Distributional assumptions
* Linear separability

This is useful in early-stage fraud investigation where feature distributions are unknown.

# Feature Design Considerations

The model is applied on engineered behavioural features rather than raw transactional fields. These features are designed to capture deviations from normal user activity:

* Transaction amount (absolute value signal)
* Daily transaction volume (aggregate behaviour)
* Transaction frequency (velocity patterns)
* Time-based features (hour of day, inter-transaction time)

These features aim to represent behavioural abnormality, which is more informative than isolated transaction values.

# Interpretation of Model Output

Isolation Forest produces:

* A binary anomaly label (`-1` anomaly, `1` normal)
* A continuous anomaly score (decision function)

In this project, the binary label is used as a first-pass indicator of suspicious transactions. However, the score is more informative because it reflects relative abnormality, not just classification.

# Limitations of the Approach

While useful for exploratory fraud detection, Isolation Forest has several limitations in this context.

### 1. No notion of fraud semantics

The model does not understand fraud. It only detects statistical rarity. As a result:

* Legitimate but unusual behaviour may be flagged
* Sophisticated fraud that mimics normal patterns may not be detected

### 2. Sensitivity to feature engineering

Performance depends heavily on how features are constructed. Poorly designed features can lead to:

* Over-detection of certain user segments
* Under-detection of subtle fraud patterns

### 3. No probabilistic interpretation of fraud

The anomaly score is not a probability of fraud. It only reflects how easily a point is isolated in feature space. This limits direct business interpretability.

### 4. Lack of temporal modelling

Isolation Forest treats each observation independently. It does not inherently model:

* Sequence of transactions
* Long-term behavioural drift
* Evolving fraud strategies over time

This is a significant limitation in transaction-based fraud systems.

### 5. Threshold selection is arbitrary

The decision boundary (contamination rate) must be chosen manually. Without labelled fraud data, this introduces subjectivity into what is considered “anomalous”.

# Summary of Design Rationale

Isolation Forest is used as an unsupervised baseline anomaly detection method to:

* Identify unusual transactional behaviour without requiring labels
* Provide an initial risk signal for downstream analysis
* Support exploratory understanding of fraud-like patterns in the dataset

It is not intended to be a final fraud classification system, but rather a behavioural screening layer that helps surface suspicious activity for further modelling and investigation.
