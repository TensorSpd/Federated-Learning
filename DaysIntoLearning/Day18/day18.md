# Day 18: #30DaysOfFLCode 🚀  
**Reviewing Code Requests in PySyft**  

Today, I explored **Part 4: Review Code Request** in PySyft. This part focuses on the **Data Owner’s perspective**, showcasing how incoming project requests are reviewed, tested, and approved. Here’s what I learned:

---
## 1️⃣ Reviewing Incoming Requests  

The Data Owner (e.g., Owen) logs into the Datasite to review submitted projects. This involves accessing the project’s description and associated code to ensure alignment with privacy and security standards.  

### Key Steps:
- **Login to the Datasite**:
  Access the Datasite as the Data Owner and view the submitted project.  
- **Access the Request**:
  Review the project’s purpose and associated code.  

### Example Code:
```python
import syft as sy

# Launch the Datasite
data_site = sy.orchestra.launch(name="cancer-research-centre")

# Login as Data Owner
client = data_site.login(email="owen@cancer-research.science", password="cancer_research_syft_admin")

# View submitted projects
print(client.projects)

# Access the request and inspect the code
request = client.requests[0]
print(request.description)
print(request.code)
```

### Insights:
- The project description provides context for the study, making it easier to verify the code’s intent.
- Reviewing code thoroughly ensures alignment with privacy and security standards.

🔒 **Code Review Tip**: Always evaluate submitted code for potential security risks. Use resources like [Codacity blog post](https://blog.codacy.com/security-code-review-best-practices) for guidance.  

---

## 2️⃣ Executing the Code  

After reviewing the code, the Data Owner runs it on both **mock data** and **real data** to validate its functionality and gather results.  

### Key Steps:
- **Access Assets**: Fetch the required dataset assets (e.g., features and labels).  
- **Run the Code**: Execute the code on mock data to verify functionality, then repeat on real data.  

### Example Code:
```python
# Access dataset and assets
bc_dataset = client.datasets["Breast Cancer Biomarker"]
features, labels = bc_dataset.assets

# Run the code on mock data
result_mock_data = request.code.run(features_data=features.mock, labels=labels.mock)
print("Mock Data Results:", result_mock_data)

# Run the code on real data
result_real_data = request.code.run(features_data=features.data, labels=labels.data)
print("Real Data Results:", result_real_data)
```

### Insights:
- Mock data execution helps validate code behavior without exposing sensitive data.
- Results on real data are collected only after confirming the code's validity.

---

## 3️⃣ Approving the Request  

Once the code is reviewed and tested, the Data Owner approves the request, allowing the Data Scientist to access the results.  

### Key Steps:
- **Approve the Request**: Mark the request as approved after validating its compliance with the data owner’s policies.  
- **Verify Status**: Confirm the request status as "Approved".  

### Example Code:
```python
# Approve the request
request.approve()

# Verify request status
print(client.requests)
```

### Insights:
- Approval ensures that the results are shared securely and only after thorough validation.
- PySyft enforces strict workflows to maintain data privacy and security throughout the process.

---

### Key Takeaways:  

1️⃣ **Secure Code Review**:  
   - Reviewing incoming requests is crucial for maintaining privacy and security.  
   - A detailed code review ensures alignment with the project description and compliance with data access policies.  

2️⃣ **Validation Workflow**:  
   - Mock data execution is a vital step to validate code without risking sensitive information.  
   - Real data execution happens only after code passes initial checks.  

3️⃣ **Controlled Approval**:  
   - Approvals are necessary to ensure transparency and trust between Data Owners and Data Scientists.  

---

💡 **What’s Next?**  

In Part 5, I’ll explore how Data Scientists retrieve results after their requests are approved. This will complete the entire Federated Learning workflow using PySyft.  
