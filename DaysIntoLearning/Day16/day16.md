
# Day 16: #30DaysOfFLCode 🚀  
**Learning Federated Learning with PySyft**

---

### What I Learned Today:  
I explored **Part 2: Clients and Datasite Access** in PySyft, focusing on configuring access credentials and user management for a Datasite. Here’s the breakdown:

---

## 1️⃣ Reconnecting to the Datasite  
After setting up the **Datasite** in Part 1, I learned how to reconnect to the server while preserving persistency. This is crucial when working with local development servers.  

### Key Points:
- **Persistency**: Reconnecting with the same `name` parameter ensures that previously uploaded datasets remain available.
- **Reset Parameter**:
  - `reset=False` (default): Maintains existing data and configurations.
  - `reset=True`: Used only for the first initialization or to reset the server.  

### Example Code:
```python
import syft as sy

# Reconnecting to the cancer-research-centre Datasite
data_site = sy.orchestra.launch(name="cancer-research-centre", reset=False)

# Logging in as root client with default credentials
client = data_site.login(email="info@openmined.org", password="changethis")

# Verify if the previously uploaded dataset is still present
print(client.datasets)
```

### Insights:
- Using `reset=False` allowed me to confirm that the previously uploaded "Breast Cancer Dataset" was still available.
- Reconnecting with the same `name` creates continuity between sessions.

---

## 2️⃣ Updating Admin Credentials  
I learned how to update the default admin credentials provided by PySyft to secure the Datasite. This step is essential to ensure proper authentication and customization of the admin profile.

### Key Points:
- The admin account (`client.account`) can be updated to reflect new credentials and profile information.
- Functions used:
  - **Update Email**: `client.account.set_email(new_email)`
  - **Update Password**: `client.account.set_password(new_password, confirm=False)`
  - **Update Profile**: `client.account.update(name, institution, website, role)`

### Example Code:
```python
# Update admin email and password
OWEN_EMAIL = "owen@cancer-research.science"
OWEN_PASSWD = "cancer_research_syft_admin"

client.account.set_email(OWEN_EMAIL)
client.account.set_password(OWEN_PASSWD, confirm=False)

# Update admin profile information
client.account.update(name="Owen, the Data Owner", 
                      institution="Cancer Research Centre")
```

### Verification:
- After updating the credentials, I learned how to log back in using the new details:
  ```python
  client = data_site.login(email=OWEN_EMAIL, password=OWEN_PASSWD)
  print(client.users)  # Check registered users
  ```

Insights:
- Updating credentials immediately secures the Datasite.
- The updated profile information makes the admin account more identifiable.

---

## 3️⃣ Registering New Users  
Learned how to register a new user (e.g., Rachel, the Data Scientist) to the Datasite. This is critical for enabling collaboration and assigning specific roles to users.

### Key Points:
- Function Used: `client.users.create()`  
  - **Mandatory Parameters**:  
    - `name`: The user’s full name.  
    - `email`: The user’s email address.  
    - `password` and `password_verify`: The user’s password for login.  
  - **Optional Parameters**:  
    - `institution`: The user’s affiliated institution.  
    - `website`: The user’s professional website.

### Example Code:
```python
# Registering a new user (Rachel) as Data Scientist
rachel_account_info = client.users.create(
    email="rachel@datascience.inst",
    name="Dr. Rachel Science",
    password="syftrocks",
    password_verify="syftrocks",
    institution="Data Science Institute",
    website="https://datascience_institute.research.data"
)

# Print details of the newly registered user
print(f"New User: {rachel_account_info.name} ({rachel_account_info.email}) registered as {rachel_account_info.role}")
```

### Verification:
- Listing all registered users to confirm Rachel’s account has been added:
  ```python
  print(client.users)
  ```

Insights:
- By default, Rachel is assigned the role of **Data Scientist**.
- User registration is straightforward and ensures secure access to the Datasite.

---

### Key Takeaways:  
1. **Reconnecting to Datasites**: Using the same `name` parameter maintains persistency. The `reset=False` parameter ensures previously uploaded datasets remain available.  
2. **Updating Admin Credentials**: Secure the admin account by changing the default credentials and updating the profile information.  
3. **User Registration**: Adding new users with specific roles like Data Scientist enables collaboration while maintaining role-based access control.

---
### Next up
I’ll explore the workflow from a Data Scientist’s perspective and perform operations on the Datasite! 🎉  

---

