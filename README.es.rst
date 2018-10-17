`⚙️Automate your publishing to PyPI with PBR and Travis`
=========================================================

Para la documentacion completa:

Dirigirse a `ReadTheDocs <https://automate-your-publishing-to-pypi-with-pbr-and-travis.rtfd.io>`_


`✔️Paso 1: escribir la configuracion de tu proyecto python`
***************************************************

Si estas buscando publicar tu proyecto en Pypi, primero tenes que crear el archivo de configuracion

En este tutorial, vamos a realizarlo con el uso de PBR_ que nos simplifcara el proceso. 

`Setup.py`
----------

Como se puede ver en el archivo `python setup`_ incluido en este repositorio, la sintaxis es bastante basica.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 Simplemente define que vas a usar PBR_ y eso es todo! 🎉

`Setup.cfg`
-----------

Como se puede ver en `config setup`_ archivo incluido en este repositorio, la sintaxis no es mas complicada que este ultimo archivo.

Veamos cada seccion juntos.

Primero de todo, la seccion de metadata:

.. code:: yaml

    # Tipo de distribucion de python
    [bdist_wheel]
    universal=0

    [metadata]
    # Nombre de la aplicacion
    name = Publishing to PyPI with pbr and Travis
    # Quien hizo esto?
    author = Maël Pedretti
    # Es necesario explicar lo siguiente?
    author_email = mael.pedretti@he-arc.ch
    # Una descripcion corta de la aplicacion
    summary = Publishing to Pypi with PBR and Travis.
    # Tipo de licencia
    license = MIT
    # Archivo que va a contener una descripcion larga
    description-file =
        README.rst
    # Donde puedo tener acceso al proyecto?
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Que version de python necesito para correr la aplicacion?
    python_requires = >=3.6
    # Como se clasifica tu aplicacion? https://pypi.python.org/pypi?%3Aaction=list_classifiers
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

    # Automaticamente busca el paquete raiz
    [options]
    packages = find:

    # Que archivos no son del codigo fuente que deseas implementar?
    [files]
    data_files =
        some/example = some/example/*

    # Donde empieza la aplicacion?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 Despues de algunos ajustes, estamos listos para seguir! 🎉

`✔️Paso 2: Habilitar Travis!`
***************************

Dos formas de habilitar Travis, Una usando `Travis CLI`_ y otra sin.

`Usando travis CLI`
-------------------

Ejecute :code:`travis login` y logueese en travis.

Ahora podes ejecutar :code:`travis init`.

Si estas en un repositorio git, Travis va a detectar esto y te va a preguntar si esta correcto.

De todas maneras, te va a decir si podes detectar el repositorio.

Una vez presionado :code:`Enter`, Travis te pregunta el lenguaje principal. En este caso, escriba :code:`Python`.

Ahora se creo un archivo llamado :code:`.travis.yml` y esta disponible en tu repositorio.
Travis esta disponible ahora para este repo.

Vamos a volver por este archivo mas tarde.

`Manualmente`
----------

- Ir a la página de inicio de `Travis`_.
- Regístrate o Inicia sesión.
- Ir a tu página de perfil y sincronizar tu cuenta.
- Tus repositorios públicos de Github ahora están listados arriba.
- Cambia al proyecto que quieras.


`.travis.yml`
-------------

Ahora vamos a escribir nuestro archivo de configuración.

Como el documento está realmente bien hecho, sugiero que se revise primero, porque no lo voy a explicar todo. Se puede encontrar aca https://docs.travis-ci.com/user/getting-started/.

Sin embargo, voy a explicar la configuración que suelo usar.

.. code:: yaml

    # Es necesario explicar esta linea?
    language: python

    # Se puede usar la cache para realizar el build mas rapido
    cache: pip

    # version de python. Se pueden definir mas de uno si se quiere realizar multiples pruebas.
    python:
      - '3.6'

    # your install script or your install list
    install: pip install rstcheck

    # your test script or your install list
    script: rstcheck --recursive .

    # configurando las notificaciones , a mi personalmente no me gusta que me llegue spam a mi email.
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # la parte interesante!
    deploy:
      # If you need to deploy files Travis has built, use the next line
      skip_cleanup: true
      # In this case we want to deploy to pypi
      provider: pypi
      # What distribution we want to deploy
      distributions: sdist bdist_wheel
      # When do we want to deploy?
      on:
        # In this case I want to deploy only when a tag is present...
        tags: true
        # ... and when tag is on master and respects the form "v0.0.0"
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # Your pypi username
      user: 73VW
      # Your Pypi password secured by Travis if you have Travis CLI installed
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Use the following if you don't have Travis CLI
      password : ${PYPI_PASSWORD}

`Password`
----------

If you don't have `Travis CLI`_ installed, use the second option I mentioned above and do the following:

- In your profile page, find your project and click on the little gear ⚙️. This will bring you to the settings.
- Go to the :code:`Environment Variables` section and add a new variable.
- If you take my example, its name will be PYPI_PASSWORD and the value your password.

.. image:: ./docs/_static/Add_pypi_password.PNG
    :width: 100%
    :alt: How-to add your password to Travis

If you have `Travis CLI`_, this one is for you.

- Leave blank the password section, like the following.

.. code:: yaml

    user: 73VW
    # Your Pypi password
    password:

- Now let's encrypt it! Simply run :code:`travis encrypt --add deploy.password` and Travis will ask for your password, encrypt it and paste it to the file.

🎉 Now your are ready to go! 🎉

`✔️Bien, ahora que?!`
******************

Bueno, ¡intentemos enviar todo al repositorio para comprobar si todo está bien y si las pruebas pasan!

Vamos a `Travis home page`_ y chequeamos que este todo bien!

Como recordarás, no configuramos ningun tag en github, por lo que este commit no se debe implementar.

Travis tambien nos va a decir esto:

:code:`Salteando la implementacion porque esto no es tagged commit`

`✔️Vamos a taggear esto!`
******************

Ahora, creamos un tag. Esto es muy facil con git, la documentacion puede encontarse aca: <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

Tener en cuenta que con :code:`git tag` la opcion :code:`-a` te permite especificar la version y :code:`-m` el mensaje.

Entonces el comando deberia ser el siguiente:

:code:`git tag -a 0.0.1 -m "Primer implementacion pypi"`

Ahora se puede chequear si este fue creado corriendo :code:`git tag`.
El resultado deberia parecerse a lo siguiente:

.. code:: bash

    $ git tag
    v0.0.1

Ahora vamos a realizar un push y chequear nuevamente que Travis y Pypy y tu paquete deberian ser implementados.

PSA: No olvidarse de agregar :code:`--tags` para realizar el push, sino esto solo quedara en tu repositorio local.

**✔️Implementando!**

`⚠️Notas globales`
*****************

✔️ Tu proyecto debe ser público para poder utilizar Travis. De lo contrario teenes que actualizar a Travis pro.

✔️ Tu dirección de correo electrónico debe ser verificada en Pypi para poder cargar un nuevo proyecto. De lo contrario se rechazará la subida.

✔️ Tu versión de etiqueta ** DEBE ** tener el formato [DIGITO.DIGITO.DIGITO]. Consulte https://docs.openstack.org/pbr/3.1.0/semver.html para obtener más información.

.. Bibliografia:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`python setup`: ./setup.py
.. _`config setup`: ./setup.cfg
.. _`Travis home page`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
