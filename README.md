# What is a Medallion Architecture
[A Medallion architecture is a data design pattern](https://www.databricks.com/glossary/medallion-architecture) used to logically organize data in a lakehouse, with the goal of incrementally and progressively improving the structure and quality of data as it flows through each layer of the architecture. The layers are typically referred to as Bronze, Silver, and Gold. <br/>
[Databricks recommends taking a multi-layered approach](https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion#bronze) to building a single source of truth for enterprise data products. This architecture guarantees atomicity, consistency, isolation, and durability as data passes through multiple layers of validations and transformations before being stored in a layout optimized for efficient analytics. The terms bronze (raw), silver (validated), and gold (enriched) describe the quality of the data in each of these layers. <br/>
![image](https://github.com/user-attachments/assets/dfd7e8e5-edac-4321-80e3-f0de5652f666) <br/>

# Appendix
Below are some additional resources and references for further learning: <br/>
