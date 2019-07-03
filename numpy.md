### Create an array
np.array
np.zeros
np.ones
np.full
np.range
np.linspace
np.random.random
np.random.normal
np.random.randint
np.eye
np.empty

### Attributes
.ndim
.shape
.size
.dtype
.itemsize
.nbytes

### Indexing
x[0], x[-1], x[0,0]

### Slicing
x[:, 1]
x.copy()

### Reshaping
x.reshape((3, 3))
x[np.newaxis, :] == x.reshape((1, 3))
x[:, np.newaxis] == x.reshape((3, 1))

### Concatenation and Splitting
np.concatenate([x, y])
np.concatenate([grid, grid], axis=1)
np.vstack
np.hstack

np.split(x, [3, 5])
np.hsplit
np.vsplit

### Vectorized operations by ufuncs
x + 5
x / 5
-x
x ** 2
x % 2
x // 2
np.abs(x)

np.sin(x)
np.cos(x)
no.tan(x)
np.arcsin(x)
np.arccos(x)
np.arctan(x)

np.exp(x)
np.exp2(x)
np.power(3, x)
np.log(x)
np.log2(x)
np.log10(x)

np.expm1(x)
np.log1p(x)

```python
from scipy import special
special.gammar(x)
special.gammarln(x)
special.beta(x)
```
