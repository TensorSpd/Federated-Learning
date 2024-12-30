# **Day 26 of #30DaysOfFLCode** 🚀  
**A Quickstart Guide to the OpenVector Network**  

Today, I took a deep dive into [**OpenVector’s Quickstart Guide**](https://openvector.gitbook.io/docs/quick-start/overview), focusing on performing computations over encrypted data using the CoFHE (Collaborative Fully Homomorphic Encryption) library. This exploration covered the entire pipeline: from setting up a client node to verifying encrypted computation results, showcasing the practicality and security of decentralized computation.

---

## **🔍 Key Steps in the Quickstart**

### **1️⃣ Setting Up the Client Node**  
The client node is the entry point for computations in the OpenVector network. It connects to the setup node and initializes cryptographic operations.

#### **Steps**:  
1. Include necessary headers (`cofhe.hpp`, `client_node.hpp`).
2. Parse command-line arguments to initialize node details.
3. Use `make_client_node` to create a CPU-based client node.  
4. Obtain the cryptosystem instance for cryptographic operations.

#### **Key Features**:  
- Establishes a secure connection with the setup node.
- Retrieves the cryptosystem instance to handle encryption, decryption, and tensor management.

---

### **2️⃣ Encrypting Data**  
Encrypted tensors are created for secure computation, ensuring privacy at every step.

#### **Steps**:  
1. Define the dimensions of tensors.  
2. Populate tensors with plaintext values.  
3. Encrypt tensors using the network's public key.  
4. Serialize tensors into strings for seamless network transmission.

#### **Code Example**:
```cpp
// Encrypt and serialize tensors
auto ct1 = cs.encrypt_tensor(client_node.network_public_key(), pt1);
std::string serialized_ct1 = cs.serialize_ciphertext_tensor(ct1);
std::cout << "Tensor encrypted and serialized." << std::endl;
```

#### **Highlights**:  
- Keeps sensitive data encrypted throughout the process.  
- Serialized tensors ensure compatibility and efficient transmission over the network.

---

### **3️⃣ Tensor Multiplication**  
Matrix multiplication over encrypted tensors is performed using the ComputeRequest API.

#### **Steps**:  
1. Create `ComputeOperationOperand` objects for serialized tensors.  
2. Define a `ComputeOperationInstance` for binary multiplication.  
3. Send the request using `ComputeRequest` to the network.  
4. Receive the encrypted result as a response.

#### **Code Example**:
```cpp
// Define operands and operation
ComputeRequest::ComputeOperationOperand operand1(serialized_ct1);
ComputeRequest::ComputeOperationOperand operand2(serialized_ct2);
ComputeRequest::ComputeOperationInstance operation(
    ComputeRequest::ComputeOperationType::BINARY,
    ComputeRequest::ComputeOperation::MULTIPLY,
    {operand1, operand2}
);

// Create and send compute request
ComputeRequest req(operation);
client_node.compute(req, &res);

// Verify result
if (res && res->status() == ComputeResponse::Status::OK) {
    std::cout << "Computation successful, encrypted result received." << std::endl;
}
```

#### **Highlights**:  
- Encrypted computations ensure privacy without exposing intermediate values.  
- The API allows seamless integration of secure operations into any workflow.

---

### **4️⃣ Decrypting the Output**  
The result tensor must be decrypted to extract meaningful values.

#### **Steps**:  
1. Create a decryption request for the encrypted result.  
2. Use the compute method to decrypt the tensor.  
3. Verify the decryption status before proceeding.

#### **Code Example**:
```cpp
// Define decryption operation
ComputeRequest::ComputeOperationOperand decrypt_operand(res->data());
ComputeRequest::ComputeOperationInstance decrypt_operation(
    ComputeRequest::ComputeOperationType::UNARY,
    ComputeRequest::ComputeOperation::DECRYPT,
    {decrypt_operand}
);

// Perform decryption
ComputeRequest req_d(decrypt_operation);
client_node.compute(req_d, &res);

// Check decryption status
if (res && res->status() == ComputeResponse::Status::OK) {
    std::cout << "Decrypted result tensor received." << std::endl;
}
```

#### **Highlights**:  
- Decryption ensures only authorized users can access the results.  
- Seamless integration with encryption workflows.

---

### **5️⃣ Verifying the Output**  
The decrypted tensor is deserialized and verified for correctness.

#### **Steps**:  
1. Deserialize the plaintext tensor from the response.  
2. Flatten the tensor for traversal and display.  
3. Calculate computation and decryption times for analysis.

#### **Code Example**:
```cpp
// Deserialize and display tensor
auto result_tensor = cs.deserialize_plaintext_tensor(res->data());
result_tensor.flatten();
std::cout << "Decrypted Matrix:" << std::endl;
for (size_t idx = 0; idx < result_tensor.size(); ++idx) {
    float value = cs.get_float_from_plaintext(*result_tensor[idx]);
    std::cout << value << " ";
    if ((idx + 1) % matrix_cols == 0) std::cout << std::endl;
}

// Output times
std::cout << "Time taken for computation: " << duration.count() << " microseconds." << std::endl;
```

#### **Highlights**:  
- Deserialized results make it easy to interpret outputs.  
- Time tracking provides insights into performance.

---

## **💡 Key Takeaways**

1. **End-to-End Workflow**: The guide demonstrated the full lifecycle of secure computations, from setup to result verification.  
2. **Secure and Scalable**: CoFHE enables privacy-preserving computations without compromising scalability or performance.  
3. **Practical Usability**: OpenVector’s API makes integrating secure computations straightforward, enhancing real-world adoption.

---
