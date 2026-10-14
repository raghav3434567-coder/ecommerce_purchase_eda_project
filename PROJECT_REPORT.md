# Project Report: E-commerce Purchases Analysis

## 1. Introduction

This project performs exploratory data analysis on an e-commerce purchases dataset. The goal is to practise core pandas skills (loading data, inspecting structure, filtering, boolean masking and aggregation) while answering concrete business-style questions about customers and their purchases.

## 2. Objectives

- Understand the structure and quality of the dataset.
- Summarise purchase prices.
- Segment customers by language, job title, card provider and time of purchase.
- Look up individual customers by IP address and card number.
- Identify cards by expiry year.

## 3. Tools and Technologies

| Tool | Purpose |
|---|---|
| Python | Programming language |
| pandas | Data loading, filtering, aggregation |
| NumPy | Numerical support |
| Jupyter Notebook | Interactive development |

## 4. Dataset Description

The dataset (`Ecommerce-Purchases.csv`) has **10,000 rows and 14 columns**. Two columns are integers (`Credit Card`, `CC Security Code`), one is a float (`Purchase Price`), and the remaining eleven are text (object) columns: Address, Lot, AM or PM, Browser Info, Company, CC Exp Date, CC Provider, Email, Job, IP Address and Language.

The data looks synthetic (generated names, addresses and card numbers), which makes it safe for learning purposes.

## 5. Methodology

1. **Load** the CSV with `pd.read_csv`.
2. **Inspect** with `head(10)`, `tail(10)`, `dtypes` and `shape`.
3. **Summarise** numeric data. The sensitive columns `Credit Card` and `CC Security Code` were dropped first so that `min`, `max` and `mean` only describe meaningful numeric data (Purchase Price).
4. **Filter** using boolean masks and string methods:
   - `data['Language'] == 'fr'`
   - `data['Job'].str.contains('engineer')`
   - `(data['CC Provider'] == 'Mastercard') & (data['Purchase Price'] > 50)`
   - `data['AM or PM'] == 'AM'` / `'PM'`
   - `data['CC Exp Date'].str.contains('/20')`
5. **Look up** records by `IP Address` and `Credit Card`.

## 6. Findings

### 6.1 Dataset size
10,000 purchases across 14 attributes.

### 6.2 Purchase price
| Statistic | Value |
|---|---|
| Minimum | 0.00 |
| Maximum | 99.99 |
| Mean | 50.35 |

Prices span the range 0 to 100 and the mean sits almost exactly at the midpoint, which suggests an approximately uniform distribution (typical of synthetic data). This was inferred from the summary statistics only; the notebook does not plot the distribution.

### 6.3 Customer segments
| Segment | Count | Share of 10,000 |
|---|---|---|
| French speakers (`fr`) | 1,097 | ~11.0% |
| Job title contains "engineer" | 531 | ~5.3% |
| Mastercard and price > 50 | 405 | ~4.1% |
| Cards expiring in 2020 | 988 | ~9.9% |

### 6.4 Time of purchase
| Period | Purchases | Share |
|---|---|---|
| AM | 4,932 | 49.3% |
| PM | 5,068 | 50.7% |

Purchases are split almost evenly, with a very slight lean towards PM.

### 6.5 Customer lookups
- IP address `132.207.160.22` belongs to `amymiller@morales-harrison.com` (record index 2).
- Credit card `4664825258997302` returned an empty result, meaning no customer in the dataset used that card number.

## 7. Issues and Recommended Improvements

| # | Issue | Impact | Fix |
|---|---|---|---|
| 1 | The null check uses `data.empty`, which only detects an entirely empty DataFrame. The printed message "no null values" is therefore not backed by a real check. | Missing data could go unnoticed. | `data.isnull().sum()` or `data.info()` |
| 2 | `str.contains("engineer")` is case-sensitive. | Titles starting with "Engineer" (e.g. "Engineer, civil") are excluded, so 531 is likely an undercount. | `str.contains("engineer", case=False, na=False)` |
| 3 | Expiry year found via `str.contains("/20")`. | Works for `MM/YY`, but matches by substring rather than parsing. | `data['CC Exp Date'].str[-2:] == '20'` |
| 4 | Sensitive data (card numbers, security codes, emails, IPs) is displayed in notebook output. | Privacy risk if real data is ever used. | Mask or drop these columns before sharing. |
| 5 | Nested same-type quotes inside f-strings (e.g. `data['Language']=='fr'` within `f'...'`). | Only valid on Python 3.12+. | Use double quotes outside, single inside. |
| 6 | Several cells print messages and raw output in a way that is hard to read (e.g. the "No. Email" header followed by a Series). | Readability. | Use `.to_string()` or show a tidy DataFrame. |
| 7 | No visualisations. | Patterns such as price distribution are not shown. | Add histograms and bar charts. |

## 8. Conclusion

The notebook successfully demonstrates core pandas filtering and summarising techniques on a clean, 10,000-row purchases dataset. The data shows balanced AM/PM activity, a roughly uniform price distribution between 0 and 100, and French as a notable language segment at about 11% of customers. The main improvements needed are a proper missing-value check, case-insensitive text matching, and handling of sensitive fields.

## 9. Future Work

- Add visualisations (price histogram, purchases by language, card provider share).
- Compute average purchase price by card provider, language, job category and AM/PM.
- Extract the email domain and browser type for deeper segmentation.
- Parse `CC Exp Date` into a real date and analyse upcoming expiries.
- Check for duplicate cards and IP addresses (possible fraud signals).
- Wrap the repeated filtering logic in reusable functions.
