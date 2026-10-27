# InterventionROP-Edge: Do-Calculus-Guided Threshold-Aware Compute-Bandwidth Orchestration for Retinopathy of Prematurity Triage in Neonatal Edge-IoT Systems

Code and results for the InterventionROP-Edge manuscript (under review). InterventionROP-Edge combines a gestational age (GA) stratified structural causal model of ROP progression, a counterfactual threshold estimator that scores how urgently each infant needs treatment review, and a threshold aware causal resource orchestrator that allocates compute and bandwidth across a NICU Edge-IoT network according to that causal urgency.

## Contents

1. Overview
2. What is measured, derived, and assumed
3. Repository layout
4. Installation
5. Reproducing the results
6. Main results
7. Figures
8. Data sources and licences
9. Limitations
10. Citation and licence

## Overview

The causal layer is fitted on the clinical metadata of the Retinal Image Dataset of Infants and ROP (188 patients, 484 examination series, four GA strata: ≤28, 29 to 32, 33 to 36, ≥37 weeks). Individual and average treatment effects are estimated with the GA stratified SCM and compared with correlational, unstratified, and AIPW estimators, including a sweep over confounding strength. The orchestration layer runs on a simulated 20 node IEEE 802.15.4 NICU topology with simulated SpO2 and incubator telemetry and is compared with five recent baselines (HGAT-PPO, PPO-RA, PD-NoisyDQN, DT-QMIX, JOFO-FedDQN) and five ablations (without do-constraint, without twin, without urgency priority, correlational urgency, unstratified), over 10 evaluation seeds, node counts of 10 to 40, channel noise offsets of −6 to +6 dB, and a GA distribution shift.

## What is measured, derived, and assumed

| Category | Quantity |
|---|---|
| Real | Patient and series level clinical metadata: GA, diagnosis codes, ROP stage, follow up interval, treatment indication |
| Derived | Treatment effects (ATE, ITE, PEHE), urgency ranking, and attribution metrics from the GA stratified SCM |
| Simulated | NICU telemetry (SpO2, incubator), traffic, channel, and node energy |
| Assumed | Network topology, radio and power parameters, confounding strength γ (default 1.5) |

## Repository layout

```
InterventionROP-Edge/
├── InterventionROP_Edge.ipynb     # end to end Colab pipeline (add before publishing)
├── results/
│   └── results.json               # config, cohort statistics, causal estimates,
│                                  # confounding sweep, per seed metrics, node,
│                                  # channel and GA shift sweeps, lifetime, summary
├── figures/                       # result figures (PNG)
├── requirements.txt
└── LICENSE
```

## Installation

```
git clone https://github.com/Jingum741/InterventionROP-Edge.git
cd InterventionROP-Edge
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## Reproducing the results

Open `InterventionROP_Edge.ipynb` and run the cells from top to bottom. Download the dataset metadata from the original source (see below). The archived `results/results.json` allows every table and figure to be regenerated without rerunning the experiments.

## Main results

All numerical results, including means and standard deviations per method and Wilcoxon tests against the best baseline, are in `results/results.json` under the keys `default`, `per_seed`, `node_sweep`, `channel_sweep`, `ga_shift`, `lifetime`, and `summary`.

## Figures

| File | Content |
|---|---|
| `fig_results_01_ate_groups.png` | ATE per GA group and estimator |
| `fig_results_02_ite_by_stage.png` | ITE by ROP stage |
| `fig_results_03_pehe_vs_confounding.png` | PEHE versus confounding strength |
| `fig_results_04_urgency_rank_vs_confounding.png` | urgency ranking versus confounding |
| `fig_results_05_attribution_vs_confounding.png` | attribution versus confounding |
| `fig_results_06_convergence.png` | training convergence |
| `fig_results_07_cross_latency_vs_nodes.png`, `fig_results_08_energy_vs_nodes.png` | node count sweep |
| `fig_results_09_qos_vs_channel.png`, `fig_results_10_energy_vs_channel.png` | channel noise sweep |
| `fig_results_11_ga_shift_latency.png`, `fig_results_12_ga_shift_energy.png` | GA distribution shift |
| `fig_results_13_network_lifetime.png` | network lifetime |
| `fig_results_14_latency_cdf.png` | latency CDF |
| `fig_results_15_energy_latency.png` | energy and latency trade off |
| `fig_results_16_ablation.png` | ablation study |

## Data sources and licences

Retinal Image Dataset of Infants and ROP, Timkovič et al., Scientific Data, 2024, DOI 10.1038/s41597-024-03409-7. Only clinical metadata is used; no images are redistributed. Obtain the data from the original source under its terms.

## Limitations

Retinal images were not used; the causal layer relies on clinical metadata only. NICU telemetry and the network are simulated. The dataset contains 20 ROP patients and 27 treated series, so treatment effect estimates in the older GA strata rest on very few treated cases.

## Citation and licence

A citation will be added once the manuscript is published. The code is released under the MIT licence (see `LICENSE`). Data retain the licences of their original sources.
