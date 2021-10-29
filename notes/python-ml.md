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

### tensorflow
