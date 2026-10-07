# Does the Energy Meter Pick the Winner?

**Measurement-Tool-Induced Instability in Energy-Aware Model Selection for Small Machine Learning Models**

Mubbasher Ahmed · BS Computer Science, Beaconhouse National University, Lahore
[mubbasherahmed3@gmail.com](mailto:mubbasherahmed3@gmail.com) · [LinkedIn](https://www.linkedin.com/in/mubbasher-ahmed-72912a240)

> **Status (October 2026):** setup phase. Code, data and results will be added here as the study progresses.

## The question

Energy measurement tools such as CodeCarbon, Intel RAPL and NVIDIA NVML are known to disagree on absolute energy values. This study asks whether that disagreement also changes **which model is ranked or recommended as the most energy-efficient**.

## The gap

Of the 20 papers I reviewed on energy measurement for machine learning, none tests whether disagreement between tools changes model rankings, and some assume rankings stay reliable despite tool error. Rankings are what practitioners act on, so their stability matters more than any single absolute value.

## Research questions

1. **Agreement:** How strongly do model energy rankings agree across measurement tools?
2. **Top-1 stability:** How often does tool choice change the most efficient model, overall and under an accuracy constraint?
3. **Pareto front:** Which models enter or leave the energy–accuracy Pareto front across tools?
4. **Noise:** Are rank reversals larger than within-tool run-to-run noise?
5. **Hardware:** Is instability larger for CPU-bound classical models than GPU-bound CNNs?

## Study design

Every tool measures the same inference run at the same time, so differences come from the tools rather than the workload.

| Component | Details |
|---|---|
| Models | 6 classical ML (logistic regression, random forest, gradient boosting, SVM, k-NN, MLP) and 6 small CNNs (MobileNetV2/V3, ResNet-18, EfficientNet-B0, SqueezeNet, ShuffleNet) |
| Data | 2–3 public tabular datasets (OpenML/UCI); CIFAR-10 for CNN inference |
| Hardware | Intel laptop (CPU, RAPL) and desktop with NVIDIA GPU (NVML), both on bare-metal Ubuntu |
| Tools | CodeCarbon (including estimation-only mode), pyRAPL, raw RAPL counters, NVML, smart-plug wall meter as reference |
| Protocol | Runs ≥ 60 s, warm-up excluded, 60 s cool-down, ≥ 10 repetitions in randomised order, idle baseline per block |
| Analysis | Kendall τ-b and Spearman ρ between tools; pairwise reversal rates; top-1/top-3 retention; Pareto-front Jaccard similarity; bootstrap noise floor (~1,000 resamples) |

Comparisons are made both as practitioners use the tools (headline totals) and component-matched (CPU vs CPU, GPU vs GPU).

## Roadmap

- [ ] **Phase 1** – Machine and tool setup *(in progress)*
- [ ] **Phase 2** – Orchestrator script
- [ ] **Phase 3** – Pilot run and pre-registered analysis plan
- [ ] **Phase 4** – Main measurement campaign
- [ ] **Phase 5** – Analysis
- [ ] **Phase 6** – Write-up and open release of data and code

## Repository structure

```
src/        measurement orchestrator and tool wrappers
configs/    machine, model and run configurations
data/       raw measurement data (large files kept out of git)
analysis/   ranking, agreement and bootstrap analysis
results/    tables and figures
docs/       analysis plan and environment notes
```

## License

- **Code:** MIT (see [`LICENSE`](LICENSE)).
- **Data and results** (measurement CSVs, tables, figures in `data/` and `results/`): [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (see [`LICENSE-DATA`](LICENSE-DATA)). Reuse is welcome with credit.
