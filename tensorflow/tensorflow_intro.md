

```python
import numpy as np
import tensorflow as tf
```


```python
# 使用 NumPy 生成假数据(phony data), 总共 100 个点.
x_data = np.float32(np.random.rand(2, 100))
```


```python
y_data = np.dot([0.100, 0.200], x_data) + 0.300
```


```python
# 构造一个线性模型
b = tf.Variable(tf.zeros([1]))
```


```python
b
```




    <tf.Variable 'Variable:0' shape=(1,) dtype=float32_ref>




```python
w = tf.Variable(tf.random_uniform([1, 2], -1.0, 1.0))
```


```python
w
```




    <tf.Variable 'Variable_1:0' shape=(1, 2) dtype=float32_ref>




```python
y = tf.matmul(w, x_data) + b
```


```python
# 最小化方差
loss = tf.reduce_mean(tf.square(y - y_data))
```


```python
optimizer = tf.train.GradientDescentOptimizer(0.5)
```


```python
train = optimizer.minimize(loss)
```


```python
# 初始化变量
init = tf.global_variables_initializer()
```


```python
# 启动图 (graph)
sess = tf.Session()
```


```python
sess.run(init)
```


```python
# 拟合平面
for step in range(0, 201):
    sess.run(train)
    if step % 10 == 0:
        print(step, sess.run(w), sess.run(b))
```

    0 [[-0.49365962  0.70615953]] [0.70794547]
    10 [[-0.22166364  0.38485175]] [0.37053967]
    20 [[-0.04454216  0.28939435]] [0.3283294]
    30 [[0.03514266 0.24355792]] [0.3109489]
    40 [[0.07093791 0.22137588]] [0.30395576]
    50 [[0.08699904 0.21056563]] [0.3012565]
    60 [[0.09419563 0.20525955]] [0.3002835]
    70 [[0.0974149  0.20263642]] [0.2999759]
    80 [[0.09885208 0.20133038]] [0.29990762]
    90 [[0.09949211 0.2006756 ]] [0.29991463]
    100 [[0.0997763  0.20034514]] [0.29993808]
    110 [[0.09990202 0.2001773 ]] [0.2999595]
    120 [[0.09995739 0.20009154]] [0.299975]
    130 [[0.09998165 0.20004748]] [0.2999851]
    140 [[0.09999219 0.20002472]] [0.29999137]
    150 [[0.09999675 0.20001292]] [0.29999506]
    160 [[0.09999867 0.20000675]] [0.2999972]
    170 [[0.09999947 0.20000356]] [0.29999846]
    180 [[0.09999981 0.20000188]] [0.29999915]
    190 [[0.09999993 0.20000099]] [0.29999954]
    200 [[0.09999999 0.20000052]] [0.29999974]

