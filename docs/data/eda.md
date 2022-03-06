# EDA Exploratory Data Analysis

Exploratory Data Analysis (EDA) is used on the one hand to answer questions, test business assumptions, generate hypotheses for further analysis. On the other hand, you can also use it to prepare the data for modeling. The thing that these two probably have in common is a good knowledge of your data to either get the answers that you need or to develop an intuition for interpreting the results of future modeling.

There are a lot of ways to reach these goals: you can get a basic description of the data, visualize it, identify patterns in it, identify challenges of using the data, etc.

---
## Data Exploration Tools
### Pandas Profiling 
[Documentation](https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/index.html)

Generates profile reports from a pandas `DataFrame`. The pandas `df.describe()` function is great but a little basic for serious exploratory data analysis. `pandas_profiling` extends the pandas DataFrame with `df.profile_report()` for quick data analysis.

For each column the following statistics - if relevant for the column type - are presented in an interactive HTML report:

- **Type inference**: detect the types of columns in a dataframe.
- **Essentials**: type, unique values, missing values
- **Quantile statistics**: like minimum value, Q1, median, Q3, maximum, range, interquartile range
- **Descriptive statistics**: like mean, mode, standard deviation, sum, median absolute deviation, coefficient of variation, kurtosis, skewness
- **Most frequent values**:
- **Histograms**:
- **Correlations**: highlighting of highly correlated variables, Spearman, Pearson and Kendall matrices
- **Missing values**: matrix, count, heatmap and dendrogram of missing values
- **Duplicate rows**: Lists the most occurring duplicate rows
- **Text analysis**: learn about categories (Uppercase, Space), scripts (Latin, Cyrillic) and blocks (ASCII) of text data

#### Basic Usage
```python
import numpy as np
import pandas as pd
from pandas_profiling import ProfileReport

# build dataframe
df = pd.DataFrame(np.random.rand(100, 5), columns=["a", "b", "c", "d", "e"])

# generate a report
profile = ProfileReport(df, title="Pandas Profiling Report", explorative=True)

# Save report
profile.to_file("your_report.html")

# As a string
json_data = profile.to_json()

# As a file
profile.to_file("your_report.json")
```
![Pandas Profiling](https://pandas-profiling.github.io/pandas-profiling/docs/master/rtd/_images/iframe.gif)

#### Working with Larger datasets
Pandas Profiling by default comprehensively summarizes the input dataset in a way that gives the most insights for data analysis. For small datasets these computations can be performed in real-time. For larger datasets, we have to decide upfront which calculations to make. Whether a computation scales to big data not only depends on it’s complexity, but also on fast implementations that are available.

`pandas-profiling` includes a minimal configuration file, with more expensive computations turned off by default. This is a great starting point for larger datasets.
```python
# minimal flag to True
# Sample 10.000 rows
sample = large_dataset.sample(10000)

profile = ProfileReport(sample, minimal=True)
profile.to_file("output.html")
```

#### Privacy Features
When dealing with sensitive data, such as private health records, sharing a report that includes a sample would violate patient’s privacy. The following shorthand groups together various options so that only aggregate information is provided in the report.

```python
report = df.profile_report(sensitive=True)
```

---

### SweetViz
[Documentation](https://github.com/fbdesignpro/sweetviz)

Sweetviz is an open-source python auto-visualization library that generates a report, exploring the data with the help of high-density plots. It not only automates the EDA but is also used for comparing datasets and drawing inferences from it. A comparison of two datasets can be done by treating one as training and the other as testing.

The Sweetviz library generates a report having:
- An overview of the dataset
- Variable properties
- Categorical associations
- Numerical associations
- Most frequent, smallest, largest values for numerical features

[See Example Titanic Dataset](http://cooltiming.com/SWEETVIZ_REPORT.html)

```python
import sweetviz
import pandas as pd
train = pd.read_csv("train.csv")
test = pd.read_csv("test.csv")

my_report = sweetviz.compare([train, "Train"], [test, "Test"], "Survived")
```
Running this command will perform the analysis and create the report object. To get the output, simply use the `show_html()` command:
```python
# Not providing a filename will default to SWEETVIZ_REPORT.html
my_report.show_html("Report.html")
```

![SweetViz](https://miro.medium.com/max/700/1*7sw4PM1hXkt0QgWBJbGrkg.png)

[More Details Here](https://towardsdatascience.com/powerful-eda-exploratory-data-analysis-in-just-two-lines-of-code-using-sweetviz-6c943d32f34)

---

### Dtale
[Documentation](https://github.com/man-group/dtale)

D-Tale is the combination of a Flask back-end and a React front-end to bring you an easy way to view & analyze Pandas data structures. It integrates seamlessly with ipython notebooks & python/ipython terminals. Currently this tool supports such Pandas objects as DataFrame, Series, MultiIndex, DatetimeIndex & RangeIndex.

To start without any data right away you can run `dtale.show()` and open the browser to input a `CSV` or `TSV` file.

![Dtale](https://raw.githubusercontent.com/aschonfeld/dtale-media/master/images/no_data.png)

```python
import dtale
import pandas as pd

df = pd.DataFrame([dict(a=1,b=2,c=3)])
# Assigning a reference to a running D-Tale process
d = dtale.show(df)
```

![Dtale](https://raw.githubusercontent.com/aschonfeld/dtale-media/master/gifs/dtale_demo_mini.gif)

---

## Data Exploration Manual

### Python Pandas

```python
import pandas as pd
df = pd.read_csv("path/to/file.csv")
df.describe()
```

More to come...

