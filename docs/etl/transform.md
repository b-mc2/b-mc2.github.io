# ETL (Transform)

Extract Transform Load is the process whereby some data is obtained, (extracted) cleaned, wrangled (transformed), and placed into a user-friendly data structure like a data frame (loaded).

Transforming is 

---
## Pandas
### JSON 
[Documentation](https://www.kaggle.com/jboysen/quick-tutorial-flatten-nested-json-in-pandas)

Often JSON files directly translate to `pd.DataFrame` but nested JSON usually leave the JSON array in a single column 

```python
import json 
import pandas as pd 
from pandas.io.json import json_normalize #package for flattening json in pd

df = pd.DataFrame(json_file)
works_data = json_normalize(data = df['programs'],
                            record_path ='works', 
                            meta =['id', 'orchestra', 'programID', 'season'])

# record_path is the column name you want to "flatten"
# by that it makes JSON keys as a new
# column name and places the value as the row value.
# Also pass the parent metadata we wanted to append
```

---

### Get only numeric columns from dataframe
```python
df = pd.read_csv("data.csv")
numeric_columns = df.select_dtypes(include=['number'])

```

---

### JSONL
[Documentation](https://jsonlines.org/examples/)

Pandas can work with `JSONL` as well 

```python
import json 
import pandas as pd 

df = pd.read_json(jsonl_file, lines=True)

# using lines=True lets Pandas read each line as a valid JSON file
```
[Additional Info](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_json.html)

---
[JSONL]: ../data/fileformats.md#JSONL


## Pyspark
### Add new/rename columns
```python
import pyspark.sql.functions as F
import pyspark.sql.types as T
import pyspark.sql.dataframe
import datetime

df = df.withColumn('Todays_Date', F.lit(datetime.datetime.now()))

## Lowercase column names
for col in df.columns:
    df = df.withColumnRenamed(col, col.lower())
```