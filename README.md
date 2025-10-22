# AI Portfolio Manager Project

## Overview
This project designs a multi-agent AI system for quantitative trading, emphasizing profitability through macro-to-micro daily flows: Broad market analysis (macro) informs strategy generation, risk assessment, and micro-level trade executions with probability of profit assignments. Agents collaborate via A2A (agent-to-agent) interactions (e.g., shared data formats), with reflection management for iterative learning (e.g., backtesting outcomes refine edges via ML/experience). Integration with Interactive Brokers (IBKR) API enables real trades. The system is built for scalability and funding audits, with all decisions backed by traceable reasoning in text files.

No code generation yet—focus on planning and organization in VS Code using text/Markdown files.

## Key Components
- **Multi-Agent System**: Agents handle specialized tasks (e.g., data ingestion, strategy proposal, execution), with A2A for collaboration and reflection loops for self-improvement.
- **Inspired Resources**: From awesome-quant GitHub repo (e.g., Qlib for ML pipelines, Zipline for backtesting)—see docs/resources-overview.md.
- **IBKR Linkage**: Configured for data pulls and executions—see config/ibkr-integration.txt.
- **Structural Reasoning**: All files include "Reasoning" sections for backups (e.g., why mappings enhance profitability).

## Evaluation Mechanisms
- **POP Evaluations**: Learning Agent compares actual (Execution/IBKR logs) vs theoretical POP (Risk Agent), using weekly stochastic batches with 1 SD thresholds for triggers.
- **Batching and Adjustments**: Daily JSON logs aggregate to weekly DataFrames; Learning consolidates problem trades; shares references via A2A to Data/Strategy/Risk/Execution for refinements.
- **Changelogs**: High-level summaries (with SD metrics) in core/learning-data-changelog.txt for Learning-to-Data changes; general CHANGELOG.md for milestones.

## File Structure
- **docs/**: High-level documentation and backups (e.g., matrices, mappings).
- **agents/**: Per-agent notes, focusing on A2A and reflection.
- **core/**: Shared utility outlines (e.g., data handling concepts).
- **config/**: IBKR-specific setups.

See docs/architecture.md for a system diagram.

## Next Steps
- Populate agent notes with resource mappings.
- Refine for funded prototypes: Ensure all reasoning ties to profitability (e.g., reflection reduces losses via learned edges, now via SD-batched evaluations).

Reasoning: High-level manifesto backs executive summaries for funding; ties A2A/reflection to ROI, with weekly batching ensuring stable experiential growth.