# Task Prerequisites 

- Kafka installed
- Power BI desktop installed
- Ensure all necessary dependencies are included in `pom.xml`

## Task Initialization

1. Start Kafka ZooKeeper service:
    ```bash
    bin/zookeeper-server-start.sh config/zookeeper.properties
    ```
2. Start Kafka server:
    ```bash
    bin/kafka-server-start.sh config/server.properties
    ```

## Java Project

1. Open the KafkaJob project.
2. Run the `Kafka_main` class in the default package:
    - Kafka producer and consumer will be started and run in parallel.

### ProducerRunnable Class

- Creates Kafka producer.
- Reads data from CSV file on the local machine.
- Sends each line to the Kafka broker.

### ConsumerRunnable Class

- Creates Kafka consumer.
- Subscribes to the broker and reads messages based on predefined topic.
- Sends message to `HBaseTableCreator.loadDataToHBase()` method.

### HBaseTableCreator.loadDataToHBase()

- Creates connection to HBase service.
- Splits message by delimiter and populates columns with the split text.
- Sends values into a table in HBase.

### Hive
- Create a Hive Table based on its data source "HBash Table"
- Build the jar and use command line "Spark Submit"
- ```bash
    spark-submit \
      --class com.example.className \
      --master local[*] \
      --conf spark.sql.catalogImplementation=hive \
      --conf spark.hadoop.hive.metastore.uris=thrift://127.0.0.1:9083 \
      target/filename.jar
    ```
---

## ODBC Configuration and Power BI Setup

### Step 1: Ensure Hive and ODBC Are Configured on the Cloudera VM

- Ensure Hive is running and accessible on your CentOS VM.
- Check that HiveServer2 is up and running, as ODBC connections typically interact with HiveServer2.

### Step 2: Install the Hive ODBC Driver on Your Local Machine

- Download and install the Hive ODBC driver on your local machine.
- You can download the Hive ODBC driver from the Cloudera website: [Cloudera ODBC Drivers](https://www.cloudera.com/downloads.html).

### Step 3: Configure the Hive ODBC Data Source on Your Local Machine

1. Open ODBC Data Source Administrator:
    - Select the **System DSN** tab (to make the DSN available to all users).
    - Add a new ODBC Data Source:
        - Click **Add** and choose the Cloudera Hive ODBC Driver from the list of drivers.
2. Configure the ODBC Data Source:
    - **Host**: IP address or hostname of your CentOS VM.
    - **Port**: The default port for HiveServer2 is 10000 unless you changed it.
    - **Database**: The name of the Hive database that contains the table.
    - **Hive Server Type**: Choose HiveServer2.
    - **Authentication**: Enter your Hive authentication details (username and password).
3. Test the Connection:
    - Click on the **Test** button to verify that the ODBC connection is working.
    - If it connects successfully, proceed.

### Step 4: Open Power BI Desktop

- Launch Power BI Desktop on your local machine.

### Step 5: Connect Power BI to the Hive ODBC Data Source

1. In Power BI, click on **Get Data** from the top menu.
2. In the **Get Data** window, select **ODBC** from the list of data sources, then click **Connect**.
3. Select the ODBC DSN that you configured for the Hive connection in the previous steps.
    - The ODBC DSN should appear in the dropdown list in the ODBC window.
    - Click **OK**.
4. If prompted, enter the Hive credentials (username and password), then click **Connect**.

### Step 6: Load Data from Hive

- After connecting, Power BI will display the list of tables from the Hive database.
- Select the Hive table from which you want to extract data.
- Click **Load** to import the data into Power BI.

### Step 7: Create Visualizations in Power BI

- Once the data from the Hive table is loaded, you can start building visualizations, reports, and dashboards in Power BI using the data extracted from Hive.
