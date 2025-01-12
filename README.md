# Cloud-Based-ETL-Pipeline-for-Financial-Analysis

## Project Overview

This project demonstrates the creation of a cloud-based ETL (Extract, Transform, Load) pipeline using Talend Open Studio for Data Integration, Azure SQL Database, and Snowflake. The pipeline extracts financial transaction data, filters and transforms it, and loads the cleaned data into Snowflake for analysis.

### Key Features

Data Extraction: Extracts transaction data from Azure SQL Database.

Data Transformation: Filters completed transactions and ensures accurate data type formatting.

Data Loading: Loads transformed data into a Snowflake table.

Automation and Monitoring: Automated job scheduling with monitoring and error handling.

### Technologies Used

ETL Tool: Talend Open Studio

Data Sources: Azure SQL Database, Azure Blob Storage

Data Warehouse: Snowflake

Cloud Services: Azure SQL, Azure Monitor

Programming: SQL, JavaScript (for Snowflake procedures)

### Data Flow Pipeline

[tAzureSqlInput] --> [tMap] --> [tFilterRow] --> [tSnowflakeOutput]

Extract: Extracts transaction data from Azure SQL using tAzureSqlInput.

Transform: Filters and formats data using tMap and tFilterRow.

Load: Inserts transformed data into Snowflake using tSnowflakeOutput.

### Snowflake Table Schema

CREATE OR REPLACE TABLE transaction_temp (
  Transaction_ID STRING,
  Customer_ID STRING,
  Transaction_Date DATE,
  Amount FLOAT,
  Currency STRING
);

### ETL Procedure

CREATE OR REPLACE PROCEDURE Transaction_Data()
RETURNS STRING NOT NULL
LANGUAGE JAVASCRIPT
AS
$$
    var drop_cmd = `
        DROP TABLE IF EXISTS transaction_temp;
    `;

    var sql_drop = snowflake.createStatement({sqlText: drop_cmd});
    sql_drop.execute();

    var select_cmd = `
        CREATE TABLE transaction_temp AS
        SELECT Customer_ID, Transaction_ID, Transaction_Date, Amount, Currency
        FROM P4_TRANSACTION
        WHERE Transaction_Status = 'completed';
    `;

    var sql_select_stream = snowflake.createStatement({sqlText: select_cmd});
    sql_select_stream.execute();

    return 'Procedure executed successfully';
$$;

### Setup Instructions

1. Clone the Repository

git clone <repository-url>

2. Configure Talend Job

Import the Talend job design into Talend Open Studio.

Configure Azure SQL Database connection parameters.

Set up Snowflake credentials.

3. Run the ETL Pipeline

Execute the Talend job to extract, transform, and load data.

4. Validate Data in Snowflake

Verify the data in the transaction_temp table using SQL queries.

### Automation and Monitoring

Use Azure Data Factory or Talend Scheduler for job automation.

Monitor job performance with Azure Monitor.

### Testing and Validation

Validate that only completed transactions are loaded.

Ensure data type integrity and accurate mapping.

### Potential Enhancements

Implement additional data validation steps.

Expand transformation logic to include currency conversion.

Integrate with visualization tools for dashboard reporting.

Contributing

Feel free to submit issues or pull requests for enhancements and bug fixes.

License

This project is licensed under the MIT License.

