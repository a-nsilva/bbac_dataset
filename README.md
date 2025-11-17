```markdown
# BBAC-ICS Dataset

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXX)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

**Behavior-Based Access Control for Industrial Cyber-Physical Systems**

## Overview

50,000 access control events from industrial shopfloor with human-robot collaboration.

**Features:**
- 🤖 6 agents (3 robots + 3 humans)
- 📅 30 days operations
- 🔒 8 system resources
- ⚠️ 10 anomaly types
- 📊 Quality: Δ=0.368

## Quick Start

\`\`\`python
import pandas as pd
df = pd.read_csv('data/full.csv')
\`\`\`

## Data Schema

| Field | Type | Description |
|-------|------|-------------|
| timestamp | datetime | Event time |
| agent_id | string | R01-R03, H01-H03 |
| resource | string | RES_PLAN, RES_CAM, etc |
| behavioral_score | float | 0-1 |
| anomaly_type | string | NORMAL or anomaly |

## Statistics

- Total: 50,000 events
- Normal: 80% | Anomalies: 20%
- Success rate: 95.14%
- Separability: 0.368

## Citation

\`\`\`bibtex
@dataset{alexandre2025bbacics,
  author = {Alexandre et al.},
  title = {BBAC-ICS Dataset},
  year = 2025,
  doi = {10.5281/zenodo.XXXXXX}
}
\`\`\`

## License

CC BY 4.0
```

---

## 📄 LICENSE

```
CC BY 4.0 - 2025 Alexandre [Sobrenome] et al.
Full text: https://creativecommons.org/licenses/by/4.0/
```

---

## 📄 CITATION.cff

```yaml
cff-version: 1.2.0
message: "If you use this dataset, please cite it."
authors:
  - family-names: "[Sobrenome]"
    given-names: "Alexandre"
title: "BBAC-ICS Dataset"
version: 1.0.0
doi: 10.5281/zenodo.XXXXXX
date-released: 2025-11-16
license: CC-BY-4.0
```

---

## 📄 requirements.txt

```
pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.3.0
```

---

## 📄 examples/load.py

```python
import pandas as pd

# Load data
df = pd.read_csv('../data/full.csv')
df['timestamp'] = pd.to_datetime(df['timestamp'])

print(f"Loaded {len(df):,} events")
print(df.head())
```

---

## 🎯 **ESTRUTURA**

```
bbac-ics-dataset/
│
├── README.md                  # Descrição completa
├── LICENSE                    # CC BY 4.0
├── CITATION.cff               
│
├── data/                      
│   ├── full.csv               # 50k
│   ├── train.csv              # 35k
│   ├── validation.csv         # 7.5k
│   ├── test.csv               # 7.5k
│   ├── profiles.json          # Perfis comportamentais
│   ├── policies.json          # Políticas RuBAC  
│   └── metadata.json          # Estatísticas
│
├── code/                      
│   ├── generate.py            # Script
│   └── requirements.txt       # Dependências
│
└── USAGE.txt                  # Como carregar
```

---
