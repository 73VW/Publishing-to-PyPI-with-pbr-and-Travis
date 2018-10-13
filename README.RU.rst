`⚙️Автоматизация публикации в PyPI с PBR и Travis`
=========================================================

`✔️Шаг 1: напишите инсталлятор для вашего python проекта`
**********************************************************

Если вы хотите опубликовать свой проект в PyPi, то для начала необходимо создать файл инсталлятора.

Я покажу вам как использовать PBR_ для упрощения данного процесса.

`Setup.py`
----------

Как вы можете заметить, в репозитории есть `скрипт установки`_ с достаточно простым содержанием.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 Вы просто указываете, что хотите использовать PBR_ и всё! 🎉

`Setup.cfg`
-----------
Синтаксис `файла конфигурации`_, находяшегося в репозитории, не сильно сложнее предыдущего файла.

Давайте пройдёмся по каждой секции

Для начала, метаданные:

.. code:: yaml

    # Тип распространяемого пакета
    [bdist_wheel]
    universal=0

    [metadata]
    # Название
    name = Publishing to PyPI with pbr and Travis
    # Кто сделал это?
    author = Maël Pedretti
    # Правда нужно объяснять это?
    author_email = mael.pedretti@he-arc.ch
    # Краткое описание вашего приложения
    summary = Publishing to Pypi with PBR and Travis.
    # Лицензия
    license = MIT
    # В каком файле находится полное описание?
    description-file =
        README.rst
    # Где можно найти проект?
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Какие версии Python поддерживаются?
    python_requires = >=3.6
    # В какие категории можно отнести ваше приложение? https://pypi.python.org/pypi?%3Aaction=list_classifiers
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

    # Автоматически найти корневой пакет
    [options]
    packages = find:

    # Какие файлы данных нужно включить дополнительно?
    [files]
    data_files =
        some/example = some/example/*

    # Как запускается ваше приложение?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 Немного изменений и готово! 🎉

`✔️Step 2: Включаем Travis!`
******************************

Здесь описаны два способа включения Travis. Один с помощью `Travis CLI`_ и один без.

`С помощью travis CLI`
------------------------

Выполните :code:`travis login` и войдите в travis.

Теперь вы можете выполнить :code:`travis init`.

Если текущий каталог является git репозиторием, то Travis автоматически определит это и спросит корректно ли.

Иначе, вы увидите сообщение, что не удалось определить репозиторий.

Как только вы нажмёте :code:`Enter`, Travis попросит указать язык вашего проекта. В нашем случае это :code:`Python`.

Теперь в вашем репозитории появится новый файл :code:`.travis.yml`. Вдобавок, Travis включится для вашего репозитория.

Мы рассмотрим этот файл позже.

`Ручной способ`
-----------------

- Перейдите на `главную страницу Travis`_.
- Зарегистрируйтесь или войдите.
- Перейдите на страницу со своим профилем и синхронизируйте свой аккаунт.
- Теперь ваши публичные Github репозитории отобразятся выше.
- Включите для нужного вам проекта.

`.travis.yml`
-------------

Теперь давайте напишем наш файл настроек.

Документация очень хорошо написана, и я предлагаю вам сначала прочесть её, так как я не буду объяснять всё. Вы можете найти её здесь https://docs.travis-ci.com/user/getting-started/ .

Однако, я объясню настройки, которые я обычно использую.

.. code:: yaml

    # правда нужно объяснять эиу строку?
    language: python

    # вы можете использовать кеш для ускорения сборки
    cache: pip

    # версия Python. Вы можете указать несколько, если хотите тестированть на разных
    python:
      - '3.6'

    # ваш скрипт или список для установки
    install: pip install rstcheck

    # ваш скрипт с тестами
    script: rstcheck --recursive .

    # настройки уведомлений, мне лично не нравится спам в почте
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # интересная часть!
    deploy:
      # Если вам нужно распространять файлы, собранные Travis, то используйте это
      skip_cleanup: true
      # В данном случае мы хотим выложить в pypi
      provider: pypi
      # Какой дистрибутив мы хотим выложить
      distributions: sdist bdist_wheel
      # Когда мы хотим отправлять?
      on:
        # В данном случае я хочу выложить только если указан тег...
        tags: true
        # ... указывающий на ветку master с именем вида "v0.0.0"
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # Ваш логин в pypi
      user: 73VW
      # Ваш пароль в Pypi, зашифрованный с помощью Travis (если вы ставили Travis CLI)
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Используйте эту строку, если у вас нет Travis CLI
      password : ${PYPI_PASSWORD}

`Пароль`
----------

Если вы не устанавливали `Travis CLI`_, используйте второй вариант в примере выше и сделайте следущее:

- Найдите свой проект на странице профиля и нажмите на маленькую шестерёнку ⚙️. Так вы попадёте в настройки.
- Перейдите в раздел :code:`Environment Variables` и добавьте новую переменную.
- Если вы взяли мой пример, то её именем должно быть PYPI_PASSWORD, а значением - ваш пароль.

.. image:: Add_pypi_password.PNG
    :width: 100%
    :alt: Как добавить ваш пароль в Travis

Если у вас установлен `Travis CLI`_, то далее вариант для вас.

- Оставьте секцию password пустой, как ниже.

.. code:: yaml

    user: 73VW
    # Ваш пароль в Pypi
    password:

- Теперь давайте зашифруем его! Просто выполните :code:`travis encrypt --add deploy.password` - Travis спросит ваш пароль, зашифрует его и добавит в файл.

🎉 Теперь всё готово! 🎉

`✔️Итак, что теперь?!`
***********************

Хорошо, давайте попробуем запушить всё в репозиторий и проверим, что всё хорошо и тесты проходят!

Откройте `главную страницу Travis`_ и проверьте, что всё прошло успешно!

Как вы помните, мы не добавили тег, так что данный коммит не запустит сборку дистрибутива.

Travis так же сообщит об этом:

:code:`Skipping a deployment with the pypi provider because this is not a tagged commit`

`✔️Давайте добавим тег!`
*************************

Теперь добавим тег. Это очень просто с git. Документацию по git tag можно найти `здесь <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

Укажу, что для :code:`git tag` параметр :code:`-a` позволяет вам указать версию, а :code:`-m` - сообщение.

Так что ваша команда будет похожа на:

:code:`git tag -a 0.0.1 -m "First pypi deployment"`

Теперь вы можете проверить, что тег создался, выполнив :code:`git tag`.
Вывод будет примерно таким:

.. code:: bash

    $ git tag
    v0.0.1

Теперь запушьте и проверьте Travis и pypi - ваш пакет должен быть выложен!

PSA: Не забудьте добавить :code:`--tags` к вашей команде `git push`, иначе теги останутся только в вашем локальном репозиории.

**✔️Выложено!**

`⚠️Глобальные заметки`
************************

✔️ Чтобы использовать Travis ваш проект должен быть публичным, иначе вам необходимо перейти на Travis pro.

✔️ Ваш email должен быть верифицирован в pypi, чтобы можно было загружать новые проекты, иначе загрузка будет отменена.

✔️Версия вашего тега **ДОЛЖНА** быть в формате [ЧИСЛО.ЧИСЛО.ЧИСЛО]. Больше информации по ссылке: https://docs.openstack.org/pbr/3.1.0/semver.html .

.. Bibliographie:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`скрипт установки`: ./setup.py
.. _`файла конфигурации`: ./setup.cfg
.. _`главную страницу Travis`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
