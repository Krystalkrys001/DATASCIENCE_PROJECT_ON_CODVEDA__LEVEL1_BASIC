# Task 1: Web Scraping & Structured Data Ingestion
**Codveda Technologies Data Science Internship — Level 1 (Basic)**

## Overview
An automated, fault-tolerant web scraping pipeline built to extract multi-page quote records, author attributions, and topical tags from [Quotes to Scrape](http://quotes.toscrape.com/), transforming unstructured HTML markup into validated, dual-format datasets (`.csv` and `.json`) ready for downstream text analytics.

Rather than treating web scraping as a single-page HTML parse, this pipeline enforces polite rate limiting, explicit HTTP status validation, pagination boundary handling, and UTF-8 serialization checks so special typographic characters survive intact from browser DOM to flat file.

## Objectives
- Query paginated web endpoints reliably using session headers and polite request intervals
- Parse nested HTML containers (`<div class="quote">`) to extract quote text, author names, and variable-length tag arrays
- Engineer a `tag_count` feature at ingestion time and serialize the output cleanly into both CSV and JSON formats

## 1. HTTP Request Management & Polite Scraping
All HTTP GET requests are issued with an explicit browser `User-Agent` header (`Mozilla/5.0`) rather than Python's default `python-requests` user-agent string, which many production web servers block immediately. A deliberate `time.sleep(1)` delay is enforced between consecutive page fetches to avoid server strain or rate-limit throttling.

## 2. DOM Traversal & Pagination Handling
Each page is parsed via `BeautifulSoup` (`html.parser`) by isolating every `<div class="quote">` container and extracting:
- **Quote text:** `<span class="text">`
- **Author:** `<small class="author">`
- **Tags:** all `<a class="tag">` anchors within the container, joined into a comma-separated string alongside an integer `tag_count` column

Pagination iterates across `http://quotes.toscrape.com/page/1/` through `/page/3/`, guarded by an explicit `response.status_code == 404` check so out-of-range page requests terminate the loop cleanly rather than throwing unhandled parsing errors.

## 3. Extraction Summary

| Metric | Result |
|---|---|
| Target Endpoint | `http://quotes.toscrape.com/` (Pages 1–3) |
| Total Records Extracted | 30 quotes (10 per page) |
| Engineered Features | 4 columns (`quote`, `author`, `tags`, `tag_count`) |
| Missing / Null Values | 0 |
| Output Artifacts | `codveda_level1_task1_quotes.csv`, `codveda_level1_task1_quotes.json` |

## Data Quality Notes & Limitations
- **Character encoding:** Source quotes contain typographic curly quotation marks (`“` and `”`). Exporting to CSV/JSON preserves UTF-8 encoding so special punctuation does not corrupt into unreadable symbols.
- **Fixed vs. dynamic pagination:** The current script uses an explicit `max_pages = 3` ceiling. Scaling to scrape an entire site of unknown length in production would require checking for the presence of the `<li class="next">` pagination element dynamically rather than relying solely on a 404 exit condition.

## Tech Stack
Python 3.12 (Anaconda AI Kernel), Requests, BeautifulSoup4 (`bs4`), Pandas, JSON

## Repository Structure
```text
├── data/
│   ├── codveda_level1_task1_quotes.csv
│   └── codveda_level1_task1_quotes.json
├── notebooks/
│   └── task1_web_scraping.ipynb
├── reports/
│   └── Task1_Web_Scraping_Report.docx
└── README.md
```

## Key Takeaway
Reliable data ingestion is defined by defensive engineering around the network boundary—explicit status checks, polite rate limiting, and immediate structural validation upon serialization—not just CSS/HTML selector syntax.

## Author
**Ulasi Chukwuma Obinna**  
Data Analyst | Business Intelligence & Data Science  
Email: ulasistanley1@gmail.com  
GitHub: https://github.com/Krystalkrys001  
Medium: https://medium.com/@ulasistanley1  
LinkedIn: https://www.linkedin.com/in/chukwuma-obinna-ulasi-082198413/