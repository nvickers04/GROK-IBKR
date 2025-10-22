# System Architecture

## High-Level Flow Description
This describes the multi-agent system's workflow in a sequential, bullet-point format for clarity. It emphasizes macro-to-micro daily progression (e.g., broad data analysis to granular executions), A2A interactions (e.g., shared data/metrics for decisions), reflection management (iterative reviews with learning loops), and IBKR integration. Resources like Qlib inspire pipelines, while exchange-calendars (from awesome-quant) informs time-constrained checks.

- **Macro Inputs and Data Ingestion**: Start with external market data (e.g., from IBKR or yfinance-inspired feeds). The Data Agent processes this into structured formats (e.g., time series features via tsfresh concepts), providing a broad market overview.
- **Strategy Generation**: Data Agent shares processed inputs via A2A to the Strategy Agent, which generates macro-level strategies (e.g., trend forecasts) transitioning to micro-level trade proposals with train-of-thought reasoning (e.g., step-by-step logic from backtrader/Qlib).
- **Risk Assessment**: Strategy Agent passes proposals to the Risk Agent for probability of profit evaluations (e.g., Sharpe ratios via pyfolio), incorporating risk models (tf-quant-finance). A2A ensures shared metrics for collaborative adjustments.
- **Pre-Execution Review**: Risk Agent outputs to the Execution Agent, which initiates a preliminary check before final commitment.
- **Final Reflection Before Execution**: To enforce time constraints and common-sense clarity, the Execution Agent triggers one last reflection loop—pulling from the Reflection Agent for a quick validation. This uses exchange-calendars concepts (e.g., check if current time is within market hours, holidays, or valid sessions) to avoid executions outside trading windows. Additionally, apply a common-sense test: Cross-verify trade details against predefined sanity rules (e.g., ensure quantities are feasible, no delusional elements like impossible prices, and alignment with overall portfolio logic). If it fails, loop back to Strategy/Risk for iteration; if passes, proceed.
- **Micro Execution**: Execution Agent handles IBKR-linked trades (e.g., via nautilus_trader/ib_nope concepts), logging outcomes.
- **Post-Execution Reflection and Learning**: Outcomes feed back via A2A to the Reflection Agent (Zipline-inspired backtests for reviews) and Learning Agent (FinRL/tf-quant-finance for ML refinements), closing the loop for experiential edge-finding (e.g., update probabilities based on real results).

## Additional Details
- **A2A Protocol**: Agents use standardized sharing (e.g., JSON for events, DataFrames for metrics) to enable seamless collaboration and traceability.
- **Reflection Management**: Embedded throughout, with the new final pre-execution step adding robustness against timing errors or unclear decisions.
- **IBKR Integration**: Centered in Execution Agent, with time validations ensuring compliant trades.

Reasoning: Bullet-point flow improves readability over diagrams; incorporates exchange-calendars for time constraints in final reflection, backing funding with defensible risk mitigation (e.g., prevents ~10-20% of invalid trades conceptually by enforcing market realities and common-sense clarity).