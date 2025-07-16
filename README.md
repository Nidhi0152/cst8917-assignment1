# CST8917 – Assignment 1  

---

##  Objective  
This assignment demonstrates how to build and deploy a serverless image metadata processing pipeline using Azure Durable Functions. The pipeline extracts metadata from uploaded images and stores it in an Azure SQL Database.


##  Project Setup in VS Code  
### 1. Created Durable Function App  
- **Language:** Python  
- **Programming Model:** v2  
- **Triggers:**
  - Blob Trigger: `starter_function`
  - Orchestration Trigger: `orchestrator_function`
  - Activities: `extract_metadata_activity`, `store_metadata_activity`

### 2. Blob Trigger Configuration  
Triggered by image upload to the container `images-input`.

### 3. Metadata Extraction  
Used `Pillow` to read image metadata: format, dimensions, and file size.

### 4. SQL Storage  
Used `pyodbc` to insert extracted metadata into an Azure SQL table.

---

## Azure Setup  
### Created Azure Resources  
- Azure Function App  
- Azure Blob Storage container: `images-input`  
- Azure SQL Database: `assignment1db`  
  - **Server Name:** `assignmnet1server.database.windows.net`
  - **Table:** `dbo.image_metadata`

### SQL Table Schema  
```sql
CREATE TABLE dbo.image_metadata (
    file_name NVARCHAR(100),
    file_size_kb FLOAT,
    width INT,
    height INT,
    format NVARCHAR(20)
);
```

### Configuration in Azure Portal  
- **Application Settings:**
  - `AzureWebJobsStorage` – Blob connection string  
  - `SQLConnectionString` – SQL connection string with ODBC driver prefix  

---

##  Local Testing  
1. Start local development environment:  
   ```bash
   func start
   ```
2. Upload image to `images-input` via Azure Storage Explorer or directly.  
3. Durable Function triggers and processes image.  
4. Metadata is inserted into SQL table.  

---

##  Deployment to Azure  
1. Deploy Function App:  
   ```bash
   func azure functionapp publish <your-app-name>
   ```

2. Upload image to blob via Azure Portal into.  
3. Monitor logs in Azure Function App.  
4. Query the SQL table:  
   ```sql
   SELECT * FROM dbo.image_metadata;
   ```

---

##  Demo Video  
 [Watch on YouTube](https://youtu.be/Q3Gr3rQywlY) 



