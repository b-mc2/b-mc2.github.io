# Machine Learning



[Definition](https://www.investopedia.com/terms/b/business-intelligence-bi.asp)

---
## Anomoly Detection

### Isolation Forests 
[Documentation](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html?highlight=isolation#sklearn.ensemble.IsolationForest)

Recall that decision trees are built using information criteria such as Gini index or entropy. The obviously different groups are separated at the root of the tree and deeper into the branches, the subtler distinctions are identified. 

Based on randomly picked characteristics, an isolation forest processes the randomly subsampled data in a tree structure. Samples that reach further into the tree and require more cuts to separate them have a very little probability that they are anomalies. Likewise, samples that are found on the shorter branches of the tree are more likely to be anomalies, since the tree found it simpler to distinguish them from the other data.

```python
from sklearn.datasets import make_blobs
from numpy import quantile, random, where
from sklearn.ensemble import IsolationForest
import matplotlib.pyplot as plt
```

```python
IF = IsolationForest(n_estimators=100, contamination=.03)
predictions = IF.fit_predict(X)

outlier_index = where(predictions==-1)
values = X[outlier_index]
 
plt.scatter(X[:,0], X[:,1])
plt.scatter(values[:,0], values[:,1], color='y')
plt.show()
```

![](https://machinelearningmastery.com/wp-content/uploads/2021/12/anomaly-2.png)

[More Info](https://machinelearningmastery.com/anomaly-detection-with-isolation-forest-and-kernel-density-estimation/)

---

### Sqliteviz 
Sqliteviz is a single-page offline-first PWA for fully client-side visualisation of SQLite databases or CSV files.

With sqliteviz you can:
- run SQL queries against a SQLite database and create Plotly charts and pivot tables based on the result sets
- import a CSV file into a SQLite database and visualize imported data
- export result set to CSV file
- manage inquiries and run them against different databases
- import/export inquiries from/to a JSON file
- export a modified SQLite database
- use it offline from your OS application menu like any other desktop app

[Documentation](https://github.com/rawgraphs/rawgraphs-app#readme)

Examples

![](https://github.com/lana-k/sqliteviz/blob/master/src/assets/images/Screenshot_editor.png?raw=true)

[Use the Live App](https://lana-k.github.io/sqliteviz/#/)

--- 

### Streamlit 
Streamlit is an open-source Python library that makes it easy to create and share beautiful, custom web apps for machine learning and data science. In just a few minutes you can build and deploy powerful data apps.

[Documentation](https://docs.streamlit.io/)

Examples

![](https://streamlit.io/images/uploads/gallery/apps/nyc-uber-ridesharing-data.png)

```python
import streamlit as st
import pandas as pd
import numpy as np

st.title('Uber pickups in NYC')
DATE_COLUMN = 'date/time'
DATA_URL = ('https://s3-us-west-2.amazonaws.com/'
         'streamlit-demo-data/uber-raw-data-sep14.csv.gz')

def load_data(nrows):
    data = pd.read_csv(DATA_URL, nrows=nrows)
    data[DATE_COLUMN] = pd.to_datetime(data[DATE_COLUMN])
    return data

data = load_data(10000)
```
Create a bar chart
```python
hist_values = np.histogram(
    data[DATE_COLUMN].dt.hour, bins=24, range=(0,24)
)[0]

st.bar_chart(hist_values)
```
Plot data on a map with sliding filter
```python
hour_to_filter = st.slider('hour', 0, 23, 17)  # min: 0h, max: 23h, default: 17h
filtered_data = data[data[DATE_COLUMN].dt.hour == hour_to_filter]

st.subheader(f'Map of all pickups at {hour_to_filter}:00')

st.map(filtered_data)
```

[Use the Live App](https://lana-k.github.io/sqliteviz/#/)

---

### LightDash 
Connect Lightdash to your dbt project, add metrics directly in your data transformation layer, then create and share your insights with your team.


[Documentation](https://docs.lightdash.com/)

Examples

![](https://raw.githubusercontent.com/lightdash/lightdash/main/static/screenshots/lightdashpreview.gif)

[Use the Live Demo App](https://demo.lightdash.com/login)


---

### Apache Superset 
Superset is fast, lightweight, intuitive, and loaded with options that make it easy for users of all skill sets to explore and visualize their data, from simple line charts to highly detailed geospatial charts.

[Documentation](https://superset.apache.org/docs/intro)

Examples

![](https://superset.apache.org/static/7fbdf913f3d6910fca780e9e5d0e0e7a/01620/worldbank_dashboard.png)

![](https://superset.apache.org/static/0b7531df457162c5d804a3b7e6517014/c8bc7/sqllab.png)


