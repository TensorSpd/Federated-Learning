# Day 25: #30DaysOfFLCode 🚀  
**Understanding the OpenVector Network Architecture and Node Deployment**

---

Continuing from day 23, today, I explored the **OpenVector network**, diving into its decentralized structure, subnet configurations, and the roles of various nodes like **Compute Nodes**, **CoFHE Nodes**, and **Client Nodes**. This decentralized, permissionless ecosystem is designed to enable secure and efficient computations, leveraging CoFHE (Collaborative-Fully Homomorphic Encryption) technology.

---

## **🔍 Key Learnings**

### 1️⃣ **Network Overview**  
The **OpenVector network** operates as a decentralized system comprising multiple **subnets**. Each subnet is tailored to meet specific computation, latency, and security requirements. 

- **Decentralization**: Anyone can host Compute or CoFHE nodes, contributing to the permissionless ecosystem.  
- **Low-Latency Partitioning**: Subnets optimize computations by dividing the network into smaller units with predefined parameters such as encryption keys, node counts, and thresholds.  

#### **How It Works**:  
- Users submit a **reservation request** before running computations, specifying requirements for:
  - **Latency and availability**  
  - **Security levels**  
  - **Node thresholds**  

- The network selects:
  - **Compute Nodes** first, based on computational requirements.  
  - **CoFHE Nodes**, ensuring encrypted operations within the subnet.

#### **Node Dynamics**:  
- **Joining Nodes**: New nodes can join mid-operation to meet the subnet’s requirements if slots are available.  
- **Slashing Mechanism**: Nodes failing to meet obligations or leaving before completing tasks are penalized by **slashing their stake**.  

---

### 2️⃣ **Node Types in OpenVector**

#### **CoFHE Nodes**
- Handle **secure encrypted operations** within the subnet.  
- Out of hundreds of nodes, a defined threshold participates actively.  

#### **Compute Nodes**
- Focus on **high-performance compute tasks** while ensuring decentralization through cryptographic techniques, specifically decryption key decentralization.  
- These nodes maintain scalability and flexibility in handling computational workloads.  

#### **Client Nodes**
- Devices that **initiate compute requests** to the OpenVector network.  
- Use Cases:
  - **Personal Users**: Access inference for private use (e.g., via browsers).  
  - **Web3 Oracles**: Provide services to blockchains by interacting with OpenVector.  

---

### 3️⃣ **Node Deployment Instructions**

#### **Deploying a Compute Node**  
To deploy a compute node, execute the node program with the following syntax:  
```bash
./node cofhe_compute_node <your_ip> <your_port> <setup_node_ip> <setup_node_port>
```

**Requirements**:
- Place `server.pem` and `server_key.pem` files in the working directory for **SSL communication**.  

---

#### **Deploying a CoFHE Node**  
To deploy a CoFHE node, execute the node program using the following syntax:  
```bash
./node cofhe_node <your_ip> <your_port> <setup_node_ip> <setup_node_port>
```

**Requirements**:
- Place `server.pem` and `server_key.pem` files in the working directory for **SSL communication**.  

---

#### **Deploying a Client Node**  
To deploy a client node, use the following syntax:  
```bash
./node client_node <your_ip> <your_port> <setup_node_ip> <setup_node_port>
```

**Note**:
- The client node does not currently listen on any port, but future iterations may modify this behavior to enable **push-based communication**.  
- Place `server.pem` in the working directory.  

---

### **🌟 Why This Matters**

- **Decentralization and Permissionless Access**: OpenVector democratizes the hosting of Compute and CoFHE nodes, creating a robust, decentralized ecosystem.  
- **Optimized Performance**: Subnet partitioning and node reservation ensure that computations are efficient, secure, and scalable.  
- **Flexibility**: The network supports diverse use cases, from personal AI inference to blockchain services.  
- **Trustless Security**: Cryptography and slashing mechanisms reinforce accountability, ensuring reliable node participation.  

---

## **🚀 Next Steps**  

- Explore how the reservation system optimizes subnet configurations for specific workloads.  
- Investigate how slashing penalties maintain reliability in decentralized systems.

