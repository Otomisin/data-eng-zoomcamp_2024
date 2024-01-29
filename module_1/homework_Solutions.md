# Module 1 Homework
 Docker, Database Queries, and Terraform Practice

## Question 1: Knowing docker tags
**Question**: Which tag has the following text? - Automatically remove the container when it exits.  
**Code**:
```shell
docker run --help
```
**Answer**: `--rm`

## Question 2: Understanding docker first run
**Question**: Understanding docker first run. Run docker with the python:3.9 image in an interactive mode and the entrypoint of bash. Now check the python modules that are installed (use pip list). What is the version of the package wheel?  
**Code**:
```shell
docker run -it --entrypoint=bash python:3.9
pip list
```
**Answer**: `0.42.0`

## Question 3: Count Records
**Question**: How many taxi trips were totally made on September 18th, 2019? Tip: started and finished on 2019-09-18.  
**Code**:
```sql
pgcli -h localhost -dp 5432 -u root -d ny_taxi
SELECT COUNT(*) AS trip_count
FROM green_taxi_data
WHERE DATE(lpep_pickup_datetime) = '2019-09-18'
    AND DATE(lpep_dropoff_datetime) = '2019-09-18';
```
**Answer**: `15612`

## Question 4: Largest Trip for Each Day
**Question**: Which was the pick-up day with the largest trip distance? Use the pick-up time for your calculations.  
**Code**:
```sql
SELECT DATE(lpep_pickup_datetime) AS pickup_day,
MAX(trip_distance) AS max_trip_distance
FROM green_taxi_data
GROUP BY pickup_day
ORDER BY max_trip_distance DESC
LIMIT 1;
```
**Answer**: `2019-09-26`

## Question 5: Three Biggest Pick Up Boroughs
**Question**: Consider lpep_pickup_datetime on '2019-09-18' and ignoring Borough as Unknown. Which were the 3 pick-up Boroughs that had a sum of total_amount superior to 50000?  
**Code**:
```python
import pandas as pd
# Load your dataset
df = pd.read_csv('green_taxi_data.csv')
# Convert 'lpep_pickup_datetime' to datetime format
df_result['lpep_pickup_datetime'] = pd.to_datetime(df_result['lpep_pickup_datetime'])
# Filter rows based on conditions
filtered_df = df_result[(df_result['lpep_pickup_datetime'].dt.date == pd.to_datetime('2019-09-18').date()) & (df_result['Borough'] != 'Unknown')]
# Group by 'Borough' and calculate the sum of 'total_amount'
borough_totals = filtered_df.groupby('Borough')['total_amount'].sum()
# Filter Boroughs with sum of 'total_amount' greater than 50000
boroughs_over_50000 = borough_totals[borough_totals > 50000]
# Select the top 3 Boroughs
top_3_boroughs = boroughs_over_50000.nlargest(3)
print(top_3_boroughs)
```
**Answer**: `"Brooklyn", "Manhattan", "Queens"`

## Question 6: Largest Tip
**Question**: For the passengers picked up in September 2019 in the zone name Astoria, which was the drop-off zone that had the largest tip? We want the name of the zone, not the id.  
**Code**:
```python
import pandas as pd
# Assuming df_result is your DataFrame
# Filter rows based on conditions
filtered_df = df_result[
    (df_result['lpep_pickup_datetime'].dt.year == 2019) & 
    (df_result['lpep_pickup_datetime'].dt.month == 9) & 
    (df_result['Zone'] == 'Astoria')
]
df_irene = filtered_df[['lpep_pickup_datetime', 'DOLocationID', 'tip_amount']]
# Merge with another DataFrame (assuming df_iter is the other DataFrame)
df_result2 = pd.merge(df_irene, df_iter, how='left', left_on='DOLocationID', right_on='LocationID')
# Find the drop-off zone with the largest tip
max_tip_zone = df_result2.loc[df_result2['tip_amount'].idxmax(), 'Zone']
```
**Answer**: `JFK Airport`

## Question 7: Terraform Resource Creation
**Question**: After updating the `main.tf` and `variable.tf` files, what is the command to apply changes in Terraform?  
**Code**:
```shell
terraform apply
```
**Answer**: Running `terraform apply` will apply the changes specified in the Terraform configuration files (`main.tf` and `variable.tf`). This command initializes the Terraform configuration and applies the changes to reach