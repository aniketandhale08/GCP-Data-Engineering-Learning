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
