# -relationship-between-trader-performance-and-market-sentiment
📊 Bitcoin Market Sentiment & Trader Performance Analysis

Primetrade.ai Data Science Assignment — Exploring the relationship between Bitcoin Fear & Greed Index and Hyperliquid trader performance across 2 years of live trading data.


🗂️ Project Overview
This project investigates how Bitcoin market sentiment influences trader profitability. By merging the Fear & Greed Index with Hyperliquid historical trade data, we uncover counter-intuitive patterns that challenge conventional trading wisdom.
DetailValue📅 PeriodMay 2023 – May 2025📈 Total Trades211,218👥 Unique Traders32🪙 Unique Assets246📆 Overlapping Days479

📁 Dataset Files
FileDescriptionhistorical_data.csvRaw Hyperliquid trade records with PnL, account, coin, direction, sizefear_greed_index.csvDaily Bitcoin Fear & Greed Index (value 0–100 + classification)merged_data.csvFinal merged dataset used for all analysis and dashboard

🔀 Data Merging Logic
The two datasets are joined on date using an inner join:
pythonimport pandas as pd

trades = pd.read_csv('historical_data.csv')
fg     = pd.read_csv('fear_greed_index.csv')

# Extract date from timestamp
trades['date'] = pd.to_datetime(trades['Timestamp IST'], dayfirst=True).dt.normalize()
fg['date']     = pd.to_datetime(fg['date'])

# Inner join on date
merged = trades.merge(fg[['date', 'classification', 'value']], on='date', how='inner')
merged.to_csv('merged_data.csv', index=False)
Each trade row is stamped with the sentiment label (classification) and numeric score (value) for that day.

🔑 Key Findings
1. 😱 The Fear Premium

Traders earn 4.7x more on Extreme Fear days than on Greed days

SentimentAvg Daily PnLWin Rate🔴 Extreme Fear$52,79432.7%🟠 Fear$36,89232.9%🟡 Neutral$19,29733.2%🟢 Greed$11,14133.6%💚 Extreme Greed$23,81746.7%
2. 🎯 Win Rate Paradox
Extreme Greed has the highest win rate (46.7%) but the lowest average PnL — profits come from trade sizing and conviction, not frequency of wins.
3. 🔄 Asset Rotation Rule
SentimentBest AssetPnL GeneratedExtreme FearHYPE$482,084FearHYPE$840,306NeutralSOL$303,376Greed@107$724,342Extreme Greed@107$1,988,619
4. 📉 Weak Negative Correlation
Pearson r = -0.095 (p = 0.037) — statistically significant inverse relationship between Fear/Greed score and daily PnL. As greed rises, profits slightly fall.

📊 Dashboard (Power BI)
The Power BI dashboard includes:
RowVisualFields UsedRow 15 KPI CardsSum of Closed PnL, Sum of Size USD, Count of Transaction Hash, Distinct AccountRow 2Avg PnL by Sentiment (Bar)classification × Avg Closed PnLRow 2Trade Count by Sentiment (Bar)classification × Count Transaction HashRow 3Cumulative PnL Over Time (Line)date × Sum Closed PnLRow 4Top 10 Traders (Horizontal Bar)Account × Sum Closed PnL — Top N filterRow 4Volume by Sentiment (Donut)classification × Sum Size USDRow 5Asset × Sentiment Heatmap (Matrix)Coin × classification × Sum Closed PnLRow 6Slicerclassification

💡 Strategic Recommendations

Increase activity during Fear periods — higher volatility creates asymmetric opportunities
Rotate assets by sentiment — HYPE in fear, @107 in greed, SOL in neutral
Accept low win rates in fear regimes — 33% win rate is normal; focus on trade sizing
Do not over-trade during Greed — highest activity, lowest returns per trade
Study top trader strategies — Trader 1 earned $2.14M, 34% more than Trader 2


🛠️ Tools Used
ToolPurposePython (pandas, matplotlib, seaborn, scipy)Data cleaning, merging, analysis, chartsPower BI ServiceInteractive dashboardGoogle DriveDataset source

📂 File Structure
📦 project/
├── 📄 historical_data.csv       # Raw trade data
├── 📄 fear_greed_index.csv      # Fear & Greed Index
├── 📄 merged_data.csv           # Final merged dataset
├── 📊 dashboard.pbix            # Power BI dashboard file
└── 📝 README.md                 # This file
