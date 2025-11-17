```
bbac-ics-dataset/
├── README.md                  # Description, citation
├── LICENSE                    # CC BY 4.0
├── CITATION.cff              
├── USAGE.txt                  # How to use (Python, R, MATLAB)
├── data/                      # Processed data
│   ├── full.csv
│   ├── train.csv
│   ├── val.csv
│   ├── test.csv
│   ├── profiles.json
│   ├── policies.json
│   └── metadata.json
└── code/                      # Reproducibility
    ├── generate.py
    └── requirements.txt
```



```
bbac-ics-simulator/
├── README.md
├── requirements.txt
├── simulator/
│   ├── data_loader.py         # Downloads dataset do GitHub
│   ├── authentication.py
│   ├── behavioral_analysis.py
│   ├── policy_engine.py
│   ├── decision_maker.py
│   └── modules/
├── dashboard/
│   └── app.py
└── config.yaml
```


┌─────────────────────┬────────────────┬────────────────┬──────────────┬────────────────┐
│ Criterion           │ IEEE Data Desc.│ Scientific Data│ IEEE Access  │ Data in Brief  │
├─────────────────────┼────────────────┼────────────────┼──────────────┼────────────────┤
│ Specific datasets   │ Yes            │ Yes            │ No           │ Yes            │
│ Pages               │ 4-8            │ ~10            │ <20          │ 2-3            │
│ GitHub accepted     │ Yes + DOI      │ No             │ Yes + DOI    │ Yes + DOI      │
│ Timeline            │ 2-4 months     │ 3-6 months     │ 2-4 months   │ 1-2 months     │
└─────────────────────┴────────────────┴────────────────┴──────────────┴────────────────┘



### Existing dataset

-   CASPER (2023): IEEE DataPort
-   Energy Cobots (2021): IEEE DataPort
-   TON_IoT (2020): IoT security reference
-   ICS-Flow (2023): most recent in ICS


### Key characteristics

| Aspect | Description |
|---------|----------|
| **Simulated period** | 30 days |
| **Total events** | 50.000 |
| **Normal events** | 40.000 (80%) |
| **Behavioral anomalies** | 7.500 (15%) |
| **Policy violations** | 2.500 (5%) |
| **Agents** | 6 (3 robots + 3 humans) |
| **Resources** | 8 sistemas diferentes |
| **Split** |	train/validation/test (70/15/15) |
| **Anomaly** | 10 types |


### System agents

| ID | Type | Role | Shift | Events | Typical resources     |
|----|------|--------|-------|---------|----------------------|
| R01 | Robot | Assembly | 24/7 | 12.600 | assembly plans, |
|     |      |          |      |        | part sensors   |
| R02 | Robot | Quality inspection | 24/7 | 10.074 | cameras, |
|     |      |          |      |        | specification data |
| R03 | Robot | Transport/AGV | 24/7 | 9.911 | factory floor map, |
|     |      |          |      |        | inventory status |
| H01 | Human | Senior operator | 06:00-14:00 | 7.359 | all systems |
| H02 | Human | Maintenance technician | 14:00-22:00 | 5.118 | diagnostic logs, |
|     |      |          |      |        | control panel |
| H03 | Human | Supervisor | 08:00-17:00 | 4.938 | management dashboards, |
|     |      |          |      |        | reports |

### System resources

| ID | Name	| Sensitivity	| Events	| Normal | Access Hours |
|----|------|---------------|---------|--------------------------|
| RES_INV | inventory data	| medium | 12.766 | 06:00-22:00 |
| RES_PLAN | assembly plans	| high | 8.448 | 24/7 |
| RES_CTRL | control panel	| critical | 5.617 | supervisors only |
| RES_SPEC | quality specifications	| medium | 5.143 | 24/7 |
| RES_MAP | navigation map	| low | 5.079 | 24/7 |
| RES_CAM | camera system	| medium | 5.050 | 24/7 |
| RES_DASH | management dashboard	| high | 4.978 | 08:00-17:00 |
| RES_DIAG | diagnostic logs	| high | 2.919 | 06:00-22:00 |


### Anomaly types

| Anomaly type | Quantity | Description |
|------------------|------------|-----------|
| NORMAL | 40,000 | expected behavior |
| UNUSUAL_DURATION | 1,538 | atypical action duration |
| HIGH_FREQUENCY | 1,501 | unusually high frequency |
| WRONG_SEQUENCE | 1,499 | incorrect action sequence |
| UNUSUAL_TIME | 1,493 | unusual timing |
| UNAUTHORIZED_RESOURCE | 1,469 | unauthorized resource |
| ACCESS_DENIED_ROLE | 641 | inappropriate role |
| MULTIPLE_LOGIN_ATTEMPTS | 628 | multiple login attempts |
| ACCESS_DENIED_TIME | 627 | time not permitted |
| UNAUTHORIZED_LOCATION | 604 | unauthorized location |


