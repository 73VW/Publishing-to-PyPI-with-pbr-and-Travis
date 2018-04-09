`⚙️Automate your publishing to PyPI with PBR and Travis`
=========================================================

`✔️Step 1: write the setup for your python project`
***************************************************

If you want to publish your project to PyPi, you first have to make a setup file.

In this tutorial, I will cover the use of PBR_ which simplifies the process.

`Setup.py`
----------

As you can see in the `setup.py`_ file included in this repository, the syntax is quite basic.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 You simplify define that you want to use PBR_ and that's it! 🎉

`Setup.cfg`
-----------

As you can see in the `setup.cfg`_ file included in this repository, the syntax not more complicated than the last file.

Let's go through every section together.

First of all, the metadata section:

.. code:: yaml

    [metadata]
    # App name
    name = Test Publishing
    # Who made it?
    author = Maël Pedretti
    # Do I really need to explain the following?
    author_email = mael.pedretti@he-arc.ch
    # The short description of your app
    summary = Publishing to Pypi with PBR and Travis.
    # License type
    license = MIT
    # Which file contains the long description?
    description-file =
        README.rst
    # Where can I access the project?
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Which version of Python does it need to run?
    python_requires = >=3.6
    # How do you classify your app? https://pypi.python.org/pypi?%3Aaction=list_classifiers
    classifier =
        Development Status :: 4 - Beta
        Environment :: Other Environment
        Intended Audience :: Education
        Operating System :: MacOS :: MacOS X
        Operating System :: Microsoft :: Windows
        Operating System :: POSIX
        Programming Language :: Python :: 3 :: Only
        Programming Language :: Python :: 3.6
        Programming Language :: Python :: Implementation :: CPython
        Topic :: Home Automation

    # Which files that are not source code do you want to deploy?
    [files]
    data_files =
        some/example = some/example/*

    # Where does your app start?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 After a few tweaking you are now ready to go! 🎉

`✔️Step 2: Enable Travis!`
***************************

.. Bibliographie:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`setup.py`: ./setup.py
.. _`setup.cfg`: ./setup.cfg
