INDACO
======

This page is a quick reference to commands for working with the INDACO
platform.

General utilities
-----------------

Show the partitions available to your account
    After logging in, you can check which partitions are associated with your
    account by running:

    .. code-block:: bash

       sacctmgr show User "$USER" --associations

    Check the *Partition* column for the partitions associated with each
    project, shown in the *Account* column.

    You can also list the partitions configured on the cluster with:

    .. code-block:: bash

       scontrol show partition

    This command may show partitions that are not available to your account.

Request an interactive allocation
    Once you know which partition you can use, request an interactive
    allocation with:

    .. code-block:: bash

       salloc -p <partition_name> --time=30

    The ``--time=30`` option sets a 30-minute time limit for the allocation.
    If you omit ``--time``, Slurm uses its default time limit, which in some cases can have
    a relatively small default value.

Working with modules
--------------------

Before loading a module, find its name and check which versions are available.

For example, to search for BLAS-related modules, run:

.. code-block:: bash

   module avail 2>&1 | grep -i blas

Choose a module version that is compatible with your partition's architecture
and the compiler version you plan to use.

Submitting jobs
---------------

To see the jobs currently in the cluster queue, run:

.. code-block:: bash

   squeue

To show only your jobs, run:

.. code-block:: bash

   squeue -u "$USER"

The output includes each job's ID. To cancel one of your jobs, run:

.. code-block:: bash

   scancel <job_id>

To submit a batch job, first prepare a Slurm script following the guidelines
on the INDACO website. Then submit it with:

.. code-block:: bash

   sbatch myscript.slurm
