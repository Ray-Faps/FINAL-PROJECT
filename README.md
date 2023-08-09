# FINAL-PROJECT

## INTRODUCTION
Data they say is the new gold and how its mined determines the value of insights one can get from it. A properly executed extraction, transformation and loading (ETL) of the data plays a vital role in the ease of getting value from the data.
In this project, the aim is to perform the ETL process on the salary dataset (fact dataset) and a created dimension data set. The fact dataset can be viewed by clicking on - [Salary Dataset](https://www.kaggle.com/datasets/mohithsairamreddy/salary-data)

## Question
The project will be aiming to ascertain based on the data if there is gender discrimination in the average earnings of males and females.

Before importing the dataset into the MySQL database, I carried out some transformations on my dimension table using Power query on Microsoft Excel. The process involved the following steps:
- I added 2 new columns (Type of Degree and an Index column) which were merged to form the ID column, which is the common column between both files.
- I removed nulls from the dataset to ensure data uniformity
- I created a second table by splitting the dataset into a fact and dimension table.

The datasets used for this project are Salary_Data(Fact) & salary_experience (Dimension).

## Github Folders
3 Github folders were created with details of this project
- Snapshot -  this folder contains a screenshot of the data before and after the transformation
- codes_file - this folder contains the line of codes used for the ETL process
- readme - this folder contains details about the entire project

******************************************************************************************************************************************
## THE ETL PROCESS
**Data Extraction Stage**

In the extraction stage, Salary_Data had 6,705 rows, while salary_experience had 6,702 rows. I checked the raw CSV files and noticed that this problem existed in the CSV files as well, however, after deleting the nulls from both files, we achieved a uniformity of 6,698 rows for both files.


**Data Transformation**

In this stage,  I created a table showing the **average** salary by Gender by doing a **left join** with Salary_Data & salary_experience and **grouped by** Gender. 

      SELECT Gender,
      ROUND(avg(Salary),2) AS Average_Salary
      FROM salary_data
      LEFT JOIN salary_experience
      ON salary_data.ID = salary_experience.ID
      GROUP BY Gender;

This operation returned 3 rows (Male, Female, & Others), instead of 2 rows for Male & Female. 

To correct this, I used an **update** statement to change 'Others' to 'Female'. I also had to disable safe mode to be able to make this update.

      SET SQL_SAFE_UPDATES = 0;

      update salary_data
      set Gender = 'Female'
      where Gender = 'Other';

Further, I used the **round** function to limit the decimal places in the **average** salary to 2.
        ROUND(avg(Salary),2) AS Average_Salary


**Data Loading**

Finally, I created a view of the transformation and loaded the view into the database.

I loaded the data into a view because views are important for row-level security. It can be used to control access to data by exposing only specific columns or rows to certain users or roles. This prevents unauthorized access to sensitive information. Views allow you to abstract complex queries and data structures into simpler, more manageable entities, instead of writing and maintaining complex queries every time certain information is needed.

**Outcome**

From the data, we gather that males have a higher average salary than females. To gather more insights on this finding, I could check the level of education, age (or age range) and years of experience.

**Reflection**

With my experience in this project, I will first check raw data to identify any discrepancies that could obstruct or affect extraction into the database, because dealing with this type of problem could be time-consuming.

Manipulating data using SQL was fun, and I found that there could be several ways to make transformations better, for example, using the **round** function to limit the decimal places in the **average** salary to 2.

