# Real-Time Data Streaming using Apache Nifi, AWS, Snowpipe, Stream & Task

### Architecture:
![architecture-diagram](https://github.com/Mina2kamel/Real-Time-Data-Streaming-using-Apache-Nifi-AWS-Snowpipe-Stream-Task./blob/main/architecture.jpg)

## Project Description:
- **Docker Compose Deployment:**
  - Utilizes Docker Compose to pull and run three images: JupyterLab, Zookeeper, and Apache NiFi.
  - These services are organized and run together on an AWS EC2 instance, acting as a virtual machine to host these applications.
  
- **Data Generation:**
  - Python scripts in Jupyter Notebook are used to generate synthetic customer data.
  - The generated data is saved in CSV format within the JupyterLab environment.
    
- **Apache NiFi:**
  - Acts as a data integration tool for designing, controlling, and monitoring data flows.
  - A process group in NiFi contains multiple processors connected to allow flow files to move from one to another:
    
      - ListFile Processor: Lists the synthetic data files from the JupyterLab folder.
      - FetchFile Processor: Fetches the data from the listed files.
      - PutS3Object Processor: Stores the fetched files into an S3 bucket's "stream" folder.

- **Snowpipe:**
  - A cloud-based service provided by Snowflake DB for real-time data ingestion.
  - Once a customer data file is created in the S3 bucket, Snowpipe automatically retrieves it and stores it in a staging table in Snowflake DB.
  
- **Change Data Capture (CDC) Implementation:**
  - **Snowflake Stream:** Tracks DML changes (insert, update, delete) on the staging table to implement CDC.
  - **Snowflake Task:** Schedules SQL queries to manage customer data. It creates two types of tables:
    
      - ***SCD1 Table:*** Contains the current state of customer data (slowly changing dimension type 1).
      - ***SCD2 Table:*** Maintains historical customer data (slowly changing dimension type 2).

- This setup ensures efficient data generation, processing, and real-time ingestion into Snowflake DB, supporting both current and historical data analysis.
