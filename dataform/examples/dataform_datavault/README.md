## Data Vault in BigQuery using Dataform
In this repository, we provide an example Dataform project that transforms data in BigQuery following Data Vault 2.0 modeling technique.
Data Vault is a recent modeling technique for data warehouses in addition to the traditional 3NF and Dimensional with the main goals of agility and auditability. 
Fundamentally, Data Vault moves relationships between the entities to separate tables so that entities can grow independently of each other. 
New relationships can be created or removed without any change in the existing tables.

Dataform is an application to manage data pipelines for data warehouses like BigQuery, Snowflake, and Redshift. 
It enables data teams to build scalable, tested, SQL based data transformation pipelines using version control and engineering led best practices. It compiles hundreds of data models in under a second using SQLX. SQLX extends your existing SQL warehouse dialect to add features that support dependency management, testing, documentation and more.

### Example DataVault implementation on BigQuery by using Dataform

Use Case: As an energy company would like to store, analyze and provide insights on data from meters and meter readings. 
We have stored source data in staging dataset about meter and meter readings. And, in "raw" dataset datavault tables(hubs, links & satellites) are created in BigQuery using dataform lib. 

The sqlx files for hubs, satellites & links are placed under [definitions](https://github.com/prabhaarya/bigquery-utils/tree/feature/dataform-datavault/dataform/examples/dataform_datavault/definitions/raw) folder and any functions used needs to placed under [includes](https://github.com/prabhaarya/bigquery-utils/tree/feature/dataform-datavault/dataform/examples/dataform_datavault/includes) folder. 

### Source table schemas:
Meter Staging table:

  ```shell
  CREATE OR REPLACE TABLE staging.meter_load
  (
  meter_id INTEGER,
  manufacturer STRING,
  service_name STRING,
  source STRING,
  loaddate DATE
  );
  ```

Meter Readings Staging table:

  ```shell
  CREATE OR REPLACE TABLE staging.meter_readings_load
  (
  readings_id INTEGER,
  meter_id INTEGER,
  readings FLOAT64,
  readings_time TIMESTAMP,
  source STRING,
  loaddate DATE
  );
  ```

### Source table exports:
1. meter_load --> Meter table contains data about meter.
   ![](screenshots/meter_load.png)

2. meter_readings_load --> Meter readings table contains data about meter readings.
   ![](screenshots/meter_readings_load.png)

## How to use?

### Pre-requsites:
1. npm lib needs be installed
2. [Google cloud project](https://developers.google.com/workspace/guides/create-project) should be available
3. BigQuery api is enabled
4. Access to create service account keys
5. [Dataform lib ](https://docs.dataform.co/dataform-cli) lib using NPM


### Steps to follow:
All the steps below can be used via terminal or Use the CloudShell editor.

In order to test and run the dataform, please run the following steps:

1. Initialize a Dataform project and then create a new Dataform google cloud project ([*optional if you already have one*])
    ```shell
    dataform init bigquery dataform-demo --default-database $(gcloud config get-value project) --include-schedules
    ```

2. Set up authentication to BigQuery
  - Go to https://console.cloud.google.com/iam-admin/serviceaccounts and
      create a service account. Give this account the role of BigQuery Admin (it helps Dataform can create new tables etc.). Then, download the JSON key to
      the project and upload the file to CloudShell. (optional if you already have a account with service account key uploaded into *optional if you already have a account with service account key uploaded into it*)
    - Dataform requires a credentials file in order to connect to your warehouse. 
      ```shell
      cd dataform-demo
      dataform init-creds bigquery
    - Provide the path to the JSON key file when asked. Then make sure to add
      the key file to .gitignore so that you don’t check it in by mistake.
      ```shell
      echo filename.json > .gitignore
      git add -f .gitignore
      ```
3. User has to replace project id, please add them into this [file](./dataform.json). 

4. Compile & Run
    ```shell
    dataform compile
    dataform run
    ```
  - The overview of project structure will look like the following image: 

    <img src="screenshots/datavault_tables.png" width=250>

  - The below video, shows all the steps visually:

    <image src="screenshots/dataform_demo_gif.gif" width=450>



## LICENSE
All solutions within this repository are provided under
Copyright 2022 Google LLC

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
