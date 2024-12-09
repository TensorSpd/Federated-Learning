# Day 15: PySyft Tutorial – Datasets and Assets 

Welcome to **Day 15** of our **30 Days of FL Code** challenge! Today, we delved into **PySyft**, a cutting-edge library that empowers secure and privacy-preserving machine learning. This exploration was guided by the [PySyft Documentation](https://docs.openmined.org/en/latest/getting-started/part1-dataset-and-assets.html), allowing us to grasp the essential steps for handling non-public data using PySyft.

---

## 🔍 Introduction

Handling sensitive and non-public data securely is a critical aspect of modern data science. **PySyft** offers a robust solution by enabling data scientists to perform analyses without directly accessing the raw data. This approach ensures data privacy while still allowing for meaningful insights and collaborations.

---

## 👩‍🔬 Example Use Case: Breast Cancer Data Study

To better understand PySyft's capabilities, we explored a practical scenario involving breast cancer data. This case study introduced us to two key roles:

### **Rachel, Data Scientist** 🧑‍💻

- **Role:** Data Scientist and Researcher
- **Objective:** Utilize the non-public “Breast Cancer Biomarker” dataset to develop machine learning models for studying breast cancer.
- **Challenge:** Accessing and analyzing sensitive data without compromising its privacy.

### **Owen, Data Owner** 🏥

- **Role:** Laboratory Data Manager at the Cancer Biomarker Research Group
- **Responsibilities:**
  - Organize and curate the database of clinical data from anonymized patient samples.
  - Ensure compliance with legal and regulatory constraints, preventing the dataset from being made publicly available or its copies from leaving the research center premises.
  - Set up and manage a PySyft Datasite to host the dataset, including uploading data, managing credentials, and reviewing project proposals from external data scientists like Rachel.

---

## 🔄 PySyft Data Science Workflow

Through today's tutorial, we followed a structured workflow that illustrated how PySyft facilitates secure data science collaborations:

1. **Setup Datasite:** Owen uploads the dataset and configures Rachel's access credentials.
2. **Connect and Prepare Code:** Rachel connects to the Datasite, prepares her machine learning code, and submits her research study.
3. **Review Study:** Owen reviews Rachel's code for approval.
4. **Execute Study:** Upon approval, Rachel remotely executes her code on the Datasite and retrieves the results.

This workflow highlighted how PySyft ensures data privacy while enabling effective collaboration between data owners and data scientists.

---

## 🚀 Getting Started

Before diving into the specifics, we ensured that **PySyft** was properly installed. Verifying the installation and checking the version was straightforward:

```bash
$ python -c "import syft; print(syft.__version__)"
```

- **If PySyft is installed:** The version number is displayed.
- **If not installed:** A `ModuleNotFoundError` is encountered.

**📋 Requirements:**

- **PySyft Version:** 0.9
- **Python Version:** 3.10 or later

For installation guidance, the [PySyft Quick Installation Guide](https://docs.openmined.org/en/latest/quick-install.html) was invaluable.

---

## 📁 Part 1: Datasets and Assets

### 🛠 What We Learned

In this segment, we focused on:

- Launching a local Datasite using PySyft.
- Creating and configuring assets with both public and non-public information.
- Uploading a dataset to the Datasite.

### 🖥️ 1.1 Launching a Local Development Datasite

We explored various methods to launch a Datasite server:

- **Using `syft.orchestra.launch`:** Best suited for local development.
- **Using a Single Container (Docker, Podman):** Ideal for lightweight deployments.
- **Using Kubernetes:** Optimal for production environments.

For our purposes, we opted for `syft.orchestra.launch` due to its simplicity in a development setting.

**Steps Undertaken:**

1. **Importing PySyft:**

    ```python
    import syft as sy
    ```

2. **Launching the Datasite Server:**

    ```python
    data_site = sy.orchestra.launch(name="cancer-research-centre", reset=True)
    ```

    - **Parameters:**
      - `name`: Unique identifier for the Datasite.
      - `reset=True`: Initializes the server instance for the first time.

3. **Logging into the Datasite:**

    ```python
    client = data_site.login(email="info@openmined.org", password="changethis")
    ```

    > **🔑 Note:** Initially, default admin credentials are used. Future steps will cover updating and personalizing these credentials and managing user accounts.

### 📥 1.2 Downloading Our Example Dataset

To simulate Owen’s “Breast Cancer Biomarker” dataset, we utilized the **Breast Cancer Wisconsin Diagnostic** dataset.

**Steps Undertaken:**

1. **Installing the `ucimlrepo` Package:**

    ```bash
    $ pip install ucimlrepo
    ```

    > **💡 Ensure:** This package is installed in the same Python environment where PySyft is installed.

2. **Downloading the Dataset:**

    ```python
    from ucimlrepo import fetch_ucirepo 

    # Fetch dataset 
    breast_cancer_wisconsin_diagnostic = fetch_ucirepo(id=17) 

    # Data (as pandas DataFrames) 
    X = breast_cancer_wisconsin_diagnostic.data.features 
    y = breast_cancer_wisconsin_diagnostic.data.targets

    # Metadata 
    metadata = breast_cancer_wisconsin_diagnostic.metadata

    # Variable information 
    variables = breast_cancer_wisconsin_diagnostic.variables
    ```

3. **Exploring the Dataset:**

    ```python
    X.head(n=5)  # Preview the first 5 rows
    X.shape      # Output: (596, 30)

    y.sample(n=5, random_state=10)  # Preview sample targets
    ```

    - **Features (`X`):** 596 samples with 30 clinical features each.
    - **Targets (`y`):** Categorical outcomes indicating tumor status: B (Benign) or M (Malignant).

### 🛠️ 1.3 Creating Assets and Dataset

**PySyft** enables interaction with data without direct access by utilizing **Datasites**, **Datasets**, and **Assets**.

#### 🌀 Creating Mock Data

To allow Rachel to develop machine learning models without accessing the real data, Owen created mock data.

**Steps Undertaken:**

1. **Importing NumPy and Setting Seed:**

    ```python
    import numpy as np

    # Fix seed for reproducibility
    SEED = 12345
    np.random.seed(SEED)
    ```

2. **Generating Mock Features and Targets:**

    ```python
    X_mock = X.apply(lambda s: s + np.mean(s) + np.random.uniform(size=len(s)))
    y_mock = y.sample(frac=1, random_state=SEED).reset_index(drop=True)
    ```

    - **Features (`X_mock`):** Original features with added mean and random noise.
    - **Targets (`y_mock`):** Shuffled targets to preserve distribution without revealing patterns.

    > **🔒 Note:** This method ensures no private information is leaked, effectively anonymizing the data for our use case.

#### 📦 Creating Assets

An **Asset** in PySyft represents a specific piece of data within a dataset, containing both real and mock data.

**Steps Undertaken:**

1. **Creating Features Asset:**

    ```python
    features_asset = sy.Asset(
        name="Breast Cancer Data: Features",
        data = X,      # Real data
        mock = X_mock  # Mock data
    )
    ```

2. **Creating Targets Asset:**

    ```python
    targets_asset = sy.Asset(
        name="Breast Cancer Data: Targets",
        data = y,      # Real data
        mock = y_mock  # Mock data
    )
    ```

3. **Inspecting Assets:**

    ```python
    features_asset.data.head(n=3)
    features_asset.mock.head(n=3)
    ```

#### 🗂️ Creating a Dataset

A **Dataset** in PySyft aggregates multiple assets and includes metadata for better organization.

**Steps Undertaken:**

1. **Preparing Metadata:**

    ```python
    # Description
    description = f'{metadata["abstract"]}\n{metadata["additional_info"]["summary"]}'

    # Citation
    paper = metadata["intro_paper"]
    citation = f'{paper["authors"]} - {paper["title"]}, {paper["year"]}'

    # Summary
    summary = "The Breast Cancer Wisconsin dataset can be used to predict whether the cancer is benign or malignant."
    ```

2. **Creating Dataset Object:**

    ```python
    breast_cancer_dataset = sy.Dataset(
        name="Breast Cancer Biomarker",
        description=description,
        summary=summary,
        citation=citation,
        url=metadata["dataset_doi"],
    )
    ```

3. **Adding Assets to Dataset:**

    ```python
    breast_cancer_dataset.add_asset(features_asset)
    breast_cancer_dataset.add_asset(targets_asset)
    ```

4. **Viewing the Dataset:**

    ```python
    breast_cancer_dataset
    ```


### ☁️ 1.4 Uploading the Dataset to the Datasite

With the dataset and assets prepared, uploading them to the Datasite was the next step.

**Steps Undertaken:**

1. **Uploading Dataset:**

    ```python
    client.upload_dataset(dataset=breast_cancer_dataset)
    ```


2. **Verifying Upload:**

    ```python
    client.datasets
    ```


### 🛑 1.5 Shutting Down the Datasite

After completing the upload, shutting down the Datasite server was necessary.

**Steps Undertaken:**

1. **Shutting Down Server:**

    ```python
    data_site.land()
    ```

---

## 🎉 Conclusion

We learned how to:-

- Set up local Datasite using PySyft.
- Create mock data to ensure data privacy without compromising the dataset's integrity.
- Defined assets for both features and targets, integrating real and mock data seamlessly.
- Compiled these assets into a cohesive dataset enriched with comprehensive metadata.
- Uploade the dataset to the Datasite and verified its availability, ensuring readiness for collaborative research.


---
