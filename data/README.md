# Data for Lectures 1 to 4

RetailCorp is a fictional Danish online retailer of home and kitchen goods. It ships orders to customers across Denmark from two warehouses, in Aarhus and Copenhagen, using two parcel carriers. Lectures 1 to 4 follow RetailCorp through questions about delivery times, late deliveries, basket values, customer satisfaction and a newsletter test. The folder also holds one public dataset, an airline passenger survey, used from Lecture 3.

The company and all its data are invented. We simulated the data so that every number in the course can be reproduced and checked. Because we simulated all orders, and not only a sample, Lecture 1 can compare sample results with the true values for the whole population. With real data that is almost never possible, since the population is unknown.

The files are clean: there are no missing or impossible values. Messy data come later in the course, in separate files.

## Files

| File | Rows | Content | First used |
|---|---|---|---|
| `orders_population.csv` | 50,000 | All orders from January to June 2026 (the population) | Lecture 1 |
| `orders_sample.csv` | 200 | A simple random sample of orders from the population | Lecture 1 |
| `survey.csv` | 400 | Customer survey before and after a website redesign | Lecture 4 |
| `newsletter_test.csv` | 10,000 | Newsletter test with two versions, A and B | Lecture 4 |
| `airline_survey.csv` | 25,976 | Airline passenger satisfaction survey (public data, see below) | Lecture 3 |

## Loading a file

```python
import pandas as pd
url = "https://raw.githubusercontent.com/aaubs/AQM-2026/main/data/"
sample = pd.read_csv(url + "orders_sample.csv")
```

## Orders (`orders_population.csv` and `orders_sample.csv`)

One row per order.

| Column | Type | Description |
|---|---|---|
| `order_id` | text | Order number, "RC-000001" and upwards |
| `order_date` | date | Date of the order, 2026-01-05 to 2026-06-28 |
| `warehouse` | text | "Aarhus" or "Copenhagen" |
| `carrier` | text | "Carrier A" or "Carrier B" |
| `region` | text | Danish region of the customer: Hovedstaden, Midtjylland, Syddanmark, Sjælland or Nordjylland |
| `delivery_hours` | number | Hours from order to delivery |
| `late` | 0 or 1 | 1 if the order took more than 48 hours |
| `order_value_dkk` | number | Basket value in DKK |

## Survey (`survey.csv`)

400 customers who answered the same survey before and after a website redesign.

| Column | Type | Description |
|---|---|---|
| `customer_id` | text | Customer number, "C-0001" and upwards |
| `satisfaction_before` | 1 to 10 | Satisfaction before the redesign |
| `satisfaction_after` | 1 to 10 | Satisfaction after the redesign |
| `recommend` | 0 or 1 | 1 if the customer would recommend RetailCorp (asked after the redesign) |

## Newsletter test (`newsletter_test.csv`)

| Column | Type | Description |
|---|---|---|
| `recipient_id` | text | Recipient number, "R-00001" and upwards |
| `version` | text | "A" or "B", 5,000 recipients each |
| `converted` | 0 or 1 | 1 if the recipient bought something within seven days |

## Airline passenger survey (`airline_survey.csv`)

A public dataset, "Airline Passenger Satisfaction" from Kaggle (the test file, 25,976 passengers). Unlike the RetailCorp files it is not simulated and not cleaned. The source does not document the airline, the period or how passengers were selected, so it is unclear which population the data describe. We removed one leftover row-number column without a name; everything else is as published.

| Column | Type | Description |
|---|---|---|
| `id` | number | Passenger number |
| `Gender`, `Customer Type`, `Type of Travel`, `Class` | text | Passenger characteristics |
| `Age`, `Flight Distance` | number | Age in years, flight distance |
| `Inflight wifi service` to `Cleanliness` | 0 to 5 | 14 service ratings; 0 means "not rated" in some columns |
| `Departure Delay in Minutes`, `Arrival Delay in Minutes` | number | Delays in minutes |
| `satisfaction` | text | "satisfied" or "neutral or dissatisfied" |

Known issues:

- Rating 0 means "not rated" in six columns: `Inflight wifi service` (813 passengers), `Departure/Arrival time convenient` (1,381), `Ease of Online booking` (1,195), `Online boarding` (652), `Leg room service` (126) and `Food and drink` (25). Counted as a score, a 0 pulls the mean down. Lecture 3 uses only rating columns without zeros (`Seat comfort`, `Baggage handling`, `Checkin service`, `Gate location`).
- `Arrival Delay in Minutes` is missing for 83 passengers.
- `satisfaction` puts "neutral" and "dissatisfied" together.
- `Customer Type` is written "Loyal Customer" and "disloyal Customer" (lower case d).
- 1,916 passengers are under 18; the youngest is 7.

