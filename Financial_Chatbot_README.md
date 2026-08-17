# GFC Financial Chatbot

## Conversational Financial Data Assistant

GFC Financial Chatbot is a lightweight financial data assistant built with **Python**, **Flask**, and **Pandas**.

The application provides a conversational interface for querying predefined financial data for **Apple, Microsoft, and Tesla** across the years **2021–2023**.

Users can ask natural-language questions about metrics such as **Total Revenue, Net Income, and Operating Cash Flow**. A rule-based intent parser identifies relevant keywords, retrieves the matching record from a structured CSV dataset, and returns the result through a simple web-based chat interface.

This project demonstrates how a structured financial dataset can be exposed through a conversational web application without requiring a large NLP or machine-learning model.

---

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Solution](#solution)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [How It Works](#how-it-works)
- [Query Processing Workflow](#query-processing-workflow)
- [Supported Data](#supported-data)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Running the Application](#running-the-application)
- [Usage](#usage)
- [Example Queries](#example-queries)
- [Engineering Highlights](#engineering-highlights)
- [Interview Talking Points](#interview-talking-points)
- [Technical Design Considerations](#technical-design-considerations)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)
- [Screenshots](#screenshots)
- [Security](#security)
- [Author](#author)
- [License](#license)

---

## Overview

Financial reports and structured datasets can contain large amounts of information that are difficult for non-technical users to navigate directly.

A user may know what information they want but not where to find it in a spreadsheet or dataset.

For example:

```text
What was Apple's total revenue in 2023?
```

Instead of manually locating the company, year, and metric in a dataset, the chatbot accepts the question through a conversational interface and returns the corresponding financial value.

The project focuses on a simple and transparent architecture:

```text
User Question
      |
      v
Flask Web Application
      |
      v
Rule-Based Query Parser
      |
      v
Pandas DataFrame
      |
      v
Financial Record
      |
      v
Formatted Response
```

---

## Problem Statement

Financial datasets are often structured for analysis rather than direct interaction.

Users may need to:

- Identify the correct company.
- Identify the required financial metric.
- Identify the relevant year.
- Search through rows and columns.
- Interpret the resulting value.

This project explores how a conversational interface can simplify access to predefined financial information.

The objective is not to build a general-purpose financial AI system, but to demonstrate a practical, lightweight approach to structured financial data retrieval.

---

## Solution

The chatbot loads a prepared financial dataset into memory using Pandas.

When a user submits a question:

1. Flask receives the request.
2. The application passes the question to the response-generation logic.
3. The rule-based engine identifies relevant keywords.
4. The application determines the company, metric, and year from the query.
5. Pandas filters the dataset.
6. The matching financial record is retrieved.
7. A human-readable response is generated.
8. Flask returns the response to the web interface.

The workflow is intentionally simple and transparent, making it easy to understand, debug, and extend.

---

# Key Features

## Conversational Queries

Users can ask financial questions using natural-language phrasing.

Example:

```text
What was the total revenue for Apple in 2023?
```

---

## Company Coverage

The current dataset covers:

- Apple
- Microsoft
- Tesla

---

## Financial Metric Coverage

The chatbot is designed to answer questions about:

- Total Revenue
- Net Income
- Operating Cash Flow

---

## Historical Data

The documented dataset covers:

```text
2021
2022
2023
```

---

## Rule-Based Intent Parsing

The chatbot uses keyword and string matching rather than a complex NLP or machine-learning model.

For example, a query containing:

```text
Apple
Revenue
2023
```

can be mapped to the corresponding company, metric, and year in the dataset.

---

## Fast Data Retrieval

The financial CSV is loaded into a Pandas DataFrame and kept in memory while the application is running.

This avoids repeatedly loading the dataset for each request.

---

## Lightweight Flask Backend

Flask provides the HTTP routing and application backend while keeping the overall implementation simple.

---

## Responsive Web Interface

The frontend uses standard:

- HTML
- CSS

to provide a simple conversational interface.

---

# Architecture

```text
                         GFC FINANCIAL CHATBOT
                                  |
                                  v
                    +---------------------------+
                    |       Web Interface       |
                    |        index.html         |
                    +-------------+-------------+
                                  |
                                  | POST Request
                                  v
                    +---------------------------+
                    |       Flask Backend       |
                    |          app.py           |
                    +-------------+-------------+
                                  |
                                  v
                    +---------------------------+
                    |    Rule-Based Parser      |
                    |   Keyword / String Match  |
                    +-------------+-------------+
                                  |
                    +-------------+-------------+
                    |                           |
                    v                           v
              Company Detection          Metric Detection
                    |                           |
                    +-------------+-------------+
                                  |
                                  v
                            Year Detection
                                  |
                                  v
                    +---------------------------+
                    |       Pandas DataFrame    |
                    |                           |
                    | final_financial_data.csv  |
                    +-------------+-------------+
                                  |
                                  v
                    +---------------------------+
                    |    Financial Record       |
                    +-------------+-------------+
                                  |
                                  v
                    +---------------------------+
                    |   Response Formatting    |
                    +-------------+-------------+
                                  |
                                  v
                    +---------------------------+
                    |      Chat Interface       |
                    +---------------------------+
```

---

# How It Works

## 1. Data Loading

When the Flask application starts, the financial dataset is loaded from:

```text
data/final_financial_data.csv
```

The data is loaded into a Pandas DataFrame.

```text
CSV Dataset
     |
     v
Pandas
     |
     v
DataFrame
```

Keeping the dataset in memory allows the application to query the existing DataFrame for subsequent user requests.

---

## 2. User Interaction

The user enters a question through:

```text
templates/index.html
```

Example:

```text
How did Microsoft's net income change from 2021 to 2023?
```

The frontend sends the request to the Flask backend.

---

## 3. Flask Request Handling

The Flask application receives the user's query through the backend route defined in:

```text
app.py
```

The query is passed to the application's response-generation logic.

---

## 4. Rule-Based Query Parsing

The core response logic uses the `get_response` function.

The parser examines the user's query for relevant keywords.

For example:

```text
Company:
Apple

Metric:
Revenue

Year:
2023
```

The parser uses these values to determine which record should be retrieved.

---

## 5. Data Filtering

Once the relevant values have been identified, the application filters the Pandas DataFrame.

Conceptually:

```text
Company == "Apple"
AND
Year == 2023
AND
Metric == "Total Revenue"
```

The matching record is then retrieved from the dataset.

---

## 6. Response Generation

The retrieved financial value is converted into a human-readable response.

For example:

```text
Apple's total revenue for 2023 was ...
```

If the requested information cannot be found, the application returns a fallback response.

---

## 7. Response Display

The formatted response is returned to the frontend and displayed in the chatbot interface.

The complete request-response cycle is therefore:

```text
Question
   |
   v
Flask
   |
   v
Keyword Extraction
   |
   v
Dataset Filtering
   |
   v
Financial Value
   |
   v
Formatted Response
   |
   v
Chat Interface
```

---

# Query Processing Workflow

The rule-based query processing approach can be represented as:

```text
                    User Query
                        |
                        v
              Normalize / Inspect Text
                        |
                        v
              Identify Company
                        |
                        v
              Identify Financial Metric
                        |
                        v
                  Identify Year
                        |
                        v
              Filter Pandas DataFrame
                        |
                        v
                Matching Record?
                  /           \
                Yes            No
                 |              |
                 v              v
        Format Financial      Fallback
             Response         Response
                 |
                 v
             User Output
```

This approach is intentionally deterministic and easy to trace during development.

---

# Supported Data

The current chatbot is designed around a predefined financial dataset.

| Category | Current Scope |
|---|---|
| Companies | Apple, Microsoft, Tesla |
| Years | 2021–2023 |
| Revenue | Supported |
| Net Income | Supported |
| Operating Cash Flow | Supported |
| Data Source | `final_financial_data.csv` |
| Storage | CSV loaded into Pandas |

The chatbot's knowledge is limited to the information contained in the configured dataset and the query patterns implemented in the application.

---

# Technology Stack

| Technology | Purpose |
|---|---|
| **Python** | Core programming language |
| **Flask** | Backend web framework |
| **Pandas** | Data loading and filtering |
| **HTML** | Web interface structure |
| **CSS** | Interface styling |
| **CSV** | Structured financial data storage |
| **Visual Studio Code** | Development environment |

---

# Project Structure

```text
GFC-Financial-Chatbot/
│
├── app.py
│
├── templates/
│   └── index.html
│
├── data/
│   └── final_financial_data.csv
│
├── static/
│
├── requirements.txt
├── .gitignore
└── README.md
```

## File Description

| File / Directory | Description |
|---|---|
| `app.py` | Main Flask application, routing, and chatbot response logic |
| `templates/index.html` | HTML template for the chatbot interface |
| `data/final_financial_data.csv` | Pre-cleaned financial dataset |
| `static/` | Static assets such as CSS and frontend resources |
| `requirements.txt` | Python dependencies |
| `.gitignore` | Files and directories excluded from Git |
| `README.md` | Project documentation |

---

# Installation

## Prerequisites

Make sure the following are installed:

- Python 3.x
- Git
- A web browser

---

## 1. Clone the Repository

```bash
git clone https://github.com/costaspinto/Financial_Chatbot.git
cd Financial_Chatbot
```

---

## 2. Create a Virtual Environment

### Windows

```bash
python -m venv venv
.\venv\Scripts\activate
```

### macOS / Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

---

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# Running the Application

Start the Flask application:

```bash
python app.py
```

The application should be available at:

```text
http://127.0.0.1:5000
```

Open the URL in a web browser to access the chatbot.

---

# Usage

## Step 1 — Start the Application

Run:

```bash
python app.py
```

## Step 2 — Open the Web Interface

Navigate to:

```text
http://127.0.0.1:5000
```

## Step 3 — Enter a Financial Question

Ask a question about the supported companies, metrics, and years.

## Step 4 — Review the Response

The application identifies the relevant parameters, queries the dataset, and displays the corresponding financial information.

---

# Example Queries

Examples of supported query patterns include:

```text
What was the total revenue for Apple in 2023?
```

```text
What was Apple's total revenue in 2023?
```

```text
How did net income change for Microsoft from 2021 to 2023?
```

```text
Tell me about Tesla's operating cash flow.
```

The exact questions supported depend on the keyword and rule patterns implemented in the application.

---

# Engineering Highlights

## Deterministic Query Processing

The chatbot uses a transparent rule-based approach rather than a large NLP or machine-learning model.

This makes the system:

- Lightweight
- Fast
- Easy to debug
- Easy to understand
- Simple to extend for additional predefined query patterns

---

## In-Memory Data Processing

The CSV dataset is loaded into a Pandas DataFrame when the application starts.

This allows subsequent queries to operate directly on the in-memory dataset.

---

## Separation of Concerns

The repository separates:

```text
Backend Logic
     |
     +-- app.py

Frontend
     |
     +-- templates/index.html

Data
     |
     +-- data/final_financial_data.csv

Static Assets
     |
     +-- static/
```

This structure makes the project easier to maintain and extend.

---

## Web Application Development

The project demonstrates the integration of:

```text
Python
   +
Flask
   +
Pandas
   +
HTML/CSS
   =
Interactive Data Application
```

---

# Interview Talking Points

This project can demonstrate practical experience with:

- Python
- Flask
- Pandas
- Data preprocessing
- Structured data retrieval
- Rule-based intent parsing
- Keyword extraction
- Web application development
- HTTP request handling
- HTML/CSS integration
- Data-driven application design
- Separation of frontend and backend concerns

---

## 60-Second Interview Explanation

> GFC Financial Chatbot is a lightweight conversational interface for querying structured financial data. I built it using Python, Flask, and Pandas. The application loads a predefined financial dataset into a Pandas DataFrame when the Flask server starts. When a user submits a question, the backend uses rule-based keyword matching to identify the company, financial metric, and year. It then filters the DataFrame to retrieve the relevant financial record and formats the result into a human-readable response. I intentionally used a deterministic rule-based approach rather than a complex NLP model because the dataset and query scope were predefined. This kept the application lightweight, explainable, and easy to extend.

---

# Technical Design Considerations

## Why Flask?

Flask provides a lightweight backend framework for exposing the chatbot functionality through HTTP routes without introducing unnecessary application complexity.

## Why Pandas?

Pandas provides a convenient interface for loading, filtering, and working with the structured CSV dataset.

## Why a Rule-Based Approach?

The application has a predefined scope of companies, metrics, years, and query patterns.

A rule-based approach is therefore simple, deterministic, and easy to debug.

It also provides a clear baseline before introducing more advanced NLP techniques.

## Why Load the CSV Into Memory?

Loading the dataset during application startup avoids repeatedly reading the CSV file for every user request.

This is suitable for the relatively small, static dataset used by the project.

## Is This a General-Purpose Financial AI?

No.

The application is intentionally scoped to predefined financial data and rule-based query patterns.

It should not be presented as a real-time financial advisory system or general-purpose financial LLM.

---

# Limitations

The current implementation has several limitations:

- The chatbot supports a predefined set of companies.
- The supported financial metrics are predefined.
- The historical dataset covers a limited range of years.
- The financial dataset is static.
- New data requires updating the CSV dataset.
- Query understanding depends on implemented keywords and rules.
- The application does not use advanced NLP or machine-learning-based intent recognition.
- The application does not provide real-time financial market data.
- The application is not designed to provide financial advice.

---

# Future Improvements

Potential improvements include:

- [ ] Add NLP-based intent recognition
- [ ] Add entity extraction for companies, metrics, and dates
- [ ] Support more companies
- [ ] Support additional financial metrics
- [ ] Expand historical coverage
- [ ] Integrate a live financial data API
- [ ] Add database storage
- [ ] Add SQL-based querying
- [ ] Add financial data validation
- [ ] Add conversation history
- [ ] Add user authentication
- [ ] Add interactive charts
- [ ] Add financial trend analysis
- [ ] Add automated testing
- [ ] Add API endpoints
- [ ] Containerize the application with Docker
- [ ] Deploy the application to a cloud environment

---

# Screenshots

Add screenshots of the chatbot interface here.

Recommended structure:

```text
docs/
└── screenshots/
    ├── home.png
    ├── query.png
    └── response.png
```

Example:

```markdown
![GFC Financial Chatbot](docs/screenshots/home.png)
```

---

# Security

This project does not require API credentials in its current documented implementation.

However, if external financial APIs, authentication, or database credentials are added in future versions, sensitive values should be stored using environment variables rather than committed directly to the repository.

Recommended `.gitignore` entries include:

```gitignore
venv/
__pycache__/
*.pyc
.env
```

---

# Reproducibility

To reproduce the project locally:

```bash
git clone https://github.com/costaspinto/Financial_Chatbot.git
cd Financial_Chatbot

python -m venv venv
```

### Windows

```bash
.\venv\Scripts\activate
```

### macOS / Linux

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Start the application:

```bash
python app.py
```

Then open:

```text
http://127.0.0.1:5000
```

---

# Skills Demonstrated

```text
Python
Flask
Pandas
Data Processing
Rule-Based NLP
Keyword Matching
Structured Data Retrieval
HTML
CSS
Web Application Development
HTTP Request Handling
Data-Driven Application Design
```

---

# Project Summary

GFC Financial Chatbot demonstrates how a structured financial dataset can be transformed into an accessible conversational web application using a lightweight Python stack.

The core workflow is:

```text
Financial CSV
      |
      v
Pandas DataFrame
      |
      v
User Question
      |
      v
Rule-Based Query Parsing
      |
      v
Company + Metric + Year
      |
      v
DataFrame Filtering
      |
      v
Financial Record
      |
      v
Formatted Response
      |
      v
Flask Web Interface
```

The project provides a practical foundation for evolving a deterministic financial data assistant into a more advanced system using NLP, databases, live financial APIs, analytics, and conversational AI.

---

# Author

**Costas Pinto**

MCA — Artificial Intelligence & Machine Learning

- GitHub: [Costas Pinto](https://github.com/costaspinto)

---

# Project Context

**2025 | BCG AI Insights | GFC Project**

---

# License

This project is licensed under the **MIT License**.
