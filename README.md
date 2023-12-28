# NYC Green Taxi Trip Analysis <https://colab.research.google.com/drive/1HxxpKTMb4ronSSNuCQ6ozPtX48lTV21V#scrollTo=f8RAkE7-g0uR>
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

## Data Preparation and Cleaning
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
