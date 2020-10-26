# Struct2Tensor

[![Python](https://img.shields.io/pypi/pyversions/struct2tensor.svg?style=plastic)](https://github.com/google/struct2tensor)
[![PyPI](https://badge.fury.io/py/struct2tensor.svg)](https://badge.fury.io/py/struct2tensor)

<!--*
freshness: { owner: 'zhuo' reviewed: '2020-07-07' }
*-->

## Introduction
struct2tensor is a library for parsing structured data inside of tensorflow.
In particular, it makes it easy to manipulate structured data, e.g., slicing,
flattening, copying substructures, and so on, as part of a TensorFlow model
graph. The notebook in 'examples/prensor_playground.ipynb' provides a few
examples of struct2tensor in action and an introduction to the main
concepts. You can
[run the notebook in your browser](https://colab.research.google.com/github/google/struct2tensor/blob/master/examples/prensor_playground.ipynb)
through Google's colab environment, or [download the
file](examples/prensor_playground.ipynb) to run it in your own Jupyter
environment.


There are two main use cases of this repo:

1.  To create a PIP package. The PIP package contains plug-ins (OpKernels) to an
    existing tensorflow installation.
2.  To staticlly link with tensorflow-serving.

As these processes are independent, one can follow either set of directions
below.

## Use a pre-built Linux PIP package.


From a virtual environment, run:

```bash
pip install struct2tensor
```

## Creating a PIP package.

The struct2tensor PIP package is useful for creating models.
It works with tensorflow 2.x.

In order to unify the process, we recommend compiling struct2tensor inside
a docker container.


### Downloading the Code

Go to your home directory.

Download the source code.

```bash
git clone https://github.com/google/struct2tensor.git
cd ~/struct2tensor
```

### Use docker-compose
Install [docker-compose](https://docs.docker.com/compose/).

Use it to build a pip wheel for Python 3.6 with tensorflow version 2:

```bash
docker-compose build manylinux2010
docker-compose run -e PYTHON_VERSION=36 -e TF_VERSION=RELEASED_TF_2 manylinux2010
```

Or build a pip wheel for Python 3.7 with tensorflow version 2 (note that if you
run one of these docker-compose commands after the other, the second will erase
the result from the first):

```bash
docker-compose build manylinux2010
docker-compose run -e PYTHON_VERSION=37 -e TF_VERSION=RELEASED_TF_2 manylinux2010
```

This will create a manylinux package in the ~/struct2tensor/dist directory.



## Creating a static library.

In order to construct a static library for tensorflow-serving, we run:

```bash
bazel build -c opt struct2tensor:prensor_kernels_and_ops
```

This can also be linked into another library.

## Compatibility

struct2tensor                                                          | tensorflow
---------------------------------------------------------------------- | ----------
[0.22.0](https://github.com/google/struct2tensor/releases/tag/v0.22.0) | 2.2.0
[0.21.1](https://github.com/google/struct2tensor/releases/tag/v0.21.1) | 2.1.0
[0.21.0](https://github.com/google/struct2tensor/releases/tag/v0.21.0) | 2.1.0
0.0.1.dev*                                                             | 1.15
