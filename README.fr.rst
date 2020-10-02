`⚙️Automatisez votre publication vers PyPI avec PBR et Travis`
=========================================================

Pour la documentation complète:

Allez sur `ReadTheDocs <https://automate-your-publishing-to-pypi-with-pbr-and-travis.rtfd.io>`_


`✔️Étape 1: écrire le setup de votre projet Python`
***************************************************

Si vous voulez publier votre projet Python sur PyPi, vous devrez créer un fichier de setup.

Dans ce tutoriel, je vous montrerai l’utilité de PBR_ qui devrait simplifier le processus.

`Setup.py`
----------

Comme vous le voyez dans le fichier `python setup`_ inclus dans le dépôt, la syntaxe est basique.

.. code:: python

    """Example de setup."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 Vous spécifiez simplement que vous utilisez PBR_ et c’est bon! 🎉

`Setup.cfg`
-----------

Comme vous pouvez le voir dans le fichier `config setup`_ inclus aussi dans le dépôt, la syntaxe n’est pas beaucoup plus compliquée que pour l’autre fichier.

Allons voir chaque section ensemble.

Pour commencer, la section des métadonnées: 

.. code:: yaml

    # Type de distribution Python
    [bdist_wheel]
    universal=0

    [metadata]
    # Nom de l‘application
    name = Publishing to PyPI with pbr and Travis
    # Qui l‘a fait? 
    author = Maël Pedretti
    # Ai-je vraiment besoin d’expliquer ce qui vient?
    author_email = mael.pedretti@he-arc.ch
    # Une description courte de votre application
    summary = Publishing to Pypi with PBR and Travis.
    # Type de License
    license = MIT
    # Quel fichier contient la vraie description?
    description-file =
        README.rst
    # Page d’accès au projet
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Quelle version de Python a t’elle besoin pour fonctionner?
    python_requires = >=3.6
    # Comment classifieriez-vous votre application? https://pypi.python.org/pypi?%3Aaction=list_classifiers
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

    # Trouver le paquet root automatiquement
    [options]
    packages = find:

    # Quels fichiers qui ne sont pas du code source voulez-vous déployer?
    [files]
    data_files =
        some/example = some/example/*

    # Quel fichier est le point d'entrée de votre application?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 Après quelques ajustement vous serez prêt! 🎉

`✔️Étape 2: Activer Travis!`
****************************

Il existe deux moyens d'activer Travis. Un en utilisant `Travis CLI`_ et sans.

`En utilisant Travis CLI`
-------------------

Lancez :code:`travis login` et connectez-vous à Travis.

Maintenant vous pouvez lancer :code:`travis init`.

Si vous êtes dans un dépôt GitHub, Travis le détectera automatiquement et vous demandera si c'est le cas.

Sinon, il vous dira qu'il ne peut pas détecter de dépôt.

Une fois que vous appuyez sur :code:`Enter`, Travis vous demandera votre langage principal. Dans notre cas, tapez :code:`Python`.

Maintenant un nouveau fichier appelé :code:`.travis.yml` a été créé et est disponible dans votre dépôt. Travis est maintenant activé pour ce dépôt.

Nous irons voir ce fichier après.

`Manuellement`
----------

- Allez sur la `Page d'accueil`_.
- Connectez-vous ou créez un compte.
- Allez sur votre profil et connectez-vous avec votre compte GitHub.
- Vos dépôts GitHub public sont maintenant listés au-dessus.
- Allez sur le projet que vous voulez.

`.travis.yml`
-------------

Maintenant allons écrire notre fichier de réglages.

Comme la documentation est vraiment bien faite, je suggérerai d'aller d'abord y jeter un coup d'œil  comme je ne vais pas tout expliquer. Vous pouvez la trouver ici : https://docs.travis-ci.com/user/getting-started/.

Je vais en revanche vous montrer les réglages que j'utilise couramment:

.. code:: yaml

    # J'ai vraiment besoin d'expliquer cette ligne?
    language: python

    # Vous pouvez utiliser un cacher pour compiler plus rapidement
    cache: pip

    # la version de python. Vous pouvez en définir plusieurs si vous voulez faire plusieurs tests.
    python:
      - '3.6'

    # votre script d'installation ou votre liste d'installation
    install: pip install rstcheck

    # votre script de tests ou votre liste d'installation
    script: rstcheck --recursive .

    # les réglages pour les notifications, je préfère personnellement ne pas être spammer de mails.
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # la partie intéressante
    deploy:
      # Si vous avez besoin de déployer un fichier que Travis a compilé, utilisez la prochaine ligne
      skip_cleanup: true
      # Dans le cas où vous voudriez déployer le programme sur pypi
      provider: pypi
      # Quelle distribution vous voudriez déployer
      distributions: sdist bdist_wheel
      # Quand voulez vous déployer?
      on:
        # Dans ce cas je veux déployer seulement quand un tag est présent...
        tags: true
        # ... et quand le tag est sur le master et respecte la forme "v0.0.0"
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # Votre nom d'utilisateur pypi
      user: 73VW
      # Votre mot de passe Pypi sécurisé par Travis si vous avez installé Travis CLI
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Utilisé ça si vous n'avez pas Travis CLI
      password : ${PYPI_PASSWORD}

`Mot de passe`
----------

Si vous n'avez pas installé `Travis CLI`_, utilisé la seconde option mentionnée en haut et faites ce qui suit :

- Sur votre page de profil, trouvez votre projet et cliquez sur la roue dentée ⚙️. Cela va vous emmener vers les réglages.
- Allez vers la section :code:`Environment Variables` et ajoutez une nouvelle variable.
- Si vous prenez mon exemple, son nom devra être PYPI_PASSWORD et sa valeur devra être votre mot de passe.

.. image:: ./docs/_static/Add_pypi_password.PNG
    :width: 100%
    :alt: Comment ajouter votre mot de passe à Travis

Si vous avez `Travis CLI`_, la section suivante est pour vous.

- Laissez un vide dans la section mot de passe comme ce qui suit:

.. code:: yaml

    user: 73VW
    # Votre mot de passe Pypi
    password:

- Maintenant allons l'encrypter! Lancez simplement :code:`travis encrypt --add deploy.passwor`, Travis vous demandera votre mot de passe, l'encryptera et le mettra dans le fichier.

🎉 Vous êtes maintenant fin prêt! 🎉

`✔️Alors qu'est-ce qu'on fait maintenant?!`
******************************************

Eh bien, essayons de mettre quelque chose sur le dépôt pour voir si tout marche correctement et si les tests se font!

Allez sur la `Page d'accueil`_ et regardez si tout fonctionne bien!

Comme vous pouvez le remarquer, nous n'avons pas mis de tag dans GitHub donc ça ne devrait pas être déployé.

Travis vous le dira aussi:

:code:`Skipping a deployment with the pypi provider because this is not a tagged commit`

`✔️Mettons un tag dessus!`
**************************

Maintenant, créons un tag. C'est vraiment simple avec git. La documentation pour ça peut se trouver `here <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

Notez que avec :code:`git tag` l'option :code:`-a` vous autorise à spécifier une version et :code:`-m` le message.

Donc votre commande ressemblera à ce qui suit:

:code:`git tag -a 0.0.1 -m "First pypi deployment"`

Maintenant vous pouvez vérifier si ce que vous avez créez fonctionne en lançant :code:`git tag`.
Le résultat devrait ressembler à ça:

.. code:: bash

    $ git tag
    v0.0.1

Maintenant envoyez le de nouveau et regardez Travis et pypi, votre paquet devrait être déployé!

PS: N'oubliez pas d'ajouter :code:`--tags` à votre commande de push ou ça va rester dans votre dépôt local.

**✔️Déployé!**

`⚠️Notes globales`
*****************

✔️ Votre projet doit être public pour pouvoir utiliser Travis. Sinon vous devrez acheter Travis pro.

✔️ Votre adresse email devra être vérifié sur pypi pour pouvoir envoyer votre nouveau projet. Sinon il sera rejeté.

✔️ Votre version **DOIT** être de la forme [DIGIT.DIGIT.DIGIT]. Regardez sur https://docs.openstack.org/pbr/3.1.0/semver.html pour plus d'informations.

.. Bibliographie:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`python setup`: ./setup.py
.. _`config setup`: ./setup.cfg
.. _`Page d'accueil`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
