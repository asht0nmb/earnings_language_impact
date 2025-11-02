# Earnings Call Tone Impact on Short-Term Stock Returns  
**Author:** Ashton Meyer-Bibbins  

---

## Overview  
This project investigates whether the **tone** expressed during corporate earnings calls predicts **short-window abnormal stock returns** — the firm’s excess return over the S&P 500 ETF (SPY) in the [0, +1]-day window surrounding the call.

By linking text-based sentiment measures with event-window return calculations, the study explores how qualitative language signals influence immediate market reactions, combining financial communication, behavioral finance, and data science.

---

## Research Questions  
1. Does the tone (positive, negative, uncertain) of earnings calls predict short-window abnormal returns?  
2. Do tone–return relationships vary across **industries, firm sizes, or leadership structures**?  
3. Are tone effects amplified or dampened during **high-volatility** market days (large SPY movements)?  
4. After controlling for **EPS surprise**, does tone still explain residual abnormal returns?  

---

## Motivation  
Earnings calls serve as the bridge between companies and investors. They contextualize numbers with language that conveys confidence, caution, or uncertainty — elements that can influence how markets interpret results.  

This project isolates the **short-window** reaction ([0,+1]) to capture how tone affects prices immediately after disclosure, avoiding long-term confounds and post-announcement drift.  

Beyond its financial implications, it demonstrates a modern approach that integrates **natural language processing (NLP)** with event-study econometrics — illustrating how unstructured text can yield measurable, quantitative insights.

---

## Data Sources  

| Dataset | Description | Source |
|----------|--------------|--------|
| **Earnings Call Transcripts** | ~18,000 quarterly transcripts including ticker, date, and text | Motley Fool / Kaggle |
| **NASDAQ Daily Prices** | Daily OHLCV data for U.S. equities (2015–2024) | Paul Mooney / Kaggle |
| **S&P 500 ETF (SPY)** | Benchmark for abnormal return computation | Kaggle |
| **Loughran–McDonald Dictionary** | Finance-specific tone lexicon for word classification | Academic resource |

All data are stored in CSV format and merged on **ticker** and **date** to align tone features with event-window returns.

---

## Methodology  

### 1. Data Preparation  
- Load and align earnings call, NASDAQ, and SPY datasets  
- Standardize tickers and date formats  
- Verify merge keys (ticker, date)  

### 2. Text Cleaning & Tone Computation  
- Clean transcripts using regex (remove punctuation, lowercase text)  
- Map words using the **Loughran–McDonald dictionary**  
- Compute tone metrics:  
  - `pos_pct` — percentage of positive words  
  - `neg_pct` — percentage of negative words  
  - `uncert_pct` — percentage of uncertainty words  

### 3. Event-Window & Abnormal Returns  
- Define event window: `[0, +1]` trading days  
- Compute daily and cumulative abnormal returns (CAR = firm return − SPY return)  

### 4. Merge & Model  
- Merge tone metrics with CARs and firm/sector controls  
- Run regressions of the form:
    CAR_0p1 ~ pos_pct + neg_pct + uncert_pct + sector + size_proxy

- Evaluate coefficients and significance by sector and size  

### 5. Visualization & Reporting  
- Plot tone distributions and tone–return relationships  
- Display regression coefficients and confidence intervals  
- Test robustness via alternate event windows (e.g., [−1,+1])  

---

## Potential Challenges  
- **Coverage bias:** Motley Fool transcripts skew toward large, public firms  
- **Timing mismatch:** Calls after market close require careful event alignment  
- **Text inconsistency:** Varying transcript formats can distort tone scoring  

---

## Folder Structure  
earnings_language_impact/
│
├── data/ 
├── notebooks/   
├── figures/
├── reports/
├── .gitignore
└── README.md


---

## Environment & Tools  
- **Language:** Python 3.x  
- **Libraries:** pandas, numpy, re, statsmodels, matplotlib, seaborn  
- **Environment:** JupyterHub  
- **Version Control:** Git + GitHub (SSH authentication)  

---

## Project Plan  
| Phase | Description |
|--------|--------------|
| Setup & Data Prep | Load, standardize, and merge datasets |
| Text Cleaning & Tone | Clean text and compute tone features |
| Return Calculations | Compute abnormal returns and event windows |
| Modeling & Testing | Regression analysis and interpretation |
| Visualization & Reporting | Generate figures, robustness checks |

---

## Current Status  
Tone computation and event-window construction in progress  

---

## Acknowledgments  
This work relies on open financial data and public sentiment dictionaries provided by Kaggle contributors and the Loughran–McDonald research team.  

---
