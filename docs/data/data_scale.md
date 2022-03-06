# Data Scaling

Small-Medium Data - 0mb - 100mb

Large Data - 100mb - 1TB

Big Data - 1TB+



---
## Tools

### Pandas
[Documentation](https://pandas.pydata.org/pandas-docs/stable/user_guide/scale.html)

1. Read CSV file data in chunk size:
The parameter essentially means the number of rows to be read into a dataframe at any single time in order to fit into the local memory.
```python
import pandas as pd
# read the large csv file with specified chunksize 
df_chunk = pd.read_csv(r'../input/data.csv', chunksize=1_000_000)
```
The operation above resulted in a TextFileReader object for iteration. Strictly speaking, df_chunk is not a dataframe but an object for further operation in the next step.
```python
chunk_list = []  # append each chunk df here 

# Each chunk is in df format
for chunk in df_chunk:  
    # perform data filtering 
    chunk_filter = chunk_preprocessing(chunk)
    
    # Once the data filtering is done, append the chunk to list
    chunk_list.append(chunk_filter)
    
# concat the list into dataframe 
df_concat = pd.concat(chunk_list)
```

2. Filter out unimportant columns to save memory
```python
# Filter out unimportant columns
df = df[['col_1','col_2', 'col_3', 'col_4', 'col_5', 'col_6','col_7', 'col_8', 'col_9', 'col_10']]
```
3. Change dtypes for columns

The simplest way to convert a pandas column of data to a different type is to use `astype()`.
```python
# Change the dtypes (int64 -> int32)
df[['col_1','col_2', 
    'col_3', 'col_4', 'col_5']] = df[['col_1','col_2', 
                                      'col_3', 'col_4', 'col_5']].astype('int32')

# Change the dtypes (float64 -> float32)
df[['col_6', 'col_7',
    'col_8', 'col_9', 'col_10']] = df[['col_6', 'col_7',
                                       'col_8', 'col_9', 'col_10']].astype('float32')
```

### Modin
[Documentation](https://modin.readthedocs.io/en/latest/)

Modin is a DataFrame for datasets from 1MB to 1TB+

Modin uses Ray or Dask to provide an effortless way to speed up your pandas notebooks, scripts, and libraries. Pandas 
traditionally loads data into a single CPU core, Modin will spread that dataset over multiple cores,
this makes it easy to scale and also to build out multiple clusters.

To use Modin, you do not need to know how many cores your system has and you do not need to specify how to distribute the data. In fact, you can continue using your previous pandas notebooks while experiencing a considerable speedup from Modin, even on a single machine. Once you’ve changed your import statement, you’re ready to use Modin just like you would pandas.

If you don’t have Ray or Dask installed, you will need to install Modin with one of the targets:
```shell
pip install "modin[ray]" # Install Modin dependencies and Ray to run on Ray
pip install "modin[dask]" # Install Modin dependencies and Dask to run on Dask
pip install "modin[all]" # Install all of the above
```
If you want to choose a specific compute engine to run on, you can set the environment variable MODIN_ENGINE and Modin will do computation with that engine:
```shell
export MODIN_ENGINE=ray  # Modin will use Ray
export MODIN_ENGINE=dask  # Modin will use Dask
```
In pandas, you are only able to use one core at a time when you are doing computation of any kind. With Modin, you are able to use all of the CPU cores on your machine. Even in `read_csv`, we see large gains by efficiently distributing the work across your entire machine.
```python
import modin.pandas as pd

df = pd.read_csv("my_dataset.csv")
# 31.4 Seconds with Pandas
# 7.73 Seconds with Modin
```


---
### Dask
[Documentation](https://dask.org/)

Dask is composed of two parts:
- Dynamic task scheduling optimized for computation. This is similar to Airflow, Luigi, Celery, or Make, but optimized for interactive computational workloads.
- “Big Data” collections like parallel arrays, dataframes, and lists that extend common interfaces like NumPy, Pandas, or Python iterators to larger-than-memory or distributed environments. These parallel collections run on top of dynamic task schedulers.

```shell
python -m pip install dask                # Install only core parts of dask

python -m pip install "dask[complete]"    # Install everything
python -m pip install "dask[array]"       # Install requirements for dask array
python -m pip install "dask[dataframe]"   # Install requirements for dask dataframe
python -m pip install "dask[diagnostics]" # Install requirements for dask diagnostics
python -m pip install "dask[distributed]" # Install requirements for distributed dask
```

---

### Pola.rs

[Documentation](https://pola-rs.github.io/polars-book/user-guide/index.html)

Polars is written in Rust but has Python Pandas API

Polars is a lightning fast DataFrame library/in-memory query engine. Its embarrassingly parallel execution, cache efficient algorithms and expressive API makes it perfect for efficient data wrangling, data pipelines, snappy APIs and so much more.



```python
import polars as pl

q = (
    pl.scan_csv("iris.csv")
    .filter(pl.col("sepal_length") > 5)
    .groupby("species")
    .agg(pl.all().sum())
)

df = q.collect()
```
