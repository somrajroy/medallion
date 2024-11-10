# What is a Medallion Architecture
[A Medallion architecture, coined by Databricks, is a data design pattern](https://www.databricks.com/glossary/medallion-architecture) used to logically organize data in a lakehouse, with the goal of incrementally and progressively improving the structure and quality of data as it flows through each layer of the architecture. The layers are typically referred to as Bronze, Silver, and Gold.This architecture consists of three distinct layers – bronze (raw), silver (validated) and gold (enriched) – each representing progressively higher levels of quality. <br/>
The Medallion architecture is a "tier"-based architecture that consists of three main layers: Bronze, Silver, and Gold. The bronze layer contains unvalidated data, and data is ingested incrementally.<br/>
[Databricks recommends taking a multi-layered approach](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion) to building a single source of truth for enterprise data products (medallion lakehouse architecture). This architecture guarantees atomicity, consistency, isolation, and durability as data passes through multiple layers of validations and transformations before being stored in a layout optimized for efficient analytics. The terms bronze (raw), silver (validated), and gold (enriched) describe the quality of the data in each of these layers. <br/>
Data flows through the medallion architecture in a linear fashion, from bronze to silver to gold. At each layer, the data is processed and transformed to improve its quality and usability. The bronze layer is the simplest layer in the medallion architecture. It is simply a storage layer for raw data. The data in the bronze layer is typically stored in a data lake, such as Azure ADLS Gen-2 or Amazon S3. <br/>
[Microsoft Azure has various data storage models](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/data-store-overview?source=recommendations) and it is essential to have a high level overview of the same to implement polyglot persistence.<br/>
![image](https://github.com/user-attachments/assets/dfd7e8e5-edac-4321-80e3-f0de5652f666) <br/><br/>
# Setting up the medallion architecture storage account in Microsoft Azure
The below YouTube Video demostrates setting up the [medallion architecture storage account ADLS Gen-2.](https://www.databricks.com/product/data-lake-on-azure). <br/>Details of the [3 layers of Medallion archcitecture can be found in this link.](https://erstudio.com/blog/understanding-the-three-layers-of-medallion-architecture/?form=MG0AV3)<br/><br/>
[Setting Up a ADLS Gen-2 medallion Storage account](https://www.youtube.com/watch?v=divjURi-low&t=302s)<br/>
# Good practices for setting up cost-effective and high-performance storage in Microsoft Azure.
* [Recommendations for optimizing data costs.](https://learn.microsoft.com/en-us/azure/well-architected/cost-optimization/optimize-data-costs)<br/>
* [Medallion Architecture in Synapse & strategies](https://learn.microsoft.com/en-us/answers/questions/2034379/medallion-architecture)<br/>
* `Data Tiering and Access Tiers` : [Organize data into different access tiers (Hot, Cool, Archive) based on access frequency and performance requirements](https://learn.microsoft.com/en-us/azure/well-architected/service-guides/storage-accounts/cost-optimization). This allows to store data in the most cost-effective tier while maintaining performance for frequently accessed data.<br/>
* `Implement Data Lifecycle Management` : [Automatically transition data to less expensive tiers](https://learn.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview) based on age and access patterns.<br/>
* `Evaluate premium performance tiers within ADLS Gen2` :Ensure the [use of premium tiers is justified by performance needs](https://learn.microsoft.com/en-us/azure/virtual-machines/premium-storage-performance), as they come with higher costs compared to the standard tier. [Mission-critial worklods](https://learn.microsoft.com/en-us/azure/well-architected/mission-critical/) typically would need premium tier. However some business-critical workloads needs KPI's of mission-critical systems.<br/>
* `Leverage Azure Storage Reserved Capacity` : By committing to a specific amount of storage over a period ( 1 or 3 years), [customers can benefit from significant cost savings compared to pay-as-you-go rates](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-reserved-capacity). <br/>
* `Automate data pipelines`: Utilize tools like Azure Data Factory to [automate the scheduling and orchestration of data pipelines](https://dzone.com/articles/medallion-architecture-efficient-batch-and-stream).<br/>
* `Data Segmentation and Categorization` : [Leverage data segmentation and categorization](https://learn.microsoft.com/en-us/azure/well-architected/cost-optimization/optimize-data-costs) based on type, usage patters and importance. <br/>
* `File Format Optimization` : [Different file formats (like Avro, Parquet, ORC) can be chosen](https://learn.microsoft.com/en-us/azure/well-architected/cost-optimization/optimize-data-costs) based on I/O patterns and query requirements to optimize performance and storage efficiency. <br/>
* `Data Compression` : Leverage data compression to reduce storage costs and improve data transfer efficiency. Formats like Parquet and ORC provide built-in compression that maintains query performance while reducing space.<br/>
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
