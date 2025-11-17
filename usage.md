BBAC-ICS Dataset - Usage Instructions
======================================

LOADING DATA IN PYTHON
----------------------

import pandas as pd

# Load full dataset
df = pd.read_csv('data/full.csv')

# Convert timestamp
df['timestamp'] = pd.to_datetime(df['timestamp'])

# View basic info
print(f"Total events: {len(df)}")
print(df.head())

# Filter by agent
robot_events = df[df['agent_type'] == 'Robot']
human_events = df[df['agent_type'] == 'Human']

# Filter by anomaly
normal = df[df['anomaly_type'] == 'NORMAL']
anomalies = df[df['anomaly_type'] != 'NORMAL']


LOADING DATA IN R
-----------------

library(readr)

# Load data
df <- read_csv("data/full.csv")

# Convert timestamp
df$timestamp <- as.POSIXct(df$timestamp)


LOADING DATA IN MATLAB
----------------------

% Load data
data = readtable('data/full.csv');

% Convert timestamp
data.timestamp = datetime(data.timestamp, 'InputFormat', 'yyyy-MM-dd HH:mm:ss');


DATA SCHEMA
-----------

timestamp       - Event timestamp (YYYY-MM-DD HH:MM:SS)
agent_id        - Agent identifier (R01-R03, H01-H03)
agent_type      - Robot or Human
role            - Agent function (Assembly, Inspection, etc.)
resource        - Accessed resource (RES_PLAN, RES_CAM, etc.)
action          - Type of action (READ, UPDATE, LOGIN)
location        - Physical location
duration_sec    - Action duration in seconds
success         - Access granted (TRUE/FALSE)
behavioral_score - Behavioral analysis score (0.0-1.0)
policy_score    - Policy compliance score (0.0-1.0)
anomaly_type    - NORMAL or anomaly category


PRE-SPLIT DATASETS
------------------

train.csv - 35,000 events (70%) for training
val.csv   - 7,500 events (15%) for validation
test.csv  - 7,500 events (15%) for testing


METADATA FILES
--------------

profiles.json - Behavioral profiles for each agent
policies.json - Access control policies (RuBAC)
metadata.json - Dataset statistics


For questions or issues, please contact: [email]
