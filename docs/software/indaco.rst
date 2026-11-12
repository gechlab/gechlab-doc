INDACO
==================

This document is simply a `cheat sheet` of some commands useful to
interact with the INDACO platform.

General utilities
-------------------------------

Show the available partitions
    Once you are loggen in, it is often
    useful to know to which *partition* you user has access to.
    This can be verifie with:

    .. code-block:: bash
		    
       sacctmgr show User $USER --associations

    The *Partition* column contains the names of the available
    partitions for the corresponding *project* (*Account*).

    An alternative command should be

    .. code-block:: bash

       scontrol show partition

    
Log into a partition in interactive mode
    Once you know which partition you can use, you can have access to it in an
    interactive mode with:

    ..  code-block:: bash

	salloc -p <name_of_the_partition> --time=30

    The optional argument ``--time`` is useful if you want to interact with the
    partition for at maximum 30 minuters, which is more than the defaul value available
    when this option is not specified.


Working with modules
-------------------------------

Of course, before loading a module, you should first find its name.

For example, if you want to check if the library `BLAS` is available and on which module name, you can do

.. code-block:: bash

   module avail 2>&1 | grep blas

Then, if available, you should select the library in agreement with
the architecture of your partition and with the compiler version.


Job sumission
---------------------------

If you want to know how busy the cluster is, you can type

.. code-block:: bash

   squeue

To have a shorter list, containing only informations related to your user, use

.. code-block:: bash

   squeue -u $USER

This will also list the ID of your submitted jobs, which allows you to
kill it in case of need.

If you want to submit a job using a slurm batch file, you should first prepare a suitable script following the guidelines provided in the INDACO web page.
Then, the batch script can be submitted as

.. code-block:: bash

   sbatch myscript.slurm

   



    

    
