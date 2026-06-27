# Financial Data Warehouse

A multi-asset financial data warehouse built in PostgreSQL, ingesting real market data and calculating realized/unrealized PnL using industry-standard FIFO accounting logic.

## Overview

This project ingests real market price data across 40 instruments (equities, crypto, FX, fixed income) via the `yfinance` API into a PostgreSQL data warehouse — 11,320 rows of price data in total. It uses pure SQL (window functions and range-overlap joins) to implement a FIFO lot-matching algorithm for realized PnL, consistent with standard brokerage accounting methodology.

## Tech Stack

- **PostgreSQL** — relational data warehouse
- **Docker / Docker Compose** — containerized PostgreSQL + pgAdmin
- **Python** — data ingestion via `yfinance`
- **SQL** — CTEs, window functions, FIFO lot-matching logic
- **ReportLab** — automated PDF report generation

## How It Works

1. **Ingestion** — Python script pulls historical price data for 40 instruments across equities, crypto, FX, and fixed income via `yfinance`, loading it into PostgreSQL
2. **FIFO PnL Calculation** — SQL queries using window functions and range-overlap joins match buy/sell lots in First-In-First-Out order to compute realized PnL per trade
3. **Analytics Queries** — additional SQL using CTEs and window functions calculate:
   - Unrealized PnL
   - Portfolio exposure by asset class
   Monthly performance tracking (average/peak/trough portfolio value)
 **Reporting**:results are compiled into an automated PDF report

## Setup

1. Start PostgreSQL and pgAdmin:
```bash
   docker-compose up -d
```
2. Run the data ingestion script to populate the warehouse with market data
3. Run the analytics SQL queries (via pgAdmin or psql) to generate PnL and exposure metrics
4. Generate the final PDF report

## Key Challenges Solved

- Designed a FIFO lot-matching algorithm in pure SQL (no procedural code) using window functions and range-overlap joins
- Modeled a multi-asset-class schema (equities, crypto, FX, fixed income) in a single warehouse
- Automated the full pipeline from data ingestion to PDF reporting

## Sample Output

See the included PDF report for a full breakdown of realized/unrealized PnL, portfolio exposure, and monthly performance.

## Future Improvements

- Add automated daily data refresh via a scheduled job
- Build an interactive dashboard on top of the warehouse (Power BI / Metabase)
- Export the FIFO PnL SQL logic as reusable views/stored procedures
