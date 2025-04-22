# dp-203-DE-08

# Automating an Azure Databricks Notebook with Azure Data Factory

This project demonstrates how to orchestrate a data processing task in Azure Databricks using Azure Data Factory. The goal is to run a notebook that ingests product data and saves it to the Databricks File System (DBFS) as part of a pipeline.

## 🔧 Prerequisites

- An active Azure subscription with sufficient quota
- Permission to create resources in the subscription
- Access to Azure portal and Azure Cloud Shell
- Basic knowledge of Azure Databricks and Azure Data Factory

---

## 📁 Step-by-Step Instructions

### 1. Provision Azure Databricks Workspace
- Use Azure Cloud Shell (PowerShell) to run the setup script:
  ```powershell
  rm -r mslearn-databricks -f
  git clone https://github.com/MicrosoftLearning/mslearn-databricks
  ./mslearn-databricks/setup.ps1
2. Create Azure Data Factory
Go to the same resource group and create a Data Factory (v2) in the same region as your Databricks workspace.

3. Create a Databricks Notebook
Create a notebook named Process Data.

Add the following code:

Cell 1:

python
Kopyala
Düzenle
dbutils.widgets.text("folder", "data")
folder = dbutils.widgets.get("folder")
Cell 2:

python
Kopyala
Düzenle
import urllib3

response = urllib3.PoolManager().request('GET', 'https://raw.githubusercontent.com/MicrosoftLearning/mslearn-databricks/main/data/products.csv')
data = response.data.decode("utf-8")

path = "dbfs:/{0}/products.csv".format(folder)
dbutils.fs.put(path, data, True)
4. Enable Integration Between Azure Data Factory and Databricks
Generate Access Token
In Azure Databricks: User Settings > Developer > Access Tokens > Generate New Token.

Create Linked Service in ADF
Go to ADF Studio > Manage > Linked Services > +New > Azure Databricks

Use:

New Job Cluster

Token-based authentication

Cluster version: 13.3 LTS

Cluster node type: Standard_D4ds_v5

Workers: 1

5. Create and Run Pipeline
In ADF Studio > Author > + New Pipeline

Add a Databricks Notebook activity:

Notebook: Process Data

Base parameter: folder = product_data

Validate and Publish

Run using "Trigger Now"

✅ Output
Monitor pipeline run

Check Databricks logs and output

Output includes run URL and billing details in JSON format

🧹 Clean-up
To avoid additional charges, delete the resource group from the Azure portal after completing your tests.

Screenshots
![Screenshot 2025-04-22 at 19 57 41](https://github.com/user-attachments/assets/c45869f6-723b-42d8-a47e-5112ed198f8a)
![Screenshot 2025-04-22 at 19 45 31](https://github.com/user-attachments/assets/347386da-2e71-45fa-8d10-a10690117c71)
![Screenshot 2025-04-22 at 19 45 15](https://github.com/user-attachments/assets/912a019e-0b13-44c0-a0be-3a541e4b4440)
![Screenshot 2025-04-22 at 19 42 43](https://github.com/user-attachments/assets/b40d1539-0bd1-43ac-a8c3-2a6710fe3198)
![Screenshot 2025-04-22 at 19 42 25](https://github.com/user-attachments/assets/94b9f604-31f7-4652-b1c6-ce99bd5fb067)
![Screenshot 2025-04-22 at 19 39 22](https://github.com/user-attachments/assets/c207e2c2-30b0-4422-b656-774774353a87)
![Screenshot 2025-04-22 at 19 38 35](https://github.com/user-attachments/assets/91089276-e398-434b-a987-cfb6c2c08b4b)
![Screenshot 2025-04-22 at 19 37 28](https://github.com/user-attachments/assets/fe3e277e-ac03-430a-b15e-0a0ce3b282ef)
![Screenshot 2025-04-22 at 19 36 45](https://github.com/user-attachments/assets/556f59c8-ce00-4ca0-b644-f2d066f88caf)
![Screenshot 2025-04-22 at 19 36 40](https://github.com/user-attachments/assets/be74476b-d123-461c-8748-ed3b7d21e41b)
![Screenshot 2025-04-22 at 19 34 36](https://github.com/user-attachments/assets/d4647d2a-28f7-4647-bb83-c003638e1d84)
![Screenshot 2025-04-22 at 19 33 56](https://github.com/user-attachments/assets/6faddd7c-142e-4589-90ca-369d88538d6e)

