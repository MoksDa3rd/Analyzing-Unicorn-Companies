Task Instructions
Your task is to analyze the dataset and identify the three best-performing industries based on the number of new unicorns created in the years 2019, 2020, and 2021 combined. Follow these steps to complete the task:

Identify Top Industries:

Determine the three industries with the highest number of new unicorns from 2019 to 2021.
Gather Industry Data:

For the identified industries, find:
The number of unicorns within each industry.
The year they became unicorns.
Their average valuation, converted to billions of dollars and rounded to two decimal places.
Prepare Final Query:

Create a query to return a table with the following columns: industry, year, num_unicorns, and average_valuation_billions.
Sort the results by year and num_unicorns, both in descending order.
Final Output:

Ensure that your final query containing the desired output is stored as a pandas DataFrame called df at the top of the cell for validation.
Example Query Structure
sql
Copy code
-- Step 1: Identify the top 3 industries
WITH top_industries AS (
    SELECT industry, COUNT(*) AS num_unicorns
    FROM unicorns
    WHERE year IN (2019, 2020, 2021)
    GROUP BY industry
    ORDER BY num_unicorns DESC
    LIMIT 3
)

-- Step 2: Gather industry data
, industry_data AS (
    SELECT 
        industry,
        year,
        COUNT(*) AS num_unicorns,
        ROUND(AVG(valuation / 1e9), 2) AS average_valuation_billions
    FROM unicorns
    WHERE industry IN (SELECT industry FROM top_industries)
    GROUP BY industry, year
)

-- Step 3: Prepare final output
SELECT *
FROM industry_data
ORDER BY year DESC, num_unicorns DESC;
Example Python Code
python
Copy code
import pandas as pd
import sqlite3

# Connect to your database
conn = sqlite3.connect('your_database.db')

# Query to get the desired output
query = """
WITH top_industries AS (
    SELECT industry, COUNT(*) AS num_unicorns
    FROM unicorns
    WHERE year IN (2019, 2020, 2021)
    GROUP BY industry
    ORDER BY num_unicorns DESC
    LIMIT 3
),
industry_data AS (
    SELECT 
        industry,
        year,
        COUNT(*) AS num_unicorns,
        ROUND(AVG(valuation / 1e9), 2) AS average_valuation_billions
    FROM unicorns
    WHERE industry IN (SELECT industry FROM top_industries)
    GROUP BY industry, year
)
SELECT *
FROM industry_data
ORDER BY year DESC, num_unicorns DESC;
"""

# Execute the query and store the result in a pandas DataFrame
df = pd.read_sql_query(query, conn)

# Close the database connection
conn.close()

# Display the DataFrame
print(df)
Notes:
Ensure the final DataFrame df is defined at the top of the cell for validation.
Adapt the connection details and query as per your dataset and database schema.
This example assumes the dataset is stored in a SQL database; adapt as needed if using a different data source.
