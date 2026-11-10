Python
====================

If you do not have any specific requirement for the Python libraries
to be used in INDACO, then you can `load` the provided Python modules
and that should be enough.

If instead you need some specific needs for some Python libraries,
then it is probably better to install a Python environment directly in
your project. One of the possibilities is to install Miniconda.

Installing Miniconda
-----------------------------

To install Miniconda, you can follow the guidelines provided in the
`Miniconda Installation web page
<https://continuumio-docs.readthedocs-hosted.com/miniconda/install/>`_.

In brief, suppose that you have a folder ``software`` inside your project directory.

1. Move to the installation folder, for example:

   .. code:: bash

	     cd /exa/myproject/software
	     
2. Download Miniconda:

   .. code:: bash

	     wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

3. Verify installer's integrity (Optional, see the link for more details)
4. Install:

   .. code:: bash

	     bash Miniconda3-latest-Linux-x86_64.sh

5. Skip the initialization of the default ``base`` virtual environment.


Create a virtual environment
-------------------------------------------

Sometimes it is required to at least temporarily set up your environment variable to use ``conda``.
You can do by writing something like:

.. code:: bash

	  eval "$(/home/users/username/myproject/software/miniconda3/bin/conda shell.bash hook)"

In this way to should be able to create a Python virtual environment
(replace ``myenvname`` with an appropriate name for your environment.)

.. code:: bash

	  conda create --name myenvname

Once the environment is created, you can activate it an install the required libraries, for example

.. code:: bash

	  conda activate myenvname
	  conda install numpy scipy matplotlib h5py

Once finished, you can deactivate the environment

.. code:: bash

	  conda deactivate


Use Python within a script
--------------------------------------

If you have a `bash` script that you want to run during an interactive
session or a `slurm` batch file to submit with `sbatch`, you have to
add a couple of lines to

- set up the environment variables to allow loading `conda` and the corresponding `Python`;
- activate the corresponding virtual environment

The two lines to add should look something like this:

.. code:: bash

	  source /exa/projects/myproject/software/miniconda3/etc/profile.d/conda.sh
	  conda activate myenvname

Then, inside your script, you can call any python script like ``python myscript.py``.




