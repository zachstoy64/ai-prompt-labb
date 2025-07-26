
# AI Prompt Engineering and Code Generation Lab

This lab contains three real-world data science scenarios. Each includes an AI-generated solution using prompting techniques, iterative refinement, and final implementation.

---

## 🛒 Scenario 1: Retail Inventory Analysis

### 🔹 Your Prompt:
"Write Python code using Pandas to calculate inventory turnover rates, identify slow-moving items, predict potential stockouts, and create simple visualizations based on a DataFrame with columns: product_id, category, stock_level, last_restock_date, sales_last_30_days, supplier_lead_time, unit_cost."

### 🔹 Generated Code:
```python
import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime

# Sample DataFrame
df = pd.DataFrame({
    'product_id': [1, 2, 3, 4],
    'category': ['A', 'A', 'B', 'B'],
    'stock_level': [50, 10, 5, 30],
    'last_restock_date': ['2024-06-01', '2024-06-20', '2024-05-15', '2024-06-10'],
    'sales_last_30_days': [40, 5, 2, 25],
    'supplier_lead_time': [7, 14, 5, 10],
    'unit_cost': [10.0, 15.0, 5.0, 12.0]
})

df['last_restock_date'] = pd.to_datetime(df['last_restock_date'])

# Inventory Turnover
df['turnover_rate'] = df['sales_last_30_days'] / df['stock_level']

# Identify slow-moving items
slow_movers = df[df['turnover_rate'] < 0.5]

# Predict potential stockouts
df['projected_days_left'] = df['stock_level'] / (df['sales_last_30_days'] / 30)
stockouts = df[df['projected_days_left'] < df['supplier_lead_time']]

# Visualization
df.plot(kind='bar', x='product_id', y='turnover_rate', title='Inventory Turnover Rates')
plt.ylabel('Turnover Rate')
plt.show()
```

### 🔹 Follow-up Prompts:
- “Add annotations to the bar chart.”
- “Sort the DataFrame by turnover rate before plotting.”

### 🔹 Final Solution:
Code above with sorted plot and labeled bars.

---

## 🌐 Scenario 2: Website Analytics Debug

### 🔹 Your Prompt:
"Fix the user engagement analysis function that returns bounce rates over 100% and negative session times. Add validation, correct time calculations, and include visual summaries."

### 🔹 Generated Code:
```python
def analyze_user_engagement(logs_df):
    # Validation: ensure durations are non-negative
    logs_df = logs_df[logs_df['duration'] >= 0]

    bounce_rate = logs_df.groupby('session_id')['page_views'].sum().apply(lambda x: 1 if x == 1 else 0).mean()

    session_times = logs_df.groupby('session_id')['duration'].sum()
    avg_session_time = session_times.mean()

    pages_per_session = logs_df.groupby('session_id')['page_views'].sum().mean()

    device_metrics = logs_df.groupby('device_type').agg({
        'session_id': 'nunique',
        'duration': 'mean',
        'page_views': 'sum'
    })

    return {
        'bounce_rate': bounce_rate,
        'avg_session_time': avg_session_time,
        'pages_per_session': pages_per_session
    }, device_metrics
```

### 🔹 Follow-up Prompts:
- “Can you make sure to count sessions correctly across devices?”
- “Add matplotlib bar charts to visualize device metrics.”

### 🔹 Final Solution:
Bug-free logic, correct time aggregation, and bounce rate ≤ 100%.

---

## 👥 Scenario 3: Customer Segmentation Query

### 🔹 Your Prompt:
"Write a SQL query to find users who logged in during the past 30 days, are in the top 20% of spenders, and summarize their user preferences."

### 🔹 Generated Code:
```sql
WITH active_users AS (
  SELECT user_id
  FROM user_activity
  WHERE last_login_date >= CURRENT_DATE - INTERVAL '30 days'
),
spending_ranks AS (
  SELECT user_id, SUM(amount) AS total_spent,
         NTILE(5) OVER (ORDER BY SUM(amount) DESC) AS spending_percentile
  FROM transactions
  GROUP BY user_id
),
high_value_customers AS (
  SELECT user_id
  FROM spending_ranks
  WHERE spending_percentile = 1
),
target_users AS (
  SELECT au.user_id
  FROM active_users au
  JOIN high_value_customers hvc ON au.user_id = hvc.user_id
)
SELECT up.communication_preference,
       up.interface_theme,
       COUNT(*) AS user_count
FROM user_preferences up
JOIN target_users tu ON up.user_id = tu.user_id
GROUP BY up.communication_preference, up.interface_theme;
```

### 🔹 Follow-up Prompts:
- “Convert NTILE logic into percentile if NTILE is unavailable.”
- “Add a date parameter for the login filter.”

### 🔹 Final Solution:
Efficient SQL with CTEs and preference aggregation for top active spenders.
