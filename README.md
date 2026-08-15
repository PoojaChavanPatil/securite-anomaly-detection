# Securite-anomaly-detection
Exploratory data analysis and unsupervised modeling on the CIC-IDS-2017 network intrusion  dataset.


## Goal

Detect network attacks (starting with PortScan) without using labels during training — 
train a model purely on benign traffic, then evaluate whether it can flag attack traffic 
as anomalous, using real labels only for evaluation afterward.

## Contents

- `SecurITe_Monday.ipynb` — EDA and cleaning of Monday's 100% benign baseline traffic. 
  Includes discovery of a CICFlowMeter integer overflow bug affecting several columns.
- `SecurITe_Friday_Aft_port.ipynb` — EDA and cleaning of Friday afternoon's PortScan traffic, 
  compared against Monday's baseline to identify separating features (packet count, duration, 
  byte volume).
- `SecurITe_Unsupervised_Model.ipynb` — First modeling pass using Isolation Forest and DBSCAN, 
  trained on Monday's benign data and tested against Friday's PortScan traffic.

## Current Status

First-pass model trained on a single day (Monday) and tested against one attack type 
(PortScan). Isolation Forest and DBSCAN both showed limited separation when using all 78 
features.

Next step: expand training data to include benign traffic from all five days, 
and test against additional attack types (Brute Force, DoS, Web Attacks, Infiltration, Bot, 
DDoS) for a more complete evaluation.

## Key Insights — What Worked, What Didn't

### What worked
- **Data quality investigation paid off.** Found a real integer overflow bug in CICFlowMeter 
  (the tool that generated this dataset) — impossible negative values in Header Length and 
  related columns, traced back to the root cause rather than just deleted blindly.
- **EDA correctly predicted the model's strongest signals.** Packet count, flow duration, 
  and byte volume showed the clearest separation between BENIGN and PortScan traffic in 
  both statistics and visualizations — before any modeling was done.
- **Adjusting Isolation Forest's contamination setting, based on real EDA findings** (not a 
  guess), improved PortScan recall roughly 10x — from 201 to 2,078 caught out of ~158,000.

### What didn't work (yet)
- **Isolation Forest, even after tuning, still had weak overall recall** (~1.3%) — improving 
  contamination came at the cost of a 3x increase in false positives on benign traffic, a 
  real precision/recall tradeoff.
- **DBSCAN struggled to separate BENIGN from PortScan across all 78 features**, even though 
  the two groups formed a visually distinct cluster in a simplified 2-feature scatter plot. 
  Suggests the clean separation seen in 2D doesn't hold as cleanly once all features are 
  considered together.

### Working hypothesis for next steps
The gap between "clear 2-feature visual separation" and "weak full-feature model 
performance" suggests dimensionality reduction (e.g. PCA) or targeted feature selection 
may be needed before clustering/isolation-based methods can perform well. Also planning to 
expand training data beyond Monday alone (using benign traffic from all 5 days) for a more 
robust baseline.

## Dataset

[CIC-IDS-2017](https://www.unb.ca/cic/datasets/ids-2017.html) — Canadian Institute for 
Cybersecurity, 5-day network capture with simulated benign traffic and real attack scenarios.
