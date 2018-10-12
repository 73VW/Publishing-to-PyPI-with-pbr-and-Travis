`⚙️Otomatiskan penerbitan anda ke PyPI dengan PBR dan Travis`
=============================================================

`✔️Langkah 1: tuliskan setup untuk python project kamu`
***************************************************
jika kamu ingin mempublish project kamu ke PyPi, pertama kamu harus membuat setup file.

ditutorial ini, saya akan membahas menggunakan PBR yang memudahkan proses.

`Setup.py`
----------

seperti yang bisa kamu lihat di file `python setup`_ sudah termasuk kedalam repository ini, syntaxnya sangat basic

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 kamu menjelaskan dengan sederhana ketika menggunakan PBR_ 🎉

`Setup.cfg`
-----------

seperti yang bisa kamu lihat di file `config setup`_ sudah termasuk ke dalam respository ini, syntaxnya tidak lebih sulit dari pada file yang terakhir.

Mari kita bahas setiap bagian bersama-sama

Pertama bagian metadata:

.. code:: yaml

    # Type of python distribution
    [bdist_wheel]
    universal=0

    [metadata]
    # App name
    name = Publishing to PyPI with pbr and Travis
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
        Topic :: Education

    # Automatically find root package
    [options]
    packages = find:

    # Which files that are not source code do you want to deploy?
    [files]
    data_files =
        some/example = some/example/*

    # Where does your app start?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 Setelah sedikit mengutak-atik sekarang kamu telah siap 🎉

`✔️Step 2: Enable Travis!`
***************************

Dua cara untuk aktifkan Travis diberikan disini. Satu dengan menggunakan `Travis CLI`_ dan yang satunya tidak.

`Using travis CLI`
-------------------

Run :code:`travis login` login ke travis.

Sekarang kamu bisa menjalankan :code:`travis init`.

Jika kamu ada di dalam git repository, Travis akan mendeteksi itu dan dan tanyakan apakah ini benar.

Jika tidak, dia akan memberitahu kamu kalau itu bisa mendeteksi repo.

Sekali kau menekan tombol :code:`Enter`, Travis akan menanyakan bahasa pemograman yang digunakan, di kasus ini, typenya :code:`Python`.

Sekarang file baru yang namanya :code:`.travis.yml` telah dibuat dan tersedia di repo kamu. Bahkan, Travis bisa digunakan di untuk repo ini. 
 
