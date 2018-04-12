

```python
%matplotlib inline
```


```python
from pandas import Series, DataFrame
import pandas as pd
import numpy as np
```


```python
obj = Series([4, 7, -5, 3])
```


```python
obj
```




    0    4
    1    7
    2   -5
    3    3
    dtype: int64




```python
obj.dtype
```




    dtype('int64')




```python
obj.values
```




    array([ 4,  7, -5,  3])




```python
obj.index
```




    RangeIndex(start=0, stop=4, step=1)




```python
obj2 = Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
```


```python
obj2
```




    d    4
    b    7
    a   -5
    c    3
    dtype: int64




```python
obj2['a']
```




    -5




```python
obj2.index
```




    Index([u'd', u'b', u'a', u'c'], dtype='object')




```python
obj2[['c', 'a', 'd']]
```




    c    3
    a   -5
    d    4
    dtype: int64




```python
obj2['d'] = 6
```


```python
obj2[['c', 'a', 'd']]
```




    c    3
    a   -5
    d    6
    dtype: int64




```python
obj2[obj2 > 0]
```




    d    6
    b    7
    c    3
    dtype: int64




```python
obj2 * 2
```




    d    12
    b    14
    a   -10
    c     6
    dtype: int64




```python
# exp，高等数学里以自然常数e为底的指数函数, e是一个常数为2.71828
np.exp(obj2)
```




    d     403.428793
    b    1096.633158
    a       0.006738
    c      20.085537
    dtype: float64




```python
obj2
```




    d    6
    b    7
    a   -5
    c    3
    dtype: int64




```python
'b' in obj2
```




    True




```python
'e' in obj2
```




    False




```python
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
```


```python
obj3 = Series(sdata)
```


```python
obj3
```




    Ohoi      35000
    Oregon    16000
    Texas     71000
    Utah       5000
    dtype: int64




```python
states = ['California', 'Ohio', 'Oregon', 'Texas']
```


```python
obj4 = Series(sdata, index=states)
```


```python
obj4
```




    California        NaN
    Ohio          35000.0
    Oregon        16000.0
    Texas         71000.0
    dtype: float64




```python
pd.isnull(obj4)
```




    California     True
    Ohio          False
    Oregon        False
    Texas         False
    dtype: bool




```python
pd.notnull(obj4)
```




    California    False
    Ohio           True
    Oregon         True
    Texas          True
    dtype: bool




```python
obj4.isnull()
```




    California     True
    Ohio          False
    Oregon        False
    Texas         False
    dtype: bool




```python
obj4.notnull()
```




    California    False
    Ohio           True
    Oregon         True
    Texas          True
    dtype: bool




```python
obj3 + obj4
```




    California         NaN
    Ohio               NaN
    Ohoi               NaN
    Oregon         32000.0
    Texas         142000.0
    Utah               NaN
    dtype: float64




```python
obj3
```




    Ohoi      35000
    Oregon    16000
    Texas     71000
    Utah       5000
    dtype: int64




```python
obj4
```




    California        NaN
    Ohio          35000.0
    Oregon        16000.0
    Texas         71000.0
    dtype: float64




```python
obj4.name
```


```python
obj4.name = 'population'
```


```python
obj4.name
```




    'population'




```python
obj4.index.name
```


```python
obj4.index.name = 'state'
```


```python
obj4
```




    state
    California        NaN
    Ohio          35000.0
    Oregon        16000.0
    Texas         71000.0
    Name: population, dtype: float64




```python
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
```


```python
obj
```




    Bob      4
    Steve    7
    Jeff    -5
    Ryan     3
    dtype: int64


