

Keras 官方



https://blog.csdn.net/qq_36926037/article/details/107977792

1. tensor【类似于numpy，但只是用于GPU和TPU的；求导，分布式】

2. tf.constant【特殊的tensor，用于保存常量】tf.ones  tf.zeros

3. tf.Variable【保存变化的状态】.assign(value)`, `.assign_add(increment)`, or `.assign_sub(decrement)

   通过以上方式进行进行更新变化量。

4. 求导，一阶导数。

```
with tf.GradientTape() as tape:
    tape.watch(a)  # Start recording the history of operations applied to `a`
    c = tf.sqrt(tf.square(a) + tf.square(b))  # Do some math using `a`
    # What's the gradient of `c` with respect to `a`?
    dc_da = tape.gradient(c, a)  
    print(dc_da)
```

$$
\frac{dc}{da} 求导
$$

5. 高阶导，比如二阶导数。

```
with tf.GradientTape() as outer_tape:
    with tf.GradientTape() as tape:
        c = tf.sqrt(tf.square(a) + tf.square(b))
        dc_da = tape.gradient(c, a)
    d2c_da2 = outer_tape.gradient(dc_da, a)
    print(d2c_da2)
```



keras layer

微分编程：用于张量，变量，导数。

```python
class Linear(keras.layers.Layer):
    """y = w.x + b"""

    def __init__(self, units=32, input_dim=32):
        super(Linear, self).__init__()
        w_init = tf.random_normal_initializer()  #随机初始化
        self.w = tf.Variable(
            initial_value=w_init(shape=(input_dim, units), dtype="float32"),
            trainable=True,
        ) # 变量
        b_init = tf.zeros_initializer() #偏置初始化
        self.b = tf.Variable(
            initial_value=b_init(shape=(units,), dtype="float32"), trainable=True
        )

    def call(self, inputs):
        return tf.matmul(inputs, self.w) + self.b
    
    
# Instantiate our layer.
linear_layer = Linear(units=4, input_dim=2)

# The layer can be treated as a function.
# Here we call it on some data.
y = linear_layer(tf.ones((2, 2)))
assert y.shape == (2, 4)
```

The weight variables (created in `__init__`) are automatically tracked under the `weights` property:

```Python
assert linear_layer.weights == [linear_layer.w, linear_layer.b]
```

这个weights变量自动回生成。



添加权重的而另一种方式。

```python
class Linear(keras.layers.Layer):
    """y = w.x + b"""

    def __init__(self, units=32):
        super(Linear, self).__init__()
        self.units = units  # 定义的输出

    def build(self, input_shape):
        self.w = self.add_weight(
            shape=(input_shape[-1], self.units),  # 实例化传入的是输入。
            initializer="random_normal",
            trainable=True,
        )
        self.b = self.add_weight(
            shape=(self.units,), initializer="random_normal", trainable=True
        )

    def call(self, inputs):
        return tf.matmul(inputs, self.w) + self.b


# Instantiate our lazy layer.
linear_layer = Linear(4) # 输出的是4

# This will also call `build(input_shape)` and create the weights.
# 应该会自动调用build，这是在
y = linear_layer(tf.ones((2, 2)))  # 输入的是2，因为build函数取的是input[-1]
```



Layer 能够得到一层的梯度权重通过调GradientTape。

```python
# Prepare a dataset.
(x_train, y_train), _ = tf.keras.datasets.mnist.load_data()
dataset = tf.data.Dataset.from_tensor_slices(
    (x_train.reshape(60000, 784).astype("float32") / 255, y_train)
)
dataset = dataset.shuffle(buffer_size=1024).batch(64)

# Instantiate our linear layer (defined above) with 10 units.
linear_layer = Linear(10)

# Instantiate a logistic loss function that expects integer targets.
loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)

# Instantiate an optimizer.
optimizer = tf.keras.optimizers.SGD(learning_rate=1e-3)

# Iterate over the batches of the dataset.
for step, (x, y) in enumerate(dataset):

    # Open a GradientTape.
    with tf.GradientTape() as tape:

        # Forward pass.
        logits = linear_layer(x) # 预测值

        # Loss value for this batch.
        loss = loss_fn(y, logits)  #真实值与预测值进行对比

    # Get gradients of the loss wrt the weights.
    gradients = tape.gradient(loss, linear_layer.trainable_weights) #将loss对训练参数求导

    # Update the weights of our linear layer.
    optimizer.apply_gradients(zip(gradients, linear_layer.trainable_weights)) # 更新梯度

    # Logging
    if step % 100 == 0:
        print("Step:", step, "Loss:", float(loss))
```





Trainable and non-trainable weights。权重分为可训练和不可训练的。

```Python
class ComputeSum(keras.layers.Layer):
    """Returns the sum of the inputs."""

    def __init__(self, input_dim):
        super(ComputeSum, self).__init__()
        # Create a non-trainable weight.
        self.total = tf.Variable(initial_value=tf.zeros((input_dim,)), trainable=False)
		# 在定义的时刻，trainable=False就可以实现
        
    def call(self, inputs):
        self.total.assign_add(tf.reduce_sum(inputs, axis=0))
        return self.total


my_sum = ComputeSum(2)
x = tf.ones((2, 2))

y = my_sum(x)
print(y.numpy())  # [2. 2.]

y = my_sum(x)
print(y.numpy())  # [4. 4.]

# 实例化.weights,.non_trainable_weights,.trainable_weights  都可以
assert my_sum.weights == [my_sum.total]
assert my_sum.non_trainable_weights == [my_sum.total]
assert my_sum.trainable_weights == []
```



循环嵌套layer变成更大的网络

计算weights的参数的数量：

https://zhuanlan.zhihu.com/p/108767040

```python

class MLP(keras.layers.Layer):
    """Simple stack of Linear layers."""

    def __init__(self):
        super(MLP, self).__init__()
        self.linear_1 = Linear(32)
        self.linear_2 = Linear(32)
        self.linear_3 = Linear(10)

    def call(self, inputs):
        x = self.linear_1(inputs)
        x = tf.nn.relu(x)
        x = self.linear_2(x)
        x = tf.nn.relu(x)
        return self.linear_3(x)


mlp = MLP()

# The first call to the `mlp` object will create the weights.
y = mlp(tf.ones(shape=(3, 64)))

# Weights are recursively tracked.
assert len(mlp.weights) == 6

for i in mlp.weights:
  print(i.shape)

"""
(64, 32) 权重是64*32，输出是32
(32,)	输出偏置项是32
(32, 32)权重是32*32，输出是32
(32,)	输出偏置项是32
(32, 10)权重是32*10，输出是10
(10,)	输出偏置项是10
"""
激活函数和Dropout不算参数。

```



简化写法

```python
mlp = keras.Sequential(
    [
        keras.layers.Dense(32, activation=tf.nn.relu),
        keras.layers.Dense(32, activation=tf.nn.relu),
        keras.layers.Dense(10),
    ]
)
```



```python
# 定义层
class ActivityRegularization(keras.layers.Layer):
    """Layer that creates an activity sparsity regularization loss."""

    def __init__(self, rate=1e-2):
        super(ActivityRegularization, self).__init__()
        self.rate = rate

    def call(self, inputs):
        # We use `add_loss` to create a regularization loss
        # that depends on the inputs.
        self.add_loss(self.rate * tf.reduce_sum(inputs))
        return inputs


# Let's use the loss layer in a MLP block.
# 定义模型
class SparseMLP(keras.layers.Layer):
    """Stack of Linear layers with a sparsity regularization loss."""

    def __init__(self):
        super(SparseMLP, self).__init__()
        self.linear_1 = Linear(32)
        self.regularization = ActivityRegularization(1e-2)
        self.linear_3 = Linear(10)

    def call(self, inputs):
        x = self.linear_1(inputs)
        x = tf.nn.relu(x)
        x = self.regularization(x)
        return self.linear_3(x)


mlp = SparseMLP()
y = mlp(tf.ones((10, 10)))

print(mlp.losses)  # List containing one float32 scalar


# 应用
# Losses correspond to the *last* forward pass.
mlp = SparseMLP()
mlp(tf.ones((10, 10)))
assert len(mlp.losses) == 1
mlp(tf.ones((10, 10)))
assert len(mlp.losses) == 1  # No accumulation.

# Let's demonstrate how to use these losses in a training loop.

# Prepare a dataset.
(x_train, y_train), _ = tf.keras.datasets.mnist.load_data()
dataset = tf.data.Dataset.from_tensor_slices(
    (x_train.reshape(60000, 784).astype("float32") / 255, y_train)
)
dataset = dataset.shuffle(buffer_size=1024).batch(64)

# A new MLP.
mlp = SparseMLP()

# Loss and optimizer.
loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
optimizer = tf.keras.optimizers.SGD(learning_rate=1e-3)

for step, (x, y) in enumerate(dataset):
    with tf.GradientTape() as tape:

        # Forward pass.
        logits = mlp(x)
        
		# 预测值与真实值的损失
        # External loss value for this batch.
        loss = loss_fn(y, logits)
        
        # 前向传播的损失
        # Add the losses created during the forward pass.
        loss += sum(mlp.losses)

        # Get gradients of the loss wrt the weights.
        gradients = tape.gradient(loss, mlp.trainable_weights)

    # Update the weights of our linear layer.
    optimizer.apply_gradients(zip(gradients, mlp.trainable_weights))

    # Logging.
    if step % 100 == 0:
        print("Step:", step, "Loss:", float(loss))
```

可以选择记录训练过程中的多种指标，acc，loss等



Compiled functions  编译函数。编译会将计算到静态图上，运算加快。通过装饰器@tf.function修饰即可。

```python
# Prepare our layer, loss, and optimizer.
model = keras.Sequential(
    [
        keras.layers.Dense(32, activation="relu"),
        keras.layers.Dense(32, activation="relu"),
        keras.layers.Dense(10),
    ]
)
loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
optimizer = tf.keras.optimizers.Adam(learning_rate=1e-3)

# Create a training step function.


@tf.function  # Make it fast.
def train_on_batch(x, y):
    with tf.GradientTape() as tape:
        logits = model(x)
        loss = loss_fn(y, logits)
        gradients = tape.gradient(loss, model.trainable_weights)
    optimizer.apply_gradients(zip(gradients, model.trainable_weights))
    return loss


# Prepare a dataset.
(x_train, y_train), _ = tf.keras.datasets.mnist.load_data()
dataset = tf.data.Dataset.from_tensor_slices(
    (x_train.reshape(60000, 784).astype("float32") / 255, y_train)
)
dataset = dataset.shuffle(buffer_size=1024).batch(64)

for step, (x, y) in enumerate(dataset):
    
    # 直接调用该过程即可
    loss = train_on_batch(x, y)  
    
    if step % 100 == 0:
        print("Step:", step, "Loss:", float(loss))			
```

---

For  engineering

主要了解的是DL的特性用在实际的产品上。

- 准备用于训练模型的数据，转换成numpy arrays, 或者tf.data.Dataset
- 数据预处理，**标准化**。
- 使用keras 函数式API来创建模型
- 直接用内置函数**fit**进行训练，注意checkpoint，metrics monitoring， fault tolerance
- 评估模型再测试数据集上的表现
- 利用多GPU进行加速
- 超参调节模型



##### 数据加载和预处理

神经网络不处理原始数据，如文本文件、编码的 JPEG 图像文件或 CSV 文件。 他们处理矢量化和标准化的表示。

- 文本文件需要读入字符串张量，然后拆分成单词。 最后，需要对单词进行索引并转换为整数张量。
- 图像需要被读取并解码为整数张量，然后转换为浮点数并归一化为小值（通常在 0 和 1 之间）。
- CSV 数据需要解析，数值特征转换为浮点张量，分类特征索引并转换为整数张量。 然后每个特征通常需要归一化为零均值和单位方差。



DataLoading

- Numpy 数组。在内存中

- **[TensorFlow `Dataset` objects](https://www.tensorflow.org/guide/data)**。高级操作，适合从硬盘或者分布式文件系统。
- Python的生成器。不断返回一个batch的data









