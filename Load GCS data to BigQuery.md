1. list the active account
      ```
   gcloud auth list
      ```

2. list the project ID
   ```
   gcloud config list project
   ```
3. This command will create a new dataset called `finance` in your Google BigQuery project with the project ID specified by `$PROJECT_ID`.
   ```
   bq mk --dataset $PROJECT_ID:finance
   ```
   this is the name of your CSV file `PS_20174392719_1491204439457_log.csv` which is present in GCS
   
## Load data to BigQuery tables
To load your data into BigQuery you can either use the BigQuery user interface or the command terminal in Cloud Shell. Choose one of the options below to load your data.

### Option 1: Command line

1. This command will load data from Google Cloud Storage into the `fraud_data` table within the `finance` dataset in BigQuery.
   
   ```
   bq load --autodetect --source_format=CSV --max_bad_records=100000 finance.fraud_data gs://$PROJECT_ID/$DATA_FILE
   ```
      BigQuery will automatically create the fraud_data table within the finance dataset during the loading process, if it doesn't already exist.

### Option 2: BigQuery user interface

1. Click View actions(3 dot) next to the finance dataset and then click Create Table.

![image](https://github.com/user-attachments/assets/8c093a42-ada6-4ca7-be9e-923ba5626e58)

2. In Source dropdown select the `Google Cloud Storage` and select the raw csv file by clicking on browse in your Cloud Storage bucket.
3. Enter table name as fraud_data and select the Auto detect option under Schema so that the variable names will be read from the first line of the raw file automatically.
4. Click Create table.
5. Once completed, in the Explorer panel view in BigQuery, click on the finance dataset and find the table fraud_data to view the metadata and preview the table data.
