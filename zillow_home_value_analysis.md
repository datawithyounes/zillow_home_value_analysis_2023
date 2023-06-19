# Zillow Home Value Analysis 2020-2023

### Contact Information

**Author**: Younes AGEGAL

**Email**: yagegal@gmail.com

[**UpWork**](https://www.upwork.com/freelancers/younesa11)

[**LinkedIn**](https://www.linkedin.com/in/younesagegal/)

[**Twitter**](https://twitter.com/httpsyns)

#### What is the average annual growth rate of home values in different states, and which states have experienced the highest growth in home values ?

````sql
SELECT
  state,
  CAST("2020" AS MONEY),
  CAST("2021" AS MONEY),
  CAST("2022" AS MONEY),
  ROUND(((POWER("2022" / "2020",0.3) - 1) *100),2) as home_growth_rate_percentage
FROM (
  SELECT
    state,
    AVG(q1_2020 + q2_2020 + q3_2020 + q4_2020) / 4 AS "2020",
    AVG(q1_2021 + q2_2021 + q3_2021 + q4_2021) / 4 AS "2021",
    AVG(q1_2022 + q2_2022 + q3_2022 + q4_2022) / 4 AS "2022"
  FROM
    home_v_state
  GROUP BY
    state
) AS YEAR
ORDER BY home_growth_rate_percentage DESC
LIMIT 10;

````

**Results:**

| state          | 2020        | 2021        | 2022        | home_growth_rate_percentage |
|----------------|-------------|-------------|-------------|-----------------------------|
| Montana        | $290,984.21 | $360,583.45 | $437,280.06 | 13.00                       |
| Idaho          | $318,043.51 | $417,032.60 | $474,049.02 | 12.72                       |
| Arizona        | $291,498.76 | $357,587.75 | $430,781.11 | 12.43                       |
| Florida        | $257,685.87 | $301,529.71 | $375,905.78 | 11.99                       |
| Georgia        | $214,821.16 | $255,111.43 | $309,264.20 | 11.55                       |
| North Carolina | $218,678.02 | $258,495.78 | $309,918.74 | 11.03                       |
| Utah           | $376,746.33 | $458,798.66 | $533,641.03 | 11.01                       |
| Tennessee      | $207,699.95 | $243,620.78 | $293,238.33 | 10.90                       |
| Maine          | $253,680.07 | $307,897.47 | $356,563.10 | 10.75                       |
| South Carolina | $197,121.99 | $227,691.69 | $271,070.17 | 10.03                       |


#### What is the average rental price for each state/city, and how has it changed over time ?

````sql
SElECT state, city,
CAST(ROUND(AVG("q1_2020"+"q2_2020"+"q3_2020"+"q4_2020") / 4 ,2) AS MONEY) AS "2020_avg_rent_price" ,
CAST(ROUND(AVG("q1_2021"+"q2_2021"+"q3_2021"+"q4_2021") / 4 ,2) AS MONEY) AS "2021_avg_rent_price",
CAST(ROUND(AVG("q1_2022"+"q2_2022"+"q3_2022"+"q4_2022") / 4 ,2) AS MONEY) AS "2022_avg_rent_price"
FROM city_rental_prices
GROUP BY city, state
ORDER BY state 
LIMIT 10;
````

**Results:**
| state | city         | 2020_avg_rent_price | 2021_avg_rent_price | 2022_avg_rent_price |
|-------|--------------|---------------------|---------------------|---------------------|
| AK    | Anchorage    | $1,389.50           | $1,468.86           | $1,632.66           |
| AL    | Birmingham   | $1,168.25           | $1,261.61           | $1,379.48           |
| AL    | Huntsville   | $1,169.82           | $1,340.84           | $1,473.34           |
| AL    | Mobile       | $873.10             | $992.50             | $1,096.59           |
| AL    | Montgomery   | $1,006.18           | $1,107.54           | $1,225.72           |
| AL    | Tuscaloosa   | $1,248.24           | $1,303.92           | $1,418.76           |
| AR    | Fayetteville | $1,141.57           | $1,247.69           | $1,420.87           |
| AR    | Little Rock  | $962.58             | $1,060.72           | $1,151.07           |
| AZ    | Phoenix      | $1,422.75           | $1,913.48           | $1,919.97           |
| AZ    | Tucson       | $1,184.96           | $1,365.18           | $1,554.22           |


#### Which city/region has the highest rental price ?

````sql
SElECT state, city,
CAST(ROUND(AVG("q1_2020"+"q2_2020"+"q3_2020"+"q4_2020") / 4 ,2) AS MONEY) AS "2020_avg_rent_price" ,
CAST(ROUND(AVG("q1_2021"+"q2_2021"+"q3_2021"+"q4_2021") / 4 ,2) AS MONEY) AS "2021_avg_rent_price",
CAST(ROUND(AVG("q1_2022"+"q2_2022"+"q3_2022"+"q4_2022") / 4 ,2) AS MONEY) AS "2022_avg_rent_price"
FROM city_rental_prices
GROUP BY city, state
ORDER BY "2022_avg_rent_price" desc,"2021_avg_rent_price" desc,"2020_avg_rent_price" desc
LIMIT 10;
````

**Results:**
| state | city          | 2020_avg_rent_price | 2021_avg_rent_price | 2022_avg_rent_price |
|-------|---------------|---------------------|---------------------|---------------------|
| CA    | Santa Maria   | $2,501.68           | $2,896.28           | $3,400.24           |
| CA    | San Jose      | $3,057.87           | $2,970.62           | $3,266.36           |
| CA    | Santa Cruz    | $2,611.76           | $2,846.08           | $3,205.24           |
| NY    | New York      | $2,870.37           | $2,710.32           | $3,168.54           |
| CA    | San Francisco | $2,898.42           | $2,864.31           | $3,102.99           |
| CA    | Oxnard        | $2,401.28           | $2,667.66           | $2,938.35           |
| CA    | San Diego     | $2,268.55           | $2,520.67           | $2,933.32           |
| CA    | Los Angeles   | $2,582.29           | $2,548.52           | $2,872.24           |
| FL    | Naples        | $1,873.81           | $2,271.84           | $2,847.24           |
| MA    | Boston        | $2,598.69           | $2,542.12           | $2,827.14           |


#### Which city/region has lowest rental prices ?

````sql
SElECT state, city,
CAST(ROUND(AVG("q1_2020"+"q2_2020"+"q3_2020"+"q4_2020") / 4 ,2) AS MONEY) AS "2020_avg_rent_price" ,
CAST(ROUND(AVG("q1_2021"+"q2_2021"+"q3_2021"+"q4_2021") / 4 ,2) AS MONEY) AS "2021_avg_rent_price",
CAST(ROUND(AVG("q1_2022"+"q2_2022"+"q3_2022"+"q4_2022") / 4 ,2) AS MONEY) AS "2022_avg_rent_price"
FROM city_rental_prices
GROUP BY city, state
ORDER BY "2022_avg_rent_price" asc,"2021_avg_rent_price" asc,"2020_avg_rent_price" asc
LIMIT 10;
````

**Results:**
| state | city       | 2020_avg_rent_price | 2021_avg_rent_price | 2022_avg_rent_price |
|-------|------------|---------------------|---------------------|---------------------|
| WV    | Charleston | $726.33             | $752.66             | $795.23             |
| OH    | Huntington | $731.28             | $773.20             | $818.19             |
| OH    | Youngstown | $684.32             | $765.53             | $831.20             |
| IN    | Evansville | $732.84             | $789.21             | $855.05             |
| OH    | Canton     | $759.94             | $824.52             | $920.20             |
| WI    | Green Bay  | $897.79             | $830.25             | $930.30             |
| IL    | Davenport  | $822.80             | $870.92             | $936.00             |
| KS    | Wichita    | $818.72             | $876.30             | $949.39             |
| PA    | Erie       | $813.67             | $854.71             | $962.06             |
| IL    | Rockford   | $818.26             | $892.20             | $971.85             |


#### What is the average annual growth rate of rent prices in different states?

````sql
SELECT
  state,
  CAST("2020" AS MONEY),
  CAST("2021" AS MONEY),
  CAST("2022" AS MONEY),
  ROUND(((POWER("2022" / "2020",0.3) - 1) *100),2) as rent_growth_rate_percentage
FROM (
  SELECT
    state,
    AVG(q1_2020 + q2_2020 + q3_2020 + q4_2020) / 4 AS "2020",
    AVG(q1_2021 + q2_2021 + q3_2021 + q4_2021) / 4 AS "2021",
    AVG(q1_2022 + q2_2022 + q3_2022 + q4_2022) / 4 AS "2022"
  FROM
    city_rental_prices
  GROUP BY
    state
) AS YEAR
ORDER BY rent_growth_rate_percentage DESC
LIMIT 10;
````

**Results:**
| state | 2020      | 2021      | 2022      | rent_growth_rate_percentage |
|-------|-----------|-----------|-----------|-----------------------------|
| FL    | $1,441.34 | $1,672.84 | $1,990.13 | 10.16                       |
| AZ    | $1,303.86 | $1,639.33 | $1,737.10 | 8.99                        |
| ID    | $1,377.75 | $1,658.65 | $1,824.68 | 8.79                        |
| NM    | $1,115.63 | $1,280.56 | $1,459.28 | 8.39                        |
| GA    | $1,163.89 | $1,323.30 | $1,510.71 | 8.14                        |
| TN    | $1,133.23 | $1,275.46 | $1,468.17 | 8.08                        |
| NC    | $1,235.47 | $1,384.54 | $1,589.82 | 7.86                        |
| NH    | $1,453.61 | $1,644.00 | $1,865.74 | 7.78                        |
| UT    | $1,351.71 | $1,522.93 | $1,733.24 | 7.74                        |
| NJ    | $1,691.37 | $1,887.43 | $2,151.43 | 7.48                        |



#### How do rental prices compare to home values in terms of growth rates ?
##### Creating a temporary table for rent prices growth rate :

````sql
CREATE TEMPORARY TABLE rent_prices_growth_rate AS

SELECT
  state,
  CAST("2020" AS MONEY),
  CAST("2021" AS MONEY),
  CAST("2022" AS MONEY),
  ROUND(((POWER("2022" / "2020",0.3) - 1) *100),2) as rent_growth_rate_percentage
FROM (
  SELECT
    state,
    AVG(q1_2020 + q2_2020 + q3_2020 + q4_2020) / 4 AS "2020",
    AVG(q1_2021 + q2_2021 + q3_2021 + q4_2021) / 4 AS "2021",
    AVG(q1_2022 + q2_2022 + q3_2022 + q4_2022) / 4 AS "2022"
  FROM
    city_rental_prices
  GROUP BY
    state
) AS YEAR
ORDER BY rent_growth_rate_percentage DESC ;

````
##### Creating a temporary table for home values growth rate :
````sql
CREATE TEMPORARY TABLE home_v_growth_rate AS
SELECT
  state,
  CAST("2020" AS MONEY),
  CAST("2021" AS MONEY),
  CAST("2022" AS MONEY),
  ROUND(((POWER("2022" / "2020",0.3) - 1) *100),2) as home_growth_rate_percentage
FROM (
  SELECT
    state,
    AVG(q1_2020 + q2_2020 + q3_2020 + q4_2020) / 4 AS "2020",
    AVG(q1_2021 + q2_2021 + q3_2021 + q4_2021) / 4 AS "2021",
    AVG(q1_2022 + q2_2022 + q3_2022 + q4_2022) / 4 AS "2022"
  FROM
    home_v_state
  GROUP BY
    state
) AS YEAR
ORDER BY home_growth_rate_percentage DESC;
````
##### How do rental prices compare to home values in terms of growth rates ?
````sql
SELECT city_rental_prices.state,
       city_rental_prices.city,
       home_v_growth_rate.home_growth_rate_percentage ,
       rent_prices_growth_rate.rent_growth_rate_percentage,
       (home_v_growth_rate.home_growth_rate_percentage -
       rent_prices_growth_rate.rent_growth_rate_percentage) as growth_rate_difference
       
FROM  
       city_rental_prices,
       rent_prices_growth_rate,
       home_v_growth_rate
GROUP BY 
      city_rental_prices.state, 
      city_rental_prices.city,
      home_v_growth_rate.home_growth_rate_percentage ,  
      rent_prices_growth_rate.rent_growth_rate_percentage,
      growth_rate_difference
ORDER BY 
      home_v_growth_rate.home_growth_rate_percentage DESC,
      rent_prices_growth_rate.rent_growth_rate_percentage DESC
      
LIMIT 10
````

**Results:**
| state | city         | home_growth_rate_percentage | rent_growth_rate_percentage | growth_rate_difference |
|-------|--------------|-----------------------------|-----------------------------|------------------------|
| TX    | Houston      | 13.00                       | 10.16                       | 2.84                   |
| GA    | Atlanta      | 13.00                       | 10.16                       | 2.84                   |
| VA    | Washington   | 13.00                       | 10.16                       | 2.84                   |
| MI    | Detroit      | 13.00                       | 10.16                       | 2.84                   |
| NY    | New York     | 13.00                       | 10.16                       | 2.84                   |
| CA    | Los Angeles  | 13.00                       | 10.16                       | 2.84                   |
| FL    | Miami        | 13.00                       | 10.16                       | 2.84                   |
| TX    | Dallas       | 13.00                       | 10.16                       | 2.84                   |
| PA    | Philadelphia | 13.00                       | 10.16                       | 2.84                   |
| FL    | Tampa        | 13.00                       | 10.16                       | 2.84                   |


#### What is the average difference between homes sold above list prices and the ones sold below list prices over the last year ?
##### Creating a temporary table for the average homes sold above list prices in the last year :

````sql
CREATE TEMPORARY TABLE 
        avg_homes_sold_above_list_p AS 
SELECT 
        state, 
        city, 
        ROUND(AVG("Q1_2022"+"Q2_2022"+"Q3_2022"+"Q4_2022") / 4,2)*100 AS average_homes_s_above_list
FROM  
        homes_sold_above_list
GROUP BY 
        state,city;
````
##### Creating a temporary table for the average homes sold below list prices in the last year :
````sql
CREATE TEMPORARY TABLE 
        avg_homes_sold_below_list_p AS 
SELECT 
        state,
        city, 
        ROUND(AVG("Q1_2022"+"Q2_2022"+"Q3_2022"+"Q4_2022") / 4,2)*100 AS average_homes_s_below_list
FROM 
        homes_sold_below_list
GROUP BY 
        state,city;
````
##### Calculating the average difference between homes sold above list price and the ones below list prices :
````sql
SELECT 
        avg_homes_sold_below_list_p.state, 
        avg_homes_sold_below_list_p.city, 
         average_homes_s_above_list,
        average_homes_s_below_list,
        (average_homes_s_above_list-average_homes_s_below_list) AS average_difference_percentage
FROM 
        avg_homes_sold_below_list_p, avg_homes_sold_above_list_p
WHERE 
        avg_homes_sold_below_list_p.city=avg_homes_sold_above_list_p.city
GROUP BY 
        avg_homes_sold_below_list_p.state, 
        average_difference_percentage,
        avg_homes_sold_below_list_p.city, 
        average_homes_s_above_list,
        average_homes_s_below_list
ORDER BY 
        average_difference_percentage ASC
LIMIT 10
````
**Results:**
| state | city             | average_homes_s_above_list | average_homes_s_below_list | average_difference_percentage |
|-------|------------------|----------------------------|----------------------------|-------------------------------|
| FL    | Key West         | 40.00                      | 65.00                      | -25.00                        |
| OH    | Wheeling         | 44.00                      | 69.00                      | -25.00                        |
| TX    | Midland          | 37.00                      | 61.00                      | -24.00                        |
| AL    | Florence         | 38.00                      | 60.00                      | -22.00                        |
| KY    | London           | 41.00                      | 63.00                      | -22.00                        |
| AL    | Daphne           | 39.00                      | 59.00                      | -20.00                        |
| OH    | Weirton          | 42.00                      | 61.00                      | -19.00                        |
| AZ    | Lake Havasu City | 37.00                      | 55.00                      | -18.00                        |
| FL    | Panama City      | 38.00                      | 56.00                      | -18.00                        |
| IL    | Carbondale       | 41.00                      | 59.00                      | -18.00                        |


#### How long do properties typically stay on the market in average before being sold in different cities/states over the last year ?

````sql
SELECT 
        state, 
        city,
        ROUND(AVG("Q1_2022"+"Q2_2022"+"Q3_2022"+"Q4_2022") / 4 ,0) AS average_days_to_close
    
FROM 
        median_days_to_close
GROUP BY 
        state,
        city
LIMIT 10;
````

**Results:**
| state | city        | average_days_to_close |
|-------|-------------|-----------------------|
| FL    | Punta Gorda | 36                    |
| PA    | York        | 43                    |
| NC    | Hickory     | 35                    |
| CA    | Riverside   | 30                    |
| NY    | New York    | 65                    |
| MS    | Jackson     | 36                    |
| TN    | Kingsport   | 38                    |
| IL    | Peoria      | 37                    |
| GA    | Atlanta     | 31                    |
| MA    | Boston      | 43                    |


#### Which cities properties stay the longest in the market before being sold ?
##### Creating a temporary table of the average days to close :

````sql
CREATE TEMPORARY TABLE avg_days_to_close AS 
SELECT 
        state, 
        city,
        ROUND(AVG("Q1_2022"+"Q2_2022"+"Q3_2022"+"Q4_2022") / 4 ,0) AS average_days_to_close
    
FROM 
        median_days_to_close
GROUP BY 
        state,
        city;
````
##### Filtering the cities that have the stay the longest on the market ?
````sql
SELECT 
        state, 
        city,
        average_days_to_close
FROM 
        avg_days_to_close
ORDER BY 
        average_days_to_close DESC
LIMIT 10;
````

**Results:**
| state | city          | average_days_to_close |
|-------|---------------|-----------------------|
| NY    | Utica         | 69                    |
| NJ    | Atlantic City | 69                    |
| NY    | Kingston      | 68                    |
| NY    | Buffalo       | 66                    |
| NY    | New York      | 65                    |
| NY    | Albany        | 65                    |
| NY    | Syracuse      | 63                    |
| NJ    | Trenton       | 59                    |
| NY    | Binghamton    | 58                    |
| NY    | Poughkeepsie  | 56                    |


#### Which cities properties stay the shortest in the market before being sold ?
##### Creating a temporary table of the average days to close :

````sql
 CREATE TEMPORARY TABLE avg_days_to_close AS 
SELECT 
        state, 
        city,
        ROUND(AVG("Q1_2022"+"Q2_2022"+"Q3_2022"+"Q4_2022") / 4 ,0) AS average_days_to_close
    
FROM 
        median_days_to_close
GROUP BY 
        state,
        city;
````
##### Filtering the cities that have the stay the longest on the market ?
````sql
SELECT 
        state, 
        city,
        average_days_to_close
FROM 
        avg_days_to_close
ORDER BY 
        average_days_to_close ASC
LIMIT 10;
````

**Results:**
| state | city           | average_days_to_close |
|-------|----------------|-----------------------|
| WI    | Appleton       | 11                    |
| WI    | Green Bay      | 14                    |
| ND    | Fargo          | 22                    |
| CA    | Santa Rosa     | 25                    |
| UT    | Salt Lake City | 25                    |
| UT    | St. George     | 26                    |
| UT    | Provo          | 26                    |
| TX    | Austin         | 26                    |
| WI    | Milwaukee      | 26                    |
| CA    | San Diego      | 26                    |


#### How does the percentage of home sold vary across different cities/states over the past year ?
##### Creating a temporary table of the average homes sold over the past year :

````sql
CREATE TEMPORARY TABLE 
        avg_homes_sold_above_list_p AS 
SELECT 
        state, 
        city, 
        ROUND(AVG("Q1_2022"+"Q2_2022"+"Q3_2022"+"Q4_2022") / 4,2)*100 AS average_homes_sold
FROM  
        homes_sold_above_list
GROUP BY 
        state,
        city;
````
##### Calculating the closed deals/sales of the past year in differenct states/cities :
````sql
SELECT
        state,
        city,
        average_homes_sold as sales_percentage
FROM   
        avg_homes_sold_above_list_p
LIMIT 10;
````

**Results:**
| state | city           | sales_percentage |
|-------|----------------|------------------|
| FL    | Punta Gorda    | 40.00            |
| NC    | Rocky Mount    | 42.00            |
| OH    | Weirton        | 42.00            |
| MO    | Jefferson City | 41.00            |
| CA    | Riverside      | 42.00            |
| OH    | Mansfield      | 37.00            |
| MS    | Jackson        | 41.00            |
| IL    | Peoria         | 44.00            |
| GA    | Atlanta        | 41.00            |
| MA    | Boston         | 45.00            |



#### Which cities/states have made the most sales in the last year ?
##### Creating a temporary table of the average homes sold over the past year :

````sql
CREATE TEMPORARY TABLE 
        avg_homes_sold_above_list_p AS 
SELECT 
        state, 
        city, 
        ROUND(AVG("Q1_2022"+"Q2_2022"+"Q3_2022"+"Q4_2022") / 4,2)*100 AS average_homes_sold
FROM  
        homes_sold_above_list
GROUP BY 
        state,
        city;
````
##### Filtering the states/cities that have made the most sales in the past year :
````sql
SELECT
        state,
        city,
        average_homes_sold as sales_percentage
FROM   
        avg_homes_sold_above_list_p
ORDER BY
        sales_percentage DESC
LIMIT 10;
````

**Results:**
| state | city         | sales_percentage |
|-------|--------------|------------------|
| AL    | Florence     | 60.00            |
| NC    | Sanford      | 58.00            |
| NY    | Poughkeepsie | 56.00            |
| PA    | Indiana      | 54.00            |
| MS    | Gulfport     | 54.00            |
| LA    | Alexandria   | 53.00            |
| NY    | Buffalo      | 47.00            |
| CA    | San Jose     | 46.00            |
| NY    | Rochester    | 46.00            |
| NH    | Concord      | 46.00            |


#### Which cities/states have made the least sales in the last year ?
##### Creating a temporary table of the average homes sold over the past year :

````sql
CREATE TEMPORARY TABLE 
        avg_homes_sold_above_list_p AS 
SELECT 
        state, 
        city, 
        ROUND(AVG("Q1_2022"+"Q2_2022"+"Q3_2022"+"Q4_2022") / 4,2)*100 AS average_homes_sold
FROM  
        homes_sold_above_list
GROUP BY 
        state,
        city;
````
##### Filtering the states/cities that have made the least sales in the past year :

````sql
SELECT
        state,
        city,
        average_homes_sold as sales_percentage
FROM   
        avg_homes_sold_above_list_p
ORDER BY
        sales_percentage ASC
LIMIT 10;
````
**Results:**
| state | city       | sales_percentage |
|-------|------------|------------------|
| KY    | Somerset   | 15.00            |
| WI    | Appleton   | 17.00            |
| ND    | Fargo      | 17.00            |
| WI    | Oshkosh    | 18.00            |
| KY    | Paducah    | 18.00            |
| FL    | Palatka    | 19.00            |
| TX    | Huntsville | 19.00            |
| KS    | Lawrence   | 20.00            |
| TN    | Crossville | 21.00            |
| FL    | Lake City  | 22.00            |

