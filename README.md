# Earnings Call Tone and Short-Window Market Reaction  
**Author:** Ashton Meyer-Bibbins  

## Overview  
This project examines how executive language in earnings call transcripts relates to the immediate market response in the [0, +1] trading-day window. By linking thousands of transcripts with daily price data, the analysis measures whether tone signals such as positive, negative, and uncertain language correspond to short-term abnormal returns relative to the SPY benchmark.

The work combines financial text processing, event-study construction, and regression modeling. It also tackles real challenges like memory limits, noisy textual data, and irregular event timing. The final dataset integrates transcript tone features with firm returns for more than six thousand earnings events.

## Key Findings  
**Tone predicts short-window abnormal returns.**  
Positive language corresponds to higher CAR[0,+1], negative language corresponds to lower CAR[0,+1], and uncertainty language shows its own positive association. The overall regression model is statistically significant, and tone meaningfully explains variation in very noisy short-horizon returns.

**Price level does not moderate tone effects.**  
The relationship between tone and abnormal returns is stable across high- and low-priced stocks. Interaction terms between tone and log-price are not significant, which indicates that investors interpret executive language in a consistent way regardless of price level.

**Uncertainty does not amplify or dampen other tone effects.**  
Uncertainty language has its own positive association with returns, but it does not change the strength of positive or negative tone. Both interaction terms fail to reach significance.

## Motivation  
Earnings calls influence how the market interprets quantitative results. They supply context through commentary, forward-looking statements, and explanations of performance drivers. Markets rely on these narratives, often reacting before financial data can be fully processed. Studying how tone connects to immediate price movement helps isolate the behavioral and informational components of this reaction.

This project also highlights how unstructured text can be transformed into structured features that support econometric models. It illustrates practical applications of NLP in financial research and strengthens the understanding of how qualitative disclosures influence short-term trading behavior.

## Data Sources  
| Dataset | Description | Source |
|--------|-------------|--------|
| Earnings Call Transcripts | ~18k transcripts with ticker, date, and full text | Motley Fool (Kaggle) |
| NASDAQ OHLCV | Daily prices for U.S. equities (1980s to 2024) | Kaggle |
| SPY Prices | Benchmark for abnormal returns | Kaggle |
| Loughran–McDonald Dictionary | Finance-specific sentiment dictionary | Academic resource |

All datasets were merged on standardized ticker and aligned trading dates. Files were processed under strict memory limits, which required building custom loading routines and filtering by relevant tickers.

## Methodology  

### Data Preparation  
I standardized date formats, normalized tickers, adjusted after-hours call times, and built helper functions to transform heterogeneous date fields. Because the raw price data exceeded available memory, only tickers present in the transcript dataset were loaded. Dates were normalized to midnight to support reliable merging. All of these steps were validated with assertions and small manual test cases, which ensured correct behavior before running the full pipeline.

### Text Processing and Tone Extraction  
Transcripts were cleaned using regex to remove punctuation and normalize spacing. Tone features were computed using the Loughran–McDonald dictionary. The implementation was optimized to avoid storing large intermediate lists due to memory limits. Each transcript was mapped to the proportion of total tokens that matched positive, negative, uncertain, litigious, constraining, strong modal, weak modal, and complexity categories.

### Event-Window Construction  
Daily returns were calculated for each ticker and for SPY using adjusted close prices. Day 0 was defined as the first market session on or after the call date. If the call occurred after 5 p.m. Eastern, the event was shifted to the next trading day. CAR[0,+1] was computed as the sum of abnormal returns from Day 0 and Day +1. All window logic was validated on synthetic data to confirm correctness around weekends and holidays.

### Modeling  
I ran OLS regressions to examine whether tone predicts abnormal returns and whether these relationships depend on price or uncertainty. Features were mean-centered before creating interaction terms to reduce multicollinearity. Models were evaluated with standard significance tests and by inspection of coefficient signs, magnitudes, and confidence intervals.

### Visualization  
Scatter plots with fitted regression lines were used to illustrate tone–return relationships, price-splits, and uncertainty-splits. These visualizations helped verify that the numerical results matched the patterns in the underlying data.

## Results  

### Does tone predict short-window abnormal returns?  
Yes.  
Positive language is associated with higher abnormal returns. Negative language is associated with lower abnormal returns. Uncertainty language also shows a positive association. These findings are statistically significant at conventional levels and support the hypothesis that language in earnings calls conveys information that influences the market reaction.

### Does price level change how tone affects returns?  
No.  
Interaction terms between tone and log-price are not significant. This suggests that both low-priced and high-priced stocks respond similarly to executive language, which aligns with the idea that institutional investors dominate the reaction window.

### Does uncertainty amplify or dampen the effect of tone?  
No.  
Uncertainty language has its own positive effect on abnormal returns, but it does not change the slope of the positive or negative tone relationships.

## Challenges and Limitations  
The project required careful control of memory usage while working with millions of price rows and thousands of transcripts. This led to custom data-loading functions and careful filtering. Tone measures rely on dictionary matching, which does not capture deeper semantics or speaker intent. The event window focuses on immediate reactions and does not capture delayed market responses. Abnormal returns are benchmarked only against SPY, which may miss sector-specific effects.

## What This Project Demonstrates  
This project showcases the ability to merge large text datasets with time-series market data, process them efficiently under resource constraints, and derive observable financial insights through statistical modeling. It provides evidence that qualitative language in corporate communication carries measurable information content. It also demonstrates competency in data engineering, NLP, econometrics, and reproducible research.

## Repository Structure  
```
earnings_language_impact/
│
├── data/              Raw and intermediate files (excluded from Git)
├── notebooks/         Full analysis and modeling pipeline
├── figures/           Visualizations and regression plots
├── reports/           Model summaries and generated outputs
├── utils/             Custom loaders and processing functions
└── README.md          Project documentation
```
## Tools and Environment  
- Python 3  
- pandas, numpy, regex, statsmodels  
- seaborn and matplotlib for visualization  
- JupyterHub environment  
- GitHub for version control  

## Acknowledgments  
This work relied upon the open datasets provided by Kaggle contributors and the Loughran–McDonald research team. It is their resources that made this analysis possible.
