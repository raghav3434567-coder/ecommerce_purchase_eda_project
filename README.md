# E-commerce Purchases Analysis

A beginner-friendly exploratory data analysis (EDA) of an e-commerce purchases dataset using **Python, pandas and NumPy**. The notebook answers a series of practical questions about customers, payment methods and purchase behaviour.

## Project Structure

```
.
├── ecommerce_purchases_project.ipynb   # Analysis notebook
├── Ecommerce-Purchases.csv             # Dataset (10,000 rows x 14 columns)
├── PROJECT_REPORT.md                   # Detailed project report
└── README.md                           # This file
```

> The dataset file is not bundled with this write-up. Place `Ecommerce-Purchases.csv` in the same folder as the notebook before running it.

## Dataset

10,000 purchase records with 14 columns:

| Column | Type | Description |
|---|---|---|
| Address | text | Customer address |
| Lot | text | Lot code (two digits + two letters) |
| AM or PM | text | Time of day of purchase |
| Browser Info | text | Browser user-agent string |
| Company | text | Customer's company |
| Credit Card | int | Card number |
| CC Exp Date | text | Card expiry (MM/YY) |
| CC Security Code | int | Card security code |
| CC Provider | text | Card network (e.g. Mastercard) |
| Email | text | Customer email |
| Job | text | Customer job title |
| IP Address | text | Customer IP |
| Language | text | Language code (e.g. `en`, `fr`) |
| Purchase Price | float | Purchase amount |

**Privacy note:** The dataset contains card numbers, security codes, emails and IP addresses. It appears to be synthetic, but if you adapt this work to real data, anonymise or remove these fields first.

## Questions Answered

1. What are the dataset's shape, column types and first/last rows?
2. What are the min, max and average purchase prices?
3. How many customers speak French?
4. Which customers have "engineer" in their job title?
5. Who used a specific IP address or credit card number?
6. How many Mastercard customers made a purchase over 50?
7. How many purchases were made in the AM vs PM?
8. How many credit cards expire in 2020?

## Key Results

| Metric | Result |
|---|---|
| Rows / columns | 10,000 / 14 |
| Purchase price (min / max / mean) | 0.00 / 99.99 / 50.35 |
| French speakers | 1,097 |
| Job titles matching "engineer" | 531 |
| Mastercard purchases over 50 | 405 |
| AM / PM purchases | 4,932 / 5,068 |
| Cards expiring in 2020 | 988 |
| Email for IP `132.207.160.22` | `amymiller@morales-harrison.com` |
| Customer with card `4664825258997302` | No match found |

## How to Run

```bash
# 1. Install dependencies
pip install numpy pandas jupyter

# 2. Launch Jupyter
jupyter notebook ecommerce_purchases_project.ipynb
```

Then run all cells (Kernel > Restart & Run All). Python 3.12+ is recommended, because the notebook uses nested same-type quotes inside f-strings.

## Known Limitations

- The "null values" check uses `data.empty`, which only tests whether the DataFrame is empty, not whether it contains missing values. Use `data.isnull().sum()` instead.
- The engineer search `str.contains("engineer")` is case-sensitive, so titles like "Engineer, civil" would be missed. Use `case=False`.
- The expiry-year check `str.contains("/20")` works for the `MM/YY` format but is a fragile pattern match.

See `PROJECT_REPORT.md` for details and suggested improvements.

## Tech Stack

Python, pandas, NumPy, Jupyter Notebook
