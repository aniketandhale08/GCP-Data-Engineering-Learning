### What is Dataflow?

- **Unified stream and batch data processing service**  
- **Serverless**  
- Based on **Apache Beam** open-source programming model
- Beam --> B of Batch and eam of Stream
- Supports both **Batch** and **Stream processing**  
- Provides **built-in templates** in Google Cloud  
- These templates can be used instead of writing pipeline code

### 1. Load Data from GCS to BigQuery using Dataflow  

1. **Create Required Files**:  
   - **JavaScript (UDF)**: Map CSV file columns to BigQuery table columns.  
   - **JSON Schema**: Define the BigQuery schema (column names and data types).  
   - **CSV File**: Source data file with all columns.  

2. **Upload Files to Cloud Storage**:  
   - Upload the `UDF.js`, `schema.json`, and `data.csv` files to a Cloud Storage bucket.  

3. **Prepare BigQuery**:  
   - Create a dataset in BigQuery. The table can be created at runtime by Dataflow.  

4. **Set Up Dataflow Job**:  
   - Use the template: **"Text on Cloud Storage to BigQuery"**.  
   - Provide paths for:  
     - **UDF JavaScript file** (Cloud Storage path).  
     - **JSON schema file** (Cloud Storage path).  
     - **CSV file** (Cloud Storage path).  
   - Specify the output BigQuery table:  
     - Format: `project_name:dataset_name.table_name`.  

5. **Run the Job**:  
   - Start the Dataflow job. Monitor its status through the Dataflow UI.  

6. **Verify Output**:  
   - Check the BigQuery table to ensure the specified columns from the CSV file are loaded correctly.  

---

### 2.Extracting Data from Cloud Spanner to GCS Using Dataflow

### Summary
This tutorial demonstrates the process of extracting data from a Cloud Spanner table and saving it as a CSV file in a Google Cloud Storage (GCS) bucket using Dataflow with an inbuilt template. Key steps include setting up the Spanner database, configuring the Dataflow job, troubleshooting errors, and verifying successful file creation in GCS.

1. **Set Up Cloud Spanner**
- **Create a Cloud Spanner Instance:**
  - Name: `DF demo`
  - Region: `us-central1`
- **Create a Database:**
  - Name: `dfdb`
- **Create a Table:**
  - Use SQL to create a `Singers` table with appropriate schema.
- **Insert Data into Table:**
  - Add test records (e.g., `3 records`) into the `Singers` table.

2. **Configure Dataflow Job**
- **Job Name:** `spanner-to-gcs`
- **Template:** `Cloud Spanner to Text Files on Cloud Storage`
- **Parameters:**
  - Spanner Instance: `DF demo`
  - Database: `dfdb`
  - Table: `Singers`
  - Output GCS Bucket: Specify bucket name (e.g., `df-demo-bucket`).
  - Temporary Location: Create and specify a temp directory (e.g., `tmp/`).

3. **Run the Dataflow Job**
- Monitor job progress in the GCP console.
- Address errors (e.g., case-sensitive table names or missing fields).

4. **Troubleshoot Common Errors**
- Ensure table names are **case-sensitive** and match the Spanner schema.
- Validate all required parameters for the Dataflow job.
- Correct bucket paths with necessary trailing slashes (`/`).

5. **Verify Output**
- After the job is completed, check the GCS bucket for the generated CSV file.
- Open the file and confirm the data matches the source records from Spanner.

### Learnings
- Case sensitivity in table names is critical for Cloud Spanner.
- Providing correct paths and bucket names is essential to avoid runtime errors.
- Errors encountered during the process can help with learning how to troubleshoot pipelines.

---

### 2. Build a Dataflow Job Using SQL Queries  

### Summary  
This guide demonstrates the creation of a Dataflow job using SQL queries in BigQuery. The goal is to filter and extract specific data (e.g., Europe region) from an existing BigQuery table and store it in a new table.  

1. **Explore BigQuery Table**  
- Check the available datasets and tables in BigQuery (e.g., `sales`).  
- Query the table to understand its structure and rows.  

2. **Write a Filter Query**  
- Use a `WHERE` clause to filter records (e.g., region = Europe).  
- Validate the query in BigQuery to ensure correctness.  

3. **Access Dataflow SQL Editor**  
- Use the **Dataflow SQL Workspace** for creating the job.  
- Use a fully qualified resource ID (`project.dataset.table`) for tables to avoid errors.  
  - Example: `SELECT * FROM \`project.dataset.table\``

4. **Enable APIs**  
- Ensure that **Dataflow API** and **Data Catalog API** are enabled.  

5. **Create a Service Account**  
- Navigate to **IAM and Service Accounts** and create a new service account:  
  - Name: `dataflow-sa`  
  - Role: **Editor** (to interact with GCP services).  

6. **Configure Dataflow Job**  
- Specify job name: e.g., `sales-europe`.  
- Provide necessary parameters:  
  - BigQuery dataset and output table (e.g., `sales_europe`).  
  - Service account details.  
  - Network and subnetwork (default settings are fine).  

7. **Run the Dataflow Job**  
- Monitor the job in the **Dataflow Jobs** dashboard.  
- Validate task statuses (e.g., SQL query execution) and logs.  

8. **Verify the Output**  
- Check if the new table (e.g., `sales_europe`) is created in BigQuery.  
- Query the table to confirm it contains filtered data (e.g., Europe region).  

### Learnings  
- The Dataflow SQL Workspace requires fully qualified table names for BigQuery datasets.  
- Proper configuration (APIs, service accounts) is essential for successful job execution.  
- Dataflow enables automating repetitive SQL operations, creating pipelines efficiently.  


---

### 3. Data from PubSub to BigQuery Using Dataflow

### Summary  
This guide demonstrates how to construct a **streaming pipeline** on **Google Cloud Platform (GCP)** using services like **Pub/Sub**, **BigQuery**, **Cloud Storage**, and **Dataflow**.

1. **Create a Cloud Storage Bucket:**
   - **Name**: `temp_bucket_BQ_BQ`.
   - **Region**: `us-east4`.
   - Add a folder named `temp` for logs.

2. **Set Up BigQuery:**
   - **Dataset**: `streaming_pup_sub_to_BQ`.
   - **Region**: `us-east4`.
   - **Table**: `stream_pup_sub_to_BQ` with schema (e.g., a single field `data` of type `STRING`).

3. **Create a Pub/Sub Topic:**
   - **Name**: `streaming_pup_sub_to_BQ`.
   - Subscription is optional.

4. **Configure Dataflow:**
   - Use a **predefined template**: `Pub/Sub Topic to BigQuery`.
   - Input details:
     - **Pub/Sub Topic**: The topic created earlier.
     - **BigQuery Table**: The table created.
     - **Temporary Storage**: `temp_bucket_BQ_BQ/temp`.

5. **Publish Messages to Pub/Sub:**
   - Example: Messages like `KF is a kind person` can be published multiple times.

6. **Verify BigQuery Data:**
   - Query the BigQuery table to confirm data ingestion from Pub/Sub.

### Learnings  
- **Dataflow Job**: Transforms and streams data from Pub/Sub to BigQuery. Ensure the job runs successfully.
- **Enable APIs**: Activate all required APIs for Cloud Storage, BigQuery, Pub/Sub, and Dataflow.
- **Predefined Templates**: Simplifies the pipeline creation, removing the need for custom Dataflow code.

---




