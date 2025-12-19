# Time-Budgeted Recommendation Lists with Reinforcement Learning

This project models recommendation as **constructing an ordered list of items under a finite user time budget**. Each recommended item consumes some amount of the user’s time (attention / evaluation cost). The agent must decide *which* items to include and *in what order*, balancing **expected usefulness** against the **time constraint**.

The key idea: time is a scarce resource, and optimal recommendations should reflect tradeoffs under that constraint—similar to other budgeted decision problems in economics and operations research.

---

## What this repo does

- Implements a **Gymnasium environment** for time-budgeted list construction:
  - At each step the agent selects one item to append to a list.
  - Each selected item consumes its per-item time cost immediately.
  - The episode ends when the **time budget is exhausted** or the list reaches a max length (`horizon`).
- Trains a **DQN (Deep Q-Network)** baseline in PyTorch.
- Provides a small evaluation script that reports:
  - Average episode return
  - Average number of “successes”
  - Average time used and time utilization

This repo is intentionally compact and easy to modify for research experiments.

---
## Installation
python -m venv .venv
source .venv/bin/activate
pip install -e .
---
## Train
python -m bcrl.train_dqn \
  --total-steps 50000 \
  --time-budget 10 \
  --horizon 20 \
  --n-items 100

## Evaluate
python -m bcrl.evaluate \
  --checkpoint checkpoints/dqn_latest.pt \
  --episodes 200 \
  --time-budget 10 \
  --horizon 20 \
  --n-items 100

## Example learning curve

Over the first ~10k steps, the agent behaves near-randomly (high ε), then:

Learns to avoid invalid actions almost entirely

Increases average return steadily

Stabilizes after ~35k steps

Typical final behavior:

Uses ~80–90% of the time budget per episode

Selects a mix of “high-value expensive” items and “medium-value cheap” items



