Python
======

There are two ways to use Python on INDACO:

- **Provided modules**: if you have no specific requirements for Python
  libraries, load one of the Python modules provided on INDACO
  (``module load``). This is usually enough.
- **Your own environment**: if you need specific libraries or versions,
  install a Python environment in your project directory. One option is
  `Miniconda <https://docs.conda.io/en/latest/miniconda.html>`_, described below.

.. note::

   In the examples below, replace ``/exa/projects/myproject`` with the path of
   your own project directory, and ``myenvname`` with a name of your choice
   for the environment.


Installing Miniconda
--------------------

For the full details, see the `Miniconda installation page
<https://continuumio-docs.readthedocs-hosted.com/miniconda/install/>`_.
In brief, assuming you have a ``software`` folder inside your project
directory:

1. Move to the installation folder:

   .. code:: bash

      cd /exa/projects/myproject/software

2. Download the Miniconda installer:

   .. code:: bash

      wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

3. (Optional) Verify the integrity of the installer. See the installation
   page linked above for details.

4. Run the installer:

   .. code:: bash

      bash Miniconda3-latest-Linux-x86_64.sh

5. When asked, **do not** initialize the default ``base`` environment.


Creating a virtual environment
------------------------------

Since you skipped the initialization of ``base``, you first need to make
``conda`` available in your current shell session:

.. code:: bash

   eval "$(/exa/projects/myproject/software/miniconda3/bin/conda shell.bash hook)"

You can then create a virtual environment:

.. code:: bash

   conda create --name myenvname

Once created, activate the environment and install the libraries you need,
for example:

.. code:: bash

   conda activate myenvname
   conda install numpy scipy matplotlib h5py

When you are done, deactivate the environment:

.. code:: bash

   conda deactivate


Using Python in a script
------------------------

To use your environment in a ``bash`` script (run in an interactive session)
or in a ``slurm`` batch file (submitted with ``sbatch``), add two lines
before any Python command:

- the first makes ``conda`` available, so the right ``python`` can be found;
- the second activates your virtual environment.

.. code:: bash

   source /exa/projects/myproject/software/miniconda3/etc/profile.d/conda.sh
   conda activate myenvname

After that, you can run Python scripts as usual, for example
``python myscript.py``.
