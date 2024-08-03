# WWide World Importer Analysis - Microsoft Fabric End-End Project- Shortcut, Dataflow, & Data Pipeline Ingestion.
This project outlines the end-to-end analysis of the wide world imports of various products to different countries. It encompasses the different aspects of Microsoft fabric including data engineering, data factory and data analytics. 

---


Interested in understanding some of the different methods of data ingestion in Microsoft Fabric? If yes, explore with me.
Wide-World Importer Microsoft Fabric End-End Project - Image by AuthorThis project outlines the end-to-end analysis of the wide world imports of various products to different countries. It encompasses the different aspects of Microsoft Fabric (a SAAS platform for data ingestion, ETL, business intelligence reporting, and real time analytics) encompassing data engineering, data factory and data analytics.
Four things were taken into consideration in this project which are;
1. Data Ingestion
2. Data Transformation
3. Data modelling, and
4. Data visualization
Purpose of the Project
To ingest data into a newly created Lakehouse (a storage that hold both structured and unstructured data) using various methods, transforming the data using dataflow Gen2 and finally creating a compelling story via data visualization using Microsoft Fabric Power BI.
Data Sources
Seven data tables were used for this project analysis;
1. FactOrders data - CSV file
2. OrderDetails data - CSV file
3. Shippers data -CSV file
4. Product data - XLSX file
5. Categories data -CSV file
6. Date table - CSV file
7. Customers table - XLSX file
A. Data Ingestion to Lakehouse
Data Engineering was switched to in Microsoft Fabric and a new Lakehouse named "FabricProjetLakehouse" was created into a folder created in a workspace. Four (4) different ways was used in getting the data to the created Lakehouse located in a workspace newly created on Microsoft fabric
Step 1: Shortcut from a warehouse –
This method avoids the duplication of tables. To achieve this, a new data warehouse was created named "FB_Warehouse" and dataflow Gen2 was used to ingest the "FactOrders" csv file into the warehouse using import from text/csv file button as seen below. The table was named "FB_FactOrders"
Image 1: Ingestion of FactOrders table into the warehouse using dataflow Gen2
After this has been done, in my new Lakehouse, I clicked on new shortcut to get the data from the warehouse to the Lakehouse, selected the internal sources since the warehouse is located inside Fabric. I navigated my way to the warehouse destination to select the FB_FactOrders for creation.
Image 1b: Ingestion of FactOrders using Shortcut from the warehouse to lakehouse -Image by AuthorStep 2: Ingestion using upload files (Shippers, categories and Date CSV files)
This method ensured all CSV dim files were uploaded from the local machine or rather can be uploaded on the OneDrive and selected from therein. However, note that it's only CSV files that can be uploaded into the Lakehouse.
Image 2a: Upload of CSV files data into Microsoft Fabric Lakehouse - Image by AuthorImage 2b: Files are loaded as Delta tables in the Table section of the Lakehouse- Image by AuthorAfter successful upload as files into the File section, they were loaded as delta tables into the Table section of the Lakehouse.
Step 3: Ingestion using data factory pipeline -
This method was used to get the OrderDetails table from SQL database to the Lakehouse.
Image 3a: Querying of the OrderDetails table on SSMS database before ingestion to Microsoft Fabric Lakehouse - Image by AuthorIn the Lakehouse, I created a new pipeline and get data using copy data. I selected SQL database, and a new connection setting was configured using server name, database name and password as seen below.
After this has been done, I exited and selected the copy data assistant in pipeline, I ensured there is no encryption of data selected in the edit button pop up of the settings as shown below.
Image 3b: Edit connection to remove encrypted connection - Image by AuthorI selected the manual entry of table name as written in my SQL database after which I previewed the data. In the destination settings, A new table name was created as WW1_OrderDetails In my Lakehouse and loaded which might take some minutes.
Image 3c: Edit the source settings and enter manually the table name and preview data Image 3d: Pipeline successfully created and run - Image by AuthorStep 4: Ingestion using dataflow Gen2 for all XLSX file (Products and Customers)
Since the xlsx file could not be uploaded and loaded directly to the Lakehouse, the get data option using dataflow Gen2 was used to ingest the customers and products table. This is also like the power query we have on Power BI desktop. I changed the data type of all columns after loading for transformation. I then ensured that the correct Lakehouse was selected as the destination of the tables. They were renamed as FB_Products and WW1_customers respectively.

---

Transformations in Data Flow Gen2
The FB_OrderDetails, FB_Product and FB_Categories were loaded to dataflow for normalization. This was necessary as the categories table could only reference the product table using productid but was showing no relationship with the OrderDetails table. In other word, the categories table could not be used to filter the OrderDetails . Hence, the need for the normalizations.
Steps taken
The FB_categories table was collapsed to the product table using merge queries option (Left join product to categories to add the categoryname, category description and picture url fields to the product table.

Image 4a: Merging Queries Transformation in Dataflow Gen2 (FB_Categories & FB_Product)2. Merged the new FB_Product to the FB_OrderDetails table using Left join on productid. The categoryID was returned as a new field in the FB_Orderdetails which was later reloaded to Lakehouse as Silver_FB_OrderDetails.
Image 4b: Merging Queries Transformation in Dataflow Gen2 (FB_OrderDetails & FB_Product)3. Ensure to select the data destination by selecting the Lakehouse and click on existing table if already created, else write a new table name.
Image 4c: Choose data destination to be loaded into either new/existing table in the LakehouseIn my case,  the table existed, replace existing table was selected and settings saved.
Image 4d: Update method either to append or replace data in the destination targetThe new tables named Silver_OrderDetail and Silver_FB_Product were loaded back to the Lakehouse
Image 4e: The new look of the FabricProjectLakehouse after all ingestion method has been usedImage 4f: Querying from the Lakehouse SQL endpoint - Image by AuthorYou can choose to switch to query to run some queries from the table as seen above before modeling.
Data Modeling
I switched to the SQL Analytics endpoint and clicked on the model tab where 5 relationships were built amongst the tables.
1. One to many from FB_shippers to FB_FactOrders on ShipperID key
2. One to many from FB_Date to FB_FactOrders on OrderDate key
3. One to many from FB_FactOrders to Silver_FB_OrderDetails on OrderID key
4. One to many from Silver_FB_product to Silver_FB_OrderDetails on ProductID key
5. One to many from FB_Customers to FB_FactOrders on customerID key
Image 5: Data modelling in Microsoft Fabric - Image by AuthorThe relationship between the tables maintains a snowflake schema structure. DAX (Data analysis expressions) Measures were created including Total sales, Total quantity, Total Orders, Average Turn around Time (TAT) for product delivery etc.
Image 6a: Measure creationEnsure that the measure button is not greyed out when trying to create one. If it is, then go to "manage default semantic model". All tables to be used in your reporting should be made active and can also select all tables if they are necessary - click okay. With this, new measure button shouldn't be grey out any longer.
Data Visualization
Power BI was switched to start the creation of report for insights. I clicked on "New Report" and started with getting to know the summary of the data. 
The below report was created and can also be interacted with using this link.
Image 7: Wide World Importer Data visualization Analysis

---

Data Insights amongst many more:
1. Beverages is the most product imported into various countries
2. USA has the highest order sales followed by Germany
3. Most of the customers chose United Package as their shipping company and on average takes 8 days to ship product to each customers. 
Interact more with the report to gain more insights - link

---

Appendix
Always remember to schedule your dataflow refresh. This can be daily, weekly or depending on the time you would want it to refresh for daily report update.

---

Thank you for reading.
