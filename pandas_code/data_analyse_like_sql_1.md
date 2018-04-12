

```python
# 【Python实战】Pandas：让你像写SQL一样做数据分析（一）
# https://www.cnblogs.com/en-heng/p/5630849.html
```


```python
from pandas import Series, DataFrame
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
df = DataFrame({
    'total_bill': [16.99, 10.34, 23.68, 23.68, 24.59],
    'tip': [1.01, 1.66, 3.50, 3.31, 3.61],
    'sex': ['Female', 'Male', 'Male', 'Male', 'Female']
})
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.dtypes
```




    sex            object
    tip           float64
    total_bill    float64
    dtype: object




```python
df.index
```




    RangeIndex(start=0, stop=5, step=1)




```python
df.columns
```




    Index([u'sex', u'tip', u'total_bill'], dtype='object')




```python
df.values
```




    array([['Female', 1.01, 16.99],
           ['Male', 1.66, 10.34],
           ['Male', 3.5, 23.68],
           ['Male', 3.31, 23.68],
           ['Female', 3.61, 24.59]], dtype=object)




```python
df.shape
```




    (5, 3)




```python
# select
```


```python
df.loc[1:3, ['total_bill', 'tip']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
    </tr>
    <tr>
      <th>2</th>
      <td>23.68</td>
      <td>3.50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
    </tr>
  </tbody>
</table>
</div>




```python
# loc，基于列label，可选取特定行（根据行index）
df.loc[1:3, 'total_bill']
```




    1    10.34
    2    23.68
    3    23.68
    Name: total_bill, dtype: float64




```python
# iloc，基于行/列的position
df.iloc[1:3, [1, 2]]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.iloc[1:3, 1:3]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
# at，根据指定行index及列label，快速定位DataFrame的元素
df.at[3, 'tip']
```




    3.31




```python
# iat，与at类似，不同的是根据position来定位的
df.iat[3, 1]
```




    3.31




```python
# ix，为loc与iloc的混合体，既支持label也支持position: dprecated
df.ix[1:3, [1, 2]]
```

    c:\python27\lib\site-packages\ipykernel_launcher.py:1: DeprecationWarning: 
    .ix is deprecated. Please use
    .loc for label based indexing or
    .iloc for positional indexing
    
    See the documentation here:
    http://pandas.pydata.org/pandas-docs/stable/indexing.html#ix-indexer-is-deprecated
      """Entry point for launching an IPython kernel.





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.ix[1:3, ['total_bill', 'tip']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
    </tr>
    <tr>
      <th>2</th>
      <td>23.68</td>
      <td>3.50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[1:3]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[['total_bill', 'tip']]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.99</td>
      <td>1.01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
    </tr>
    <tr>
      <th>2</th>
      <td>23.68</td>
      <td>3.50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.59</td>
      <td>3.61</td>
    </tr>
  </tbody>
</table>
</div>




```python
# where
df[df['sex'] == 'Female']
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df['total_bill'] > 20]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.query('total_bill > 20')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 在where子句中常常会搭配and, or, in, not关键词，Pandas中也有对应的实现
```


```python
# and
df[(df['sex'] == 'Female') & (df['total_bill'] > 20)]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# or
df[(df['sex'] == 'Female') | (df['total_bill'] > 20)]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# in
df[df['total_bill'].isin([21.01, 23.68, 24.39])]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
# not
df[-(df['sex'] == 'Make')]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[-df['total_bill'].isin([21.01, 23.68, 24.59])]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
total = df.loc[df['tip'] == 1.66, 'total_bill']
```


```python
total
```




    1    10.34
    Name: total_bill, dtype: float64




```python
total.values[0]
```




    10.34




```python
total = df.get_value(df.loc[df['tip'] == 1.66].index.values[0], 'total_bill')
```

    c:\python27\lib\site-packages\ipykernel_launcher.py:1: FutureWarning: get_value is deprecated and will be removed in a future release. Please use .at[] or .iat[] accessors instead
      """Entry point for launching an IPython kernel.



```python
total
```




    10.34




```python
# distinct
```


```python
df.drop_duplicates(subset=['sex'], keep='first', inplace=True)
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
# group
```


```python
df.groupby('sex').size()
```




    sex
    Female    1
    Male      1
    dtype: int64




```python
df.groupby('sex').count()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby('sex')['tip'].count()
```




    sex
    Female    1
    Male      1
    Name: tip, dtype: int64




```python
df = DataFrame({
    'total_bill': [16.99, 10.34, 23.68, 23.68, 24.59],
    'tip': [1.01, 1.66, 3.50, 3.31, 3.61],
    'sex': ['Female', 'Male', 'Male', 'Male', 'Female']
})
```


```python
df.groupby('sex').agg({
    'tip': np.max, 'total_bill': np.sum
})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
    </tr>
    <tr>
      <th>sex</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>41.58</td>
      <td>3.61</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>57.70</td>
      <td>3.50</td>
    </tr>
  </tbody>
</table>
</div>




```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
      <th>tip</th>
      <th>total_bill</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>3.50</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = DataFrame({
    'total_bill': [16.99, 10.34, 23.68, 23.68, 24.59],
    'tip': [1.01, 1.66, 1.66, 3.31, 3.61],
    'sex': ['Female', 'Male', 'Male', 'Male', 'Female']
})
```


```python
df.groupby('tip').agg({
    'sex': Series.nunique
})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sex</th>
    </tr>
    <tr>
      <th>tip</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1.01</th>
      <td>1</td>
    </tr>
    <tr>
      <th>1.66</th>
      <td>1</td>
    </tr>
    <tr>
      <th>3.31</th>
      <td>1</td>
    </tr>
    <tr>
      <th>3.61</th>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# as
```


```python
df.columns = ['total', 'pit', 'xes']
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total</th>
      <th>pit</th>
      <th>xes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>1.66</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.rename(columns={'total': 'total_bill', 'pit': 'tip', 'xes': 'sex'}, inplace=True)
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>1.66</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# join
# df.join
# df.merge
```


```python
# order
```


```python
df.sort_values(['total_bill', 'tip'], ascending=[False, True])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Male</td>
      <td>1.66</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>1.01</td>
      <td>16.99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# top
```


```python
df.nlargest(3, columns=['tip'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>Female</td>
      <td>3.61</td>
      <td>24.59</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Male</td>
      <td>3.31</td>
      <td>23.68</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>1.66</td>
      <td>10.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
"""
# 1.
df.assign(rn=df.sort_values(['total_bill'], ascending=False)
          .groupby('sex')
          .cumcount()+1)\
    .query('rn < 3')\
    .sort_values(['sex', 'rn'])
    
# 2.
df.assign(rn=df.groupby('sex')['total_bill']
          .rank(method='first', ascending=False)) \
    .query('rn < 3') \
    .sort_values(['sex', 'rn'])
"""
```




    "\n# 1.\ndf.assign(rn=df.sort_values(['total_bill'], ascending=False)\n          .groupby('sex')\n          .cumcount()+1)    .query('rn < 3')    .sort_values(['sex', 'rn'])\n    \n# 2.\ndf.assign(rn=df.groupby('sex')['total_bill']\n          .rank(method='first', ascending=False))     .query('rn < 3')     .sort_values(['sex', 'rn'])\n"




```python
# replace
```
