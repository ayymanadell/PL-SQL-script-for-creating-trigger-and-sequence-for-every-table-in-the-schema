# PL/SQL Trigger and Sequence Generator for Schema Tables
## Overview
This PL/SQL script automates the process of creating triggers and sequences for every table within a specified schema in the database. The script generates unique sequences and triggers for each table, which can be used to manage automatic primary key generation in the absence of an auto-increment feature in the database.

## Prerequisites
Oracle Database: This script is designed to work with Oracle Database.

## How to Use
Open the script file (create_trigger_and_sequence_for_tables.sql) in an SQL editor or an Oracle database management tool.

## Modify the following parameters as needed to suit your schema and requirements:

1- SCHEMA_NAME: Replace with the name of the target schema where you want to create the triggers and sequences.

2- SEQUENCE_PREFIX: Replace with a prefix for the sequence names (optional).

3- TRIGGER_PREFIX: Replace with a prefix for the trigger names (optional).

4- Execute the script in the target schema by running the entire script. The script will automatically create a sequence and a trigger for each table within the schema.

- The script generates sequences and triggers with names based on the table names. For example, if the table name is CUSTOMER, the sequence and trigger names will be like SEQ_CUSTOMER_ID and TRG_CUSTOMER_ID, respectively.

## How It Works
1- The script retrieves a list of all tables within the specified schema using the Oracle Data Dictionary views.

2- For each table, it generates a unique sequence name based on the SEQUENCE_PREFIX (if provided) and the table name.

3- It creates the sequence with specified properties such as the starting value, increment, and maximum value.

4- The script then generates a trigger for each table. The trigger automatically populates the primary key column with the next value from the corresponding sequence before an INSERT operation.

5- Once all sequences and triggers are generated, they are ready to be used for automatic primary key generation.

## Important Notes
- Ensure that you have appropriate privileges to create sequences and triggers within the specified schema.

- Make sure to review the script before execution and backup your database to avoid any unintended consequences.

- If there are tables with existing primary keys, the script will not generate triggers and sequences for those tables.

- It is recommended to customize the script according to your specific requirements, such as setting the appropriate starting values and increment values for the sequences.
