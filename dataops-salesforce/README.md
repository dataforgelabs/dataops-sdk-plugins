# **DataOps Salesforce Plugin**
This code allows users to extract data from Salesforce using the DataOps Custom Ingest SDK and the [spark-salesforce](https://github.com/springml/spark-salesforce) library.

It also includes a build.sbt file for opening and editing in IntelliJ. You can clone this repository, then open the dataops-salesforce folder in IntelliJ using [this guide](https://www.jetbrains.com/help/idea/sbt-support.html) to import the sbt project and allow for easy editing/modification if needed.

For those who just want to use this as-is, here are the setup steps:
1. Navigate to the src->main->scala->notebook folder and copy the code within the dataops-salesforce.scala file
2. Paste this code into a notebook within your DataOps Databricks environment
3. Follow this guide to ensure you have the following dependencies installed on the cluster that will execute this code: https://docs.databricks.com/libraries/index.html
    - [spark-salesforce](https://github.com/springml/spark-salesforce) - NOTE! You MUST use version 1.1.4+ targeting scala 2.12 and spark 3.0+
    - DataOps SDK - Avaialble in the DataOps DataLake bucket/container within your environment
4. Configure a custom connection to include your username and password (including token from salesforce) under private connection parameters 
    - To configure your token, follow this guide from [salesforce](https://help.salesforce.com/articleView?id=sf.user_security_token.htm&type=5)
    - Example: `{"password":"mypasswordmyusersecuritytoken","username":"myusername@awesomedomain.com"}`
5. Create a Source using the created custom Connection and specifying a custom Cluster Type, and initiation type of Notebook, and the Notebook Path equal to the path of the newly created notebook from step 2.
    - Example Custom Cluster Type for a single node cluster:
    
        `
      {
      "libraries":[
      {
      "jar":"s3://reportingprod-datalake-wmp/spark-salesforce-assembly-1.1.3.jar"
      },
      {
      "jar":"s3://reportingprod-datalake-wmp/dataops-sdk.jar"
      },
      {
      "jar":"s3://reportingprod-datalake-wmp/sparky.jar"
      }
      ],
      "new_cluster":{
      "spark_conf":{
      "spark.master":"local[*]",
      "spark.databricks.cluster.profile":"singleNode",
      "spark.databricks.service.server.enabled":"true"
      },
      "custom_tags":{
      "ResourceClass":"SingleNode"
      },
      "num_workers":0,
      "spark_version":"7.2.x-scala2.12",
      "aws_attributes":{
      "instance_profile_arn":"arn:aws:iam::999999999999:instance-profile/myenvironment-db-instance-profile-WMP"
      },
      "instance_pool_id":"9999-9999-nervy29-pool-AAAAAAA"
      }
        `
      
6. Navigate down to the paramteres section in the source settings and enter the Custom Parameters aligned witht eh object you want to extract from Salesforce
    - Example: `{"sfObject":"Account"}`
    
7. Pull now and debug any issues

Please contact your project team for managed service support provider for any issues or assistance debugging the setup or ongoing execution of this custom ingestion codebase