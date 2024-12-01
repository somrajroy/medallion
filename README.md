# What is a Medallion Architecture
[A Medallion architecture, coined by Databricks, is a data design pattern](https://www.databricks.com/glossary/medallion-architecture) used to logically organize data in a lakehouse, with the goal of incrementally and progressively improving the structure and quality of data as it flows through each layer of the architecture. The layers are typically referred to as Bronze, Silver, and Gold.This architecture consists of three distinct layers – bronze (raw), silver (validated) and gold (enriched) – each representing progressively higher levels of quality. <br/>
The Medallion architecture is a "tier"-based architecture that consists of three main layers: Bronze, Silver, and Gold. The bronze layer contains unvalidated data, and data is ingested incrementally.<br/>
[Databricks recommends taking a multi-layered approach](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion) to building a single source of truth for enterprise data products (medallion lakehouse architecture). This architecture guarantees atomicity, consistency, isolation, and durability as data passes through multiple layers of validations and transformations before being stored in a layout optimized for efficient analytics. The terms bronze (raw), silver (validated), and gold (enriched) describe the quality of the data in each of these layers. <br/>
![image](https://github.com/user-attachments/assets/e008ef92-d3d0-4fa3-94ea-4d3cf784db29) <br/>

Data flows through the medallion architecture in a linear fashion, from bronze to silver to gold. At each layer, the data is processed and transformed to improve its quality and usability. The bronze layer is the simplest layer in the medallion architecture. It is simply a storage layer for raw data. The data in the bronze layer is typically stored in a data lake, such as Azure ADLS Gen-2 or Amazon S3. <br/>
[Microsoft Azure has various data storage models](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/data-store-overview?source=recommendations) and it is essential to have a high level overview of the same to implement polyglot persistence.<br/>
![image](https://github.com/user-attachments/assets/dfd7e8e5-edac-4321-80e3-f0de5652f666) <br/><br/>
# Data Lakehouse and Data Lakehouse Architecture
The [data lakehouse is an emerging data management](https://www.databricks.com/glossary/data-lakehouse) architecture that combines the benefits of data lakes and data warehouses (scalability and flexibility of data lakes with the reliability and performance of data warehouses). It provides a unified platform for managing structured, semi-structured, and unstructured data, enabling organizations to perform both real-time analytics and traditional business intelligence on a single system.  It supports interoperability between data lake formats. It supports ACID transactions and can run fast queries, typically through SQL commands, directly on object storage in the cloud or on-prem on structured and unstructured data. [This hybrid approach supports advanced analytics](https://cloud.google.com/discover/what-is-a-data-lakehouse), machine learning, and real-time data processing, while reducing complexity and lowering costs. [Key components of a data lakehouse include robust data governance](https://www.montecarlodata.com/blog-data-lakehouse-architecture-5-layers/), ACID transactions, and support for diverse data formats. 
# Understanding layers in Medallion Architecture
Below are the standard definitions in a medallion architecture although elsewhere in this blog I primarily focus on Bronze, Silver and Gold layer only.<br/>
* `Bronze Layer (Raw Data)` : Contains raw, unprocessed data that serves as the foundation for further data transformations.<br/>
   * Purpose : Ingest raw data from various sources with minimal/zero transformation.<br/>
   * `Performance Tier`: Standard. Data is typically accessed infrequently.<br/>
   * `Storage Tier` : Cool, as data access frequency is low and the retention is primarily for compliance/audits. Enable versioning only if regulatory requirements demand it. Implement lifecycle policies to transition older data to Archive. Store data in its native format (JSON, CSV, etc.).Apply compression where possible.<br/>
   * `Lifecycle Management`: Implement lifecycle management policies to move data to cooler storage tiers automatically.<br/>
   * `Data Compression`: Enable data compression to reduce storage costs.<br/>
* `Silver Layer(Cleansed, validated & Enriched data)` : Contains cleansed, validated, and enriched data. Data in this layer undergoes transformations to correct errors and improve quality.<br/>
  * `Performance Tier`: Standard unless pipelines require high IOPS <br/>
  * `Storage Tier`: Hot, as this data is accessed for regular processing and analytics (Hot for frequently accessed data and Cool for less frequently accessed data). Partition data based on query patterns (e.g., by date or category).Use Parquet format for optimized storage and analytics.<br/>
  * `Cost Optimization` : Optimize read performance to reduce compute costs.Avoid keeping intermediate datasets longer than necessary by implementing cleanup jobs.<br/>
  *  Leverage Azure Data Factory (ADF) or Databricks with Delta Lake for efficient processing, reducing unnecessary reads. <br/>
  * `Resource Scheduling`: Schedule data transformation jobs during off-peak hours to reduce compute costs.<br/>
  * Avoid duplication of datasets by establishing clear data lineage. <br/>
  * `Optimize Compute Resources`: Use Azure Spot VMs for non-critical workloads to save costs.<br/>
* `Gold Layer (Curated/Business-Level Data)`: Refined data for analysis and reporting.<br/>
  * `Purpose`: Provide refined data for reporting and analytics. <br/>
  * `Performance Tier`: Premium, especially for business-critical workloads requiring high throughput and low latency. Else standard can suffice.<br/>
  * `Storage Tier`: Primarily Hot, as this data is accessed frequently for reports and dashboards.<br/>
  * `Best Practices`: Store data in analytics-ready formats (Parquet, Delta). Leverage snapshots/versioning for point-in-time recovery.Retain only the most relevant datasets in Hot tier and move older versions to Cool/Archive tiers.Ensure proper indexing to minimize query runtime costs. <br/>
  * `Reserved Instances`: Purchase reserved instances for Synapse Analytics to get discounts. Use Databricks Reserved Capacity to reduce compute costs for Gold Layer workloads if customer have consistent usage. <br/>
  * `Compute Time` : Schedule ADF pipelines efficiently to avoid idle compute times. <br/>
  *  Archive historical data periodically into Cool or Archive tiers. <br/>
  *  Maintain a clear SLA for data freshness and retention policies to avoid over-provisioning.<br/>
  * `Query Optimization`: Optimize queries to reduce compute resource usage.<br/>
* `Staging Layer`: Temporary storage for intermediate processing.<br/>
  * `Performance Tier`: Standard. <br/>
  * `Storage Tier`: Hot, given frequent and short-term access. <br/>
  * `Cost Optimization` : Implement auto-delete policies to remove temporary data after a set period. Use Hot tier only when processing is active and transition to Cool after completion. <br/>
  * `Optimize Compute Resources`: Use Azure Spot VMs for non-critical workloads to save costs.<br/>
  *  Use ephemeral storage patterns for highly transient data. <br/>
  *  `Archive Layer`: Long-term storage for historical & compliance data .<br/>
  *  Performance Tier: Standard. <br/>
  *  Storage Tier: Always Archive <br/>
  *  Set up Blob Tiering Policies to move unused data from Cool or Hot tiers to Archive automatically. <br/>
  *  Store compressed, immutable data to save space and ensure compliance. <br/>
  *  Leverage Blob-level Tiering for granular control over lifecycle transitions. <br/>
* `Sandbox/Experimentation Layer` : Stores isolated data for data scientists and analysts to experiment with. <br/>
  * Performance Tier: Standard, unless real-time analysis is required. <br/>
  * Storage Tier: Primarily Hot, as access patterns are frequent/unpredictable. <br/>
  * `Cost Optimization` : Restrict sandbox size and enforce quotas using Azure Storage Policies.Archive unused experiments after X days automatically.Use tagging for cost attribution and ensure unused resources are cleaned up. <br/>
  Below is the typical recommended Configurations summary table. By following this and the guidelines in this blog, you can implement a cost-effective and high-performing medallion architecture in ADLS Gen2 for your customers. <br/>
![image](https://github.com/user-attachments/assets/6fe072d3-b7f9-4d8e-83c5-3d26f551bc85) <br/>


# Setting up the medallion architecture medallion storage accounts in Microsoft Azure
The below YouTube Video demostrates setting up the [medallion architecture storage account ADLS Gen-2.](https://www.databricks.com/product/data-lake-on-azure). <br/>Details of the [3 layers of Medallion archcitecture can be found in this link.](https://erstudio.com/blog/understanding-the-three-layers-of-medallion-architecture/?form=MG0AV3)<br/><br/>
[Setting Up a ADLS Gen-2 medallion Storage account](https://www.youtube.com/watch?v=divjURi-low&t=302s)<br/>
[Creating a Lakehouse on AWS](https://www.youtube.com/watch?v=vhCPgG2lz-0)<br/>
[Back to Basics: Building an Efficient Data Lake](https://www.youtube.com/watch?v=6l8HqX-P_9o)<br/>
[Build a Lake House Architecture on AWS](https://aws.amazon.com/blogs/big-data/build-a-lake-house-architecture-on-aws/)<br/>
# Good practices for setting up cost-effective and high-performance storage in Microsoft Azure.
* [Recommendations for optimizing data costs.](https://learn.microsoft.com/en-us/azure/well-architected/cost-optimization/optimize-data-costs)<br/>
* [Medallion Architecture in Synapse & strategies](https://learn.microsoft.com/en-us/answers/questions/2034379/medallion-architecture)<br/>
* `Data Tiering and Access Tiers` : [Organize data into different access tiers (Hot, Cool, Archive) based on access frequency and performance requirements](https://learn.microsoft.com/en-us/azure/well-architected/service-guides/storage-accounts/cost-optimization). This allows to store data in the most cost-effective tier while maintaining performance for frequently accessed data.<br/>
* `Implement Data Lifecycle Management` : [Implement policies to automatically tier data between tiers based on age and access patterns.](https://learn.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview) based on age and access patterns. The transition between tiers (Hot → Cool → Archive) should be automatic based on access patterns. Retire unused datasets using rules in Lifecycle Management.<br/>
* `Evaluate premium performance tiers within ADLS Gen2` :Ensure the [use of premium tiers is justified by performance needs](https://learn.microsoft.com/en-us/azure/virtual-machines/premium-storage-performance), as they come with higher costs compared to the standard tier. [Mission-critial worklods](https://learn.microsoft.com/en-us/azure/well-architected/mission-critical/) typically would need premium tier. However some business-critical workloads needs KPI's of mission-critical systems. Use Standard tier for most other workloads to reduce costs. Use Premium tier only for high-performance workloads that require low latency and high throughput.<br/>
* `Leverage Azure Storage Reserved Capacity` : By committing to a specific amount of storage over a period ( 1 or 3 years), [customers can benefit from significant cost savings compared to pay-as-you-go rates](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-reserved-capacity). <br/>
* `Automate data pipelines`: Utilize tools like Azure Data Factory to [automate the scheduling and orchestration of data pipelines](https://dzone.com/articles/medallion-architecture-efficient-batch-and-stream).<br/>
* `Data Segmentation and Categorization` : [Leverage data segmentation and categorization](https://learn.microsoft.com/en-us/azure/well-architected/cost-optimization/optimize-data-costs) based on type, usage patters and importance. <br/>
* `Tagging and Governance` : Apply Azure Resource Tags to track costs by layer, team, or purpose. <br/>
* `Azure Cost Management Tools` : Leverage Azure Cost Management and Billing to analyze cost drivers across layers. <br/>
* `File Format Optimization` : [Different file formats (like Avro, Parquet, ORC) can be chosen](https://learn.microsoft.com/en-us/azure/well-architected/cost-optimization/optimize-data-costs) based on I/O patterns and query requirements to optimize performance and storage efficiency. <br/>
* `Data Compression` : Leverage data compression to reduce storage costs and improve data transfer efficiency. Formats like Parquet and ORC provide built-in compression that maintains query performance while reducing space.<br/>
* `Networking` : Leverage private endpoints to avoid unnecessary data egress costs. <br/>
* `Optimize Redundancy` : Use Locally Redundant Storage (LRS) for non-critical data to save costs, and Geo-Redundant Storage (GRS) only for critical data. <br/>
* `Monitoring and Alerts` : Use Azure Monitor to track storage usage and set alerts for abnormal activity. <br/>
* `Efficient Data Ingestion and Processing` : Optimize data pipelines to minimize processing time and resource usage. Use serverless compute options like Azure Databricks or Azure Synapse Analytics. <br/>
* `Performance vs. Cost Trade-offs` : Balance performance requirements with cost optimization strategies. Customers need to decide when to prioritize one over the other. Balancing performance requirements with cost optimization strategies is crucial in modern data & cloud architectures. Here are some points for consideration : <br/>
    * `Prioritize Performance` : For mission-critical workloads or critical business-critical applications or high-frequency transactions, prioritize performance over cost.Use premium storage-tiers, performance-tiers or dedicated resources to ensure fast data access and processing. <br/>
    * `Optimize Costs` : For less demanding workloads or batch processing jobs, focus on cost optimization. Consider using cheaper storage tiers or shared resources for non-real-time operations. <br/>
    * `Cost-aware design` : Design data pipelines and architectures with cost-efficiency in mind. Choose appropriate data types, compression methods, and storage formats to minimize costs. <br/>
    * `Consider Long-Term Impacts` : Evaluate long-term cost implications of performance choices. Balance immediate needs with future scalability and cost projections. <br/>
# Appendix
Below are some additional resources and references for further learning: <br/>
1. [Medallion Architecture](https://dataengineering.wiki/Concepts/Medallion+Architecture#:~:text=A%20medallion%20architecture%20is%20a,it%20flows%20through%20various%20layers.)<br/>
2. [Behind the Hype - The Medallion Architecture Doesn't Work](https://www.youtube.com/watch?v=fz4tax6nKZM&t=1s) <br/>
3. [What is a Delta Lake](https://learn.microsoft.com/en-us/azure/databricks/delta/)<br/>
4. [Building the Medallion Architecture with Delta Lake](https://delta.io/blog/delta-lake-medallion-architecture/)
5. [What is a data lakehouse?](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/)<br/>
6. [Building the Lakehouse - Implementing a Data Lake Strategy with Azure Synapse](https://techcommunity.microsoft.com/t5/azure-synapse-analytics-blog/building-the-lakehouse-implementing-a-data-lake-strategy-with/ba-p/3612291)<br/>
7. [What Is a Lakehouse?](https://www.databricks.com/blog/2020/01/30/what-is-a-data-lakehouse.html)<br/>
8. [Data Engineering on Microsoft Azure](https://www.youtube.com/watch?v=HPYUuBuq1Ns&list=PLuQSde7Xvu7DCRenR1otgxAplTtnzKO9e)</br>
9. [What is Databricks dbt](https://docs.databricks.com/en/partners/prep/dbt.html) and [dbt Installation in Microsoft Azure](https://learn.microsoft.com/en-us/azure/databricks/partners/prep/dbt)<br/>
10. [Azure Storage Cost Optimization Strategies](https://www.lucidity.cloud/blog/azure-storage-cost-optimization)<br/>
11. [Building a Robust Data Lakehouse with Medallion Architecture](https://dataplatforms.ca/building-a-robust-data-lakehouse-with-medallion-architecture/)<br/>
12. [Recommendations for creating a culture of financial responsibility](https://learn.microsoft.com/en-us/azure/well-architected/cost-optimization/create-culture-financial-responsibility?source=recommendations)<br/>
14. [Medallion Architecture 101—Inside Bronze, Silver & Gold Layers](https://www.chaosgenius.io/blog/medallion-architecture/#1-more-storage-required)<br/>
14. [What is a datalakehouse](https://www.ibm.com/topics/data-lakehouse)<br/>
15. [Introduction to the well-architected data lakehouse](https://learn.microsoft.com/en-us/azure/databricks/lakehouse-architecture/)<br/>
16. [What is a Delta Lake ?](https://www.youtube.com/watch?v=W1wwDTj2NJE)<br/>
17. [What is a datalakehouse and how does it work ?](https://hudi.apache.org/blog/2024/07/11/what-is-a-data-lakehouse/)<br/>
18. [Exercise: Implement the Medallion Architecture using Azure Databricks (Bronze, Silver and Gold layers)](https://microsoft.github.io/TechExcel-Fabric-with-Databricks-for-Data-Analytics/docs/Ex_04/Ex04_implement_medallion_architecture.html#exercise-05-implement-the-medallion-architecture-using-azure-databricks-bronze-silver-and-gold-layers)<br/>
