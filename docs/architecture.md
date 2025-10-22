# System Architecture

## High-Level Flow Description
This describes the multi-agent system's workflow in a sequential, bullet-point format for clarity. It emphasizes macro-to-micro daily progression (e.g., broad data analysis to granular executions), A2A interactions (e.g., shared data/metrics for decisions), reflection management (iterative reviews with learning loops), and IBKR integration. Resources like Qlib inspire pipelines, while exchange-calendars (from awesome-quant) informs time-constrained checks.

- **Macro Inputs and Data Ingestion**: Start with external market data (e.g., from IBKR or yfinance-inspired feeds). The Data Agent processes this into structured formats (e.g., time series features via tsfresh concepts), providing a broad market overview.
- **Strategy Generation**: Data Agent shares processed inputs via A2A to the Strategy Agent, which generates macro-level strategies (e.g., trend forecasts) transitioning to micro-level trade proposals with train-of-thought reasoning (e.g., step-by-step logic from backtrader/Qlib).
- **Risk Assessment**: Strategy Agent passes proposals to the Risk Agent for probability of profit evaluations (e.g., Sharpe ratios via pyfolio), incorporating risk models (tf-quant-finance). A2A ensures shared metrics for collaborative adjustments; Risk Agent loads/enforces config/risk-constraints.yaml limits (core job: auto-adjust all metrics via sims/reflections).
- **Pre-Execution Review**: Risk Agent outputs to the Execution Agent, which initiates a preliminary check before final commitment.
- **Final Reflection Before Execution**: To enforce time constraints and common-sense clarity, the Execution Agent triggers one last reflection loop—pulling from the Reflection Agent for a quick validation. This uses exchange-calendars concepts (e.g., check if current time is within market hours, holidays, or valid sessions) to avoid executions outside trading windows. Additionally, apply a common-sense test: Cross-verify trade details against predefined sanity rules (e.g., ensure quantities are feasible, no delusional elements like impossible prices, and alignment with overall portfolio logic). If it fails, loop back to Strategy/Risk for iteration; if passes, proceed—else, opt for "no trade" (USD hold benchmarked vs inflation/gold/crypto/FX costs from YAML).
- **Micro Execution**: Execution Agent handles IBKR-linked trades (e.g., via nautilus_trader/ib_nope concepts) or no-trade holds, logging outcomes (slippage live-only; no sim accuracy).
- **Post-Execution Reflection and Learning**: Outcomes feed back via A2A to the Reflection Agent (Zipline-inspired backtests for reviews) and Learning Agent (FinRL/tf-quant-finance for ML refinements), closing the loop for experiential edge-finding (e.g., update probabilities based on real results).

Weekly Stochastic Batching and POP Evaluations
- **Daily Accumulation**: Risk Agent logs stochastic outputs (e.g., JSON for Monte Carlo sims) and Execution Agent logs actuals (e.g., JSON for trade details); Learning Agent consolidates full DataFrames for problem trades (e.g., outliers appended during aggregation).
- **Weekly Processing**: Learning Agent aggregates into DataFrames; computes variance metrics (actual vs theoretical POP) against mean +1 SD threshold (e.g., trigger if >1 SD for sustained gaps).
- **Triggers and Adjustments**: If threshold met, Learning Agent sends batched directives (DataFrames) via A2A to Data Agent for refinements (e.g., tsfresh updates); shares references to Strategy, Risk, Execution for context.
- **Handling Inconsistencies**: On incomplete logs, Learning Agent retries A2A; consults Data Agent for input traces, then Strategy/Risk/Execution for validation.
- **Changelog Backups**: High-level summaries (with SD metrics) to core/learning-data-changelog.txt for audits.

Closed-Loop Constraints Management
- **Risk Agent Oversight**: Loads config/risk-constraints.yaml for assessments; post-reflection, auto-adjusts variables (e.g., max_drawdown via SD batch references) and distributes updates via A2A (JSON diffs)—pre-launch sims (Zipline for baselines, slippage conservative); post-launch experiential.
- **Self-Improvement Loop**: No manual changes—adjustments triggered experientially (e.g., tighten limits if variance >1 SD); all agents reference YAML for enforcement, closing the system without external input.
- **Traceability**: Updates logged to core/learning-data-changelog.txt (e.g., "Week 42: SD 1.2; adjusted pop_floor to 0.62").

## Additional Details
- **A2A Protocol**: Agents use standardized sharing (e.g., JSON for events/logs, DataFrames for metrics/batches) to enable seamless collaboration and traceability.
- **Reflection Management**: Embedded throughout, with weekly batching as a system-wide loop for SD-tied reviews; dedicated changelog for Learning-to-Data changes; Risk-led constraints for closed-loop dynamism; Execution's USD/no-trade as benchmarked reflection.
- **IBKR Integration**: Centered in Execution Agent, with time validations and actual logs feeding evaluations.

Reasoning: Bullet-point flow improves readability over diagrams; incorporates weekly/SD batching, Risk-managed constraints (all metrics adjustable via sims), and Execution's USD/no-trade discipline for time constraints in reflections, backing funding with defensible risk mitigation (e.g., prevents ~10-20% of invalid trades conceptually by enforcing market realities and common-sense clarity, now self-improving via experiential loops with slippage conservatism).