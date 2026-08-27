# 🤖 AI-Powered E-commerce Data Analyst

An AI-powered e-commerce data analysis system that allows users to ask business questions in natural language. The system uses **Google Gemini** to generate SQL queries, executes them against a **Microsoft SQL Server** e-commerce database, and then uses Gemini to convert the query results into meaningful business insights.

> **Project Status:** Working prototype in Jupyter Notebook

---

## 📌 Project Overview

Traditional data analysis often requires users to understand SQL before they can retrieve information from a database.

This project provides a natural-language interface where users can ask questions such as:

* "What are the top 5 products by revenue?"
* "What is the total revenue by division?"
* "Find the bottom 5 products by revenue in each division."
* "Calculate monthly revenue for 2017."
* "Calculate month-over-month revenue growth."
* "Calculate year-over-year revenue growth."

The system automatically:

1. Understands the user's question
2. Generates a SQL query
3. Validates the SQL query
4. Executes the query against SQL Server
5. Converts the result into a DataFrame
6. Sends the result to Gemini
7. Generates business insights and recommendations

---

#### 🚀 Google Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)]([YOUR_GOOGLE_COLAB_LINK](https://colab.research.google.com/drive/18VegDvLUmt-iGxR9qxkgXDeeFYt-r4-z?usp=sharing))

[👉 Open sql_agent.ipynb in Google Colab]([YOUR_GOOGLE_COLAB_LINK](https://colab.research.google.com/drive/18VegDvLUmt-iGxR9qxkgXDeeFYt-r4-z?usp=sharing))


## 🏗️ Architecture

```text
                    User Question
                          │
                          ▼
                 ┌─────────────────┐
                 │  Google Gemini  │
                 │  SQL Generation │
                 └────────┬────────┘
                          │
                          ▼
                   Generated SQL
                          │
                          ▼
                 ┌─────────────────┐
                 │  SQL Validation │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │  SQL Server     │
                 │  E-commerce DB  │
                 └────────┬────────┘
                          │
                          ▼
                      Query Data
                          │
                          ▼
                 ┌─────────────────┐
                 │     Pandas      │
                 │    DataFrame    │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │  Google Gemini  │
                 │  Data Analysis  │
                 └────────┬────────┘
                          │
                          ▼
               Business Insights
               & Recommendations
```

---

## 🚀 Key Features

### Natural Language to SQL

Users can ask questions using normal business language instead of writing SQL manually.

Example:

```text
"What are the top 5 products by revenue?"
```

The system generates SQL similar to:

```sql
SELECT TOP 5
    i.item_name,
    SUM(f.total_price) AS total_revenue
FROM fact_table f
JOIN item_dim i
    ON f.item_key = i.item_key
GROUP BY
    i.item_key,
    i.item_name
ORDER BY
    total_revenue DESC;
```

---

### SQL Validation

The system validates generated SQL before execution.

It is designed to allow analytical queries while blocking potentially destructive operations such as:

```text
INSERT
UPDATE
DELETE
DROP
ALTER
TRUNCATE
CREATE
EXEC
```

This provides an additional safety layer when executing LLM-generated SQL.

---

### Automated Data Analysis

After SQL execution, the returned data is sent back to Gemini for analysis.

The analytical layer generates:

* Key findings
* Trends
* Top/low performers
* Business insights
* Recommendations
* Data limitations

---

### MoM Analysis

The system can generate month-over-month revenue analysis.

Example:

```text
Calculate monthly revenue for 2017
and month-over-month revenue growth percentage.
```

The SQL generation layer uses the `time_dim` table and SQL window functions such as `LAG()` when appropriate.

---

### YoY Analysis

The system can also perform year-over-year analysis.

Example:

```text
Calculate monthly revenue and
year-over-year growth percentage for 2017.
```

The system compares the current period with the corresponding period from the previous year.

---

## 🗄️ Database Schema

The project uses an e-commerce database in Microsoft SQL Server.

### fact_table

```text
payment_key
customer_key
time_key
item_key
store_key
quantity
unit
unit_price
total_price
```

### item_dim

```text
item_key
item_name
desc
unit_price
man_country
supplier
unit
```

### store_dim

```text
store_key
division
district
upzila
```

### customer_dim

```text
coustomer_key
name
contact_no
nid
```

### time_dim

```text
time_key
date
hour
day
week
month
quarter
year
```

### Trans_dim

```text
payment_key
trans_type
bank_name
```

---

## 🛠️ Technology Stack

| Technology           | Purpose                          |
| -------------------- | -------------------------------- |
| Python               | Application logic                |
| Jupyter Notebook     | Development and demonstration    |
| Google Gemini        | SQL generation and data analysis |
| LangChain            | LLM integration                  |
| Pandas               | Data processing                  |
| SQLAlchemy           | Database connectivity            |
| PyODBC               | SQL Server connectivity          |
| Microsoft SQL Server | E-commerce database              |
| Regex                | SQL cleaning and validation      |

---

## 📂 Project Structure

```text
ecommerce-ai-data-analyst/
│
├── ecommerce_ai_agent.ipynb
├── README.md
├── requirements.txt
├── config.example.py
└── .gitignore
```

---

## ⚙️ Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/ecommerce-ai-data-analyst.git
```

Move into the project:

```bash
cd ecommerce-ai-data-analyst
```

Install the required packages:

```bash
pip install -r requirements.txt
```

Start Jupyter:

```bash
jupyter notebook
```

Open:

```text
ecommerce_ai_agent.ipynb
```

---

## 🔑 Configuration

The project requires a Google Gemini API key and a connection to the SQL Server database.

Create a local `config.py` file:

```python
GEMINI_API_KEY = "YOUR_GEMINI_API_KEY"

DB_SERVER = r"YOUR_SQL_SERVER"
DB_NAME = "ecommerce"
DB_DRIVER = "ODBC Driver 17 for SQL Server"
```


## ▶️ Example Usage

After initializing the database connection and Gemini model:

```python
result = ask_data_analyst(
    "What are the top 5 products by revenue?"
)
```

The system returns:

```python
{
    "sql": "...",
    "data": [...],
    "analysis": "..."
}
```

You can display the generated SQL:

```python
print(result["sql"])
```

Display the data:

```python
display(pd.DataFrame(result["data"]))
```

Display the AI-generated analysis:

```python
print(result["analysis"])
```

---

## 📊 Example Output

### User Question

```text
What are the top 5 products by revenue?
```

### Result

```text
Red Bull 12oz
1,305,700

K Cups Daily Chef Columbian Supremo
1,245,394

K Cups Original Donut Shop Med. Roast
1,188,843

K Cups Dunkin Donuts Medium Roast
1,109,760

K Cups Folgers Lively Columbian
1,042,406
```

### AI Analysis

The system identifies the highest-performing products, revenue distribution, performance differences, and provides data-supported recommendations.

---

## 🔍 Example Business Questions

The system can answer questions such as:

```text
What is the total revenue?

What are the top 5 products by revenue?

What are the bottom 5 products by revenue?

What is revenue by division?

Which division generated the highest revenue?

Calculate monthly revenue for 2017.

Calculate month-over-month revenue growth.

Calculate year-over-year revenue growth.

Which products generate the most revenue in each division?

What are the highest and lowest performing products?
```

---

## 🔐 Security Considerations

Because the project executes SQL generated by an LLM, SQL validation is included before database execution.

The project also follows these principles:

* Only analytical SQL should be executed.
* Destructive SQL commands are blocked.
* API keys should be stored outside the public repository.
* Database credentials should never be committed to GitHub.
* Generated SQL should be reviewed during development.

---

## ⚠️ Limitations

This is currently a **Jupyter-based prototype**.

Current limitations include:

* Requires a local SQL Server database.
* Requires a Gemini API key.
* SQL generation depends on the LLM.
* Complex analytical questions may require prompt refinement.
* The system currently focuses on the defined e-commerce schema.
* No public deployment is included in the current version.

---

## 🔮 Future Improvements

Planned improvements include:

* FastAPI REST API
* Interactive web interface
* Advanced SQL validation
* Query history
* Visualization generation
* Automatic chart recommendations
* More advanced statistical analysis
* Conversational memory
* Authentication
* Cloud database integration
* Production deployment

---

## 🎯 Learning Outcomes

This project demonstrates practical experience with:

* SQL
* Python
* SQL Server
* Pandas
* Database connectivity
* LangChain
* Large Language Models
* Natural Language to SQL
* SQL validation
* Data analysis
* MoM analysis
* YoY analysis
* Business intelligence
* AI-assisted analytics

---

## 👨‍💻 Author

**Tushar Upadhyay**

Data Analyst | Python | SQL | Power BI | Data Science | Generative AI

---

## ⭐ Project Goal

The goal of this project is to demonstrate how **Generative AI can be integrated with traditional data analytics workflows** to allow business users to interact with structured data using natural language.
