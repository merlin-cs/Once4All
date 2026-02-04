Welcome to the Once4All Artifact!
=========================================

Thank you for your interest in Once4All! 
This document will guide you through the prerequisites and steps needed to set up and reproduce the results described in our paper. 
Please feel free to reach out if you encounter any issues or have feedback.


.. important:: **Announcement: Project Chimera**

  We have released a preliminary version of Chimera, which incorporates the techniques presented in our ASPLOS 2026 paper as well as those from two additional related works. The code is publicly available at https://github.com/merlin-cs/Chimera.

   **Chimera** is an integrated toolkit for testing, validating, and exploring SMT solvers.
   It brings together techniques from three publications:

   * **Once-for-All: Skeleton-Guided SMT Solver Fuzzing with LLM-Synthesized Generators**
     Maolin Sun, Yibiao Yang, Yuming Zhou.
     *ASPLOS 2026*

   * **Validating SMT Rewriters via Rewrite Space Exploration Supported by Generative Equality Saturation**
     Maolin Sun, Yibiao Yang, Jiangchang Wu, Yuming Zhou.
     *OOPSLA 2025*

   * **Validating SMT Solvers via Skeleton Enumeration Empowered by Historical Bug-Triggering Inputs**
     Maolin Sun, Yibiao Yang, Ming Wen, Yongcong Wang, Yuming Zhou, Hai Jin.
     *ICSE 2023*


Prerequisites
--------------

For optimal performance and reasonable evaluation time (approximately 10 hours), we recommend running the full evaluation on a machine with the following specifications:

- **CPU**: 16 cores or more
- **Memory**: At least 32 GB
- **Disk Space**: 20 GB free

Artifact Evaluation Roadmap
---------------------------

We suggest the following steps for artifact evaluation:

1. **Kick-the-tire**: Follow the **Getting Started** section to download and start the Docker container.
2. **Availability**: The artifact is publicly available on Zenodo with DOI `10.5281/zenodo.17946453 <https://doi.org/10.5281/zenodo.17946453>`_.
3. **Functionality**: Follow the **Usage** section to verify that ``once4all`` runs correctly.
4. **Reproducibility**:

   - **RQ1 (Table 1)**: Refer to the :doc:`bug-report` page for links to the bugs identified by Once4All.
   - **RQ2 (Table 3)**: Refer to the :doc:`evaluation` page to replicate the code coverage comparison.

   .. note::

      - **Randomness**: Due to the randomness of fuzzing, code coverage may differ slightly from the paper, but ``once4all`` should consistently achieve the best performance.
      - **Omitted Tools**: ``LaST`` and ``Fuzz4All`` are omitted from this evaluation as they require GPU resources or LLM API keys and do not outperform the included tools.

Getting Started (Kick-the-tire)
-------------------------------

Step 1: Download the Docker Image
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To get started, download the Docker image from either Zenodo or Docker Hub.

**Option A: Pull from Docker Hub (Recommended)**

For simplicity and ease of use, we recommend pulling the image from Docker Hub using the following command:

.. code-block:: console

  $ docker pull merlin07/once4all:latest

**Option B: Download from Zenodo**

Alternatively, you can download the artifact from Zenodo using this link: [`Artifact <https://doi.org/10.5281/zenodo.17946453>`_]. Once downloaded, extract the artifact archive:

.. code-block:: console

  $ gunzip -c once4all.tar.gz > once4all.tar
  $ cat once4all.tar | docker import - once4all

Step 2: Start the Docker Container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once the image is downloaded, start the Docker container.

**If you pulled the image from Docker Hub:**

.. code-block:: console

  $ docker run -itd --privileged --name once4all merlin07/once4all:latest /bin/bash

**If you downloaded the artifact from Zenodo:**

.. code-block:: console

  $ docker run -itd --privileged --name once4all once4all /bin/bash

This command will start the container in detached mode with the name ``once4all``.

Step 3: Access the Container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To access the container's shell, execute the following command:

.. code-block:: console

  $ docker exec -it once4all /bin/bash

This will open an interactive shell session inside the container, allowing you to run the necessary commands for the evaluation.

Step 4: Verify Dependencies
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Inside the container, run the setup script to ensure all dependencies are correctly installed:

.. code-block:: console

  $ /home/setup.sh

If the setup is successful, you will see a confirmation message indicating that all dependencies are ready.

Usage
-----

(Note: Run these commands inside the Docker container)

Run the tool from the repository root to start mutation-based fuzzing on existing SMT-LIB v2 formulas:

.. code-block:: bash

   python3 once4all.py \
     --solver1 z3 --solverbin1 /path/to/z3 \
     --solver2 cvc5 --solverbin2 /path/to/cvc5 \
     --bugs ./bug_triggering_formulas \
     --option default

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

