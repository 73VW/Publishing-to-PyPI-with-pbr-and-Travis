`⚙️Automatisiere deine Veröffentlichung zu PyPI mit PBR und Travis`
===================================================================

Vollständige Dokumentation:

Navigiere zu `ReadTheDocs <https://automate-your-publishing-to-pypi-with-pbr-and-travis.rtfd.io>`_


`✔️Schritt 1: Erstelle die Konfiguration für dein Python project`
****************************************************************

Um dein Project auf PyPi zu veröffentlichen musst zu zuerst die setup Datei anlegen.

In diesem Tutorial zeige ich die Benutzung von PBR_, welches diesen Prozess vereinfacht.

`Setup.py`
----------

Wie in der `python setup`_ Datei hier aus dem Repository zu sehen, ist die syntax relativ einfach.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 Du konfigurierst, dass du PBR_ nutzen möchtest, das wars! 🎉

`Setup.cfg`
-----------

Wie in der `config setup`_ zu sehen, ist die Syntax nicht viel komplizierter, als in der letzten Datei.

Lass uns gemeinsam durch die Konfiguration gehen.

Zuerst der Metadata-Abschnitt:

.. code:: yaml

    # Art der pyhton distribution
    [bdist_wheel]
    universal=0

    [metadata]
    # App name
    name = Publishing to PyPI with pbr and Travis
    # Wer hat es erstellt?
    author = Maël Pedretti
    # E-Mailadresse des Erstellers
    author_email = mael.pedretti@he-arc.ch
    # Kurzbeschreibung
    summary = Publishing to Pypi with PBR and Travis.
    # Lizenz
    license = MIT
    # Welche Datei enthält die ausführliche Beschreibung?
    description-file =
        README.rst
    # Wo kann ich das Projekt auschecken?
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Benötigte pyhton Version?
    python_requires = >=3.6
    # Klassifikation der App? https://pypi.python.org/pypi?%3Aaction=list_classifiers
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

    # Automatisch das root package suchen?
    [options]
    packages = find:

    # Welche Nichtquelltextdateien sollen integriert werden?
    [files]
    data_files =
        some/example = some/example/*

    # Womit startet die App?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 Nach ein paar Anpassungen bist du startklar! 🎉

`✔️Schritt 2: Aktiviere Travis!`
*******************************

Es gibt 2 Möglichkeiten, Travis zu nutzen. Einmal mit `Travis CLI`_ und einmal ohne.

`Mit travis CLI`
-------------------

Führe :code:`travis login` aus und logge dich bei travis ein.

Jetzt führe :code:`travis init` aus.

Falls du dich in einem Git Repository befindest, fragt dich Travis, ob das korrekt ist.

Wenn nicht, bietet es an das Repository zu suchen.

Sobald du :code:`Enter` drückst, fragt dich Travis nach der Programmiersprache. In diesem Fall schreibe :code:`Python`.

Jetzt wurde automatisch eine :code:`.travis.yml` angelegt und ist in deinem Repository verfügbar. Travis ist jetzt für das Repository aktiviert.

Wir gehen die Datei später durch.

`Manually`
----------

- Gehe auf die `Travis home page`_.
- Melde dich an oder log dich ein.
- Gehe auf deine Profilseite und synchronisiere.
- Deine öffentlichen Github repositories werden jetzt angezeigt.
- Wähle das Projekt, welches du verwenden möchtest.

`.travis.yml`
-------------

Jetzt zur Konfigurationsdatei.

Es gibt eine großartige Dokumentation, schau dort erstmal rein. Die Dokumentation ist hier zu finden https://docs.travis-ci.com/user/getting-started/.

Ich erkläre hier nur die Dinge, die ich nutze.

.. code:: yaml

    # Sprache?
    language: python

    # Cache für schnellere Builds
    cache: pip

    # python version. Es ist möglich, mehrere zu definieren.
    python:
      - '3.6'

    # Dein Installscript bzw. eine Liste der zu installierenden Pakete
    install: pip install rstcheck

    # Dein Testscript
    script: rstcheck --recursive .

    # Benachrichtigungseinstellungen
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # Der interessante Part!
    deploy:
      # Falls du die Files nach dem Build in Travis noch benötigst (deploy)
      skip_cleanup: true
      # In diesem Fall deployen wir zu pypi
      provider: pypi
      # Deploymentdistribution
      distributions: sdist bdist_wheel
      # Wann wird deployed?
      on:
        # In diesem Fall nur, wenn ein tag vorhanden ist
        tags: true
        # ... und wenn der Tag "v0.0.0" entspricht
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # Dein pypi Name
      user: 73VW
      # Dein pypi Passwort (gesichert durch die Travis CI, falls du die Travis CLI installiert hast)
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Benutze folgendes, falls die CLI nicht installiert ist
      password : ${PYPI_PASSWORD}

`Password`
----------

Wenn du keine `Travis CLI`_ installiert hast, benutze die zweite, oben erwähnte Option und mache folgendes:

- Klicke auf das kleine Zahnrad in deinem Profil ⚙️. Das führt sich zu deinen Einstellungen.
- Gehe zum :code:`Environment Variables` Abschnit und füge eine neue Variable hinzu.
- Wenn du meinem Beispiel folgst, füge PYPI_PASSWORD hinzu und als Wert dein Passwort.

.. image:: ./docs/_static/Add_pypi_password.PNG
    :width: 100%
    :alt: Füge dein Passwort zu Travis hinzu

Wenn du die `Travis CLI`_ benutzt, das hier ist dein Weg.

- Lass die Passwortsektion, wie folgt zu sehen, frei.

.. code:: yaml

    user: 73VW
    # Dein Pypi Passwort
    password:

- Jetzt lassen wir das Kennwort verschlüsseln :code:`travis encrypt --add deploy.password`. Travis wird dich nach deinem Kennwort fragen und dieses in die Datei hinzufügen.

🎉 Jetzt bist du startklar! 🎉

`✔️Was jetzt?!`
******************

Push alles zum Repository und schau, ob alle Tests erfolgreich durchlaufen!

Gehe auf die `Travis home page`_ und prüfe, ob alles geklappt hat!

Da wir auf Github keine Tags vergeben haben, sollte nichts deployed werden.

Travis wird das auch erwähnen:

:code:`Skipping a deployment with the pypi provider because this is not a tagged commit`

`✔️Und jetzt mit Tag!`
*********************

Leg einen Tag an, das ist sehr einfach mit git. Die Dokumentation dazu ist hier `hier <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_ zu finden.

Beachte, dass bei :code:`git tag` die Option :code:`-a` erlaubt, die Version festzulegen und die Option :code:`-m` die Nachricht.

Dein Befehl sieht wie folgt aus:

:code:`git tag -a 0.0.1 -m "First pypi deployment"`

Zum Überprüfen :code:`git tag`.
Die Ausgabe sollte wie folgt aussehen:

.. code:: bash

    $ git tag
    v0.0.1

Jetzt push erneut, die Travis CI sollte jetzt deployen!

PSA: Vergiss nicht :code:`--tags` zu deinem Pushbefehl hinzuzufügen, sonst bleiben die Änderungen lokal.

**✔️Deployed!**

`⚠️Anmerkungen`
*****************

✔️ Dein Projekt muss öffentlich sein, um die Travis CI zu nutzen, sonst musst du zu Travis Pro Upgraden.

✔️ Deine Mailadresse muss bei pypi verifiziert sein, um dort hochzuladen. Sonst wird der Upload abgelehnt.

✔️ Dein Tag **MUST** muss in der Form [Zahl.Zahl.Zahl] sein. Lese hier https://docs.openstack.org/pbr/3.1.0/semver.html für weitere Informationen.
.. Bibliographie:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`python setup`: ./setup.py
.. _`config setup`: ./setup.cfg
.. _`Travis home page`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
