Real time data analytics with Kinesis and Dynamodb


This real time streaming pipeline aims to capture data changes in Dynamodb using Kinesis Data
Streams,Kinesis Firehose,Glue, Lambda,S3 and Eventbridge. Finally, Athena is used to run
queries for data analysis.
Architecture Diagram

<img width="1098" height="511" alt="image" src="https://github.com/user-attachments/assets/298c2671-e29c-411b-a90b-a0e6f7b2ed43" />

Step 1: Create records in Dynamodb using Python script.
Python with boto3 pushes data to dynamodb.
<img width="975" height="428" alt="image" src="https://github.com/user-attachments/assets/438b4bae-8c42-4e2d-9ec1-22f27d771ffa" />

Step 2: Deliver stream of data to Kinesis Data Stream
The stream is then delivered by EventBridge pipe to Kinesis Data stream. Setup an IAM role for
eventbridge with access to DynamoDB and Kinesis.

<img width="975" height="394" alt="image" src="https://github.com/user-attachments/assets/a223f09a-a269-4333-8824-938d456a3b07" />
<img width="975" height="329" alt="image" src="https://github.com/user-attachments/assets/c7e306dc-8901-4ec0-b2b9-e8c3760a6ffd" />
<img width="975" height="415" alt="image" src="https://github.com/user-attachments/assets/ee5bd461-3e45-4a78-a8e5-e97e9feebcdf" />
<img width="975" height="470" alt="image" src="https://github.com/user-attachments/assets/663f7b92-9742-4de0-890b-e1f07a97e18b" />




Step 3: Create a Kinesis data stream to view the change data capture performed in DynamoDB
Choose the shard and accurate timestamp to view streamed data. Event name “Modify” implies
the change made in data .
<img width="975" height="416" alt="image" src="https://github.com/user-attachments/assets/45ad853a-81db-4b35-acc8-d912d1a3d4e2" />

Change data Capture
<img width="975" height="553" alt="image" src="https://github.com/user-attachments/assets/b032f9bf-856d-4a94-813e-c2ffe66da1db" />

Step 4: Create Kinesis Data Firehose
The purpose of kinesis firehose is to Batch data coming from kinesis stream and dump into
target. Here source is kinesis data stream and target is S3.
<img width="975" height="287" alt="image" src="https://github.com/user-attachments/assets/cfcf6dd3-da38-422b-a36c-4da2558df3a1" />

Step 5: Transformation using Lambda
Lambda function is using to transform the data from source kinesis data stream and then push it
back to firehose. We use lambda transformation since the data coming from kinesis data
stream is in json format and we want to push it to S3 in tabular format to write athena queries.
All kind of transformations can be done using lambda
Lambda code selects order id and other values from New image of kinesis data stream creates a
new json and then pushes that back firehose which then dumps to S3. IAM roles for lambda
function
IAM role for kinesis firehose
Step 6: Target S3 bucket is created where data gets dumped in real time
S3 bucket contents in near real time
Step 7: Create a Glue crawler and catalog to crawl the contents in S3
Glue crawler to crawl data in S3 so that its ready for Athena querying
Custom classifier for Glue crawler
Glue table created
Step 8: Open Athena query editor to run queries against glue catalog table.
This data is used for analysis
Athena query
