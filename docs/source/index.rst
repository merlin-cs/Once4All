Welcome to the Once4All Artifact!
=========================================

Thank you for your interest in Once4All! 
This document will guide you through the prerequisites and steps needed to set up and reproduce the results described in our paper. 
Please feel free to reach out if you encounter any issues or have feedback.

Prerequisites
--------------

For optimal performance and reasonable evaluation time (approximately 10 hours), we recommend running the full evaluation on a machine with the following specifications:

- **CPU**: 16 cores or more
- **Memory**: At least 32 GB
- **Disk Space**: 20 GB free

Getting Started (Kick-the-tire)
-------------------------------

.. warning::

   The Docker image and artifact are currently pending upload. The links and commands below will be functional once the upload is complete.

To begin, please download the artifact from Zenodo using this link: [`Artifact <https://doi.org/10.5281/zenodo.14053328>`_]. Once downloaded, extract the artifact archive:

.. code-block:: console

  $ gunzip -c once4all.tar.gz > once4all.tar
  $ cat once4all.tar | docker import - once4all

Alternatively, you can pull the Once4All Docker image directly from Docker Hub:

.. code-block:: console

  $ docker pull merlin07/once4all:latest

Starting the Container
----------------------

Once you have either imported or pulled the image, please use the following commands to start the container.

**If you downloaded the artifact from Zenodo:**

.. code-block:: console

  $ docker run -itd --privileged --name once4all once4all /bin/bash

**If you pulled the image from Docker Hub:**

.. code-block:: console

  $ docker run -itd --privileged --name once4all merlin07/once4all:latest /bin/bash

.. note::

  When the container starts successfully, a long hash string will appear on the terminal, indicating that the **Kick-the-tire** setup is complete.

You can use the following command to enter the container:

.. code-block:: console

   $ docker exec -it once4all /bin/bash

Usage
-----

(Note: Run these commands inside the Docker container)

Run the tool from the repository root:

.. code-block:: bash

   python3 once4all.py \
     --solver1 z3 --solverbin1 /path/to/z3 \
     --solver2 cvc5 --solverbin2 /path/to/cvc5 \
     --bugs ./bug_triggering_formulas \
     --option default

Modes
^^^^^

**1. Mutation Mode (Default)**
Mutates existing SMT-LIB v2 formulas found in the specified bugs directory.

**2. Standalone Mode**
Generates formulas from scratch using the generators without seed files.

.. code-block:: bash

   python3 once4all.py --standalone ...

CLI Options
^^^^^^^^^^^

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Option
     - Description
   * - ``--solver1`` / ``--solverbin1``
     - Name and binary path of the first solver (e.g., ``z3``, ``/usr/bin/z3``).
   * - ``--solver2`` / ``--solverbin2``
     - Name and binary path of the second solver.
   * - ``--bugs``
     - Directory containing seed SMT-LIB v2 formulas (default: ``bug_triggering_formulas/``).
   * - ``--option``
     - Solver options mode: ``default``, ``regular``, or ``comprehensive``.
   * - ``--processes``
     - Number of parallel processes to use (defaults to CPU count).
   * - ``--temp``
     - Temporary directory for solver output (default: ``./temp/``).
   * - ``--generator_path``
     - Path to a custom directory containing generators.
   * - ``--standalone``
     - Run in standalone generation mode (no seed files required).

Output
^^^^^^

The tool creates a timestamped directory for each run (e.g., ``YYYY-MM-DD-HH-MM-SS/``) containing intermediate files.

Triaged bugs are stored in the ``temp/`` directory:

- ``temp/crash/<N>/``: Solver crashes.
- ``temp/InvalidModel/<N>/``: Invalid model bugs.
- ``temp/soundness/<N>/``: Soundness bugs (disagreements between solvers).

Each folder contains the bug-triggering ``.smt2`` file and an ``error_logs.txt`` with details.

Contents
--------

Below is an outline of the content provided for this artifact:

.. toctree::

   evaluation
   bug-report

