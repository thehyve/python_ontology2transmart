################################################################################
Example ontology to TranSMART loader
################################################################################

|Build status| |codecov| |pypi| |downloads|

.. |Build status| image:: https://travis-ci.org/thehyve/python_ontology2transmart.svg?branch=master
   :alt: Build status
   :target: https://travis-ci.org/thehyve/python_ontology2transmart/branches
.. |codecov| image:: https://codecov.io/gh/thehyve/python_ontology2transmart/branch/master/graph/badge.svg
   :alt: codecov
   :target: https://codecov.io/gh/thehyve/python_ontology2transmart
.. |pypi| image:: https://img.shields.io/pypi/v/ontology2transmart.svg
   :alt: PyPI
   :target: https://pypi.org/project/ontology2transmart/
.. |downloads| image:: https://img.shields.io/pypi/dm/ontology2transmart.svg
   :alt: PyPI - Downloads
   :target: https://pypi.org/project/ontology2transmart/

This package contains a mapper that reads ontologies from DIMDI_ (Deutsche Institut für Medizinische Dokumentation und Information)
and translates them to the data model of the TranSMART_ platform,
an open source data sharing and analytics platform for translational biomedical research.

It also provides a utility that applies the mapper and writes the translated data to tab-separated files
that can be loaded into a TranSMART database using the transmart-copy_ tool.

⚠️ Note: this is a very preliminary version, still under development.
Issues can be reported at https://github.com/thehyve/python_ontology2transmart/issues.

.. _DIMDI: https://www.dimdi.de
.. _TranSMART: https://github.com/thehyve/transmart_core
.. _transmart-copy: https://github.com/thehyve/transmart-core/tree/dev/transmart-copy
.. _transmart-loader: https://pypi.org/project/transmart-loader


Installation
------------

The package requires Python 3.6.

To install ``ontology2transmart``, do:

.. code-block:: bash

  pip install ontology2transmart

Or from source:

.. code-block:: bash

  git clone https://github.com/thehyve/python_ontology2transmart.git
  cd python_ontology2transmart
  pip install .


Run tests (including coverage) with:

.. code-block:: bash

  python setup.py test


Usage
-----

Read ontology from a collection of TSV files from `DIMDI`_ and write the output in transmart-copy
format to ``/path/to/output``. The output directory should be
empty of not existing (then it will be created).

.. code-block:: bash

  ontology2transmart <system> <chapters> <groups> <codes> /path/to/output

Parameters:

:``system``:           Unique identifier for the ontology
:``chapters``:         Semicolon-separated file with chapters
:``groups``:           Semicolon-separated file with groups
:``codes``:            Semicolon-separated file with codes
:``/path/to/output``:  Output directory

Example: the ICD-10-GM (German modification of ICD-10) is available at icd10gm2019syst-meta.zip_.

.. _icd10gm2019syst-meta.zip: https://www.dimdi.de/dynamic/.downloads/klassifikationen/icd-10-gm/version2019/icd10gm2019syst-meta.zip

.. code-block:: bash

  # Unzip and navigate to the classification directory
  mkdir icd10gm2019
  cd icd10gm2019
  unzip ../icd10gm2019syst-meta.zip
  cd Klassifikationsdateien
  # create an output directory
  mkdir output
  # apply the mapping
  ontology2transmart http://dimdi.de/icd10gm2019 icd10gm2019syst_kapitel.txt icd10gm2019syst_gruppen.txt icd10gm2019syst_kodes.txt output

This generates the directories ``i2b2metadata`` and ``i2b2demodata`` in the ``output`` directory.
The generated data can be loaded using transmart-copy_:

.. code-block:: console

  # Download transmart-copy:
  curl -f -L https://repo.thehyve.nl/service/local/repositories/releases/content/org/transmartproject/transmart-copy/17.1-HYVE-5.9-RC3/transmart-copy-17.1-HYVE-5.9-RC3.jar -o transmart-copy.jar
  # Load data
  PGUSER=tm_cz PGPASSWORD=tm_cz java -jar transmart-copy.jar -d output


License
-------

Copyright (c) 2019 The Hyve B.V.

The ontology to TranSMART loader is licensed under the MIT License. See the file `<LICENSE>`_.
