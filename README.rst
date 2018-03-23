`⚙️Automate your publishing to PyPI with PBR and Travis`
=========================================================

`✔️Step 1: write the setup for your python project`
***************************************************

If you want to publish your project to PyPi, you first have to make a setup file.

In this tutorial, I will cover the use of PBR_ which simplifies the process.

`Setup.py`
----------

As you can see in the `setup`_ included in this repository, the syntax is quite basic.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 You simplify define that you want to use PBR_ and that's it! 🎉

.. Bibliographie:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`setup`: ./setup.py
