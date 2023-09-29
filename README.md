# Dillard’s Vendor Performance Analysis Project

Team members: Yaasir Ahmed, Michelle (Ruoxuan) Liu, Shirley (Xueqing) Wang, and Yifei Wang

Dillard’s is a department store chain with approximately 282 stores headquartered in Little Rock, Arkansas. With a large amount of data available regarding Dillard’s sales transactions, merchandise and vendor details, department information, and store descriptions, the project team aimed to __identify the best-performing and worst-performing vendors__ upon defining best-performing as low cost, high purchase frequency, high recency, and high profit. After performing the initial Exploratory Data Analysis (EDA), the __K-Means clustering algorithm__ was applied to separate the data into clusters and further investigate insights from each individual cluster. The clustering analysis result showed that beauty products and certain clothing brands dominated the best-performing vendors while categories such as shoes, luggage, and furniture tended to downgrade business performances. In addition, __ROI analysis__ was conducted on the data. The analysis indicated that the best vendors make up 51% of Dillard’s revenue, 55% of costs, and 60% of profit, while the worst vendors contribute 2% of revenue, 2% of costs, and nearly 0% of the profit. <br />

As a result, Dillard’s is recommended to drop the vendors classified as worst-performing vendors and invest more in the best-performing vendors. This will __cut total costs by $4.4 million (2% of total costs)__ and __increase department stores' revenue by 6%__ in the next fiscal year. Although the K-Means algorithm was able to correctly distinguish the best-performing and worst-performing vendors, other clustering algorithms such as DBSCAN and Hierarchical Clustering could also be considered to increase the robustness and reliability of the clusters in the future.  

# Directory Structure
<pre>
Dillard’s Vendor Performance Analysis Project
│   
├── EDA                                             <- Exploratory Data Analysis(EDA) folder       
│   ├── EDA on skstinfo.ipynb                       <- EDA on the cost and retail price of recorded goods 
│   ├── EDA on skuinfo.ipynb                        <- EDA on the brands and popularity of recorded goods
│   ├── EDA on strinfo.ipynb                        <- EDA on city, state, zip in Dillard’s stores 
│   ├── EDA on trnsact.ipynb                        <- EDA on each transactions recorded in stores 
│   ├── deptinfo.csv                                <- Preprocessed deptinfo table
│   ├── skuinfo2.csv.zip                            <- Preprocessed skuinfo tables
│   └── readme.md                                   <- Table of contents for EDA
│   
├── Feature Calculation                             <- Feature calculation folder 
│   ├── .DS_Store                                   <- Merge conflict 
│   ├── ROI_Analysis.ipynb                          <- ROI analysis 
│   └── feature_calculation.ipynb                   <- Feature calculation before modeling
│   
├── KMeans                                          <- K Means clustering and cluster analysis 
│   ├── df_best_final.csv                           <- Best vendor cluster data for analysis
│   ├── df_worst_final.csv                          <- Worst vendor cluster data for analysis
│   ├── group7_data.csv                             <- Data used for K Means clustering
│   ├── kmeans_cluster_analysis.ipynb               <- cluster analysis for K Means clustering
│   ├── kmeans_clustering.ipynb                     <- K Means clustering with all features
│   └── kmeans_clustering_features_only.ipynb       <- K Means clustering with RFM and monetary features
│   
├── Preprocessing                                   <- Preprocessing and Data preparation folder 
│   └── Preprocessing.ipynb                         <- Preprocessing on data
│
├── README.md                                       <- Documents weekly updates and project content
├── .DS_Store                                       <- Merge conflict
├── .Rhistory                                       <- Merge conflict
├── Vendor Performance Analysis Presentation.pptx   <- Project Slides
└── Vendor Performance Analysis Report.pdf          <- Project Report

</pre>


# Weekly Updates
## Week 4 Updates 10/14/2022
1. Examined and Cleaned STRINFO, SKSTINFO, TRNSACT, DEPTINFO datasets
2. Deleted irrelevant columns (10th, 11th, 12th columns) in SKUINFO dataset
3. Stripped trailing whitespaces in the columns in SKUINFO dataset
4. Cleaned the numeric columns by checking the unique values and imputing the corrupted numerical data by filling with O in SKUINFO dataset
5. Cleaned the non-numeric columns by checking the unique values and standardize the data types in skstinfo dataset in SKUINFO dataset
6. Uploaded the cleaned datasets to PostgreSQL Databases in SKUINFO dataset
7. Sanity checked the uploaded datasets by selecting the first 5 rows in each table

## Week 5 Updates 10/21/2022
1. Fixed data cleaning issues from last week: Brand name incorrectly splitted, so we re-cleaned our data to make sure the columns were correctly formatted.
2. Came up with our **business question**: Given a season, predict the merchandise and the vendor that would make the most profit
3. Selected the main columns to use for our analysis and model building: 
    - skuinfo: SKU, VENDOR (use VENDOR since brand name has so many variations, eg. 9 WEST and NINE WEST for the same brand, and department names also 1-1 related to the brand names)
    - skitinfo: cost, retail (for calculating profit)
    - trnsact: SALEDATE, ORGPRICE (use SALEDATE to find season of the year)
    - deptinfo: dept, Name(Deptdesc)
4. Explored the data using SQL queries on PgAdmin and figured out that "" should be used for columns, and '' needs to be applied on string/characters
5. Decided to split the SALEDATE column into years and months in pandas since it is currently formatted as "yyyy-mm-dd". 
6. Next steps:
    - Checking the range of date and keep the year and assign season to each row
    - Count the numbers of purchase and return for each season
    - Calculate retail-cost for each vendor for each season

## Week 6 Updates 10/28/2022
1. Finished all data cleaning
2. Started initial Exploratory Data Analysis on the individual datasets based on the discussion from last week
3. Performed EDA or stsinfo data and explored if product popularity would be affected by price
4. Performed EDA on skuinfo data to loop up data distribution
5. Performed EDA on transaction data and explored customer return rate
6. Discussed the validity of current business question since the range of SALEDATE in TRNSACT table was discovered to be "2004-08-01" to "2005-08-27", which would make time series predictions invalid
7. Started to consider the possibility of a new business question: classifying merchandise and vendors into chunks based on the monetary amount, frequency, and recency of historical purchases
8. Next steps:
    - Continue with EDA
    - Further discuss and finalize the business question
    - Start exploring the machine learning approach/algorithm that would allow us to answer the business question

## Week 7 Updates 11/04/2022
1. Sorted out the project deliverables and planned our timeline for the project
2. Defined the ROI metric:
    - ROI = 100% * profit / cost
    - Profit: profit of purchase - profit of return - cost
    - Cost: cost * quantity while ignoring stype (ignoring whether the product was purchased or returned)
3. Decided on the **Machine Learning Method** for the project: K-Means Clustering
4. Finalized on our **business question**: Upon defining best-performing as low cost, high purchase frequency, recent purchase, and high profit, which vendors can be classified as best-performing and worst-performing?
    - We then compare to the Vendors clusters we get by using K-Means clustering
    - We could analyze additional characteristic for good vendors
5. Definition of each feature based on the database:
    - Average cost--sum(skstinfo.cost * trnsact.quantity)/count(skuinfo.sku) group by skuinfo.vendor
    - Profit of purchase--(trnsact.amt * trnsact.quantity when trnsact.stype = Purchase) - (trnsact.amt * trnsact.quantity when trnsact.stype = Return) - sum(skstinfo.cost * trnsact.quantity) group by skuinfo.vendor
    - Frequency of purchase--count(trnsact.sku) group by skuinfo.vendor when trnsact.stype = Purchase
    - Recency of purchase--max(trnsact.saledate) group by skuinfo.vendor when trnsact.stype = Purchase
6. Discussed the **business significance** of our question: After understanding which features contribute the most to the performance classification of vendor, we can consider 1) how the features can inspire marketing strategies to generate even more profit for the best-performing vendors, and 2) how the worst-performing vendors could improve in terms of the features to increase their performance
7. Selected a random 10% subset of trnsact, skuinfo, and skstinfo in order to join the three tables because the entire tables are too large to work with
8. Started to calculate the features of frequency, recency, and cost
9. Started self-learning on K-Means clustering
10. Next steps:
    - Finish preparing the features for K-Means clustering
    - Start writing code for K-Means clustering
    - Further discuss and possibly modify the features we have chosen
    - Start exploring the final report, presentation, and data visualization structures

## Week 8 Updates 11/11/2022
1. Cleaned the file for data preprocessing by adding comments, and deleting unnecessary variables and procedures
2. Cleaned the three files for EDA by adding comments, deleting unnecessary variables, and summarizing findings
3. Cleaned the file for feature calculation by adding comments, and deleting unnecessary variables and procedures
4. Reorganized the repository and pushed the most recent version of files and datasets
5. Started to perform K-Means clustering, understood the current results, and discussed potential ways to improve the features and algorithm
6. Continued self-learning on K-Means clustering
7. Next steps:
    - Finish calculating the Profit of purchase feature
    - Finish ROI analysis 
    - Continue to improve the code for K-Means clustering
    - Continue to keep all files and the repository organized
    - Start planning for the final report, presentation, and data visualization structures

## Week 9 Updates 11/18/2022
1. Defined the ROI metric as “ROI = 100% * profit / cost”
2. Calculated the ROI by vendor and also the overall ROI for all Dillard’s stores
3. Applied k-means algorithm to the final cleaned data and utilized t-SNE plot to identify the clusterability
    - features of interests only: vendor, recency, frequency, avg_cost, profit
    - all features including features of interests and exploration (retail, quantity, AMT, purchase, returns)
4. Finished calculating all features defined in the business question
5. Cleaned up the file for feature calculation
6. Next Steps:
    - Finalize number of clusters we would like to use
    - Continue working on kmeans algorithm
    - Plan out initial steps for final report, presentation, and data visualization
    
## Week 11 Updates 12/02/2022
1. Finalized k-means clustering algorithm and the features to be used for the analysis
2. Decided on the number of clusters (4 clusters) to use based on the outputs from k-means
3. Performed clustering analysis, identified the best and worst vendors, and analyzed the common characteristics of the vendors in each of those two clusters
4. Queried the dataset in SQL to find out the brand name and department name for each vendor of interest, and searched the category of products the vendors sell to add to cluster analysis
5. Reorganized the repository and pushed the most recent version of files and datasets
6. Next Steps:
    - Finish ROI analysis 
    - Incorporate more data visualizations if possible
    - Start working on the final report
    - Create final presentation slides
