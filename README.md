# Adapting the Elo Rating System for Financial Markets

**Author:** Siddhartha Srinivas Rentala  
**Affiliation:** Fordham University, Quantitative Finance

---

## Abstract

This project investigates how the traditional Elo rating system can be adapted to rank securities in financial markets. By redefining monthly return comparisons as "games" between securities and reinterpreting the Elo update mechanism within a logistic regression framework, this work aims to provide a dynamic, risk-adjusted methodology for asset ranking and portfolio construction.

---

## Table of Contents

- [Assumptions](#assumptions)
- [The Basic Elo Rating System](#the-basic-elo-rating-system)
- [Connection to Logistic Regression](#connection-to-logistic-regression)
- [Extensions for Financial Markets](#extensions-for-financial-markets)
- [Conclusion and Future Work](#conclusion-and-future-work)
- [Usage](#usage)
- [License](#license)
- [Contact](#contact)

---

## Assumptions

- **Pairwise Comparison:**  
  Each security competes in a "game" defined by comparing its monthly average return with that of another security, meaning every security is pitted against all others every month.

- **Win Definition:**  
  A security wins a game if its monthly average return exceeds that of its opponent.

- **Continuous vs. Binary Outcomes:**  
  Unlike traditional sports results (win/loss), financial returns are continuous. This necessitates extending the binary Elo framework to account for the magnitude of return differences.

- **Market Dynamics:**  
  The system must capture market volatility, risk, and the evolving nature of financial performance. Extensions are needed to incorporate additional factors over time.

---

## The Basic Elo Rating System

Traditionally, the Elo rating system updates a team or player's rating based on the outcome of a game. For two competitors \( i \) and \( j \), the update rule is:

\[
\text{elo}_i^{\text{new}} = \text{elo}_i^{\text{old}} + k \left( t_{ij} - \text{Pr}(i \text{ beats } j) \right)
\]

Where:
- \( t_{ij} = 1 \) if security \( i \) wins and 0 otherwise.
- The win probability is given by:
  \[
  \text{Pr}(i \text{ beats } j) = \frac{1}{1 + 10^{-(\text{elo}_i - \text{elo}_j)/400}}
  \]
- \( k \) is a sensitivity constant.

This formulation, designed for binary outcomes, forms the basis for our adaptations.

---

## Connection to Logistic Regression

Steven Morse’s work reinterprets the Elo update as an instance of logistic regression. Key insights include:

- **Pairwise Data Representation:**  
  For a matchup between securities \( i \) and \( j \), define the input vector:
  \[
  \mathbf{x}_{(ij)} = [x_k] \quad \text{where} \quad x_k =
  \begin{cases}
    1, & k = i, \\
    -1, & k = j, \\
    0, & \text{otherwise}.
  \end{cases}
  \]

- **Logistic Model:**  
  The probability of \( i \) beating \( j \) is modeled as:
  \[
  \text{Pr}(y_{(ij)} = 1 \mid \mathbf{x}_{(ij)}; \mathbf{w}) = \sigma(w_i - w_j) = \frac{1}{1 + e^{-(w_i - w_j)}}
  \]

- **SGD Update:**  
  The corresponding stochastic gradient descent update is:
  \[
  \mathbf{w}_{k+1} = \mathbf{w}_k - \alpha \left( \sigma(\mathbf{w}_k^T \mathbf{x}_{(ij)}) - t_{ij} \right) \mathbf{x}_{(ij)}
  \]
  
This perspective provides a natural extension point for incorporating additional factors and adapting the model for continuous outcomes.

---

## Extensions for Financial Markets

To tailor the Elo system for financial applications, several enhancements are proposed:

- **Continuous Outcomes:**  
  Modify the update rule to consider the magnitude of the return difference, rewarding larger victories with greater rating changes.

- **Risk and Volatility Adjustment:**  
  Integrate measures such as volatility (e.g., a "volatility drag") to adjust for the risk profile of securities, ensuring that high-risk, high-return securities are appropriately calibrated.

- **Temporal Dynamics:**  
  Incorporate decay or momentum factors to account for the time-evolving nature of market performance, reducing biases from historical data when evaluating new securities.

- **Batching Strategies:**  
  Use mini-batches (e.g., grouping all monthly comparisons) instead of updating for each pairwise match to stabilize and improve the robustness of the updates.

- **Stochastic and Multifactor Extensions:**  
  Extend the model by integrating additional stochastic factors (e.g., market beta, momentum) into the logistic regression framework, thus transforming the system into a multifactor model for asset ranking and portfolio construction.

---

## Conclusion and Future Work

This project lays the groundwork for adapting the Elo rating system to rank securities based on monthly returns. While the basic model offers a promising starting point, further work is needed to:
- Empirically validate the proposed extensions.
- Fine-tune model parameters (such as the \( k \)-factor and learning rate).
- Incorporate additional risk factors and dynamic adjustments.

Feedback, suggestions, and collaborations are welcome as I continue to refine this model.

---

## Usage

To explore and build upon this project:

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/yourusername/elo-financial-markets.git
   cd elo-financial-markets
