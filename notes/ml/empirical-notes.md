# Practical notes

## Notes on hyperparameters

Batch sizes are kinda hard to understand. Using too small batch sizes leads to instability, but large batch sizes sometimes also causes loss to not decrease... for some reason.

## Common mistakes

### Using `np.mean()` when should be using dot product instead

E.g. COMP 790 HW4. If using `np.mean()` among multiple samples the values will not be multiplied with their labels correctly. (Imagine samples `(-1, 1)` with labels `(-1, 1)`. Averaging over the samples will completely erase the correlation between samples and labels)

### Dimension mismatch!!!

Make sure to distinguish dimension requirements e.g. `m x 1` matrix vs `m` vector. These need to be carefully managed when using functions such as `tf.reduce_sum()` through the use of `keepdims` flag. If not careful these can cause headaches such as unexpected broadcasting **in all sorts of places!**

Examples

- hw3 Naive Bayes prediction function.
- DL hw2 loss function (`y_hat` is `32x1` while `y` is `32`)

### Not using np/tf data structures

Because of numpy's operator overloading, many operations look just like Python syntax e.g. use `my_np_arr[np.where(...),:]` to select certain elements. However this only works when `my_np_arr` is actually a numpy array instead of a common python list.

### Not accumulating loss in stochastic gradient descent (SGD)

There could be several causes to loss not decreasing in SGD. One common one is not accumulating the loss over all the batches in an epoch and instead using only the last batch. Because training data is not evenly distributed, using the last batch does not guarantee a decrease in loss over the epochs.