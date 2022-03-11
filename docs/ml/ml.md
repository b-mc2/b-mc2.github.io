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
