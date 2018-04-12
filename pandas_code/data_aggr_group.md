
# 数据聚合和分组运算
对数据集进行分组并对各组应用一个函数（无论是聚合还是转换），这是数据分析工作中的重要环节。在将数据集准备好之后，通常的任务就是计算分组统计或生成透视表。pandas提供了一个灵活高效的groupby功能，它使你能以一种自然的方式对数据集进行切片，切块，摘要等操作。

本章主要聚焦在以下几点：
* 根据一个或多个键（可以是函数，数组或DataFrame列名）拆分pandas对象
* 计算分组摘要统计，如计数，平均值，标准差，或用户自定义函数
* 对DataFrame的列应用各种各样的函数
* 应用组内转换或其他运算，如规格化，线性回归，排名或选取子集等
* 计算透视表或交叉表
* 执行分位数分析以及其他分组分析


```python
from pandas import Series, DataFrame
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
```

## Groupby技术
pandas对象中的数据会根据你所提供的一个或多个键拆分（split）为多组。然后将一个函数应用（apply）到各个分组并产生一个新值。最后所有这些函数的执行结果会被合并（combine）到最后的结果对象中。


```python
df = DataFrame({
    'key1': ['a', 'a', 'b', 'b', 'a'],
    'key2': ['one', 'two', 'one', 'two', 'one'],
    'data1': np.random.randn(5),
    'data2': np.random.randn(5)
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
      <th>data1</th>
      <th>data2</th>
      <th>key1</th>
      <th>key2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.075519</td>
      <td>0.787727</td>
      <td>a</td>
      <td>one</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1.385993</td>
      <td>0.024777</td>
      <td>a</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.988892</td>
      <td>1.741039</td>
      <td>b</td>
      <td>one</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.855603</td>
      <td>-0.679512</td>
      <td>b</td>
      <td>two</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.224985</td>
      <td>-0.153181</td>
      <td>a</td>
      <td>one</td>
    </tr>
  </tbody>
</table>
</div>



按照key1进行分组，并对data1列计算平均值：


```python
grouped = df['data1'].groupby(df['key1'])
```


```python
grouped.mean()
```




    key1
    a   -0.028496
    b    0.922248
    Name: data1, dtype: float64




```python
group2 = df['data1'].groupby([df['key1'], df['key2']])
```


```python
means = group2.mean()
```

这里，我们通过两个键对数据进行了分组，得到的Series具有一个层次化索引（由唯一的键对组成）：


```python
means
```




    key1  key2
    a     one     0.650252
          two    -1.385993
    b     one     0.988892
          two     0.855603
    Name: data1, dtype: float64




```python
means.unstack()
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
      <th>key2</th>
      <th>one</th>
      <th>two</th>
    </tr>
    <tr>
      <th>key1</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0.650252</td>
      <td>-1.385993</td>
    </tr>
    <tr>
      <th>b</th>
      <td>0.988892</td>
      <td>0.855603</td>
    </tr>
  </tbody>
</table>
</div>



在上面的例子中，分组键均为Series。实际上分组键可以是任何长度适当的数组：


```python
states = np.array(['Ohio', 'California', 'California', 'Ohio', 'Ohio'])
years = np.array([2005, 2005, 2006, 2005, 2006])
df['data1'].groupby([states, years]).mean()
```




    California  2005   -1.385993
                2006    0.988892
    Ohio        2005    0.965561
                2006    0.224985
    Name: data1, dtype: float64



此外，还可以将列名（可以是字符串，数字或其他Python对象）用作分组键：


```python
df.groupby('key1').mean()
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
      <th>data1</th>
      <th>data2</th>
    </tr>
    <tr>
      <th>key1</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>-0.028496</td>
      <td>0.219774</td>
    </tr>
    <tr>
      <th>b</th>
      <td>0.922248</td>
      <td>0.530764</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['data3'] = np.random.randn(5)
```

你可能已经注意到了，在执行df.groupby('key1').mean()时，结果中并没有key2列。这是因为df['key2']不是数值数据（俗称“麻烦列”），所以从结果中排除了。


```python
df.groupby('key1').mean()
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
      <th>data1</th>
      <th>data2</th>
      <th>data3</th>
    </tr>
    <tr>
      <th>key1</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>-0.028496</td>
      <td>0.219774</td>
      <td>-0.588593</td>
    </tr>
    <tr>
      <th>b</th>
      <td>0.922248</td>
      <td>0.530764</td>
      <td>-1.182884</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby(['key1', 'key2']).mean()
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
      <th></th>
      <th>data1</th>
      <th>data2</th>
      <th>data3</th>
    </tr>
    <tr>
      <th>key1</th>
      <th>key2</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">a</th>
      <th>one</th>
      <td>0.650252</td>
      <td>0.317273</td>
      <td>-0.611045</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.385993</td>
      <td>0.024777</td>
      <td>-0.543688</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">b</th>
      <th>one</th>
      <td>0.988892</td>
      <td>1.741039</td>
      <td>-0.722945</td>
    </tr>
    <tr>
      <th>two</th>
      <td>0.855603</td>
      <td>-0.679512</td>
      <td>-1.642822</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby(['key1', 'key2']).size()
```




    key1  key2
    a     one     2
          two     1
    b     one     1
          two     1
    dtype: int64



### 对分组进行迭代
GroupBy对象支持迭代，可以产生一组二元元祖（由分组名和数据块组成）：


```python
for name, group in df.groupby('key1'):
    print(name)
    print(group)
```

    a
          data1     data2 key1 key2     data3
    0  1.075519  0.787727    a  one -0.979380
    1 -1.385993  0.024777    a  two -0.543688
    4  0.224985 -0.153181    a  one -0.242711
    b
          data1     data2 key1 key2     data3
    2  0.988892  1.741039    b  one -0.722945
    3  0.855603 -0.679512    b  two -1.642822


对于多重键的情况，元祖的第一个元素将会是由键值组成的元祖：


```python
for (k1, k2), group in df.groupby(['key1', 'key2']):
    print(k1)
    print(k2)
    print(group)
```

    a
    one
          data1     data2 key1 key2     data3
    0  1.075519  0.787727    a  one -0.979380
    4  0.224985 -0.153181    a  one -0.242711
    a
    two
          data1     data2 key1 key2     data3
    1 -1.385993  0.024777    a  two -0.543688
    b
    one
          data1     data2 key1 key2     data3
    2  0.988892  1.741039    b  one -0.722945
    b
    two
          data1     data2 key1 key2     data3
    3  0.855603 -0.679512    b  two -1.642822


当然，你可以对这些数据片段做任何操作。有一个你可能会觉得有用的运算：将这些数据片段做成一个字段：


```python
pieces = dict(list(df.groupby('key1')))
```


```python
type(pieces['a'])
```




    pandas.core.frame.DataFrame




```python
pieces['a']
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
      <th>data1</th>
      <th>data2</th>
      <th>key1</th>
      <th>key2</th>
      <th>data3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.075519</td>
      <td>0.787727</td>
      <td>a</td>
      <td>one</td>
      <td>-0.979380</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1.385993</td>
      <td>0.024777</td>
      <td>a</td>
      <td>two</td>
      <td>-0.543688</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.224985</td>
      <td>-0.153181</td>
      <td>a</td>
      <td>one</td>
      <td>-0.242711</td>
    </tr>
  </tbody>
</table>
</div>



groupby默认是在axis=0上进行分组的，通过设置也可以在其他任何轴上进行分组。拿上面例子说，我们可以根据dtype对列进行分组：


```python
df.dtypes
```




    data1    float64
    data2    float64
    key1      object
    key2      object
    data3    float64
    dtype: object




```python
grouped = df.groupby(df.dtypes, axis=1)
```


```python
for tu, group in grouped:
    print(tu)
    print(group)
```

    float64
          data1     data2     data3
    0  1.075519  0.787727 -0.979380
    1 -1.385993  0.024777 -0.543688
    2  0.988892  1.741039 -0.722945
    3  0.855603 -0.679512 -1.642822
    4  0.224985 -0.153181 -0.242711
    object
      key1 key2
    0    a  one
    1    a  two
    2    b  one
    3    b  two
    4    a  one


### 选取一个或一组列
对于由DataFrame产生的GroupBy对象，如果用一个（单个字符串）或一组（字符串数组）列名对其进行索引，就能实现选取部分列进行聚合的目的：


```python
# df['data1'].groupby(df['key1'])的语法糖
df.groupby('key1')['data1']
```




    <pandas.core.groupby.SeriesGroupBy object at 0x000000000F47CE80>




```python
# df[['data2']].groupby(df['key1'])的语法糖
df.groupby('key1')[['data2']]
```




    <pandas.core.groupby.DataFrameGroupBy object at 0x000000000F47CBE0>



尤其对于大数据集，很可能只需要对部分列进行聚合。如果只需要计算data2列的平均值并以DataFrame形式得到结果，我们可以编写：


```python
df.groupby(['key1', 'key2'])[['data2']].mean()
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
      <th></th>
      <th>data2</th>
    </tr>
    <tr>
      <th>key1</th>
      <th>key2</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">a</th>
      <th>one</th>
      <td>0.317273</td>
    </tr>
    <tr>
      <th>two</th>
      <td>0.024777</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">b</th>
      <th>one</th>
      <td>1.741039</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.679512</td>
    </tr>
  </tbody>
</table>
</div>



这种索引操作所返回的对象是一个已分组的DataFrame（如果传入的是列表或数组）或已分组的Series（如果传入的是标量形式的单个列名）:


```python
s_grouped = df.groupby(['key1', 'key2'])['data2']
```


```python
s_grouped
```




    <pandas.core.groupby.SeriesGroupBy object at 0x000000000F48BCC0>




```python
s_grouped.mean()
```




    key1  key2
    a     one     0.317273
          two     0.024777
    b     one     1.741039
          two    -0.679512
    Name: data2, dtype: float64



### 通过字典或Series进行分组
除数组外，分组信息还可以以其他形式存在。


```python
people = DataFrame(
    np.random.randn(5, 5),
    columns=['a', 'b', 'c', 'd', 'e'],
    index=['Joe', 'Steve', 'Wes', 'Jim', 'Travis']
)
```


```python
people.ix[2:3, ['b', 'c']] = np.nan
```


```python
people
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Joe</th>
      <td>-0.190953</td>
      <td>-0.812799</td>
      <td>-1.033481</td>
      <td>0.992870</td>
      <td>0.313165</td>
    </tr>
    <tr>
      <th>Steve</th>
      <td>2.282477</td>
      <td>1.565090</td>
      <td>-0.643722</td>
      <td>-0.886294</td>
      <td>0.576957</td>
    </tr>
    <tr>
      <th>Wes</th>
      <td>0.500482</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.618442</td>
      <td>0.232408</td>
    </tr>
    <tr>
      <th>Jim</th>
      <td>-1.394986</td>
      <td>-1.184395</td>
      <td>-0.491359</td>
      <td>0.521514</td>
      <td>0.435635</td>
    </tr>
    <tr>
      <th>Travis</th>
      <td>-0.752434</td>
      <td>-1.011331</td>
      <td>-1.307385</td>
      <td>0.254042</td>
      <td>-0.126686</td>
    </tr>
  </tbody>
</table>
</div>



假设已知列的分组关系，并希望根据分组计算列的总计：


```python
mapping = {
    'a': 'red', 'b': 'red', 'c': 'blue',
    'd': 'blue', 'e': 'red', 'f': 'orange'
}
```


```python
by_column = people.groupby(mapping, axis=1)
```


```python
for tu, group in by_column:
    print(tu)
    print(group)
```

    blue
                   c         d
    Joe    -1.033481  0.992870
    Steve  -0.643722 -0.886294
    Wes          NaN  1.618442
    Jim    -0.491359  0.521514
    Travis -1.307385  0.254042
    red
                   a         b         e
    Joe    -0.190953 -0.812799  0.313165
    Steve   2.282477  1.565090  0.576957
    Wes     0.500482       NaN  0.232408
    Jim    -1.394986 -1.184395  0.435635
    Travis -0.752434 -1.011331 -0.126686



```python
by_column.sum()
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
      <th>blue</th>
      <th>red</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Joe</th>
      <td>-0.040611</td>
      <td>-0.690587</td>
    </tr>
    <tr>
      <th>Steve</th>
      <td>-1.530016</td>
      <td>4.424525</td>
    </tr>
    <tr>
      <th>Wes</th>
      <td>1.618442</td>
      <td>0.732890</td>
    </tr>
    <tr>
      <th>Jim</th>
      <td>0.030155</td>
      <td>-2.143746</td>
    </tr>
    <tr>
      <th>Travis</th>
      <td>-1.053343</td>
      <td>-1.890450</td>
    </tr>
  </tbody>
</table>
</div>



Series也有同样的功能，它可以被看做一个固定大小的映射。对于上面那个例子，如果用Series作为分组键，则pandas会检查Series以确保其索引跟分组轴是对其的：


```python
map_series = Series(mapping)
```


```python
map_series
```




    a       red
    b       red
    c      blue
    d      blue
    e       red
    f    orange
    dtype: object




```python
people.groupby(map_series, axis=1).sum()
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
      <th>blue</th>
      <th>red</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Joe</th>
      <td>-0.040611</td>
      <td>-0.690587</td>
    </tr>
    <tr>
      <th>Steve</th>
      <td>-1.530016</td>
      <td>4.424525</td>
    </tr>
    <tr>
      <th>Wes</th>
      <td>1.618442</td>
      <td>0.732890</td>
    </tr>
    <tr>
      <th>Jim</th>
      <td>0.030155</td>
      <td>-2.143746</td>
    </tr>
    <tr>
      <th>Travis</th>
      <td>-1.053343</td>
      <td>-1.890450</td>
    </tr>
  </tbody>
</table>
</div>




```python
people.groupby(map_series, axis=1).count()
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
      <th>blue</th>
      <th>red</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Joe</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Steve</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Wes</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Jim</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Travis</th>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



### 通过函数进行分组
相较于字典或Series，Python函数在定义分组映射关系时可以更有创意且更为抽象。任何被当做分组键的函数都会在各自索引值上被调用一次，其返回值就会被用作分组名称。


```python
for tu, group in people.groupby(len):
    print(tu)
    print(group)
```

    3
                a         b         c         d         e
    Joe -0.190953 -0.812799 -1.033481  0.992870  0.313165
    Wes  0.500482       NaN       NaN  1.618442  0.232408
    Jim -1.394986 -1.184395 -0.491359  0.521514  0.435635
    5
                  a        b         c         d         e
    Steve  2.282477  1.56509 -0.643722 -0.886294  0.576957
    6
                   a         b         c         d         e
    Travis -0.752434 -1.011331 -1.307385  0.254042 -0.126686



```python
people.groupby(len).sum()
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
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>-1.085457</td>
      <td>-1.997194</td>
      <td>-1.524841</td>
      <td>3.132826</td>
      <td>0.981208</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2.282477</td>
      <td>1.565090</td>
      <td>-0.643722</td>
      <td>-0.886294</td>
      <td>0.576957</td>
    </tr>
    <tr>
      <th>6</th>
      <td>-0.752434</td>
      <td>-1.011331</td>
      <td>-1.307385</td>
      <td>0.254042</td>
      <td>-0.126686</td>
    </tr>
  </tbody>
</table>
</div>



将函数跟数组，列表，字典，Series混合使用也不是问题，因为任何东西最终都会转换为数组：


```python
key_list = ['one', 'one', 'one', 'two', 'two']
people.groupby([len, key_list]).min()
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
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">3</th>
      <th>one</th>
      <td>-0.190953</td>
      <td>-0.812799</td>
      <td>-1.033481</td>
      <td>0.992870</td>
      <td>0.232408</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.394986</td>
      <td>-1.184395</td>
      <td>-0.491359</td>
      <td>0.521514</td>
      <td>0.435635</td>
    </tr>
    <tr>
      <th>5</th>
      <th>one</th>
      <td>2.282477</td>
      <td>1.565090</td>
      <td>-0.643722</td>
      <td>-0.886294</td>
      <td>0.576957</td>
    </tr>
    <tr>
      <th>6</th>
      <th>two</th>
      <td>-0.752434</td>
      <td>-1.011331</td>
      <td>-1.307385</td>
      <td>0.254042</td>
      <td>-0.126686</td>
    </tr>
  </tbody>
</table>
</div>




```python
people.groupby([len, key_list]).sum()
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
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">3</th>
      <th>one</th>
      <td>0.309529</td>
      <td>-0.812799</td>
      <td>-1.033481</td>
      <td>2.611312</td>
      <td>0.545573</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.394986</td>
      <td>-1.184395</td>
      <td>-0.491359</td>
      <td>0.521514</td>
      <td>0.435635</td>
    </tr>
    <tr>
      <th>5</th>
      <th>one</th>
      <td>2.282477</td>
      <td>1.565090</td>
      <td>-0.643722</td>
      <td>-0.886294</td>
      <td>0.576957</td>
    </tr>
    <tr>
      <th>6</th>
      <th>two</th>
      <td>-0.752434</td>
      <td>-1.011331</td>
      <td>-1.307385</td>
      <td>0.254042</td>
      <td>-0.126686</td>
    </tr>
  </tbody>
</table>
</div>



### 根据索引级别分组
层次化索引数据集最方便的地方就在于它能够根据索引级别进行聚合。要实现这个目的，通过level关键字传入级别编号或名称即可：


```python
columns = pd.MultiIndex.from_arrays(
    [['US', 'US', 'US', 'JP', 'JP'], [1, 3, 5, 1, 3]],
    names=['cty', 'tenor']
)
```


```python
hier_df = DataFrame(np.random.randn(4, 5), columns=columns)
```


```python
hier_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>cty</th>
      <th colspan="3" halign="left">US</th>
      <th colspan="2" halign="left">JP</th>
    </tr>
    <tr>
      <th>tenor</th>
      <th>1</th>
      <th>3</th>
      <th>5</th>
      <th>1</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.935757</td>
      <td>-0.841348</td>
      <td>-0.302121</td>
      <td>-0.597560</td>
      <td>-0.478696</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.081459</td>
      <td>1.769887</td>
      <td>-0.393501</td>
      <td>-0.243366</td>
      <td>-1.237686</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.093765</td>
      <td>1.543197</td>
      <td>-0.272767</td>
      <td>-0.469978</td>
      <td>-0.258981</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.187279</td>
      <td>0.714735</td>
      <td>-2.038807</td>
      <td>0.669068</td>
      <td>-0.526766</td>
    </tr>
  </tbody>
</table>
</div>




```python
hier_df.groupby(level='cty', axis=1).count()
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
      <th>cty</th>
      <th>JP</th>
      <th>US</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



## 数据聚合
