# Dataflow- Cloud Storage to Big Query Batch Job

1. Create a storage bucket and Big Query Data Set
2. Place input file, java script file and JSON schema
3. Run Dataflow pipeline job to read cloud storage and write to BQ

## Sample data
```
415,95,36,2020-03-10,sddsd
261,98,14,2020-04-10,dfgfsdkk;
300,56,10,2020-06-22,fdgfdsf
```

## JSON File
```
{
 "BigQuery Schema": [{
   "name": "order_no",
   "type": "INTEGER"
  },
  {
   "name": "item_no",
   "type": "INTEGER"
  },
  {
   "name": "purchased_quantity",
   "type": "INTEGER"
  },
  {
   "name": "purchased_date",
   "type": "DATE"
  }
 ]
}
```

## Java Script File
```
function transform(line) {
 var values = line.split(',');
 var obj = new Object();
 obj.order_no = values[0];
 obj.item_no = values[1];
 obj.purchased_quantity = values[2];
 obj.purchased_date = values[3];
 var jsonString = JSON.stringify(obj);
 return jsonString;
}
```

### Explanation

This script is taking lines of data (like "415,95,36,2020-03-10,sddsd") and converting each line into a JSON object that matches the given schema. Here's a simple breakdown:

1. Sample Data: Each line has values separated by commas (e.g., order number, item number, quantity, date, and some extra text).

2. JSON Schema: This defines how the data should be structured, with specific fields like "order_no", "item_no", "purchased_quantity", and "purchased_date".

3. JavaScript Function:

- It splits each line into individual values.
- It creates an object with the relevant fields ("order_no", "item_no", etc.).
- It then converts this object into a JSON string format.

The extra text at the end of each line (like "sddsd") is ignored. Only the first four values are used to match the schema.

## Steps

1. Create the bucket in GCS, add unique name to bucket and create the folder "demo" under that upload files. (Sample data, JSON, Javascript)
2. open bigquery and create one dataset name as "demo1".
3. open dataflow and create the flow by slecting the template from dropdown "Text file on cloud stoarge to bigquery".
4. in required parameters in "JavaScript UDF path in Cloud Storage" text box paste the path of javascript file, which you can get from the cloud storage where you stoed that file (path will be named as "gsutil URI")
5. same for JSON file.
6. in "JavaScript UDF name" text box paste the name of function which you have defined in java script file. (in our case its "transform")
7. in "BigQuery output table" text box paste the dataset ID of dataset which you previously created in bigquery(demo1), from dataset info copy the dataset ID. and add ".table_name" by your choice. `ex. dataset_ID.table_name`
8. in "Cloud storage input path" past the CSV path from GCS.
9. in "temporary directory and location create the "temp1" & "temp2" for each of them in by copying GCS path"

    `ex.`
    1. gs://bucket_name/temp1
    2. gs://bucket_name/temp2
10. You know parameters under "Max workers and number of workers" you can choose 1.
11. click run job. it will take 4-5 min.

## Result

you can check out bigquery dataset which we created after job get succesfullly executed. there you will see one table is created and all the data of we provided will be in that table.


### Reference
https://youtu.be/km9ZR6gVYe0?feature=shared
