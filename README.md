# British_Airways_Flight_Report_For_January_To_June_2023

## Table of Contents
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning And Preparations](#data-cleaning-and-preparations)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results And Findings](#results-and-findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)

### Project Overview

British Airways, like many airlines, captures a wealth of data on its flights.  This project aimed to leverage this data to improve two key areas: operational efficiency and customer satisfaction.

To achieve this, I embarked on a deep dive into the provided British Airways flight data. My goal was to extract valuable insights that could inform better decision-making across the company. This included asking questions like:

- Fuel Efficiency: Which aircraft manufacturers deliver the best fuel economy on our routes? Does British Airways currently favor such manufacturers in its fleet selection?

- Passenger Trends: When do we see the highest flight cancellation rates? Which destinations are most popular with our passengers?

- Revenue Analysis: What is the financial impact of baggage delays on our operations? How does passenger volume fluctuate throughout the year?

By answering these questions and more, I aimed to identify opportunities to optimize flight schedules, reduce costs, and ultimately enhance the travel experience for our customers.

### Data Source

The primary data source used for this Analysis was gotten from British Flight Data source via https://drive.google.com/file/d/1CNHQuwyWB6v1YPNZAqzA5R46u2hiQfVd/view

### Tools
- EXCEL: Used for Data cleaning [Download Here](https://www.microsoft.com/en-us/microsoft-365/excel)
- SQL SERVER: Used for Data Analysis [Download Here](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
- POWER BI: Used for Creating a report and visualization [Download Here](https://www.microsoft.com/en-us/download/details.aspx?id=58494)

### Data Cleaning And Preparations
I performed the following task at the initial phase of the data preparation
- Data downloading and inspection/studying
- Handling missing data
- Removing duplicates
- Data cleaning and formatting

### Exploratory Data Analysis
This involved exploring the flight data to answer key questions like;

1. Which manufacturer has the best aircrafts in terms of fuel efficiency?
2. Does British Airways tend to use aircraft from manufacturers known for their superior fuel efficiency more frequently?
3. Which month did passengers cancel flights the most?
4. Which city do passengers travel to the most?
5. What is the revenue generated from baggage overtime?
6. What is the average number of passengers like for each month?

### Data Analysis
My analysis was done on SQL server with some of the following codes: These codes answers question 1,3,5 respectively

```sql
SELECT manufacturer,
	AVG(fuel_efficiency)AS Avg_fuel_efficiency
FROM ba_fuel_efficiency
GROUP BY manufacturer
ORDER BY AVG(fuel_efficiency)
LIMIT 1;

SELECT bfe.manufacturer,
	COUNT (ba.flight_id) AS Frequently_used,
	AVG (fuel_efficiency) AS Avg_fuel_efficiency
FROM ba_fuel_efficiency AS bfe
LEFT JOIN ba_aircrafts AS ba ON ba.ac_subtype = bfe.ac_subtype
GROUP BY bfe.manufacturer
ORDER BY COUNT (ba.flight_id) DESC;

SELECT TO_CHAR(bf.actual_flight_date,'Month') AS MONTHS,
	COUNT (bf.status) AS Number_of_cancelled_flights
FROM ba_flights AS bf
WHERE bf.status = 'Cancelled'
GROUP BY TO_CHAR(bf.actual_flight_date,'Month'),EXTRACT (MONTH FROM bf.actual_flight_date)
ORDER BY COUNT (bf.status) DESC
LIMIT 1;

SELECT bfr.arrival_city,
	SUM (bf.total_passengers) AS Total_passengers_travelled
FROM ba_flights AS bf
INNER JOIN ba_flight_routes AS bfr ON bfr.flight_number = bf.flight_number
GROUP BY bfr.arrival_city
ORDER BY SUM (bf.total_passengers)DESC
LIMIT 1;

SELECT '$' || SUM(bf.revenue_from_baggage) AS Total_revenue
FROM ba_flights AS bf;

SELECT TO_CHAR(bf.actual_flight_date,'Month') AS MONTHS,
	ROUND(AVG (bf.total_passengers),0) AS Average_number_of_pasengers
FROM ba_flights AS bf
GROUP BY TO_CHAR(bf.actual_flight_date,'Month'),EXTRACT (MONTH FROM bf.actual_flight_date)
ORDER BY EXTRACT (MONTH FROM bf.actual_flight_date);
```
### Results And Findings
The Analysis results are summarised as follows:
1. It was established that Mitsubishi Manufacturer has the best aircraft in terms of fuel efficiency although it's not frequently used.
2. OBSERVATION: Although Mitsubishi aircraft Manufacturer has the best fuel efficiency, However, Boeing aircraft Manufacturer is topping the list of being used more frequently
 - INSIGHTS 1: This could be Maybe because it has more ability to carry more passengers or it accomodates more baggage weight or it applies to more routes or even it's more comfortable and with experienced workers(air hostes,pilot etc) or their fare prices are just the best as compared to others 
 - INSIGHTS 2: Also it's important to note that the difference between Boeing aircrafts and Mitsubishi in terms of the fuel efficiency isn't so much actually less than 5%
3. OBSERVATION: Surprisingly, The month of April had the most cancelled flight. 
 -INSIGHTS 1: Perphaps it's because it's a festive period and most people might want to spend time with their families at home observing the holiday
 -INSIGHT 2: Or the fare price increased during this holiday period which led them to cancel
4. OBSERVATION: London was the most travelled city as comparedto other cites
5. OBSERVATION: Over a period of 6 months the revenue generated from baggage sumed up to $34,084,625
6. OBSERVATION: Each month has a unqiue avg. number of passengers with January having the highest and June having the lowest number of passengers. 
 - INSIGHT: Although June data record only stopped at half of the month, this could be one of the reasons why it's having the lowest Avg. number of passengers

### Recommendations
- Other Manufacturers should dig deep(maybe create a market survey,Questionnaires)to find out the market secret of Boeing aircraft and other competitors and create room for improvement
- Because people love discount, perphaps some discount or incentives should be applied to each manufacturers aircraft to attract traffic to the manufacturer.

### Limitations
- The absence of incomplete data for the month of June limited the scope of our analysis and prevented a complete quarterly evaluation

 

 



