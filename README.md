# BBAC-ICS Dataset

**Behavior-Based Access Control Dataset for Industrial Control Systems**

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXX)

O **BBAC-ICS Dataset** é um conjunto de dados sintéticos que modela o comportamento de robôs industriais em um ambiente ICS (Industrial Control System) submetido a políticas de controle de acesso baseadas em comportamento (*Behavior-Based Access Control — BBAC*).  

Este dataset foi projetado para pesquisa em:
- segurança comportamental em ICS,
- detecção de anomalias,
- simulação de robôs industriais,
- políticas RuBAC,
- modelos de decisão híbridos,
- Cadeias de Markov aplicadas à dinâmica operacional.

O dataset é 100% sintético — nenhum dado industrial real foi utilizado.

---

## Arquitetura de dados

### Hierarquia de dados

```
┌─────────────────────────────────────────────────────────────────┐
│                     ENTRADA DO SISTEMA                          │
│                      (RawLogEntry)                              │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                  ANÁLISE DE BASELINE                            │
│                  (BaselineMetrics)                              │
│  ┌──────────────┬──────────────┬──────────────┬──────────────┐  │
│  │ Ações        │ Horários     │ Recursos     │ Gaps         │  │
│  │ Comuns       │ Normais      │ Normais      │ Temporais    │  │
│  └──────────────┴──────────────┴──────────────┴──────────────┘  │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                    EXTRAÇÃO DE FEATURES                         │
│                    (ExtractedFeatures)                          │
│    • Temporais  • Sequenciais  • Contextuais  • Normalidade     │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│             DETECÇÃO DE ANOMALIAS (Dual Branch)                 │
│                 (AnomalyScores)                                 │
│   ┌────────────────────────────┬────────────────────────────┐   │
│   │ Branch 1: Estatístico      │ Branch 2: Sequencial       │   │
│   │ • KS Test                  │ • Predição de ação         │   │
│   │ • T-Test                   │ • Predição temporal        │   │
│   │ • MAD Score                │ • Erro de sequência        │   │
│   └────────────────────────────┴────────────────────────────┘   │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│               DECISÃO DE ACESSO (BBAC 3-Layer)                  │
│            (AccessDecisionRecord)                               │
│    Layer 1: Rules  →  Layer 2: Behavioral  →  Layer 3: ML       │
│   Decision: Grant Full / Monitor / MFA / Deny                   │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│                  MONITORAMENTO PÓS-ACESSO                       │
│                       (PostAccessLog)                           │
│    • Atividades  • Anomalias  • Feedback  • Aprendizado         │
└─────────────────────────────────────────────────────────────────┘

```

### Pipeline Completo

```
1. REQUISIÇÃO  
   RawLogEntry → Sistema  

2. FILTRAGEM E ORDENAÇÃO  
   Filter by user_id, robot_id  
   Sort by timestamp  

3. DECISÃO HFVL (High/Low Frequency)  
   Calculate action frequency
   
4. BASELINE CHECK  
   Load BaselineMetrics for user  
   If not exists → Calculate from historical data  

5. FEATURE EXTRACTION
   RawLogEntry → ExtractedFeatures  
   
6. ANOMALY DETECTION
   Branch 1: Statistical Analysis → statistical_score  
   Branch 2: Sequential Model → sequential_score  
   Combine → final_score

7. ACCESS DECISION
   Apply 3-Layer BBAC:  
     - Layer 1: Rule-based (policy_score)
     - Layer 2: Behavioral (Markov)
     - Layer 3: ML Anomaly (scores)
   Output → AccessDecisionRecord

8. POST-ACCESS
   Monitor activities → PostAccessLog  
   Feedback → Update baseline/model

```

---

## 🔍 Geração do Dataset

O dataset foi produzido pelo pipeline localizado em:

👉 **https://github.com/xxx/xxx**

Esse repositório contém:
- modelos comportamentais,  
- regras de geração,  
- parâmetros industriais,  
- scripts de exportação e validação.

O código *não* é incluído no dataset para manter a integridade dos dados publicados.

---

## 📁 Estrutura do repositório

```
bbac_ics_dataset/
│    
├── README.md             
├── LICENSE
├── data_1m/                      
│   ├── agents.json
│   ├── anomaly_metadata.json
│   ├── full.csv
│   ├── test.csv
│   ├── training.csv
│   ├── validation.csv
│   └── dataset_metadata.json
├── data_100k/
├── data_300k/
└── data_500k/

```

---

## 📊 Arquivos do Dataset

### **CSV - resultados**
- **full** — 100% eventos de comunicação robótica  
- **test** — 15% registros  
- **training** — 70% registros  
- **validation** — 15% registros  

Cada linha dos CSV representa um evento ICS no tempo:

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `log_id` | string | Identificador único do log (formato: log_YYYYMMDD_HHMMSS_microsec_user_id) |
| `timestamp` | string | Marca temporal do evento (ISO-8601: YYYY-MM-DD HH:MM:SS) |
| `session_id` | string | Identificador da sessão de trabalho do agente |
| `user_id` | string | Identificador do agente (robot_TYPE_NN ou human_ROLE_NN) |
| `agent_type` | string | Tipo de agente: `robot`, `human`, `system` |
| `robot_type` | string | Tipo do robô: `assembly_robot`, `camera_robot`, `inspection_robot`, `transport_robot` (null para humanos) |
| `human_role` | string | Papel humano: `operator`, `technician`, `supervisor`, `admin` (null para robôs) |
| `action` | string | Tipo da ação: `read`, `write`, `execute`, `delete`, `modify`, `access`, `monitor` |
| `resource` | string | Identificador do recurso acessado (ex: sensor_motion_01, database_footage) |
| `resource_type` | string | Categoria do recurso: `sensor`, `actuator`, `camera`, `database`, `config`, `tool`, `zone` |
| `human_present` | boolean | Flag indicando presença humana no ambiente (true/false) |
| `emergency_flag` | boolean | Flag de situação de emergência (true/false) |
| `location` | string | Localização física: `zone_a`, `zone_b`, `warehouse`, `production_floor`, etc. |
| `previous_action` | string | Ação anterior na sequência do mesmo agente (null para primeira ação) |

### **JSON - Metadados**

#### **agents.json**

Perfis comportamentais de cada agente (robôs e humanos):

```json
{
  "user_id": "robot_assembly_01",
  "agent_type": "robot",
  "robot_type": "assembly_robot",
  "human_role": null,
  "behavior_pattern": {
    "working_hours": [6, 7, 8, ..., 22],
    "actions": {
      "read": 0.50,
      "execute": 0.35,
      "write": 0.10,
      "monitor": 0.05
    },
    "resources": ["sensor_force_01", "actuator_gripper", ...],
    "avg_time_gap_seconds": 45.0,
    "std_time_gap_seconds": 15.0,
    "human_presence_prob": 0.8
  }
}
```

#### **anomaly_metadata.json**

Ground truth das anomalias injetadas:

```json
{
  "anomaly_id": "anomaly_000001",
  "anomaly_type": "temporal_anomaly",
  "severity": "high",
  "affected_logs": 15,
  "agent_id": "robot_camera_05"
}
```

**Tipos de anomalia:**
- `temporal_anomaly` - Acesso fora do horário normal
- `behavioral_anomaly` - Ação/recurso incomum para o agente
- `frequency_anomaly` - Burst de acessos (frequência anormal)
- `sequence_anomaly` - Ordem incorreta de ações
- `resource_anomaly` - Acesso a recurso não autorizado

#### **dataset_metadata.json**

Estatísticas globais do dataset:

```json
{
  "total_logs": 1000000,
  "total_agents": 50,
  "num_robots": 40,
  "num_humans": 10,
  "date_range": {
    "start": "2025-12-01 00:00:00",
    "end": "2026-01-07 23:27:32"
  },
  "action_distribution": {...},
  "resource_type_distribution": {...},
  "anomaly_count": 50000,
  "anomaly_rate": 0.05,
  "anomaly_type_distribution": {...}
}

```

---

## Dataset Statistics

Total agents: 50
  Robots: 40
  Humans: 10

### 100k
General:
  Total logs: 100,000  
  Date range: 2025-12-01 00:00:00 to 2025-12-03 23:58:45.411369

Actions:
  access: 2,910 (2.9%)  
  delete: 666 (0.7%)  
  execute: 18,505 (18.5%)  
  modify: 3,668 (3.7%)  
  monitor: 23,329 (23.3%)  
  read: 39,835 (39.8%)  
  write: 11,087 (11.1%)

Anomalies:
  Total: 5000
  Rate: 5.00%  
  behavioral_anomaly: 1263 (25.3%)  
  frequency_anomaly: 999 (20.0%)  
  resource_anomaly: 500 (10.0%)  
  sequence_anomaly: 738 (14.8%)  
  temporal_anomaly: 1500 (30.0%)

### 300k
General:
  Total logs: 300,000  
  Date range: 2025-12-01 00:00:00 to 2025-12-11 23:57:17.421717

Actions:
  access: 8,939 (3.0%)  
  delete: 2,090 (0.7%)  
  execute: 55,153 (18.4%)  
  modify: 10,957 (3.7%)  
  monitor: 71,354 (23.8%)  
  read: 118,957 (39.7%)  
  write: 32,550 (10.8%)

Anomalies:
  Total: 15000
  Rate: 5.00%  
  behavioral_anomaly: 3825 (25.5%)  
  frequency_anomaly: 3008 (20.1%)  
  resource_anomaly: 1488 (9.9%)  
  sequence_anomaly: 2283 (15.2%)  
  temporal_anomaly: 4396 (29.3%)

### 500k
General:
  Total logs: 500,000  
  Date range: 2025-12-01 00:00:00 to 2025-12-19 22:44:54.772359

Actions:
  access: 14,763 (3.0%)  
  delete: 3,453 (0.7%)  
  execute: 92,414 (18.5%)  
  modify: 18,537 (3.7%)  
  monitor: 117,703 (23.5%)  
  read: 198,824 (39.8%)  
  write: 54,306 (10.9%)

Anomalies:
  Total: 25000
  Rate: 5.00%  
  behavioral_anomaly: 6341 (25.4%)  
  frequency_anomaly: 5000 (20.0%)  
  resource_anomaly: 2410 (9.6%)  
  sequence_anomaly: 3699 (14.8%)  
  temporal_anomaly: 7550 (30.2%)

### 1m
General:
  Total logs: 1,000,000  
  Date range: 2025-12-01 00:00:00 to 2026-01-07 23:27:32.984393

Actions:
  access: 29,678 (3.0%)  
  delete: 6,877 (0.7%)  
  execute: 184,917 (18.5%)  
  modify: 36,638 (3.7%)  
  monitor: 236,597 (23.7%)  
  read: 396,935 (39.7%)  
  write: 108,358 (10.8%)

Anomalies:
  Total: 50000
  Rate: 5.00%  
  behavioral_anomaly: 12658 (25.3%)  
  frequency_anomaly: 10062 (20.1%)  
  resource_anomaly: 4802 (9.6%)  
  sequence_anomaly: 7484 (15.0%)  
  temporal_anomaly: 14994 (30.0%)

---

## 🚀 Quick Start

### Carregar dados em Python

```python
import pandas as pd
import json

# Carregar dataset
df = pd.read_csv('data_1m/full.csv')
df['timestamp'] = pd.to_datetime(df['timestamp'])

# Carregar metadados
with open('data_1m/agents.json', 'r') as f:
    agents = json.load(f)

with open('data_1m/anomaly_metadata.json', 'r') as f:
    anomalies = json.load(f)

# Informações básicas
print(f"Total de eventos: {len(df):,}")
print(f"Período: {df['timestamp'].min()} a {df['timestamp'].max()}")
print(f"\nDistribuição de ações:")
print(df['action'].value_counts())

# Filtrar por tipo de agente
robots = df[df['agent_type'] == 'robot']
humans = df[df['agent_type'] == 'human']
```

### Carregar dados em R

```r
library(readr)
library(jsonlite)

# Carregar dataset
df <- read_csv("data_1m/full.csv")
df$timestamp <- as.POSIXct(df$timestamp)

# Carregar metadados
agents <- fromJSON("data_1m/agents.json")
anomalies <- fromJSON("data_1m/anomaly_metadata.json")

# Informações básicas
summary(df)
table(df$action)
```

### Identificar anomalias
```python
# IDs de agentes com anomalias
anomalous_agents = {a['agent_id'] for a in anomalies}

# Separar logs normais e anômalos
normal_logs = df[~df['user_id'].isin(anomalous_agents)]
anomaly_logs = df[df['user_id'].isin(anomalous_agents)]

print(f"Logs normais: {len(normal_logs):,}")
print(f"Logs anômalos: {len(anomaly_logs):,}")
```

---

## Citation

If you use this code in your research, please cite:

```bibtex
@dataset{silva2026bbac_ics_dataset,
  author = {Alexandre do Nascimento Silva and Nastaran Farhadi-Ghalati and Sanaz Nikghadam-Hojjati and Jos{\'e} Barata and Luiz Estrada Jimenez and Roberto Luiz Souza Monteiro},
  title = {BBAC-ICS Dataset},
  journal = {Under Review},
  year = 2026,
  doi = {10.5281/zenodo.XXXXXX}
}
```

---

## License

Creative Commons Attribution 4.0 International (CC BY 4.0)

```
CC BY 4.0 - 2025 Alexandre [Sobrenome] et al.
Full text: https://creativecommons.org/licenses/by/4.0/
```

---

## 👥 Authors & Contact

- **Alexandre do Nascimento Silva** (Corresponding Author)  
  Universidade Estadual de Santa Cruz (UESC), Departamento de Engenharias e Computação. 
  Universidade do Estado da Bahia (UNEB), Programa de Pós-graduação em Modelagem e Simulação em Biossistemas (PPGMSB). 
  📧 alnsilva@uesc.br

- **Nastaran Farhadi-Ghalati**
  UNINOVA—Center of Technology and Systems (CTS). 
  📧 n.ghalati@campus.fct.unl.pt

- **Sanaz Nikghadam-Hojjati**  
  UNINOVA—Center of Technology and Systems (CTS). 
  📧 sanaznik@uninova.pt

- **José Barata**  
  UNINOVA—Center of Technology and Systems (CTS). 
  📧 lestrada@uninova.pt

- **Luiz Estrada**  
  UNINOVA—Center of Technology and Systems (CTS). 
  📧 jab@uninova.pt

- **Roberto Luiz Souza Monteiro**  
  Universidade SENAI CIMATEC. 
  Universidade do Estado da Bahia (UNEB), Programa de Pós-graduação em Modelagem e Simulação em Biossistemas (PPGMSB). 
  📧 roberto.monteiro@fieb.org.br

## 🙏 Acknowledgments

This research was supported by:
- Coordenação de Aperfeiçoamento de Pessoal de Nível Superior (CAPES)
- Universidade Estadual de Santa Cruz (UESC)
- Universidade do Estado da Bahia (UNEB)
- UNINOVA—Center of Technology and Systems (CTS)

---
