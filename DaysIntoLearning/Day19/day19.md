# Day 19: #30DaysOfFLCode 🚀  
**Retrieving Results with PySyft**  

Just finished with **Part 5: Retrieving Results** in PySyft, marking the final step in the Federated Learning workflow. This part focused on how a **Data Scientist** retrieves results from an approved code request executed on real (non-public) data.  

Here’s what I learned:

---

## 1️⃣ Checking the Status of Sent Requests  

After submitting a project, the first step is to log in to the Datasite and check the status of the request. This ensures that the Data Owner has reviewed and approved the submitted code.  

### Key Steps:
- **Login to the Datasite**:  
  Verify that the local server is running and log in with Data Scientist credentials.  
- **Check Request Status**:  
  Use `client.requests` to check whether the request is approved.  

### Example Code:
```python
import syft as sy

# Launch the Datasite
data_site = sy.orchestra.launch(name="cancer-research-centre")

# Login as Data Scientist
client = data_site.login(email="rachel@datascience.inst", password="syftrocks")

# Check request status
print(client.requests)
```

### Insights:
- Seeing an "Approved" status indicates that the code has been reviewed and executed by the Data Owner.  
- This step ensures transparency and establishes trust in the collaborative workflow.  

---

## 2️⃣ Retrieving Results on Real Data  

Once the request is approved, the Data Scientist can retrieve the results of the computation performed on real data.  

### Key Steps:
- **Access Dataset and Assets**:  
  Retrieve the required dataset (e.g., `features` and `labels`) for analysis.  
- **Retrieve Results**:  
  Execute the approved code and retrieve the results.  

### Example Code:
```python
# Access the dataset and assets
bc_dataset = client.datasets["Breast Cancer Biomarker"]
features, labels = bc_dataset.assets

# Retrieve results on real data
result = client.code.ml_experiment_on_breast_cancer_data(features_data=features, labels=labels).get()
print("Results on Real Data:", result)
```

### Insights:
- Results are securely retrieved from the Datasite only after the request is approved.  
- This step ensures that sensitive data is never directly shared, preserving privacy while enabling meaningful analysis.

---

### Key Takeaways:  

1️⃣ **Collaborative Workflow**:  
   - PySyft’s workflow ensures transparency and trust between Data Owners and Data Scientists.  
   - Every request is reviewed, tested, and approved before execution.  

2️⃣ **Secure Data Handling**:  
   - Sensitive data remains on the Datasite and is never directly exposed to Data Scientists.  
   - Results are retrieved securely only after approval.  

3️⃣ **Completing the Workflow**:  
   - This final step ties together the entire Federated Learning process, enabling secure collaboration on private datasets.  

---