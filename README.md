# securite-anomaly-detection
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

## Dataset

[CIC-IDS-2017](https://www.unb.ca/cic/datasets/ids-2017.html) — Canadian Institute for 
Cybersecurity, 5-day network capture with simulated benign traffic and real attack scenarios.
