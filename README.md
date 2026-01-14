[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=22232312)
# Pandas Wrangling Restaurant Lab

Follow these steps to complete the assignment using Databricks (or a Jupyter notebook if instructed).

## Step 1 — Set up your notebook (5–10 min)
1. In Databricks, open **Workspace → Your Name → Create → Notebook**.
2. Name it: `pandas_wrangling_restaurant_lab_<lastname>`.
3. Choose **Python** as the language and attach the notebook to your running cluster. Create a new cluster via **Compute → Create Cluster** if needed.

## Step 2 — Upload the data (10–15 min)
1. You will use two small CSV files: `menu_items.csv` and `order_details.csv`.
2. In the left sidebar, click the **Data** icon, then **Add Data → Upload files to a Volume**.
3. Select **Workplace → default** and click the **+** to create a managed volume named `pandas`.
4. Upload both files into the new volume:
   - `menu_items.csv`
   - `order_details.csv`
5. You should have two files in the volume. Example paths:
   - `/Volumes/workspace/default/pandas/menu_items.csv`
   - `/Volumes/workspace/default/pandas/order_details.csv`

## Step 3 — Load the data (15 min)
```python
import pandas as pd

menu = pd.read_csv('/Volumes/workspace/default/pandas/menu_items.csv')
orders = pd.read_csv('/Volumes/workspace/default/pandas/order_details.csv')

menu.head()
orders.head()
menu.info()
orders.info()
orders.describe()
```
Checkpoint: Identify data types for each column and note any missing or incorrect values.

## Step 4 — Clean the data (30 min)
```python
menu.isnull().sum()
orders.isnull().sum()
```
Handle missing or inconsistent data (e.g., fill missing prices, drop duplicates, standardize category names). Convert date/time columns:
```python
orders['order_date'] = pd.to_datetime(orders['order_date'])
orders['order_time'] = pd.to_datetime(orders['order_time'])
```
Checkpoint: Print how many rows remain after cleaning.

## Step 5 — Merge the DataFrames (20 min)
Join the orders and menu tables on the menu item ID:
```python
combined = orders.merge(menu, left_on='item_id', right_on='menu_item_id')
combined.head()
```
Checkpoint: Record the total number of rows in the merged dataset.

## Step 6 — Analyze the data (1 hour)
Use pandas to answer:
- Most and least popular items:
  ```python
  popular_items = combined['item_name'].value_counts()
  ```
- Categories generating the most revenue:
  ```python
  revenue_by_category = combined.groupby('category')['price'].sum().sort_values(ascending=False)
  ```
- Times of day with the most orders:
  ```python
  combined['hour'] = combined['order_time'].dt.hour
  orders_by_hour = combined.groupby('hour').size()
  ```
- Days with the highest order counts:
  ```python
  orders_by_day = combined.groupby(combined['order_date'].dt.day_name()).size()
  ```
- Top 5 most profitable menu items:
  ```python
  profit_items = combined.groupby('item_name')['price'].sum().sort_values(ascending=False).head(5)
  ```

## Step 7 — Ethics in data (10 min)
Reflect on:
- Privacy or fairness concerns when analyzing restaurant data.
- How to use data to serve customers rather than manipulate them.

## Step 8 — Summary report (20–30 min)
Add to the end of your notebook:
- Markdown summary (3–5 bullet points).
- 1–2 surprising insights.
- One way this skill could apply to your final AI project.

### Example summary
- The most popular item was “Pad Thai” — over 250 orders this quarter.
- Lunch hours (11 AM–1 PM) were busiest.
- Italian cuisine generated the highest total revenue.

### Submission checklist
Your notebook must show:
- All code cells and outputs for loading, cleaning, merging, analysis, and summary.
- Answers to each checkpoint in Markdown after the relevant step.
- Your ethics reflection and summary report.
- Ability to run top-to-bottom without errors.
