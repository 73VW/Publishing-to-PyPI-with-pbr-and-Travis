`⚙️Automatizați publicarea către PyPI cu PBR și Travis`
=========================================================

Pentru documentația completă:

Navighează către `ReadTheDocs <https://automate-your-publishing-to-pypi-with-pbr-and-travis.rtfd.io>`_


`✔️Pasul 1: scrieți setarea proiectului dvs. python`
*****************************************************

Dacă doriți să publicați proiectul către PyPi, mai întâi trebuie să faceți un fișier de instalare.

În acest tutorial, voi acoperi utilizarea PBR_ care simplifică procesul.

`Setup.py`
----------

După cum puteți vedea în fișierul `python setup`_ inclus în acest repository, sintaxa este destul de elementară.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 Trivial, definiți că doriți să utilizați PBR_ și numai atât! 🎉

`Setup.cfg`
-----------

După cum puteți vedea în fișierul `config setup`_ inclus în acest repository, sintaxa nu este mai complicată decât în fișierul precendet.

Să trecem prin fiecare secțiune împreună.

În primul rând, secțiunea metadate:

.. code:: yaml

    # Tipul distribuției python
    [bdist_wheel]
    universal=0

    [metadata]
    # Numele aplicatiei
    name = Publishing to PyPI with pbr and Travis
    # Cine a făcut-o?
    author = Maël Pedretti
    # Chiar trebuie să explic următoarele?
    author_email = mael.pedretti@he-arc.ch
    # Descrierea scurtă a aplicației dvs.
    summary = Publishing to Pypi with PBR and Travis.
    # Tipul licenței
    license = MIT
    # Care fișier conține descrierea lungă?
    description-file =
        README.rst
    # Unde pot accesa proiectul?
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Ce versiune de Python are nevoie să ruleze?
    python_requires = >=3.6
    # Cum clasificați aplicația dvs.? https://pypi.python.org/pypi?%3Aaction=list_classifiers
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

    # Găsiți automat pachetul rădăcină
    [options]
    packages = find:

    # Ce fișiere care nu sunt cod sursă doriți să le implementați?
    [files]
    data_files =
        some/example = some/example/*

    # Unde începe aplicația dvs.?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 După câteva modificări, sunteți gata de pasul următor! 🎉

`✔️Pasul 2: Activează Travis!`
*******************************

Două moduri de a activa Travis sunt prezentate aici. Unul folosind `Travis CLI`_ și altul fără.

`Folosind travis CLI`
----------------------

Execută :code:`travis login` și loghează-te în travis.

Acum poți executa :code:`travis init`.

Dacă vă aflați într-un git repository, Travis îl va detecta și vă va întreba dacă este corect.

În caz contrar, vă va spune că poate detecta repo.

Odată ce execuți :code:`Enter`, Travis întreabă limba principală. În acest caz, tastați :code:`Python`.

Un fișier nou :code:`.travis.yml` este creat și disponibil în repo. Mai mult ca atât, Travis este acum activat pentru acest repo.

Vom trece mai târziu peste acest fișier.

`Manual`
----------

- Navighează către `Travis home page`_.
- Sign up sau Sign in.
- Accesați pagina de profil și sincronizați-vă contul.
- Repositoriile publice Github care vă aparțin sunt acum listate mai sus.
- Schimbați proiectul dorit.

`.travis.yml`
-------------

Acum, să scriem fișierul de setare.

Deoarece documentarea este foarte bine făcută, vă sugerez să o verificați mai întâi, deoarece nu vă voi explica totul. O puteți găsi aici https://docs.travis-ci.com/user/getting-started/.

Cu toate acestea, voi explica setările pe care le folosesc de obicei.

.. code:: yaml

    # Chiar trebuie să explic această linie?
    language: python

    # Puteți utiliza o memorie cache pentru a construi mai repede
    cache: pip

    # versiunea python. Puteți defini mai multe dacă doriți să executați mai multe teste
    python:
      - '3.6'

    # scriptul de instalare sau lista de instalare
    install: pip install rstcheck

    # scriptul de testare sau lista de instalare
    script: rstcheck --recursive .

    # setări pentru notificări, personal nu-mi place să fiu spamat pe e-mailul meu
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # partea interesantă!
    deploy:
      # Dacă aveți nevoie să implementați fișierele create de Travis, utilizați linia următoare
      skip_cleanup: true
      # În acst caz dorim să lansăm către pypi
      provider: pypi
      # Ce distribiție dorim să lansăm
      distributions: sdist bdist_wheel
      # Când dorim să lansăm?
      on:
        # În acest caz, vreau să se lanseze numai atunci când este prezentă o etichetă...
        tags: true
        # ... și numai atuncî când eticheta este pe master și respectă the forma "v0.0.0"
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # Usernameul tău pypi
      user: 73VW
      # Parola dvs. Pypi securizată de Travis dacă aveți instalat Travis CLI
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Utilizați următoarele dacă nu aveți Travis CLI
      password : ${PYPI_PASSWORD}

`Parola`
----------

Dacă nu aveți instalat `Travis CLI`_ , utilizați a doua opțiune pe care am menționat-o mai sus și faceți următoarele

- În pagina dvs. de profil, găsiți proiectul dvs. și faceți clic pe iconița ⚙️. Astfel veți ajunge la setări.
- Mergeți la secțiunea :code:`Environment Variables` și adăugați o nouă variabilă.
- Dacă îmi luați exemplul, numele lui va fi PYPI_PASSWORD și valoarea parolei dvs..

.. image:: ./docs/_static/Add_pypi_password.PNG
    :width: 100%
    :alt: Cum să adăugați parola la Travis

Daca ai`Travis CLI`_, asta e pentru tine.

- Lăsați goală secțiunea de parolă, după cum urmează.

.. code:: yaml

    user: 73VW
    # Parola ta Pypi
    password:

- Acum să-l criptez! Pur și simplu execută :code:`travis encrypt --add deploy.password` iar Travis vă va cere parola, o va cripta și o va adăuga în fișier.

🎉 Acum sunteți gata! 🎉

`✔️Ce urmează acum!`
********************

Acum să încercăm să trimitem totul către repository pentru a verifica dacă totul este bine și dacă testele trec!

Deschideți `Travis home page`_ și verificați dacă totul a reușit!

După cum vă amintiți, nu am creat nicio etichetă în github, astfel încât această comitetare să nu se desfășoare.

Travis va raporta, de asemenea:

:code:`Skipping a deployment with the pypi provider because this is not a tagged commit`

`✔️Să-l etichetăm!`
********************

Acum, creați o etichetă. Acest lucru este ușor cu git. Documentația poate fi găsită `aici <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

Rețineți că cu :code:`git tag` opțiunea :code:`-a` vă permite să specificați versiunea și :code:`-m` mesajul.

Deci, comanda dvs. va fi următoarea:

:code:`git tag -a 0.0.1 -m "Prima implementare pypi"`

Acum puteți verifica dacă a fost creat prin executarea :code:`git tag`.
Rezultatul ar trebui să arate după cum urmează.

.. code:: bash

    $ git tag
    v0.0.1

Acum, încărcați și verificați din nou Travis și pypi și pachetul dvs. ar trebui să fie desfășurate!

PSA: Nu uitați să adăugați :code:`--tags` la comanda de încărcare în caz contrar ei vor rămâne în repo local.

**✔️Desfășurat!**

`⚠️Note globale`
*****************

✔️ Proiectul dvs. trebuie să fie public pentru a utiliza Travis. În caz contrar, trebuie să faceți upgrade la Travis pro.

✔️ Adresa dvs. de e-mail trebuie verificată pe pypi pentru a încărca un nou proiect. În caz contrar, încărcarea va fi respinsă.

✔️ Versiunea dvs. de etichetă ** TREBUIE ** să aibă forma [CIFRĂ.CIFRĂ.CIFRĂ]. Verifică https://docs.openstack.org/pbr/3.1.0/semver.html pentru mai multă informație.

.. Bibliografie:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`python setup`: ./setup.py
.. _`config setup`: ./setup.cfg
.. _`Travis home page`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
