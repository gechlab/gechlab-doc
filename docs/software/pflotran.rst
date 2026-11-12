PFLOTRAN
=================

Here we provide some guidelines to install PFLOTRAN on the INDACO computing cluster.

Almost all information is taken from the reference site of `PFLOTRAN
(Linux installation)
<https://documentation.pflotran.org/user_guide/how_to/installation/linux.html#linux-install>`_

Log into a computing node
----------------------------------

When you only log in into the cluster, you are landing on a *log-in*
node. To perform all the next step you have to connect to a computing
note, possibly with the same architecture that you expect to use for
your project.

To get a list of computing nodes you have access to, you can use:

.. code-block:: bash
   
   sacctmgr show User $USER --associations

The *Partition* column contains the names of the available partitions
for the corresponding *project* (*Account*).

Then you can log in into a computing partition using the *interactive
mode*, that allows you to interact with the computing node without
writing a SLURM file.

To log in, you can use the command `salloc`. For example, if you have access to the `light` partition, you can use:

.. code-block:: bash

   salloc -p light --time=30

The optional argument `--time=30` allows you to set the maximum time
you request for interacting with the node. After that time, you will
be logged out.


Find out the right modules
----------------------------------

Many modules are available in the cluster. First of all, it is
important to select the compiler module (for example, `gcc`) and the MPI library.

To show all the modules that contain the keyword `gcc`, you can use

.. code-block:: bash

   module avail gcc

From the list of modules, we select the one provided for the `x86_64`
architecture (in general, it should be compatible with the
architecture of the node where you will run your code). We select
version `13` of gcc. Therefore, I have to be careful to always load
libraries compatible with this version.

To load the gcc module

..  code-block:: bash

    module load gcc/13.1.0-gcc-8.5.0-65vbkj2


The next module to select is the one related to the MPI libraries.

.. code-block:: bash

   module avail openmpi

We then load

.. code-block:: bash

   module load openmpi/4.1.5-gcc-13.1.0-crgm5zc

.. note::
   
   By default the two aforementioned modules (`gcc` and `openmpi`) are
   not loaded in your computing node.  You should include them in the
   slurm/login script or load them manually when needed.












Installing PETSc
--------------------------

.. note::

   This library is already provided in a module in the cluster, but
   with a version which is not compatible with PFLOTRAN. We need to
   compile the correct version here.

1. Download PETSc

   Suppose that you created a folder `/myproject/software` where you
   will store all the required software, you can first move there with
   `cd`.

   Then, download PETSc

   .. code-block:: bash
   
      git clone https://gitlab.com/petsc/petsc petsc
      cd petsc
      git checkout v3.24.5

2. Unset some default variables (optional, it only raises some warning message)

   .. code-block:: bash

      unset CC CXX FC F77

3. Configure       

   .. code-block:: bash

      ./configure --with-cc=mpicc --with-cxx=mpicxx --with-fc=mpifort \
      --COPTFLAGS='-O3' --CXXOPTFLAGS='-O3' --FOPTFLAGS='-O3 -Wno-unused-function' \
      --with-debugging=no \
      --download-hdf5=yes --download-hdf5-fortran-bindings=yes \
      --download-fblaslapack=yes --download-metis=yes --download-parmetis=yes

   .. note::

      Some other libraries are available in the cluster. However, we
      decide to download them and install them to avoid compatibility
      issues.

4. Make
   
   The configure step should end with a message, something like:

   .. code-block::

      xxx=============================================================================xxx
      Configure stage complete. Now build PETSc libraries with:
      make PETSC_DIR=/myproject/software/petsc PETSC_ARCH=arch-linux-c-opt all
      xxx=============================================================================xxx

   Adapt the code above or simply copy the provided instruction, that is something like

   .. code-block::

      make PETSC_DIR=/myproject/software/petsc PETSC_ARCH=arch-linux-c-opt all

5. Concluding remarks

   In the end it is suggested to run some test. Probably they will not
   work even if the installation was successful, because they will
   require a proper configuration. Skip them.



Installing PFLOTRAN
------------------------------

1. First of all, you have to set up some environmental variables arisen from the PETSc installation.
   You should therefore do

   .. code-block::
      
      export PETSC_DIR=/myproject/software/petsc
      export PETSC_ARCH=arch-linux-c-opt

   If you want, you can put these variables into your user `.bashrc` file
   and probably in the SLURM file required to run the software.

2. Download (clone) PFLOTRAN. You can always start downloading from
   the `/myproject/software` folder.

   .. code-block::

      git clone https://bitbucket.org/pflotran/pflotran

3. Enter in the right folder and `make`

   .. code-block::

      cd pflotran/src/pflotran
      make pflotran

      


Calling PFLOTRAN in your scripts
-----------------------------------------------

First of all, it is in any case useful to define an environment variable
to set the folder where PFLOTRAN was installed.
Adding in your script one line like:

.. code:: bash

	  export PFLOTRAN_DIR=/exa/projects/myproject/software/pflotran

should be enough.

Then, if you want that the executable file to be easily accessible without the full definition of the installation path,
you can include it in your ``PATH`` environment variable. Add to your script the
following:

.. code:: bash

	  export PATH=$PATH:$PFLOTRAN_DIR/src/pflotran

and that should be fine.



