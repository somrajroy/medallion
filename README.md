# What is a Medallion Architecture
[A Medallion architecture, coined by Databricks, is a data design pattern](https://www.databricks.com/glossary/medallion-architecture) used to logically organize data in a lakehouse, with the goal of incrementally and progressively improving the structure and quality of data as it flows through each layer of the architecture. The layers are typically referred to as Bronze, Silver, and Gold.This architecture consists of three distinct layers – bronze (raw), silver (validated) and gold (enriched) – each representing progressively higher levels of quality. <br/>
[Databricks recommends taking a multi-layered approach](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion#bronze) to building a single source of truth for enterprise data products. This architecture guarantees atomicity, consistency, isolation, and durability as data passes through multiple layers of validations and transformations before being stored in a layout optimized for efficient analytics. The terms bronze (raw), silver (validated), and gold (enriched) describe the quality of the data in each of these layers. <br/>
Data flows through the medallion architecture in a linear fashion, from bronze to silver to gold. At each layer, the data is processed and transformed to improve its quality and usability. The bronze layer is the simplest layer in the medallion architecture. It is simply a storage layer for raw data. The data in the bronze layer is typically stored in a data lake, such as Azure ADLS Gen-2 or Amazon S3. <br/>
![image](https://github.com/user-attachments/assets/dfd7e8e5-edac-4321-80e3-f0de5652f666) <br/>
# Setting up the medallion architecture storage account in Microsoft Azure
The below YouTube Video demostrates setting up the medallion architecture storage account ADLS Gen-2. <br/>
[Setting Up a ADLS Gen-2 medallion Storage account](https://www.youtube.com/watch?v=divjURi-low&t=302s)<br/>
# Appendix
Below are some additional resources and references for further learning: <br/>
1. [Medallion Architecture](https://dataengineering.wiki/Concepts/Medallion+Architecture#:~:text=A%20medallion%20architecture%20is%20a,it%20flows%20through%20various%20layers.)<br/>
2. [Behind the Hype - The Medallion Architecture Doesn't Work](https://www.youtube.com/watch?v=fz4tax6nKZM&t=1s) <br/>
