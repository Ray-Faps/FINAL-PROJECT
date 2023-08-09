# FINAL-PROJECT
In this project, the aim is to perform the ETL process using the below dataset sourced from Kaggle
[Salary Dataset](https://www.kaggle.com/datasets/mohithsairamreddy/salary-data)

Before importing the dataset into the MySQL database, some transformations were done using Power query on Microsoft Excel.

a) I added 2 new columns (Type of Degree and an Index column) which were merged to form the ID column, which is the common column between both files.
b) we removed nulls from the dataset to ensure data uniformity
c) we created a second table by splitting the dataset into a fact and dimension table.

The datasets used for this project are Salary_Data & salary_experience.


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

: Load the clean, transformed data into a second table in your MySQL database. Describe how this process was done and why you decided to load the data in the way you did.
- CREATE VIEW ______ AS (SELECT * FROM .....)

**Reflection**
With my experience in this project, I will first check raw data to identify any discrepancies that could obstruct or affect extraction into the database, because dealing with this type of problem could be time-consuming.

Manipulating data using SQL was fun, and I found that there could be several ways to make transformations better, for example, using the **round** function to limit the decimal places in the **average** salary to 2.

