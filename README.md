# What is a Medallion Architecture
[A Medallion architecture, coined by Databricks, is a data design pattern](https://www.databricks.com/glossary/medallion-architecture) used to logically organize data in a lakehouse, with the goal of incrementally and progressively improving the structure and quality of data as it flows through each layer of the architecture. The layers are typically referred to as Bronze, Silver, and Gold.This architecture consists of three distinct layers – bronze (raw), silver (validated) and gold (enriched) – each representing progressively higher levels of quality. <br/>
The Medallion architecture is a "tier"-based architecture that consists of three main layers: Bronze, Silver, and Gold. The bronze layer contains unvalidated data, and data is ingested incrementally.<br/>
[Databricks recommends taking a multi-layered approach](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion#bronze) to building a single source of truth for enterprise data products. This architecture guarantees atomicity, consistency, isolation, and durability as data passes through multiple layers of validations and transformations before being stored in a layout optimized for efficient analytics. The terms bronze (raw), silver (validated), and gold (enriched) describe the quality of the data in each of these layers. <br/>
Data flows through the medallion architecture in a linear fashion, from bronze to silver to gold. At each layer, the data is processed and transformed to improve its quality and usability. The bronze layer is the simplest layer in the medallion architecture. It is simply a storage layer for raw data. The data in the bronze layer is typically stored in a data lake, such as Azure ADLS Gen-2 or Amazon S3. <br/>
![image](https://github.com/user-attachments/assets/dfd7e8e5-edac-4321-80e3-f0de5652f666) <br/>
# Setting up the medallion architecture storage account in Microsoft Azure
The below YouTube Video demostrates setting up the [medallion architecture storage account ADLS Gen-2.](https://www.databricks.com/product/data-lake-on-azure) <br/><br/>
[Setting Up a ADLS Gen-2 medallion Storage account](https://www.youtube.com/watch?v=divjURi-low&t=302s)<br/>
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
