============
Installation
============

It is recommended that you first create a virtual environment.
For example, you can create one using `anaconda <https://www.anaconda.com/download/>`__:

.. code-block:: bash

    conda create --name ptlflow python=3.12
    conda activate ptlflow


Next, install PyTorch following the official instructions at `https://pytorch.org/ <https://pytorch.org/>`__ (PTLFlow has been tested with Python versions >= 3.10 and <= 3.12 and PyTorch >= 2.3 and <= 2.6.0).

Afterward, you can use PTLFlow in one of two ways:

    1. :ref:`install-pypi` or
    2. :ref:`running-from-source`

.. _install-pypi:

Installing PTLFlow from PyPI
============================

To install from PyPI, run:

.. code-block:: bash

    pip install ptlflow

The PyPI package should contain the most recent stable version. If you want to install the newest
(possibly unstable) version, you can do it by:

.. code-block:: bash

    pip install git+https://github.com/hmorimitsu/ptlflow.git

.. _initial-scripts:

Getting initial scripts
-----------------------

PTLFlow offers some scripts and config files to help you start using the optical flow models.

In order to get just these scripts without having to clone the whole repository, you can
open a terminal and type:

.. code-block:: bash

    python -c "import ptlflow; ptlflow.download_scripts()"

By default, this will download and save the scripts to a folder called ``ptlflow_scripts``.
If you are going to be doing training or validation, then be sure to edit the file
``datasets.yml`` and add the paths to the datasets you want to use in your machine.

To know more details about how to use each script, please read the next pages in this documentation.

Getting the names of available models
-------------------------------------

You can run the following command to print the list of available models:

.. code-block:: bash

    python -c "import ptlflow; print(ptlflow.get_model_names())"

You can also use one of the initial scripts to get the name of the available models by providing the ``--help`` argument to one of the scripts and then checking the ``--model`` instructions.
For example, you can run:

.. code-block:: bash

    python validate.py --help

Note, however, that the list of models shown in the help will include the full module path to the model.
For example, the ``raft`` model will be listed as ``ptlflow.models.raft``.
You do not need to provide the full path to use the model, just the last name is enough.
For example, to validate using ``raft``, just type:

.. code-block:: bash

    python validate.py --model raft

Getting the names of pretrained checkpoints of a model
------------------------------------------------------

Suppose you chose a model, let's say ``raft``, but you do not know which pretrained checkpoints
are available for it. You can find that out by using one of the initial scripts and passing any invalid
checkpoint name to ``--ckpt_path`` as follows:

.. code-block:: bash

    python validate.py --model raft --ckpt_path which

This will show an error message with a list of the available checkpoint names.

Optional dependencies
=====================

The dependencies installed from pip are the minimum required to run everything. Nonetheless, there are some
other dependencies which can be installed separately to improve the performance of some models.

Many models can use the ``spatial-correlation-sampler`` package, which is not installed by default.
With this package, the speed and memory requirements of some models should improve.
If you want to install it, you can run:

.. code-block:: bash

    pip install spatial-correlation-sampler

Another useful package for decreasing memory consumption of some models is the ``alt_cuda_corr``.
It is included inside PTLFlow, but you have to manually compile it following the instructions below:

    1. Download and install the CUDA toolkit from `https://developer.nvidia.com/cuda-toolkit-archive <https://developer.nvidia.com/cuda-toolkit-archive>`__
        **IMPORTANT!** You must choose the same CUDA version that is used in your PyTorch.
    2. Enter the package directory and compile it with:

.. code-block:: bash

    cd ptlflow/utils/external/alt_cuda_corr/
    python setup.py install

Troubleshooting
===============

In some machines, the ``spatial-correlation-sampler`` package from PyPI cannot be installed.
If you also see errors when trying to install it, then you can try to install the version from GitHub:

.. code-block:: bash

    pip install git+https://github.com/ClementPinard/Pytorch-Correlation-extension.git

.. _running-from-source:

Running from the source code
============================

If you want to modify PTLFlow in some way (to add a new model, change parameters, etc.), you will have
to clone and use the source code instead. You can first clone the source code to your local machine
and enter the downloaded folder as:

.. code-block:: bash

    git clone https://github.com/hmorimitsu/ptlflow
    cd ptlflow

You may also have to install the dependencies to run PTLFlow (in case you do not have them):

.. code-block:: bash

    pip install -r requirements.txt

Another option is to install PTLFlow to your environment. The benefit is that ptlflow will be
accessible from anywhere while using the environment. The drawback is that you will have to reinstall
it everytime you modify the code. Therefore, this option is not recommended if you are
making changes to the code.

To install PTLFlow, you will have to build the wheel package and then install with ``pip``.

First, install ``build``, if you do not already have it:

.. code-block:: bash

    pip install build

Then, enter the directory you cloned, build the package and install it:

.. code-block:: bash

    cd ptlflow
    python -m build
    pip install dist/ptlflow-*.whl

Then you should be able to use ``ptlflow`` in the same as if you had installed it from ``pip``.