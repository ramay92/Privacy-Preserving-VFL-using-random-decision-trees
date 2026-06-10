# Privacy-Preserving-VFL-using-random-decision-trees
Lightweight privacy-preserving VFL framework using Totally Random Decision Trees. Combines (ε,δ)-DP for leaf weight protection and (2,3)-replicated secret sharing for secure inference. Protects features, thresholds, and query inputs across training and inference without expensive cryptographic operations.
# Privacy-Preserving Vertical Federated Learning with Totally Random Decision Trees

A lightweight VFL framework for collaborative gradient boosted tree learning over 
vertically partitioned data. Combines Totally Random Decision Trees (TRDTs) with 
(ε,δ)-differential privacy for leaf weight protection and (2,3)-replicated secret 
sharing via MP-SPDZ for secure inference.

In a typical VFL setting, an **Active Party (AP)** holds labels and a feature 
subset, while a **Passive Party (PP)** holds additional features over the same 
aligned samples. This framework trains an ensemble of TRDTs collaboratively 
without exposing raw features, labels, gradients, or thresholds to any other 
party. Inference is performed entirely within the secret-shared domain — only 
the final prediction is revealed to the querying client.

# Key Properties
- **Training-time privacy**: thresholds are secret-shared to MPC servers; labels 
  and gradients remain exclusively on the AP
- **Model-level privacy**: leaf weights are protected with (ε,δ)-DP before being 
  stored as secret shares
- **Secure inference**: client features and thresholds never leave the 
  secret-shared domain; only the final sigmoid probability is revealed

---

## System Components

| File | Role |
|---|---|
| `compare.mpc` | MP-SPDZ MPC program — handles threshold setup, secure comparison, leaf accumulation, and prediction reveal |
| `activeparty.py` | AP: holds labels and AP features, computes gradients and DP-protected leaf weights |
| `passiveparty.py` | PP: holds PP features, computes and secret-shares thresholds |
| `server.py` | Coordinator: registers features, builds trees via random split selection, orchestrates MPC inference |
| `mpc_client.py` | MPC client wrappers for AP, PP, Server, and ExternalClient |
| `message_bus.py` | In-process communication bus between AP, PP, and Server |
| `main.py` | Orchestration script: training, MPC setup, inference, and result logging |
| `inference_client.py` | Standalone external client for submitting inference queries |

---

## Architecture
