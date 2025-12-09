# Enterprise-Cloud-Outage-Analysis
Analyzing the financial impact of cloud outages on major U.S. enterprises through SEC 10-K filings

---

## Executive Summary

Cloud-based infrastructure has become the strategy for many modern enterprises for their daily operations. Yet when major providers experience outages, seen with AWS's multi-hour disruptions and Azure global service failures, the financial impact can be seen across the market.

This project answers the question: **How much revenue is at risk when cloud providers go down?**

By parsing 12000+ SEC 10-K filings from 2500+ companies (2020-2025), I mapped each of them and their dependencies to specific cloud providers and calculated hourly revenue exposure during ~34 major outages.

## Motivation

With the recent news of outages from major cloud providers, I wanted to explore and do a risk assessment on the financial impact an outage from one of the major cloud providers has on the market. When AWS experienced a DynamoDB/DNS Service Disruption (October 2025), operations across the internet halted up to 15 hours.

**PROBLEM**: Companies don't voluntarily disclose their cloud architecture details and third-party databases are incomplete or expensive.

**MY SOLUTION**: Leverage SEC-mandated 10-K filings, specifically the Risk Factors section (Item 1A), where companies are legally required to disclose material risks to their business operations, including technology dependencies.

This approach transforms required annual documents into a unique data for cloud dependency mapping for over 2500 enterprises that is difficult to get hands on elsewhere in the public domain.

---

## Key Findings

- **AWS dominates enterprise cloud adoption** at 70% market share among companies disclosing specific providers
- **Cloud dependency is accelerating**: 62% of companies mentioned cloud reliance in 2020 filings vs. 77.6% in 2025
- **A single AWS outage (DynamoDB/DNS Disruption, Oct 2025)** put $2.5+ billion in total revenue at risk across 214 affected companies
- **Information Technology, Communication, and Financial sectors** show the highest cloud concentration risk while **Real Estate, Consumer Staples, and Material sectors** show little reliance to cloud services.

### Market Composition

The analysis covers $72.78 trillion in market capitalization across 11 sectors:

![Breakdown by Company Count](./Breakdown%20by%20Company%20Count.png) ![Breakdown by Market Cap](./Breakdown%20by%20Market%20Cap.png) 

### Cloud Adoption Trend (2020-2025)

![Cloud Adoption Trend](./Cloud%20Adoption%20Trend.png) 

**Insight**: A 15+ percentage point increase in cloud risk disclosures over 5 years reflects accelerating enterprise cloud migration.

![No Mention Graphic](./No%20Mention%20Graphic.png) 

**Insight**: 25% of enterprises do not mention relying on cloud services or find it not as important.

### Provider Market Share (Among Specific Disclosures)

![Provider Market Share](./Provider%20Market%20Share.png) 

*Note: Totals exceed 100% due to multi-cloud deployments.*

### Sector-Provider Dependency Heatmap

![Sector-Provider Dependency Heatmap](./Sector-Provider%20Dependency%20Heatmap.png) 

**Insight**: IT sector shows highest absolute cloud dependency (207 provider mentions). AWS dominance is most pronounced in Communication and Consumer Discretionary sectors.

---

## Outage Scenario Analysis

### Summary by Provider

![Summary by Provider](./Summary%20by%20Provider.png) 

### AWS: DynamoDB/DNS Service Disruption (October 2025)

**One of the largest cloud disruption in recent news.**

![AWS Outage Summary](./AWS%20Outage%20summary.png) 

| Metric | Value |
|--------|-------|
| Duration | 15 hours |
| Companies Affected | 214 |
| Hourly Revenue at Risk | $176.92M |
| **Total Revenue at Risk** | **$2.5B+** |

**Top 5 Companies by Hourly Exposure:**

| Company | Sector | Hourly Revenue at Risk |
|---------|--------|------------------------|
| Amazon | Consumer Discretionary | $72.8M |
| Verizon | Communication | $15.4M |
| T-Mobile | Communication | $9.3M |
| Accenture | Information Technology | $7.4M |
| Capital One | Financials | $4.5M |

**Sector Breakdown:**
- Consumer Discretionary: $83M/hour (led by Amazon)
- Information Technology: $37M/hour
- Communication: $36M/hour

### Azure: Firewall and Data Explorer Outage (June 2022)

![Azure Outage Summary](./Azure%20Outage%20Summary.png) 

| Metric | Value |
|--------|-------|
| Duration | 24 hours |
| Companies Affected | 81 |
| Hourly Revenue at Risk | $60.84M |
| **Total Revenue at Risk** | **$1.46B** |

**Top 5 Companies by Hourly Exposure:**

| Company | Sector | Hourly Revenue at Risk |
|---------|--------|------------------------|
| Microsoft | Information Technology | $27.9M |
| Nvidia | Information Technology | $6.9M |
| Oracle | Information Technology | $6.0M |
| Medtronic | Health Care | $3.7M |
| ServiceNow | Information Technology | $1.3M |

### GCP: Fiber Cuts Network Disruption (June 2022)

![GCP Outage Summary](./GCP%20Outage%20Summary.png) 

| Metric | Value |
|--------|-------|
| Duration | 3 hours |
| Companies Affected | 62 |
| Hourly Revenue at Risk | $98.16M |
| **Total Revenue at Risk** | **$294M** |

**Notable Insight**: Despite GCP's smaller market share, its per-hour impact rivals AWS due to high-revenue companies like Alphabet (Google) being self-hosted on GCP infrastructure.

---

## Methodology

### 1. Data Extraction Pipeline
![ETL Pipeline](./ETL%20Pipeline.png) 


### 2. Cloud Provider Detection

I targeted five major cloud infrastructure providers based on market dominance:

| Provider | Detection Keywords |
|----------|-------------------|
| AWS | Amazon Web Services, AWS, EC2, S3, Lambda |
| Azure | Microsoft Azure, Azure Cloud |
| GCP | Google Cloud Platform, GCP, Google Cloud |
| Oracle | Oracle Cloud Infrastructure, OCI |
| IBM | IBM Cloud, IBM Watson |

### 3. Classification System

Each company was categorized into one of three buckets based on 10-K content:

| Category | Definition | Count |
|----------|------------|-------|
| **Specific Cloud** | Explicitly names one or more cloud providers | 306 |
| **General Cloud** | Mentions cloud dependency without naming providers | 1,626 |
| **No Cloud Mention** | No cloud-related risk factors disclosed | 628 |

### 4. Context-Aware Scoring

A critical challenge: distinguishing between companies that *use* a cloud provider versus those that *compete* with one (e.g., AWS mentioning Azure as a competitor, not a dependency).

**Solution**: Implemented a context scoring algorithm to classify cloud provider dependencies in 10-K filings. The algorithm analyzes surrounding sentences using regex pattern matching, assigning higher scores to dependency indicators like "rely," "depend," "utilize," and "infrastructure," while lower scores for wordings like "competitor" and "compete."

### 5. Revenue Risk Calculation

Pulled in market data using Yahoo Finance API (yfinance) for each companies annual revenue and market capitalization. From there I can calculate the hourly revenue and determine the total revenue at risk during each outage.

```
Hourly Revenue = Annual Revenue (2024) / 8,760 hours

Revenue at Risk (per outage) = Hourly Revenue × Outage Duration (hours)
```

---

## Data Schema

### Entity Relationship Diagram
![Data Schema](./image%20of%20diagram.png) 

## Key Insights

### Market Composition

The analysis covers $72.78 trillion in market capitalization across 11 sectors:

| Sector | Market Cap | Company Count |
|--------|------------|---------------|
| Information Technology | $22.7T | 325 |
| Financials | $8.7T | 485 |
| Consumer Discretionary | $7.7T | 296 |
| Health Care | $6.6T | 464 |
| Communication | $10.7T | 117 |
| Industrials | $6.4T | 402 |
| Consumer Staples | $3.4T | 125 |
| Energy | $2.0T | 126 |
| Materials | $1.4T | 161 |
| Real Estate | — | 161 |
| Utilities | — | 68 |

### Cloud Adoption Trend (2020-2025)

| Year | % Mentioning Cloud Dependency |
|------|------------------------------|
| 2020 | 62.0% |
| 2021 | 64.8% |
| 2022 | 69.6% |
| 2023 | 71.7% |
| 2024 | 75.5% |
| 2025 | 77.6% |

**Insight**: A 15+ percentage point increase in cloud risk disclosures over 5 years reflects accelerating enterprise cloud migration.

### Provider Market Share (Among Specific Disclosures)

| Provider | Share |
|----------|-------|
| AWS | 70% |
| Azure | 26% |
| GCP | 20% |
| Oracle | <5% |
| IBM | <5% |

*Note: Totals exceed 100% due to multi-cloud deployments.*

### Sector-Provider Dependency Heatmap

| Sector | AWS | Azure | GCP | IBM | Oracle |
|--------|-----|-------|-----|-----|--------|
| Information Technology | 105 | 62 | 40 | 1 | 4 |
| Communication | 28 | — | 6 | — | — |
| Consumer Discretionary | 22 | 2 | 5 | — | — |
| Financials | 18 | 4 | 3 | — | — |
| Health Care | 13 | 7 | 4 | — | — |
| Industrials | 19 | 6 | 3 | — | 1 |

**Insight**: IT sector shows highest absolute cloud dependency (207 provider mentions). AWS dominance is most pronounced in Communication and Consumer Discretionary sectors.

---

## Limitations & Assumptions

### Methodological Constraints

1. **Multi-Cloud Reality Understated**
   
   Russell 3000 companies likely employ multi-cloud or hybrid architectures with failover capabilities. This analysis models a worst-case scenario where primary cloud dependencies experience complete service interruption without redundancy.

2. **Revenue Timing Not Modeled**
   
   Hourly revenue calculations assume uniform revenue distribution across all hours. In reality, a retail company's 1 AM outage impacts revenue differently than a 1 PM outage. Time-of-day and seasonality adjustments were not applied.

3. **Provider Disclosure Gaps**
   
   Companies in the "General Cloud" category (1,626) acknowledge cloud dependency but don't specify providers.
   - Security-conscious disclosure practices (common in financial services)
   - Multi-cloud strategies without a dominant provider

4. **Competitor vs. Dependency Disambiguation**
   
   While the context-scoring algorithm handles most edge cases, some misclassifications may occur when companies discuss cloud providers in mixed contexts.

5. **2024 Revenue Baseline**
   
   All revenue-at-risk calculations use 2024 annual revenue, which represents the most complete and recent fiscal year data available at time of analysis.

6. **Sector Revenue Distribution**
   
   Different sectors have varying revenue models. A subscription SaaS company may experience immediate revenue impact during an outage, while a manufacturing company with cloud-dependent logistics may see delayed effects.

---

## Technical Implementation

### Technology Stack

| Component | Technology |
|-----------|------------|
| Data Extraction | Python 3.9+, SEC EDGAR API |
| Text Processing | Regex, NLTK (NLP) |
| Financial Data | yfinance |
| Data Storage | PostgreSQL / CSV |
| Visualization | Power BI |
| Version Control | Git |

### Data Sources

| Source | Purpose | Records |
|--------|---------|---------|
| SEC EDGAR API | 10-K filings (2020-2025) | ~12,000 documents |
| Yahoo Finance (yfinance) | Company financials, sector classification | ~2,700 companies |
| Public Outage Reports | Historical cloud outages | 36 incidents |

## Author

**Kenneth Nguyen**
[LinkedIn](#) | [Portfolio](#) 
