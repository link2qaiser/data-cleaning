## Python Sample Code

```python
import pandas as pd
from fuzzywuzzy import process

# Load datasets
agents_df = pd.read_excel('./agents.xlsx')
teams_df = pd.read_excel('./teams.xlsx')
transactions_df = pd.read_excel('./transactions.xlsx')

# Standardize names for brokerages and teams
def standardize_name(name):
    return name.lower().strip() if isinstance(name, str) else name

agents_df['brokerage_name'] = agents_df['brokerage_name'].apply(standardize_name)
agents_df['team_name'] = agents_df['team_name'].apply(standardize_name)

# Example fuzzy matching for brokerage names
unique_brokerages = agents_df['brokerage_name'].unique()
brokerage_mappings = {name: process.extractOne(name, unique_brokerages)[0] for name in unique_brokerages}

# Apply mappings
agents_df['brokerage_name'] = agents_df['brokerage_name'].replace(brokerage_mappings)

# Merge agents with transactions on agent ID
merged_df = pd.merge(agents_df, transactions_df, on='agent_id', how='inner')
```

## Create Brokerages Table

```sql
CREATE TABLE Brokerages (
    brokerage_id SERIAL PRIMARY KEY,
    brokerage_name VARCHAR(255) UNIQUE,
    office_address TEXT
);
```

## Query for Brokerage Performance Metrics

```sql
SELECT
    b.brokerage_name,
    COUNT(t.transaction_id) AS total_transactions,
    AVG(t.transaction_value) AS avg_transaction_value
FROM Brokerages b
JOIN Agents a ON b.brokerage_id = a.brokerage_id
JOIN Transactions t ON a.agent_id = t.agent_id
GROUP BY b.brokerage_name;

```

## Query for Brokerage Performance Metrics

```sql
SELECT
    b.brokerage_name,
    COUNT(t.transaction_id) AS total_transactions,
    AVG(t.transaction_value) AS avg_transaction_value
FROM Brokerages b
JOIN Agents a ON b.brokerage_id = a.brokerage_id
JOIN Transactions t ON a.agent_id = t.agent_id
GROUP BY b.brokerage_name;

```

## Query for Team Performance Metrics

```sql
SELECT
    tm.team_name,
    COUNT(t.transaction_id) AS total_transactions,
    SUM(t.transaction_value) AS total_sales
FROM Teams tm
JOIN Agents a ON tm.team_id = a.team_id
JOIN Transactions t ON a.agent_id = t.agent_id
GROUP BY tm.team_name;

```
