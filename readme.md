# Synapse Demos

## Introduction

The code for the demos described below are included in this repository.

<h2>Demo Menu</h2>

<dl>
  <dt>
    Cold Path Analytical Pipeline with Azure Synapse Analytics
  </dt>
  <dd>
    Wide World Importer is a fictitious retail company building analytical platform with Azure Synapse Analytics. Their data is landed in ADLS Gen2 storage container for exploration and analysis purpose. In this demo, WWI will explore Synapse components to perform analysis on their sales data to gain insights.
  </dd>
  <dt>
    Delta lake Demo with Azure Synapse
  </dt>
  <dd>
    Delta Lake is an open-source storage layer that brings ACID (atomicity, consistency, isolation, and durability) transactions to Apache Spark and big data workloads
  </dd>
  <dt>
    IoT Anomaly Detection leveraging Azure Synapse Link for Azure Cosmos DB
  </dt>
  <dd>
    The hypothetical scenario is Power Plant where signals from steam turbines are being analyzed and Anomalous signals are detected. You will ingest streaming and batch IoT data into Azure Cosmos DB using Azure Synapse Spark, perform Joins and aggregations using Azure Synapse Link and perform anomaly detection using Azure Cognitive Services on Spark (MMLSpark).
  </dd>
   <dt>
     In-engine scoring with Synapse dedicated SQL pool
  </dt>
  <dd>
    End-to-end demo to showcase Machine Learning enabled in Synapse SQL. Predict NYC Taxi Trip amount using machine learning model in ONYX format in SQL pool.
  </dd>
   <dt>
     Streaming ingestion to Synapse dedicated SQL Pool
  </dt>
  <dd>
    Azure Stream Analytics High throughput ingestion to dedicated SQL pool
  </dd>
  </dl>



<li></li>
<li></li>
<li>Streaming ingestion to Synapse Dedicated Pool</li>
<li>Predict NYC Taxi Tips using Onyx and SQL Pool scoring</li>
<li>Predict NYC Taxi Tips using Spark ML and Azure Open Datasets</li>
<li>Loop over each sql pool and:
 <ul>
  <li>Check the state of the sql pool</li>
  <li>Depending upon its status, Initiate the Pause or Restart</li>
 </ul>
</li>
</ol>
 
This requires a simple pipeline in Synapse:
![](images/simplepipeline.jpg)

Depending upon the nature of your environment, the whole process described here may not apply, and you may just want to pick and choose the appropriate step. Typically the process described here would be used to Pause or Restart all instances in a Development, Test or PoC environment where the number of instances could vary over time; however, for a live environment you are more likely to schedule Pause/Restart on a instance by instance basis so will only need step 3.

All of the steps above utilize the REST APIs for Synapse and Azure SQL:

 https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-rest-api
 
 https://docs.microsoft.com/en-us/rest/api/sql/

so you are not restricted to using Synapse Pipelines, and you can execute these commands via the tools or application of your choice.

## Step 0: Parameter setup in your pipeline
The examples below are parameter driven, which will allow you to create a generic pipeline that you can use across multiple subscriptions, resource groups, SQL servers and/or Database instances (SQL pools). These are setup in your Synapse Pipeline under parameters:

![](images/PipelineParameters.jpg)

## Step 1: Identify the list of databases (SQL pools) in your SQL server instance
This requires a Web Activity that calls the Databases - List By Server REST API request:

![](images/WebActivityListSQLPools.jpg)
 
This is a simple Get request using the following call:

<pre><code>GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Synapse/workspaces/{server-name}/sqlPools?api-version=2019-06-01-preview
</code></pre>
