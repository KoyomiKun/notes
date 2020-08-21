# Machine Learning 

## 1. Regression

### 1.1 One-variable Linear Regression

$h_\theta(x) = \theta_0 + \theta_1x$ 

***Cost Function*** :

$J(\theta_0, \theta_1) = \frac1{2m}\sum_{i=1}^{m}(y^i-h_\theta(x^i)))^2$ 

![cost function](/home/komikun/Pictures/screenshoots/2020-08-08_20-10-02_screenshot.png) 

***Correlation Coefficients*** :

$r_{XY}=\frac{\sum(X_i-\overline X)(Y_i-\overline Y)}{\sqrt{\sum{(X_i-\overline X)^2\sum(Y_i-\overline Y)^2}}}$ 

`correlation coefficients ` represents the *linear correlation* of two args `X` and `Y`. 

***Decision Coefficients*** :

`SST` :$\sum_{i=1}^n(y_i-\overline y)^2$ 

`SSR` :$\sum_{i=1}^n(\hat y -\overline y)^2$ 

`SSE` :$\sum_{i=1}^n(y _i -\hat y)^2$ 

$R^2=\frac {SSR}{SST}= 1-\frac{SSE}{SST}$ 

***Gradient Descent*** :

repeat until convergence:

$\theta_j := \theta_j - \alpha\frac{\partial}{\partial\theta_j}J(\theta_0, \theta_1)$ , for j=0 or 1

> $\alpha$ : learning rate

In [1]: <!--{"msg_id":1,"type":"code"}-->
```python
import numpy as np
import matplotlib.pyplot as plt
%matplotlib
```
```
Using matplotlib backend: Qt5Agg
```

In [5]: <!--{"msg_id":5,"type":"code"}-->
```python
data = np.genfromtxt('data/data.csv', delimiter=',')
data[:5]
```
Out [5]:
```
array([[32.50234527, 31.70700585],
       [53.42680403, 68.77759598],
       [61.53035803, 62.5623823 ],
       [47.47563963, 71.54663223],
       [59.81320787, 87.23092513]])
```

In [6]: <!--{"msg_id":6,"type":"code"}-->
```python
x_data = data[:, 0]
y_data = data[:, 1]
plt.scatter(x_data, y_data)
plt.show()
```

![p1](/home/komikun/Pictures/screenshoots/2020-08-08_21-09-15_screenshot.png) 

In [7]: <!--{"msg_id":7,"type":"code"}-->
```python
# learning rate
lr = 0.0001

b, k = 0, 0
# max recusive time
epochs = 50

def cal_error(b, k, x_data, y_data):
	total_error = 0
	for i in range(len(x_data)):
		total_error += (y_data[i] - (k * x_data[i] * b)) ** 2
	return total_error / float(len(x_data))

```

In [8]: <!--{"msg_id":8,"type":"code"}-->
```python
def gradient_descent_runner(x_data, y_data, b, k, lr, epochs):
	m = float(len(x_data))
	for _ in range(epochs):
		b_grad = 0
		k_grad = 0

		for j in range(len(x_data)):
			b_grad += -(1/m) * (y_data[j] - (k * x_data[j] + b))
			k_grad += -(1/m) * x_data[j] * (y_data[j] - (k * x_data[j] + b)) 
		b -= lr * b_grad
		k -= lr * k_grad
	return b, k

```

In [13]: <!--{"msg_id":13,"type":"code"}-->
```python
print('Starting k = {0}, b = {1}, error = {2}'.format(b, k, cal_error(b, k, x_data, y_data)))
print('Running...')
b, k = gradient_descent_runner(x_data, y_data, b, k, lr, epochs)
print('After {3} iterations, k = {0}, b = {1}, error = {2}'.format(k, b, cal_error(k, b, x_data, y_data), epochs))
```
```
Starting k = 0.033573578860511974, b = 1.4788322269278682, error = 5205.295161763159
Running...
After 50 iterations, k = 1.4788027177535437, b = 0.03507495926131317, error = 5189.498897495567
```
```
Starting k = 0.03507495926131317, b = 1.4788027177535437, error = 5189.498897495567
Running...
After 50 iterations, k = 1.4787732141468959, b = 0.0365760563874669, error = 5173.730772141054
```

In [14]: <!--{"msg_id":14,"type":"code"}-->
```python
plt.plot(x_data, y_data, 'b.')
plt.plot(x_data, k * x_data + b, 'r.')
plt.show()
```

![p2](/home/komikun/Pictures/screenshoots/2020-08-08_21-36-34_screenshot.png) 

1.8 Elastic Net


$J(\theta) = \frac1{2m}[\sum_{i=1}^m(h_\theta(x^i)- y^i)^2 + \lambda\sum_{j=1}^n|\theta_j|^q]$ 

1. q = 1, Lasso
2. q = 2, Ridge Regression

Now, Consider the following Loss function:


$J(\theta) = \frac1{2m}[\sum_{i=1}^m(h_\theta(x^i)- y^i)^2 + \lambda\sum_{j=1}^n(\alpha\theta_j^2+(1-\alpha)|\theta_j|)]$ 

it called `elastic net` 
