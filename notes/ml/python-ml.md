# Using Python for ML

## Frequently used syntaxes

### numpy
#### `numpy.where()`
```python
np.where(condition[, x, y])

# Example: calculating prediction accuracy
accu = np.mean(np.where(np.equal(Y, pred_func(X)), 1.0, 0.0))
```
Returns elements chosen from `x` if `condition` is true in the corresponding position, or `y` if it is false. The three arrays need to be broadcastable to each other.

If only `condition` is provided, indices of its element that evaluates to true will be returned. It is equivalent to `np.asarray(condition).nonzero()`. *Important: it will return one array for each dimension of the condition matrix.* So it might be required to use `np.where(condition)[0]` if the array has only one dimension.

#### `ndarray.T`
Shortcut for transposing an ndarray

#### `numpy.random`
Random number generator.
```python
# returns a random int between 0 and n
np.random.randint(n)
np.random.randint(n, size=(d0, d1, ...,dn))

# returns a random float between 0 and 1.0
np.random.rand()
np.random.rand(d0, d1, ..., dn)
np.random.random_sample(size=(d0, d1, ...,dn))

# returns a random number from standard Normal distribution
np.random.randn()
np.random.randn(d0, d1, ...,dn)
np.random.standard_normal(size=(d0, d1, ...,dn))
```
Random generators

#### `numpy.linspace()`
Returns evenly spaced numbers in array. Default is left and right inclusive.

```python
np.linspace(1, 3, 3)
# array([1., 2., 3.])

np.linspace(
    start, stop, num=50, endpoint=True, 
    retstep=False, dtype=None, axis=0
)
```
#### `numpy.meshgrid()`
Returns coordinate matrices from coordiante vectors.
```python
x = np.linspace(0, 1, 3)
y = np.linspace(0, 1, 2)
xv, yv = np.meshgrid(x, y)
print(xv)
# array([[0. , 0.5, 1. ],
#        [0. , 0.5, 1. ]])
print(yv)
# array([[0.,  0.,  0.],
#        [1.,  1.,  1.]])
```

#### `numpy.reshape()`
Gives a new shape to an array without changing its data.
```python
a = np.arange(6).reshape((3, 2))
print(a)
# array([[0, 1],
#        [2, 3],
#        [4, 5]])

# One dimension can be -1, which means it will be inferred
# from other dimensions that are provided.
a = np.arange(6).reshape((-1, 2))
print(a)
# array([[0, 1],
#        [2, 3],
#        [4, 5]])
```

#### `numpy.linalg.solve()`
Solves linear matrix equation.

```python
# Solves for x in ax = b.
np.linalg.solve(a, b)
```

#### `numpy.unravel_index()`
Converts a flat index or array of flat indices into a tuple of coordinate arrays.

```python
np.unravel_index([22, 41, 37], (7,6))
# (array([3, 6, 6]), array([4, 5, 1]))
```

### tensorflow

#### `tf.gather()`

Gather slices from params axis axis according to indices.

```python
params = tf.constant([[0,    1.0,  2.0],
                      [10.0, 11.0, 12.0],
                      [20.0, 21.0, 22.0],
                      [30.0, 31.0, 32.0]])

tf.gather(params, indices=[3,1]).numpy()
# result: [[30., 31., 32.],
#           10., 11., 12.]]

tf.gather(params, indices=[2,1], axis=1).numpy()
# result: [[2.,   1.],
#          [12., 11.],
#          [22., 21.],
#          [32., 31.]]
```

Can use repetitive indices to apply class specific operations. E.g. for Gausian Mixture Models (GMM)
```python
z = tf.transpose(tf.random.categorical([logits], N))
x = tf.gather(means, z) + y * tf.gather(np.exp(lsigmas), z)
```
where `z` represents the latent variable, `logits[i]`, `means[i]` and `lsigmas[i]` represents the logit, mean and log-sigma of the i-th Gaussian respectively.

## Common mistakes

### Dimension mismatch!!!

Make sure to distinguish dimension requirements e.g. `m x 1` matrix vs `m` vector. These need to be carefully managed when using functions such as `tf.reduce_sum()` through the use of `keepdims` flag. If not careful these can cause headaches such as unexpected broadcasting **in all sorts of places!**

Examples
- hw3 Naive Bayes prediction function.
- DL hw2 loss function (`y_hat` is `32x1` while `y` is `32`)

### Not using np/tf data structures

Because of numpy's operator overloading, many operations look just like Python syntax e.g. use `my_np_arr[np.where(...),:]` to select certain elements. However this only works when `my_np_arr` is actually a numpy array instead of a common python list.

### Not accumulating loss in stochastic gradient descent (SGD)

There could be several causes to loss not decreasing in SGD. One common one is not accumulating the loss over all the batches in an epoch and instead using only the last batch. Because training data is not evenly distributed, using the last batch does not guarantee a decrease in loss over the epochs.