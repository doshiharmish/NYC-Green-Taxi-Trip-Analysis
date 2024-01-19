# NYC Green Taxi Trip Analysis 

## Project Overview

This project aims to analyze the NYC Green Taxi Trip Data from January 2022 to January 2023. Historically, green Taxis in NYC, authorized for street-hail pickups outside the central business district and airports, were introduced to extend taxi services to underserved areas beyond Manhattan. Yellow Taxis, traditionally dominant in Manhattan and airport street-hail services, originally held exclusive rights in high-demand areas, but now share territories with Green Taxis, expanding the latter's reach. The primary focus in this project lies in utilizing machine learning techniques for regression and classification to achieve two key goals:

1. ***Fare Amount Prediction***: Utilizing features such as pickup and drop-off locations, trip distance, DateTime, and pertinent attributes, the project aims to construct a regression model. This model will predict the fare amount for individual taxi trips, enhancing accuracy in fare estimation.
2. ***Profitable Pickup Location Identification***: Leveraging pickup locations, trip distances, and fare amounts, the project intends to develop a clustering model specifically tailored to identify profitable pickup locations for green taxis. This analysis aims to assist drivers in optimizing their pickup strategies for increased profitability.

## Dataset Characteristics

1. ***Trip Record Data***: Obtained from the New York City Taxi and Limousine Commission (TLC).
***Total Recorded Trips***: 908,613

![image](https://github.com/doshiharmish/Machine-Learning/assets/16878994/e1c1d45f-e37d-45f5-937b-d915ff46915c)


2. **Taxi Zone Map Dataset**: Used to map location IDs in the main dataset with NYC Borough, Zone, and service zone.
***Number of Records***: 265

![image](https://github.com/doshiharmish/Machine-Learning/assets/16878994/deace469-9f4f-471c-afd3-be8e53d53a48)


3. **Holiday Dataset**: A new dataset was generated to explore trip details on holidays, working days, and weekends.
***Number of Holidays***: 23

## Initial Data Distribution Analysis

### 1. Trip Distance
The trip distance distribution in this dataset reveals intriguing details. Across 908,613 entries, the average distance covered per trip amounts to 78.72 miles. The range extends from 0.00 miles to an astonishing maximum of 360,068.14 miles. Most of the data lies between 1.15 and 3.73 miles, while the median distance traveled is 2.00 miles.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/7ecbe12e-173c-453c-9349-a6f93bf2311b)


### 2. Fare Amount
The typical trip averages $15.39, ranging from -$350.08 to $2020.20. The bulk of the data falls within the $7.90 to $18.20 range, with a median cost of $11.50.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/a3e6d39d-ce09-4617-8540-dea21c47c4b6)


### 3. Payment Mode

The distribution analysis reveals that the primary payment method used is Credit Card, constituting 64% of the transactions, followed by Cash at 36%. Additionally, less than 1% of trips either were not charged or encountered disputes during payment.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/f27a3439-60a6-42f1-8287-0358db00954f)



## Feature Engineering
### 1. Taxi Zone Integration
Incorporating Taxi Zone data into our main trip dataset to augment information related to Pickup and Drop Boroughs and Zones.

### 2. Column Selection and Removal
- We are dropping columns deemed unnecessary for our analysis:
  - ***ehail_fee***: Contains zero records.
  - ***passenger_count***: Prone to inaccuracies as it relies on driver input.
  - ***store_and_fwd_flag, trip_type, payment_type***: Irrelevant for price prediction.
- We are also dropping columns where there is not a single entry.
  
### 3. Column Renaming
To ensure uniformity, we rename columns like 'lpep_pickup_datetime' and 'lpep_dropoff_datetime' to a standardized format.

### 4. Trip Filtering
We filter trips based on various criteria:

- ***Fare Amount***: Removing trips below the base fare of $3.
- ***Trip Distance***: Filtering out trips with negative or excessive (over 100 miles) distances.
- ***Time Constraints***: Filtering out trips with negative or excessive (over 180 minutes) time.

### 5. Handling Missing Values
Trips with any missing data across columns are dropped.

### 6. DateTime Feature Extraction
We extract additional DateTime features into new columns:

- Features like 'dayofweek', 'pickuphour', 'drophour', 'pickupdate', 'dropoffdate' are created.
- A new column identifies trips conducted during peak hours (7 am to 10 am and 5 pm to 7 pm).
- Marking trips conducted on holidays for further analysis.

### 7. Congestion Surcharge Flagging
Incorporating a flag to denote the presence of a congestion surcharge in the trip data.

## Exploratory Data Analysis

### 1. Vendor
In New York, taxi services are offered by two vendors: Creative Mobile Technologies, LLC (Vendor 1) and VeriFone Inc. (Vendor 2). The data distribution indicates a substantial disparity in trip counts between the vendors, with Vendor 2 accounting for 87% of total trips. In comparison, Vendor 1 has a notably lower contribution with 13% of total trips.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/8018376d-927d-412a-96a1-5aa08c857caa)

### 2. Rate Code
Six different rate codes are utilized at the end of trips, with the most frequent being the Standard rate (71,000 trips), followed by the Negotiated rate code (22,179 trips). The JFK rate code was applied in 2,500 trips, whereas the "Newark and Nassau" or Westchester rate codes were least utilized.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/5086b5bb-242c-4cbb-ab63-b9038b1d5dd6)

### 3. Hourly Distribution of Trip Counts
The demand for taxi trips in New York City exhibits a distinctive pattern over a 24-hour cycle. Beginning with the morning rush, starting around 6:00 AM, demand gradually increases as commuters kickstart their day, steadily rising through the late morning. Peak demand surfaces in the late afternoon, predominantly between 4:00 PM and 6:00 PM, aligning with rush hour and the conclusion of the workday, indicating a surge in transportation needs. Following this peak, the evening witnessed a decrement in demand, gradually declining post-peak hours and notably diminishing during the late-night period, bottoming out around 4:00 AM to 5:00 AM, signifying the lowest point in trip counts during the day. This cyclical trend illustrates fluctuations in demand, mirroring the city's daily rhythms and commuter patterns.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/bc7d1d3b-404f-4002-b21e-669ca8f9d8bf)

### 4. Trip Distance

Following the data cleaning process, wherein trips with negative distances or excessive ones over 100 miles were filtered out, the dataset contained 740,737 entries. The average trip distance noticeably decreased to 2.99 miles. The trip distances now range from 0.01 miles to a maximum of 95.50 miles, and the majority of data lies between 1.28 and 3.60 miles, with the median distance being 2.03 miles.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/54d04c73-7b1e-4669-91a3-62a1423ca812)

### 5. Fare Amount

Following the removal of trips below the base fare of $3, the average trip cost decreased to $13.82, with a reduced standard deviation of $11.31. The cost range now spans from $3.50 to $499.00, with the majority of trips occurring within the $7.50 to $16.00 range. The median cost after cleaning is $10.50.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/55b615fa-262c-4b88-8926-48280fa64d69)

### 6. Trip Time

After filtering out trips with negative or excessive durations (over 180 minutes), the dataset underwent cleaning, resulting in 740,737 entries. The average trip duration decreased to approximately 14.95 minutes, with a reduced standard deviation of 11.87 minutes. The trip durations now range from 1.02 minutes to a maximum of 178.55 minutes. Most trips fall within the range of 7.75 to 18.32 minutes, with the median duration standing at 11.97 minutes. This cleaning process ensured a more accurate representation of trip durations within a reasonable timeframe for analysis.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/2133b353-191d-49fb-96e5-6331c70e9854)

### 7. Trip distribution by Day

The demand for cabs follows a fluctuating pattern throughout the week, starting from Monday (0) to Sunday (6). Wednesday (3) marks the peak demand with 117,543 trips, closely followed by Tuesday (2) and Thursday (4) with 114,668 and 114,479 trips, respectively. Monday (0) records a slightly lower demand at 103,797 trips, indicating a moderate start to the week. However, the demand gradually decreases towards the weekend, notably dropping by Saturday (5) and Sunday (6) to 96,623 and 82,432 trips, respectively. This pattern suggests higher demand during midweek, tapering off towards the weekend.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/97ab5608-0ccd-4095-993b-e3dae9dc8d7f)

### 8. Number of Trips by Pickup and Dropoff LocationID

- Pickup
  ![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/4ba3f02e-a475-4275-a6cc-5c397d83315b)

- Dropoff
  ![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/7ed833ee-07c4-4958-9296-83fd487ca199)

The analysis of cab demand reveals distinctive patterns across various neighborhoods in New York City. East Harlem North and South stand out as top pickup locations, exhibiting high demand, primarily attributed to their residential settings and cultural attractions. Similarly, Central Harlem, Morningside Heights, and Forest Hills also display substantial pickup interest. In terms of dropoff locations, East Harlem North and South, Central Harlem, and Upper East Side North are prominent, signifying frequent trip completions in these areas. These neighborhoods feature a mix of residential, commercial, and cultural elements, drawing diverse trip demands likely influenced by residential density, cultural landmarks, and commercial activity. East and West of Central Park, the Upper East and Upper West Sides boast affluent residential areas, while Morningside Heights showcases a university-centric landscape. Overall, the concentrated demand in specific neighborhoods underlines their unique characteristics, driving taxi usage trends in these distinct locales.

### 9. Distance VS Fare Amount

The fare amount typically corresponds to the distance covered during a trip, following a general pattern of increase. However, upon closer inspection, anomalies emerge within the dataset, notably instances where the fare amount significantly exceeds the expected cost for a specific distance. There were numerous trips where the fare amount spiked to as high as $500, despite the distance being relatively short, approximately 5-6 miles. These anomalies suggest potential irregularities or factors beyond distance influencing fare calculations, warranting further investigation into the specific conditions or variables contributing to these unusually high fares for comparatively short distances.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/f340ddba-a529-427e-aa6a-48193bb7d42e)

### 10. Distance VS Pickup Hour

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/f9eb0f4a-5137-403c-98ac-c89787678016)

The data predominantly clusters between trip distances of 1.28 and 3.60 miles. Interestingly, throughout the day, the average trip distance hovers around 3 to 3.5 miles. Notably, there's a consistent trend of trip distances increasing steadily from the early morning until the evening. However, during the early night hours and late night, there's a noticeable rise in the distance covered by taxi trips. This shift suggests changing travel patterns or preferences during different times of the day, potentially indicating longer rides or different travel behaviors among passengers during these hours.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/15ff1819-d66a-4211-8bad-dfb4c2943953)

### 11. Fare Amount VS Week Day

Throughout the week, the fare range shows a consistent upward trend, reaching its pinnacle on Saturday at $14.20. This escalation in fares might reflect increased demand, different travel patterns, or a higher volume of longer trips, especially on Saturdays.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/3bf38e49-6bb5-44c7-a9a9-815b68cdca1c)

### 12. Tip Amount VS Hours

There's a notable surge in tips around the 5th hour, followed by a decrease, stabilizing between $1.50 and $2.00 until approximately the 15th hour. Subsequently, there's a gradual rise in tip amounts, reaching a peak around the 20th hour. These fluctuations highlight varying tipping behaviors linked to the hour of pickup. 

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/0a300409-fcf0-4b80-a96d-99a8eb1597c1)

### 13. Tip Amount VS Week Day

Throughout the week, tips exhibit intriguing fluctuations relative to the days. Initially, there's a gradual increment observed from the start of the week Monday (day 0) to day 2 (Wednesday), followed by fluctuations between Wednesday and Friday. A noticeable surge in tip amounts becomes evident from day Friday to Sunday, signifying a substantial increase toward the week's end. This pattern suggests a trend of escalating tips as the week progresses, with higher tipping tendencies over the weekend.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/909aaf10-7153-4d63-ae2f-9783f1f78564)


## Splitting Data & Dimension Reduction

In the data preparation for regression, we designated the 'fare_amount' as the target variable 'y', and the remaining columns as features 'X'. Categorical variables such as 'VendorID', 'peak_hours', 'holiday_flag', 'pickup_dayofweek', and 'pickup_hour' were one-hot encoded to enhance machine learning model training. Subsequently, the dataset underwent a 60-40 split into training and testing sets for evaluation purposes. Additionally, we conducted dimensionality reduction on the independent variables, with the first three components capturing a significant portion of the overall variance in the dataset.

![image](https://github.com/doshiharmish/NYC-Green-Taxi-Trip-Analysis/assets/16878994/a4c7a668-6897-4085-8f71-a18d81ed5666)


## Modeling

Here's a summary of the regression models implemented to predict fare amounts:

- **KNN Regression**: The best-performing model with the lowest Mean Squared Error (9.91) and the highest R-squared (0.9233), indicating superior predictive accuracy. The Mean Absolute Error is also relatively low (1.24), suggesting good model performance.

- **Random Forest Regressor**: Exhibits competitive performance with a lower Mean Squared Error (12.02) and a reasonably high R-squared (0.9069). The Mean Absolute Error is comparable to KNN Regression, indicating effective fare prediction.

- **Decision Tree**: Performs well with a moderate Mean Squared Error (11.24) and a high R-squared (0.9129). The Mean Absolute Error is reasonable, suggesting accurate fare predictions.

- **Linear Regression**: Shows acceptable performance with a higher Mean Squared Error (14.07) and a slightly lower R-squared (0.8910). The Mean Absolute Error is comparable to the Decision Tree, indicating reasonable predictive accuracy.

- **Lasso Regression**: Performs similarly to Linear Regression with a higher Mean Squared Error (14.07) and a slightly lower R-squared (0.8910). The Mean Absolute Error is consistent with Linear Regression, suggesting acceptable predictive performance.

In summary, KNN Regression stands out as the best-performing model, while Linear Regression and Lasso Regression exhibit relatively lower predictive accuracy compared to the other models. The performance metrics used (Mean Squared Error, R-squared, Mean Absolute Error) provide insights into the accuracy and reliability of the models in predicting fare amounts.



