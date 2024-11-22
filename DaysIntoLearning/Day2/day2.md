# 🚀 30 Days of Federated Learning Code

## 📅 Day 2: Problem Formulation and Core Challenges in Federated Learning

### 📆 Date
*21st November 2024*

---

## 📚 Source
**"Federated Learning: Challenges, Methods, and Future Directions"**  
*Tian Li, Anit Kumar Sahu, Ameet Talwalkar, Virginia Smith*  
[Read the paper on arXiv](https://arxiv.org/pdf/1908.07873)

---

### 🎯 Objective
Deepen the understanding of the foundational problem formulation in federated learning and identify the core challenges that differentiate it from traditional distributed learning paradigms. 

---

### 🔄 Recap of Day 1
- **🔍 Introduction to Federated Learning**
  - Explored the basic concepts, benefits, and real-world applications.

---

### ✅ Tasks Completed

1. ### **🧠 Studied Problem Formulation**

- **Canonical Objective**: Reviewed the mathematical formulation of federated learning.  
  `min_w F(w) where F(w) = Σ (from k=1 to m) p_k * F_k(w)`


- **Explanation**:
  - \( m \): Total number of devices.
  - \( p_k \): Weighting factor for the \( k \)-th device.
  - \( F_k(w) \): Local objective function representing empirical risk on device \( k \).


2. **📋 Identified Core Challenges**
   - **Challenge 1: Expensive Communication** 🛰️
     - Communication bottlenecks due to the large number of devices.
     - Necessity for communication-efficient methods to minimize data transmission.
   - **Challenge 2: Systems Heterogeneity** 🖥️
     - Variability in device capabilities (CPU, memory, connectivity).
     - Handling unreliable devices and ensuring fault tolerance.
   - **Challenge 3: Statistical Heterogeneity** 📊
     - Non-IID (Independent and Identically Distributed) data across devices.
     - Addressing diverse data distributions to maintain model performance.
   - **Challenge 4: Privacy Concerns** 🔒
     - Risks associated with sharing model updates.
     - Implementing privacy-preserving techniques like differential privacy.

3. **📚 Literature Review**
   - Summarized key research papers addressing each core challenge.
   - Identified current methodologies and their limitations.

4. **🖊️ Documentation and Visualization**
   - Created detailed notes on problem formulation and challenges.
   - Developed diagrams illustrating federated learning architecture and data flow.

---

### 🔮 Next Steps

#### Addressing Each Problem in Detail
In the upcoming days, I will focus on addressing each identified core challenge in detail and analyzing outcomes. The plan includes:

1. **Challenge 1: Expensive Communication** 🛰️
   - Implement communication-efficient techniques (e.g., gradient compression, reduced communication rounds).
   - Analyze the trade-offs between communication cost and model performance.

2. **Challenge 2: Systems Heterogeneity** 🖥️
   - Explore methods for handling variable device capabilities (e.g., partial training, asynchronous updates).
   - Simulate a federated learning environment with heterogeneous devices and evaluate the system's robustness.

3. **Challenge 3: Statistical Heterogeneity** 📊
   - Study techniques for mitigating the impact of non-IID data (e.g., personalized models, meta-learning approaches).
   - Measure the effectiveness of these techniques on real-world datasets.

4. **Challenge 4: Privacy Concerns** 🔒
   - Experiment with privacy-preserving methods (e.g., differential privacy, secure aggregation).
   - Evaluate the balance between privacy guarantees and model accuracy.


