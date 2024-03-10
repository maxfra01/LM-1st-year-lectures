# Numpy

We need to import **numpy** in every module.

```python
import numpy as np
```

### Array: create, modify

Main objects in numpy are **multidimensional arrays** `ndarray`: we'll use 1-dimension and 2-dimension arrays (NOT `matrix`).
We can create arrays by using numpy methods, or build arrays from lists and tuples:

```python
a = numpy.array([1,2,3])

b= numpy.array([[1,2,3], [4,5,6]], dtype=numpy.float64) #add the type

c= numpy.array((2,5,3))
```

To retrive information of arrays:

```python
ndarray.size #total number of elements
ndarray.shape #array's shape in format (n,m)
ndarray.dim #number of axis
ndarray.dtype #type of elements
```

Numpy allows to create particular arrays using special functions:

```python
>>> numpy.zeros((2, 3), dtype=numpy.float32) #return a zero matrix

>>> numpy.ones(5) #return a matrix full of ones

>>> numpy.arange(0, 4, 0.5) #same logic as range, it return 1-dimension array

>>> numpy.eye(3) #identity matrix

>>> numpy.linspace(0, 5, 4) #4 evenly spaced items in 0-5 range
```

Gli operatori **aritmetici** operano **elemento per elemento** (no moltiplicazioni fra matrici): per modificare un array cerchiamo sempre di usare operatori **in place** (+=, \*=)
Per una **moltiplicazione fra matrici** uso la funzione `dot`:

```python
>>> x = numpy.array([[1,2], [3,4], [5,6]])
>>> y = numpy.array([[1,2,3], [4,5,6]])
>>> z = numpy.dot(x, y)
```

Da notare che le dimensioni degli array devono essere coerenti.

Possiamo modificare la **forma di un array**:

```python
>>> x = numpy.arange(12).reshape((2,3)) #ritorna una matrice 2x3 preservando l'ordine degli elementi

>>> y = x.ravel() #disponde tutti gli elementi in un vettore monodimensionale
```

```ad-note
title: Row vectors
Row vectors (shape (1, n)) are NOT 1-dimensional arrays (shape(n,) )
```


