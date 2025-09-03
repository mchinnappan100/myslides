 

# 📊 Markov Chains & Monte Carlo Methods  
## _A Historical and Conceptual Overview_

---

## 🎯 Agenda

1. Historical Background  
2. Markov Chains  
3. Monte Carlo Methods  
4. Applications  
5. Key Takeaways  

---

## 🕰️ Historical Background

- **Markov Chains**  
  - Introduced by **Andrey Markov (1856–1922)**, a Russian mathematician.  
  - Studied stochastic processes with the "memoryless" property.  
  - First applied to sequences of poetry and random events.  

- **Monte Carlo Methods**  
  - Popularized by **Stanislaw Ulam** and **John von Neumann** during the **1940s**.  
  - Developed at **Los Alamos National Laboratory** while working on the Manhattan Project.  
  - Named after the Monte Carlo Casino in Monaco, due to reliance on randomness.  

---

## 🔗 Markov Chains: Definition

A **Markov Chain** is a stochastic process where:

- The **future state** depends **only on the current state**,  
  not on the past history.  

Formally:  

\[
P(X_{n+1} | X_n, X_{n-1}, ..., X_0) = P(X_{n+1} | X_n)
\]

---

## 🎲 Markov Chain Example

- Weather model:  
  - Sunny → 70% chance Sunny, 30% chance Rainy  
  - Rainy → 40% chance Sunny, 60% chance Rainy  

Transition Matrix:  

\[
P = \begin{bmatrix}
0.7 & 0.3 \\
0.4 & 0.6
\end{bmatrix}
\]

---

## 🧮 Monte Carlo Methods: Idea

Monte Carlo methods rely on **random sampling** to solve problems that may be:

- Deterministic but **complex**  
- **High-dimensional integrals**  
- **Optimization** problems  

---

## 🎲 Monte Carlo Example

- Estimating **π (pi)**:  
  - Randomly sample points in a square of side length 2.  
  - Count how many fall inside the unit circle.  
  - Ratio ≈ π/4.  

---

## 🔄 Connection: MCMC

- **Markov Chain Monte Carlo (MCMC)** combines both ideas.  
- Uses a Markov Chain to sample from a **target distribution**.  
- Common algorithms:  
  - Metropolis-Hastings  
  - Gibbs Sampling  

---

## 🌍 Applications

- **Physics & Engineering**  
  - Simulation of particle transport, thermodynamics.  

- **Finance**  
  - Risk modeling, option pricing.  

- **Machine Learning**  
  - Bayesian inference, probabilistic models.  

- **Biology**  
  - Population genetics, protein folding.  

---

## ✅ Key Takeaways

- **Markov Chains**: memoryless stochastic processes.  
- **Monte Carlo Methods**: random sampling for approximations.  
- **MCMC**: powerful tool combining both.  
- Widely used in science, finance, and machine learning.  

---

# 🙌 Thank You!  
Questions?  
