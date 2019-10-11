`⚙️Automatice su publicación a PyPI con PBR y Travis`
========================================================

Para la documentacion completa:

Dirigirse a `ReadTheDocs <https://automate-your-publishing-to-pypi-with-pbr-and-travis.rtfd.io>`_


`✔️Paso 1: escribir la configuracion de tu proyecto python`
*************************************************************

Si estas buscando publicar tu proyecto en Pypi, primero tenes que crear el archivo de configuración.

En este tutorial, vamos a realizarlo con el uso de PBR_ que nos simplificara el proceso. 

`Setup.py`
----------

Como se puede ver en el archivo `python setup`_ incluido en este repositorio, la sintaxis es bastante básica.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 Simplemente se define que vas a usar PBR_ y eso es todo! 🎉

`Setup.cfg`
-----------

Como se puede ver en el archivo `config setup`_ incluido en este repositorio, la sintaxis no es más complicada que este último archivo.

Veamos cada sección juntos.

Primero de todo, la sección de metadata:
	
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

🎉 Después de algunos ajustes, estamos listos para seguir! 🎉

`✔️Paso 2: Habilitar Travis!`
******************************

Hay dos formas de habilitar Travis, usando `Travis CLI`_ y sin usarlo.

`Usando travis CLI`
-------------------

Ejecuta :code:`travis login` e identifíquese en travis.

Ahora podes ejecutar :code:`travis init`.

Si estas en un repositorio git, Travis va a detectar esto y va a preguntar si es correcto.

De lo contrario, te dirá que puede detectar el repositorio.

Una vez presionado :code:`Enter`, Travis te pregunta el lenguaje principal. En este caso, esribimos :code:`Python`.

Ahora se creó un archivo llamado :code:`.travis.yml` y esta disponible en tu repositorio.
Travis ya esta disponible para este repositorio.

Volveremos a este archivo más tarde.

`Manualmente`
----------------

- Ve a la página de inicio de `Travis home page`_.
- Regístrate o Inicia sesión.
- Ve a tu página de perfil y sincroniza tu cuenta.
- Tus repositorios públicos de Github estarán listados arriba.
- Cambia al proyecto que quieras.


`.travis.yml`
-------------

Ahora vamos a escribir nuestro archivo de configuración.

Como el documento está realmente bien hecho, sugiero que se revise primero, porque no lo voy a explicar todo. Se puede encontrar acá https://docs.travis-ci.com/user/getting-started/.

Sin embargo, voy a explicar la configuración que suelo usar.

.. code:: yaml

    # Es necesario explicar esta línea?
    language: python

    # Se puede usar la cache para realizar el build mas rápido
    cache: pip

    # Versión de python. Se pueden definir más de una si se quieren realizar multiples pruebas.
    python:
      - '3.6'

    # Su script de instalación o su lista de instalación
    install: pip install rstcheck

    # Su script de prueba o tu lista de instalación
    script: rstcheck --recursive .

    # Configuración de notificaciones, a mí personalmente no me gusta que me llegue spam a mi email.
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # La parte interesante!
    deploy:
      # Si necesita implementar archivos que Travis ha creado, use la siguiente línea
      skip_cleanup: true
      # En caso de que queramos hacer la implementacion a pypi
      provider: pypi
      # Distribuciones queremos hacer la desplegar
      distributions: sdist bdist_wheel
      # ¿Cuándo queremos hacer el despliegue?
      on:
        # En este caso solo queremos hacer el despliegue cuando el tag este presente...
        tags: true
        # ... y cuando la etiqueta está sobre la master y respeta la forma "v0.0.0"
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # Usuario de pypi
      user: 73VW
      # Contraseña de Pypi password asegurada por Travis si tienes Travis CLI instalado
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Use lo siguiente si no tiene Travis CLI
      password : ${PYPI_PASSWORD}

`Contraseña`
-------------

Si no tenes `Travis CLI`_ instalado, use la segunda opcion que yo mencione anteriormente y haga lo siguiente:

- En tu página de perfil, busca tu proyecto y presiona en un pequeño engranaje ⚙️. Esto te llevara a la configuración.
- Diríjase a la sección :code:`Environment Variables` y agregar una nueva variable.
- Si tomaste mi ejemplo, el nombre va a ser PYPI_PASSWORD y el valor de la contraseña.

.. image:: ./docs/_static/Add_pypi_password.PNG
    :width: 100%
    :alt: How-to add your password to Travis

Si tenes `Travis CLI`_, esto es para vos.

- Dejar la sesión de password en blanco, como lo siguiente.

.. code:: yaml

    user: 73VW
    # Your Pypi password
    password:

- Vamos a encriptar esto! Simplemente ejecutar :code:`travis encrypt --add deploy.password` y Travis te va a preguntar el password, va a encriptarlo y lo va a colocar en el archivo.

🎉 Ahora estás listo para seguir adelante! 🎉

`✔️Bien, ahora que?!`
**********************

Bueno, ¡intente enviarnos todo al repositorio para comprobar si todo está bien y si las pruebas pasan!

Vamos a `Travis home page`_ y comprobamos que esté todo bien!

Como recordarás, no configuramos ningun tag en github, por lo que este commit no se debe desplegar.

Travis tambien nos va a decir esto:

:code:`Skipping a deployment with the pypi provider because this is not a tagged commit`

`✔️Vamos a taggear esto!`
**************************

Ahora, creamos un tag. Esto es muy fácil con git, la documentación puede encontarse `aca <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

Tener en cuenta que :code:`git tag` la opcion :code:`-a` te permite especificar la versión y :code:`-m` el mensaje.

Entonces el comando debería ser el siguiente:

:code:`git tag -a 0.0.1 -m "Primer implementacion pypi"`

Ahora se puede comprobar si este fue creado corriendo :code:`git tag`.
El resultado debería parecerse a lo siguiente:

.. code:: bash

    $ git tag
    v0.0.1

Ahora vamos a realizar un push y comprobar nuevamente que Travis, Pypy y tu paquete estén desplegados.

Observación : No olvidarse de agregar :code:`--tags` para realizar el push, sino esto solo quedara en tu repositorio local.

**✔️Implementando!**

`⚠️Notas globales`
********************

✔️ Tu proyecto debe ser público para poder utilizar Travis. De lo contrario tendrás que actualizar a Travis Pro.

✔️ Tu dirección de correo electrónico debe ser verificada en Pypi para poder cargar un nuevo proyecto. De lo contrario se rechazará la subida.

✔️ Tu versión de etiqueta **DEBE** tener el formato [DIGITO.DIGITO.DIGITO]. Consulte https://docs.openstack.org/pbr/3.1.0/semver.html para más información.

.. Bibliografia:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`python setup`: ./setup.py
.. _`config setup`: ./setup.cfg
.. _`Travis home page`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
