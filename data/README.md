# Course data

RetailCorp is a fictional Danish online retailer of home and kitchen goods. It ships orders to customers across Denmark from two warehouses, in Aarhus and Copenhagen, using two parcel carriers. Lectures 1 to 4 follow RetailCorp through questions about delivery times, late deliveries, basket values, customer satisfaction and a newsletter test, and Lectures 6 to 9 through questions about what customers spend and which customers stop buying. The folder also holds one public dataset, an airline passenger survey, used from Lecture 3.

The company and all its data are invented. We simulated the data so that every number in the course can be reproduced and checked. Because we simulated all orders, and not only a sample, Lecture 1 can compare sample results with the true values for the whole population. With real data that is almost never possible, since the population is unknown.

The files for Lectures 1 to 4 are clean: there are no missing or impossible values. From Lecture 5 the folder also holds deliberately messy files, as data often look before cleaning.

## Files

| File | Rows | Content | First used |
|---|---|---|---|
| `orders_population.csv` | 50,000 | All orders from January to June 2026 (the population) | Lecture 1 |
| `orders_sample.csv` | 200 | A simple random sample of orders from the population | Lecture 1 |
| `survey.csv` | 300 | Customer survey before and after a website redesign | Lecture 4 |
| `newsletter_test.csv` | 10,000 | Newsletter test with two versions, A and B | Lecture 4 |
| `airline_survey.csv` | 25,976 | Airline passenger satisfaction survey (public data, see below) | Lecture 3 |
| `orders_export.csv` | 209 | The raw export behind the 200 sampled orders, from two order systems (messy) | Lecture 5 |
| `customer_survey.csv` | 612 | Customer survey linked to purchases (messy) | Workshop 5 |
| `customers_population.csv` | 60,000 | All customers with an account before 2025 who ordered in 2025 (the population) | Lecture 6 |
| `customers_sample.csv` | 1,000 | A simple random sample of customers from the population | Lecture 6 |
| `sme_ai_survey.csv` | 2,024 | Survey of Danish SMEs on the use of AI tools, linked to annual accounts (messy) | Exam assignment |

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

300 customers who answered the same survey before and after a website redesign. Each row is one customer, so the two scores in a row belong together.

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


## Order export (`orders_export.csv`, Lecture 5)

The 200 orders of `orders_sample.csv` as RetailCorp's IT department exported them. RetailCorp moved to a new order system on 1 April 2026, so the file holds the export from the old and the new system. The file is not cleaned: finding and handling its problems is the topic of Lecture 5. The data description from IT:

| Column | Content |
|---|---|
| `order_id` | Order number |
| `system` | `old` (until 31 March) or `new` (from 1 April) |
| `order_date` | Date of the order |
| `warehouse` | Warehouse that shipped the order: Aarhus or Copenhagen |
| `carrier` | Carrier A or Carrier B |
| `region` | The customer's region |
| `delivery_hours` | Hours from order to delivery |
| `late` | Delivered after more than 48 hours |
| `order_value_dkk` | Value of the order in DKK |
| `n_items` | Number of items in the order |
| `rating` | The customer's rating of the delivery, 1 to 5 stars. The old system records "no answer" as 99 |

## Customer survey (`customer_survey.csv`, Workshop 5)

In September 2026 RetailCorp invited 1,500 customers, drawn at random from the customers who ordered at least once in January to June 2026, to a short survey. 600 answered, and their answers were linked to their orders and spend in January to June. The file is not cleaned. The data description:

| Column | Content |
|---|---|
| `customer_id` | Customer number |
| `age` | Age in years |
| `region` | The customer's region |
| `channel` | Where the customer orders most often: App or Web |
| `member` | Member of the loyalty club |
| `orders_h1` | Number of orders, January to June 2026 |
| `spend_h1` | Total spend in DKK, January to June 2026, as exported from the shop system |
| `satisfaction` | "How satisfied are you with RetailCorp?", 0 to 10. "Don't know" is recorded as −1; empty if the question was skipped |
| `recommend` | "Would you recommend RetailCorp to a friend?" |

## Customers (`customers_population.csv` and `customers_sample.csv`, Lectures 6 to 9)

RetailCorp wants to understand what goes with how much a customer spends in a year, and which customers stop buying. The population is every customer who had an account before 1 January 2025 and ordered at least once in 2025: 60,000 customers. Lectures 6 to 9 use a simple random sample of 1,000 of them. As with the orders, we simulated the whole population, so the lectures can draw repeated samples from it; with real data that is not possible. The files are clean.

| Column | Type | Description |
|---|---|---|
| `customer_id` | text | Customer number, "CU-00001" and upwards |
| `age` | number | Age in years on 1 January 2025 |
| `region` | text | Danish region of the customer |
| `channel` | text | Where the customer shopped most often in 2025: "App" or "Web" |
| `member` | 0 or 1 | 1 if the customer was a member of the loyalty club on 1 January 2025 |
| `tenure_years` | number | Years since the customer's first order, on 1 January 2025 |
| `visits_2025` | number | Logged-in visits to the shop (app or website) in 2025 |
| `spend_2025` | number | Total spend in DKK in 2025 |
| `churned` | 0 or 1 | 1 if the customer placed no order in January to June 2026 |

## Exam assignment data (`sme_ai_survey.csv`)

The data for the exam assignment. The business development agency and the firms are invented, and the data are simulated for the course, so the results say nothing about real Danish firms. The file is raw, as the agency received it: it has not been checked or cleaned, and finding and handling its problems is part of the assignment. The case and the variables are described in the assignment notebook, [exam_assignment.ipynb](../exam/exam_assignment.ipynb).
