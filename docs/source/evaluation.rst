Evaluation
==========

This artifact enables the replication of the evaluation in our paper.

Reproducing Experiments
-----------------------

To reproduce the code coverage experiments, use the ``evaluation/reproduce_experiments.py`` script inside the Docker container.

.. note::

   - **Environment**: All the following commands are executed in the docker container.
   - **Randomness**: Due to the inherent randomness of fuzzing, the exact code coverage numbers may vary from those presented in the paper. However, ``once4all`` should consistently achieve the best performance among the evaluated fuzzers, demonstrating that the results are reproducible.
   - **Omitted Tools**: Tools such as ``LaST`` and ``Fuzz4All`` are omitted from this evaluation because they require GPU resources or API keys to invoke LLMs. Furthermore, they do not outperform the tools included in this artifact.

Usage
^^^^^

Navigate to the project directory:

.. code-block:: console

   $ cd /home/once4all

Run the reproduction script:

.. code-block:: console

   $ python3 evaluation/reproduce_experiments.py --output-dir ./results

This process takes approximately 10 hours to finish. We recommend using ``screen`` to run it in the background:

.. code-block:: console

   $ screen -S exp -s bash
   $ cd /home/once4all
   $ python3 evaluation/reproduce_experiments.py --output-dir ./results

Common Options
^^^^^^^^^^^^^^

.. list-table::
   :widths: 20 40 20
   :header-rows: 1

   * - Option
     - Description
     - Default
   * - ``--time-budget``
     - Time budget in seconds per run
     - 3600 (1 hour)
   * - ``--runs``
     - Number of runs per fuzzer
     - 1
   * - ``--cores``
     - Number of cores to use
     - 1
   * - ``--fuzzers``
     - List of fuzzers to evaluate (e.g., ``once4all``, ``histfuzz``)
     - ``once4all``, ``histfuzz``, ``yinyang``, ``opfuzz``, ``typefuzz``
   * - ``--output-dir``
     - Directory to store results
     - ``./experiment_results``
   * - ``--z3-path``
     - Path to Z3 binary
     - Auto-detected or ``z3``
   * - ``--cvc5-path``
     - Path to CVC5 binary
     - Auto-detected or ``cvc5``
