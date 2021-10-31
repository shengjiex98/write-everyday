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

If only `condition` is provided, indices of its element that evaluates to true will be returned. It is equivalent to `np.asarray(condition).nonzero()`.

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

### tensorflow
