# ga4-llm-value

Analyze GA4 traffic to compare LLM/AI-driven sessions with organic search and other channels using the included Colab notebook.

## What's in this repo

- `flin_ga4_llm.ipynb`: A Google Colab notebook with multiple GA4 analyses focused on LLM/AI traffic classification and benchmarking.

## Prerequisites

- A Google Cloud project with access to the GA4 properties you want to analyze.
- **APIs enabled** in the Google Cloud project:
  - Google Analytics Data API
  - Google Analytics Admin API
- An **OAuth client ID (Desktop app)** so the notebook can authenticate via OAuth.

## Connect to Google Cloud / GA4

The notebook uses OAuth via `InstalledAppFlow` and reads secrets from Colab. To connect:

1. **Create OAuth credentials** in Google Cloud Console:
   - Go to **APIs & Services → Credentials**.
   - Create an **OAuth client ID** of type **Desktop app**.
   - Copy the **Client ID** and **Client Secret**.
2. **Enable APIs** in the same project:
   - **Google Analytics Data API**
   - **Google Analytics Admin API**
3. **Store the secrets in Colab**:
   - Open the notebook in Colab.
   - In **Colab → Secrets**, add:
     - `client-id-colab` → your OAuth client ID
     - `client-secret-colab` → your OAuth client secret
4. **Run the setup and login cells**:
   - **Setup – Imports** installs the needed libraries and defines helper functions.
   - **Login – Einmalig global anmelden** calls `authenticate_analytics()` which prompts for OAuth consent.

Once authenticated, `creds` is stored globally and reused by all analysis functions.

## Analyses in the notebook

Each analysis is contained in a dedicated notebook section with configuration constants at the top.

### 📈 Verhältnis-Analyse: Organic vs. LLM (Basis 100)
- **Purpose:** Compare monthly LLM traffic to organic search where **January organic = 100**.
- **Output:** Two indexed lines: organic baseline and LLM relative to the organic baseline.

### 🤖 LLM-Solo-Analyse (Index 100 = Start)
- **Purpose:** Track **only LLM/AI traffic** over time, indexed to January = 100.
- **Output:** A single indexed LLM trend line.

### 📅 Qualitäts-Benchmark: LLM = 100
- **Purpose:** Recompute monthly **organic quality vs LLM**, where LLM is fixed at 100 each month.
- **Output:** A flat LLM = 100 line with organic showing relative over/under performance.

### 📊 5-Kanal-Benchmark: LLM = 100
- **Purpose:** Benchmark five channels (LLM, Organic, Ads, Social, Direct) with LLM fixed at 100.
- **Output:** Monthly indexed comparison across the five channels.

### 💰 Conversion-Rate-Analyse: Organic vs. LLM
- **Purpose:** Compare conversion rate (key events / sessions) for LLM vs organic traffic.
- **Output:** Monthly conversion-rate comparison lines.

### 💰 Median-CVR je Property (fairer Vergleich)
- **Purpose:** Calculate conversion rate per property, then take the **median** to avoid large sites skewing the mean.
- **Output:** Median monthly CVR for LLM vs organic.

### 💎 RPS-Analyse (Weighted Avg)
- **Purpose:** Evaluate revenue per session (RPS) **weighted** by session volume for shops with purchases in every month.
- **Output:** Monthly weighted RPS comparison for LLM vs organic.

### 🛍️ 5-Kanal-Value-Analyse (nur aktive Shops)
- **Purpose:** Compare **weighted revenue per session** across LLM, Ads, Social, Organic, Direct for active shops.
- **Output:** Absolute monthly RPS values for the five channels.

## Running an analysis

1. Open `flin_ga4_llm.ipynb` in Colab.
2. Run the **Setup** cell, then the **Login** cell once.
3. Run the analysis cell you need (each section has its own `run_*` function).
4. Adjust the **date range** and **LLM regex** in each section as needed.

## Notes

- The LLM classification uses a regex (`LLM_REGEX`) to detect LLM/AI sources.
- Analyses are built to pull data from **all GA4 properties** accessible by the authenticated user.
