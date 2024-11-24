# 🚀 30 Days of Federated Learning Code

## 📅 Day 3: Communication-Efficiency, Systems Heterogeneity, and Privacy in Federated Learning

### 📆 Date
*23rd November 2024*

---

## 📚 Source
**"Federated Learning: Challenges, Methods, and Future Directions"**  
*Tian Li, Anit Kumar Sahu, Ameet Talwalkar, Virginia Smith*  
[Read the paper on arXiv](https://arxiv.org/pdf/1908.07873)

---

### 🎯 Objective
Dive into strategies to optimize communication, manage device variability, and enhance privacy in federated learning, addressing core challenges of scalability, heterogeneity, and data sensitivity.

---

### 🔄 Recap of Day 2
- **🧠 Studied Problem Formulation**:
  - Understood the mathematical formulation of federated learning objectives.
- **📋 Identified Core Challenges**:
  - Addressed the key challenges of communication bottlenecks, systems heterogeneity, statistical heterogeneity, and privacy concerns.

---

### ✅ Tasks Completed

#### **1. Communication-Efficiency**

- **Local Updating**:
  - Devices perform **multiple updates locally** before communicating with the server, reducing the frequency of communication.
  - Example: **FedAvg**, which aggregates local updates using stochastic gradient descent (SGD).

- **Compression Schemes**:
  - Techniques like **sparsification, quantization, and subsampling** minimize the size of updates.
  - **Challenges**:
    - Stale updates (Outdated Updates) from infrequent device participation.
    - Limited error compensation in federated settings.

- **Decentralized Training**:
  - Explored replacing centralized server-based communication with **decentralized topologies** to reduce server bottlenecks.

#### **2. Systems Heterogeneity**

- **Asynchronous Communication**:
  - Allows devices to send updates independently, avoiding delays from slower devices (stragglers).

- **Active Sampling**:
  - Techniques for selecting devices based on **computation, communication capacity**, or **data representativeness**.

- **Fault Tolerance**:
  - Addressed challenges of device dropouts with strategies like **coded computation** and bias mitigation techniques.

#### **3. Privacy**

- **Differential Privacy**:
  - Ensures individual data points remain unidentifiable by adding noise to updates.
  - Balances between privacy strength and model accuracy.

- **Homomorphic Encryption**:
  - Enables computations on encrypted data but faces scalability challenges.

- **Secure Multi-Party Computation (SMC)**:
  - Allows joint computations without sharing data but incurs high communication costs.

---

### 🔮 Next Steps

#### Coding Federated Learning
Now that we’ve built a strong theoretical foundation, the next step is to **implement federated learning in code**. This will involve:

**Building a Basic Federated Learning Framework**:
   - Simulate a federated environment with multiple clients.
   - Implement **FedAvg** for aggregating local updates.
   - Evaluate model performance across various setups.

### 🚀 Get Ready to Code!
Tomorrow, we begin **coding federated learning**, starting with the basics and gradually incorporating advanced features like privacy and communication optimization. Stay tuned for practical implementations! 😊
