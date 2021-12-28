Array Basic Attributes
=======================

Basic attributes of the ndarray class

.. image:: ./images/axis/att.png
    :align: center


Index
------

用于获取 array 中的单个元素，array 中的每个元素都有一个地址，这个地址就是一个 “index”

**index values start at zero**

.. image:: ./images/axis/index.jpeg
    :align: center

Slicing 2-D arrays
~~~~~~~~~~~~~~~~~~

.. code-block:: python

    np.random.seed(72)
    squre_array = np.random.randint(low=0,high=100,size=25).reshape([5,5])
    square_array[1:3,1:4]

.. image:: ./images/axis/2_index.png
    :align: center

Dimensions
-----------

"Dimension of an Array" is the number of indices, or subscripts, that you need in order to specify an individual element of the array.

简而言之，维度就是获取array中单个元素所需的下标数。


Zero Dimensional data
~~~~~~~~~~~~~~~~~~~~~~

Scaler, has no dimensions or axis.

::

    4


One Dimensional data
~~~~~~~~~~~~~~~~~~~~~

A Vector is one-dimensional data. Vector is a collection of Scalars.
Vector has a shape(N,), where N is the number of scalars in it.

::

    [1, 2, 3]


Two Dimensional data
~~~~~~~~~~~~~~~~~~~~~

A Matrix is a example of two-dimensional data. Matrix is a collection of
vectors and has a shape of (N,M), where N is the number of vectors in it
and M is the numbers of scalers in each vector.

::

    [[1,2,3],
     [4,5,6]]

Three Dimensional data
~~~~~~~~~~~~~~~~~~~~~~

3D array is a collection of 2D data-points(matrix). The shape of 3D
data would be (N,M,P). There would be matrices of shape(M,P).

::

    [[[1,1,1],
      [3,3,3]],

     [[2,2,2],
      [4,4,4]]]

Generalizing the Concept
~~~~~~~~~~~~~~~~~~~~~~~~

A data with ``n`` dimension would have be having the following shape.

::

    (N1, N2, N3, N4, ...... Nn)

* There are ``N1`` data-points with shape ``(N2, N3, N4, .. Nn)``
* Each data-point will have ``N2`` data-points of shape ``(N3, N4, .. Nn)``
* Similary, it goes on

Shape
------

The “shape” of array is a tuple with the number of elements per dimension.

我们可以通过 shape 来获取每个维度的元素数, 以下是一个2D array。

.. code-block:: python

    import numpy as np
    data = np.array([[1,2,3],[4,5,6])
    print(data.shape)

::

    (2, 3)

Axis
-----

In Numpy, dimensions are called axes.For example,the 2-D array has 2 axis.

**Array axes in Numpy are numbered, and starting with 0.**

类似笛卡尔坐标系一样，Numpy axes 也可以用坐标系的方法来表示，对于一个
2-dimensional Numpy array 来说，axis就是沿着 Row 和 Column 的方向。

.. image:: ./images/axis/coordinate.png
    :align: center

* Axis 0 is the direction along the rows
* Axis 1 is the direction along the columns

.. image:: ./images/axis/coord.jpg
    :align: center

Examples
~~~~~~~~

**我们需要理解axis对于不同函数的作用是不同的**

Numpy Sum
^^^^^^^^^^

沿着axis的方向聚合数据，注意Rows/Columns 和 Rows/Columns的方向是两个概念。

In np.sum( ), the axis parameter controls which axis will be aggregated or collapsed.
Remember, functions like sum( ), mean( ), min( ), median( ), and other statistical functions aggregate your data.

.. code-block:: python

    import numpy as np
    np_array_2d = np.arange(0, 6).reshape([2,3])
    print(np_array_ad)

::

    array([[0, 1, 2],
           [3, 4, 5]])

**sum axis=0**

.. code-block:: python

    np.sum(np_array_2d, axis=0)

output:

::

    array([3, 5, 7])

.. image:: ./images/axis/axis0.png
    :align: center

**sum axis=1**

.. code-block:: python

    np.sum(np_array_2d, axis=1)

output:

::

    array([3, 12])

.. image:: ./images/axis/axis1.png
    :align: center

**Numpy Aggregation Array Function**

.. image:: ./images/axis/agg.png
    :align: center

Numpy Concatenate
^^^^^^^^^^^^^^^^^^

When we use the axis parameter with the np.concatenate( ) function,
the axis defines the axis along which we stack the arrays.

.. code-block:: python

    np_array_1s = np.array([[1,1,1],[1,1,1]])
    np_array_9s = np.array([[9,9,9],[9,9,9]])

output:

::

    array([[1, 1, 1],
           [1, 1, 1]])

    array([[9, 9, 9],
           [9, 9, 9]])

**concatenate axis=0**

.. code-block:: python

    np.concatenate([np_array_1s, np_array_9s], axis = 0)

output:

::

    array([[1, 1, 1],
           [1, 1, 1],
           [9, 9, 9],
           [9, 9, 9]])

.. image:: ./images/axis/concat0.png
    :align: center

**concatenate axis=1**

.. code-block:: python

    np.concatenate([np_array_1s, np_array_9s], axis = 1)

output:

::

    array([1, 1, 1, 9, 9, 9],
          [1, 1, 1, 9, 9, 9])

.. image:: ./images/axis/concat1.png
    :align: center

Warning:1-Dimensional arrays
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1-Dimensional Numpy arrays only have one axis.

.. image:: ./images/axis/dimension1.png
    :align: center

.. code-block:: python

    np_array_1s_1dim = np.array([1,1,1])
    np_array_9s_1dim = np.array([9,9,9])

output:

::

    [1 1 1]
    [9 9 9]

.. code-block:: python

    np.concatenate([np_array_1s_1dim, np_array_9s_1dim], axis = 0)

output:

::

    array([1, 1, 1, 9, 9, 9])


参考文档
---------

| `Understanding Axes and Dimensions <https://towardsdatascience.com/understanding-axes-and-dimensions-numpy-pandas-606407a5f950>`_
| `NUMPY AXES EXPLAINED <https://www.sharpsightlabs.com/blog/numpy-axes-explained/>`_
