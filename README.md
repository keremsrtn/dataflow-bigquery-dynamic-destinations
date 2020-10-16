# Dataflow BigQuery Dynamic Destinations

Occasionally you may have data of different domains being ingested into a single topic which 
requires dynamically routing to the proper output table in BigQuery. One way to accomplish this is 
to use a message attribute on the header of the Pub/Sub message to indicate the table which the 
message should be routed to. The routing of the messages to the correct table can be accomplished in 
a single pipeline by using the `BigQueryIO` transform's dynamic destination capabilities. In this 
pipeline, a `SerializableFunction` is used to extract the table attribute from the Pub/Sub message 
and then subsequently route to the proper table destination. 


## Pipeline

[PubsubToBigQueryDynamicDestinations](src/main/java/com/google/cloud/pso/pipeline/PubsubToBigQueryDynamicDestinations.java) -
A pipeline which consumes JSON messages from Pub/Sub and outputs records to BigQuery tables using a
Pub/Sub message attribute to determine the proper table to route the message to.

## Getting Started

### Requirements

* Java 8
* Maven 3

### Building the Project

Build the entire project using the maven compile command.
```sh
mvn clean && mvn compile
```

Create the pipeline
```sh
mvn compile exec:java \
-Dexec.mainClass=com.google.cloud.pso.pipeline.PubsubToBigQueryDynamicDestinations \
-Dexec.cleanupDaemonThreads=false \
-Dexec.args=" \
--project=codeway-data-platform \
--stagingLocation=gs://cdwdataflow_test/staging2 \
--tempLocation=gs://cdwdataflow_test/temp \
--runner=DataflowRunner \
--subscription=projects/codeway-data-platform/subscriptions/dataflow_template_test \
--tableNameAttr=app_id \
--outputTableProject=codeway-data-platform \
--outputTableDataset=events"
```
