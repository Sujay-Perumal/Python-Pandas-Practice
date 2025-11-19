# 🐍 Python Pandas Practice

![Python](https://img.shields.io/badge/Python-Data%20Processing-3776AB?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Notebook-Jupyter-F37626?logo=jupyter&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

A collection of Jupyter notebooks practicing **Pandas** with real datasets.  
This repo demonstrates practical skills in **data manipulation**, **cleaning**, **aggregation**, **missing-value handling**, and **exploratory analysis** using Python.

---

## 📁 Notebooks

- `Pandas Day 1.ipynb`  
- `PandasDataFrames_01 Sujay.ipynb`  
- `PandasDataFrames_02 Sujay.ipynb`  
- `03_Pandas_DataFrames_Continued_01_Sujay.ipynb`  
- `Pandas Practice Notebook Sujay.ipynb`  

Each notebook builds slightly more complexity and realism into the exercises.

---

## 🧠 Skills Demonstrated (with concrete examples)

### 1️⃣ Pandas Fundamentals — `Pandas Day 1.ipynb`

**Topics:**

- Creating Series/DataFrames  
- Reading Excel files  
- Selecting rows/columns with `[]`, `.loc`, `.iloc`  
- Boolean filtering & conditional logic  

**Examples from the notebook:**

```python
import pandas as pd

# From a Python dictionary to a DataFrame
data = {
    "calories": [420, 380, 390],
    "duration": [50, 40, 45]
}
df = pd.DataFrame(data)

# Reading an Excel file of students
df = pd.read_excel("student.xlsx")

# Quickly inspect the data
df.head(10)
df.tail(2)
df.sample(3)
# Column selection & basic stats
df[["name", "class", "mark"]].head()
df.mark.mean()
# Boolean filtering & multiple conditions
df[df["mark"] > 90]
df[(df["class"] == "Six") & (df["gender"] == "male")]
df[(df["gender"] == "female") | (df["mark"] >= 90)]

# Using .loc with filters
df.loc[df["gender"] == "male", ["name", "class"]]
```
### 2️⃣ Working with DataFrames — `PandasDataFrames_01 Sujay.ipynb`

**Topics:**

- Building DataFrames from Python objects
- Indexing & setting custom indexes
- DataFrame attributes: `shape`, `size`, `dtypes`
- Creating calculated columns
- Exporting to CSV/Excel

**Examples from the notebook:**
```python
import pandas as pd

# Creating a DataFrame from a list of dictionaries
basket = [
    {"item": "mango", "quantity": 4, "price": 2.99},
    {"item": "bread", "quantity": 2, "price": 3.25},
    {"item": "juice", "quantity": 1, "price": 5.90},
    {"item": "orange", "quantity": 3, "price": 2.99},
    {"item": "lime", "quantity": 3, "price": 0.3},
]
df = pd.DataFrame(basket)

# Basic metadata
df.shape      # (rows, columns)
df.size       # rows * columns
df.dtypes     # data types

# Calculated column & custom index
df["subtotal"] = df["quantity"] * df["price"]
df.set_index("item", inplace=True)

# Add a 'shape' column and aggregate numeric columns
df["shape"] = ["round", "loaf", "jug", "round", "round"]
df.mean()
df.median()
df.std()

# Exporting results
df.to_csv("items.csv")
df.to_excel("items.xlsx")
```
### 3️⃣ Filtering, Booleans & Copying — `PandasDataFrames_02 Sujay.ipynb`

**Topics:**

- Reading CSVs (`mpg.csv`)
- Column access & inspection
- Complex Boolean masks with `&` and `|`
- Value counts & category distribution
- Understanding `.copy()` vs direct assignment
- Creating new derived metrics

**Examples from the notebook:**
```python
import pandas as pd
from google.colab import files

uploaded = files.upload()

# Read the MPG dataset
mpg = pd.read_csv("mpg.csv")
mpg.head()

# Frequent operations & filtering
mpg.cyl.value_counts()
mpg[(mpg["class"] == "midsize") & (mpg["displ"] < 2)]
mpg[(mpg.model == "maxima") | (mpg.cyl == 5)]

# Demonstrating assignment vs .copy()
original_df = pd.DataFrame({"x": [1, 2, 3]})
new_df = original_df           # same object reference
original_df["y"] = original_df.x * 100

new_df = original_df.copy()    # independent copy
original_df["z"] = 5000

# Weighted fuel_economy metric
city_weight = 0.55
hway_weight = 0.45

mpg["fuel_economy"] = (mpg["cty"] * city_weight) + (mpg["hwy"] * hway_weight)

median = mpg.fuel_economy.median()
efficient_cars = mpg.loc[mpg["fuel_economy"] > median]
```
### 4️⃣ Handling Missing Data — `03_Pandas_DataFrames_Continued_01_Sujay.ipynb`

**Topics:**

- Creating DataFrames with missing values
- Using `.isna()`, `.sum()`, `.mean()` to profile missingness
- Dropping columns/rows with limited value
- Analysing missingness in a real dataset (`penguins.csv`)

**Examples from the notebook:**
```python
import pandas as pd

# Example data with missing values
df = pd.DataFrame([
    {
        "item": "crackers",
        "serving_size": "4 crackers",
        "calories": 10,
        "fat": "1.1g",
        "sodium": "125mg",
        "price": 2.99,
        "discount": None
    },
    {
        "item": "club soda",
        "serving_size": "240ml",
        "calories": 0,
        "fat": "0g",
        "sodium": "75mg",
        "price": 1.49,
        "discount": 0.15
    },
    # ...
])

# Missing-value profiling
df.isna()                # Boolean mask
df.isna().sum()          # Count of nulls by column
df.isna().mean()         # Proportion of nulls by column

df.isna().sum(axis=1)    # Nulls by row
df.isna().mean(axis=1)   # Proportion of nulls by row

# Dropping non-informative pieces
df.drop(columns="discount", inplace=True)
df = df[df.index != "spam"]

# Real dataset: penguins
penguins = pd.read_csv("penguins.csv")

penguins.isna().sum()

percent_missing_by_row = penguins.isna().mean(axis=1)
percent_missing_by_row.sort_values(ascending=False)
```
### 5️⃣ End-to-End Practice on GDP Data — `Pandas Practice Notebook Sujay.ipynb`

Dataset: `GDP (nominal) per Capita.csv`
**Topics:**

- Reading and inspecting economic data
- GroupBy + aggregation by UN region
- Ranking with `nlargest` / `nsmallest`
- Imputing missing values using other columns
- Histogram and barplot visualisation

**Examples from the notebook:**
```python
import pandas as pd
from google.colab import files

uploaded = files.upload()

# Read GDP dataset
df = pd.read_csv("GDP (nominal) per Capita.csv",
                 encoding="unicode_escape",
                 index_col=0)

df.head()
df.info()

# Group by UN_Region
df.groupby("UN_Region")["IMF_Estimate"].median().sort_values()
df.groupby("UN_Region")["WorldBank_Estimate"].agg(["min", "max", "mean", "median"])

# Top/bottom countries by GDP per capita
df.sort_values(by="IMF_Estimate", ascending=False).head(5)
df.nlargest(5, "WorldBank_Estimate")
df.nsmallest(5, "UN_Estimate")

import numpy as np

# Treat zeros as missing, then fill IMF_Estimate using the average of WorldBank & UN
df["IMF_Estimate"] = df["IMF_Estimate"].replace(0, np.nan)
df["avg_worldbank_un"] = df[["WorldBank_Estimate", "UN_Estimate"]].mean(axis=1)
df["IMF_Estimate"] = df["IMF_Estimate"].fillna(df["avg_worldbank_un"])

import matplotlib.pyplot as plt
import seaborn as sns

# Quick histograms
df[["IMF_Estimate", "UN_Estimate", "WorldBank_Estimate"]].hist(figsize=(12, 9))
plt.show()

# Regional bar plots
sns.barplot(x="UN_Region", y="WorldBank_Estimate", data=df, errorbar=None)
plt.show()
```
