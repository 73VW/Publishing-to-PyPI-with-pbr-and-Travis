`⚙️Otomatiskan penerbitan anda ke PyPI dengan PBR dan Travis`
==============================================================

`✔️Langkah 1: tuliskan setup untuk python project kamu`
********************************************************
jika kamu ingin mempublish project kamu ke PyPi, pertama kamu harus membuat setup file.

di tutorial ini, saya akan membahas menggunakan PBR_ dengan proses sederhana.

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

🎉 kamu hanya menjelaskan secara sederhana ketika ingin menggunakan PBR_ 🎉

`Setup.cfg`
-----------

seperti yang bisa kamu lihat di file `config setup`_ sudah termasuk ke dalam repository ini, syntaxnya tidak lebih sulit dari pada file yang terakhir.

Mari kita bahas setiap bagian bersama-sama.

Pertama bagian metadata:

.. code:: yaml

    # Type distibusi python
    [bdist_wheel]
    universal=0

    [metadata]
    # Nama App
    name = Publishing to PyPI with pbr and Travis
    # Siapa yang membuat ?
    author = Maël Pedretti
    # apakah aku harus menjelaskan bagian ini ?
    author_email = mael.pedretti@he-arc.ch
    # deskripsi singkat app kamu
    summary = Publishing to Pypi with PBR and Travis.
    # License type
    license = MIT
    # File mana yang mengandung deskripsi panjang ?
    description-file =
        README.rst
    # Dimana aku bisa mengakses project ?
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Versi python apa yang di butuhkan untuk menjalankan ?
    python_requires = >=3.6
    # Bagaimana Anda mengklasifikasikan aplikasi Anda ? https://pypi.python.org/pypi?%3Aaction=list_classifiers
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

    # otomatis mencari package root
    [options]
    packages = find:

    # File mana yang bukan termasuk source code yang ingin kamu deploy ?
    [files]
    data_files =
        some/example = some/example/*

    # Dimana app kamu akan di mulai ?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 Setelah sedikit mengutak-atik sekarang kamu telah siap 🎉

`✔️Step 2: Enable Travis!`
***************************

Dua cara untuk aktifkan Travis di jelaskan disini. Satu dengan menggunakan `Travis CLI`_ dan yang satunya tidak.

`Using travis CLI`
-------------------

Run :code:`travis login` login ke travis.

Sekarang kamu bisa menjalankan :code:`travis init`.

Jika kamu ada di dalam git repository, Travis akan mendeteksi itu dan dan tanyakan apakah ini benar.

Jika tidak, dia akan memberitahu kamu kalau itu bisa mendeteksi repo.

Sekali kamu menekan tombol :code:`Enter`, Travis akan menanyakan bahasa pemograman yang digunakan, di kasus ini, typenya :code:`Python`.

Sekarang file baru yang namanya :code:`.travis.yml` telah dibuat dan tersedia di repo kamu. Bahkan, Travis bisa digunakan untuk repo ini. 
 
Kita akan memeriksa file ini nanti.

`Manually`
----------

- Pergi ke `Travis home page`_.
- Sign up atau Sign in.
- Pergi ke halaman profile kamu dan sync akun kamu.
- Repositories public Github kamu sekarang tercantum diatas.
- Toggle project yang kamu inginkan.

`.travis.yml`
-------------

Sekarang, mari menulis setting file kita.

Karena dokumentasinya itu dibuat dengan sangat baik, saya menyarankan anda untuk memeriksa terlebih dahulu karena saya tidak akan menjelaskan semuanya. Kamu bisa mencarinya di sini https://docs.travis-ci.com/user/getting-started/.

Namun, saya hanya menjelaskan settingan yang biasa saya gunakan.

.. code:: yaml

    # apakah perlu aku menjelaskan baris ini ?
    language: python

    # Kamu bisa menggunakan cache untuk build lebih cepat
    cache: pip

    # python version. You can define more than one if you want to run multiple tests
    # versi python. kamu bisa menjelaskan lebih dari satu jika kamu ingin menjalankan multiple test
    python:
      - '3.6'

    # install script kamu atau list install kamu
    install: pip install rstcheck

    # test script kamu atau daftar list install kamu
    script: rstcheck --recursive .

    # settings untuk notifikasi, Saya personal tidak suka spam ke email saya.
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # the interresting part!
    deploy:
      # jika kamu ingin deploy file Travis yang telah dibangun, gunakan baris selanjutnya
      skip_cleanup: true
      # di kasus ini kita ingin deploy ke pypi
      provider: pypi
      # distibusi apa yang kita ingin deploy
      distributions: sdist bdist_wheel
      # Kapan kita ingin deploy ?
      on:
        # di kasus ini saya hanya ingin deploy ketika ada tag...
        tags: true
        # ... and when tag is on master and respects the form "v0.0.0"
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # username pypi kamu
      user: 73VW
      # Pypi password kamu aman dengan Travis jika kamu menginstall Travis CLI
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Gunakan berikut ini jika kamu tidak punya Travis CLI
      password : ${PYPI_PASSWORD}

`Password`
----------

Jika kamu tidak menginstall `Travis CLI`_, gunakan option kedua yang telah saya sebutkan diatas dan lakukan yang berikut ini:

- Di halaman profil kamu, kamu cari project kamu dan click little gear ⚙️. Ini akan membawa kamu pergi ke settings.
- Pergi ke bagian :code:`Environment Variables` dan tambahkan variable baru. 
- Jika kamu mengambil dari contohku, namanya akan menjadi PYPI_PASSWORD dan isinya adalah password kamu.

.. image:: Add_pypi_password.PNG
    :width: 100%
    :alt: How-to add your password to Travis

Jika kamu mempunyai `Travis CLI`_, yang satu ini untuk kamu.

- Biarkan kosong bagian password, seperti dibawah ini.

.. code:: yaml

    user: 73VW
    # Your Pypi password
    password:

- Ayo kita mengenkripsi, jalankan :code:`travis encrypt --add deploy.password` dan Travis akan menanyakan password kamu, itu adalah enskripsi dan paste itu di file.

🎉 Sekarang kamu telah siap untuk pergi 🎉

`✔️Jadi, Apa sekarang?!`
*************************

Ayo kita coba push semuanya ke repository untuk mengecek apakah semuanya sudah benar dan test lulus.

Pergi ke `Travis home page`_ dan cek apakah semuanya seperti yang diinginkan.

Untuk kamu ingat, kita tidak set up semua tag di github jadi commit ini tidak seharusnya di deploy.

Travis juga akan bilang seperti ini:

:code:`Skipping a deployment with the pypi provider because this is not a tagged commit`

`✔️Let's tag it!`
******************

Sekarang buat tag. ini sangat mudah dengan git. Dokumentasi Git tag bisa di temukan di `here <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

Perhatikan bahwa dengan :code:`git tag` pilihan :code:`-a` memungkinkan anda menentukan versi dan :code:`-m` pesan.

Jadi, perintah Anda adalah sebagai berikut:

:code:`git tag -a 0.0.1 -m "First pypi deployment"`

Sekarang kamu bisa memeriksa jika sudah dibuat dengan menjalankan :code:`git tag`.
Hasilnya akan terlihat seperti berikut:

.. code:: bash

    $ git tag
    v0.0.1

dan sekarang push dan periksa kembali Travis dan pypi dan package kamu yang harus di deploy

PSA: jangan lupa tambahkan :code:`--tags` ke command push kamu jika tidak mereka akan tetap berada di repo local kamu.

**✔️Deployed!**

`⚠️Global notes`
*****************

✔️ Project kamu harus public untuk menggunakan Travis. Jika tidak kau bisa upgrade ke Travis pro.

✔️ Alamat email kamu harus sudah di verifikasi di pypi untuk mengunggah project baru. Jika tidak unggahan akan di tolak.

✔️ Tag versi kamu **HARUS** dalam bentuk [DIGIT.DIGIT.DIGIT]. cek https://docs.openstack.org/pbr/3.1.0/semver.html untuk info lebih lanjut.

.. Bibliographie:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`python setup`: ./setup.py
.. _`config setup`: ./setup.cfg
.. _`Travis home page`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
