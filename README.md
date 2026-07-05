# Comment Ranking Intervention Simulation for Social Media Opinion Polarization

This repository contains the experimental source code for the study *Design and Simulation of Comment Ranking Interventions for Social Media Opinion Polarization*.

The code implements an agent-based simulation framework for evaluating how comment ranking mechanisms influence opinion polarization in social media discussions. It uses real-world comment streams, separates hot-ranked and time-ranked comment orders, applies ranking interventions, and generates the main experimental summary tables.

## Overview

The simulation evaluates whether comment ranking can amplify or mitigate opinion polarization. Comments are represented by continuous opinion scores in the range `[0, 1]`, where values near 0 indicate negative opinions, values near 1 indicate positive opinions, and values near 0.5 indicate neutral opinions.

For each ranking condition, the model constructs a global comment stream from the top 100 comments of each topic. Agents are then exposed to comments through rank-weighted random sampling. Higher-ranked comments receive greater exposure probability.

## Main Features

- Loads comment-level continuous opinion scores.
- Separates comments into two ranking conditions:
  - `HotRank`: popularity-based comment ranking.
  - `TimeRank`: chronological comment ranking.
- Constructs topic-level top-100 comment streams.
- Simulates seven initial public-opinion scenarios.
- Uses heterogeneous agents with bounded-confidence opinion updating.
- Implements rank-weighted random comment exposure.
- Tests two ranking intervention strategies:
  - scarcity-based promotion of underrepresented opinion regions.
  - suppression of extreme opinions.
- Repeats each experimental condition 10 times and reports averaged metrics.
- Generates summary tables for baseline polarization and optimal intervention results.

## Exposure Model

At each time step, each agent independently samples one comment from the global comment stream with replacement. The sampling probability is rank-weighted. For a comment ranked `r` within a topic-level top-100 list, its exposure weight is:

```text
w_r = 1 / r^alpha
```

The default setting is:

```text
alpha = 1.0
```

In the intervention condition, comments are first re-ranked, the new top-100 comments are retained, and exposure weights are recalculated from the intervention-adjusted ranks.

## Environment Setup

Python 3.10 or later is recommended.

Create a virtual environment:

```bash
python -m venv .venv
```

Activate the virtual environment on Windows:

```powershell
.\.venv\Scripts\activate
```

Activate the virtual environment on macOS or Linux:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Data Preparation

The original comment data and continuous opinion score file may contain comment text and are therefore not included in this public repository.

Before running the experiments, create a `data` directory in the project root and place the continuous score file there:

```text
data/sentiment_scores_16files.json
```

The file should follow the schema expected by `data_loader.py`.

## Usage

Run all commands from the project root.

Quick smoke test:

```bash
python run_experiments.py --quick
```

Run the full experiment grid:

```bash
python run_experiments.py
```

Generate the summary tables:

```bash
python make_tables.py
```

## Outputs

Generated results are saved under the `outputs` directory.

Main output files:

```text
outputs/paper_reproduction_results.csv
outputs/table3_baseline_polarization.csv
outputs/table4_best_full_intensity.csv
outputs/table5_best_low_intensity.csv
```

Table descriptions:

- `table3_baseline_polarization.csv`: baseline polarization under hot-ranked and time-ranked comment streams.
- `table4_best_full_intensity.csv`: best intervention results over the full intervention-intensity range.
- `table5_best_low_intensity.csv`: best intervention results under low-intensity intervention constraints.

## File Structure

```text
config.py             Experiment parameters
data_loader.py        Data loading and ranking-group construction
model.py              Agent-based opinion evolution model
interventions.py      Ranking intervention and exposure construction
metrics.py            Polarization and acceptance metric calculation
run_experiments.py    Main experiment runner
make_tables.py        Summary table generation
requirements.txt      Python dependencies
```
