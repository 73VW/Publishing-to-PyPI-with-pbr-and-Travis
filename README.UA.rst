`⚙️Автоматизація публікації в PyPI с PBR и Travis`
=========================================================

`✔️Крок 1: напишіть інсталятор для вашого python проекту`
**********************************************************

Якщо ви хочете опублікувати свій проект в PyPi, то для початку необхідно створити файл інсталятор.

Я покажу вам як використовувати PBR_ для спрощення даного процесу.

`Setup.py`
----------

Як ви можете помітити, в репозиторії є `скрипт установки`_ з достатньо простим змістом.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 Ви просто вказуєте, що хочете використовувати PBR_ і все! 🎉

`Setup.cfg`
-----------
Синтаксис `файлу конфігурації`_, що знаходиться в репозиторії, не набагато складніше попереднього файлу.

Давайте пройдемося по кожній секції

Для початку, метадані:

.. code:: yaml

    # Тип розповсюджуваного пакету
    [bdist_wheel]
    universal=0

    [metadata]
    # Назва
    name = Publishing to PyPI with pbr and Travis
    # Хто зробив це?
    author = Maël Pedretti
    # Правда потрібно пояснювати це?
    author_email = mael.pedretti@he-arc.ch
    # Короткий опис вашого додатку
    summary = Publishing to Pypi with PBR and Travis.
    # Ліцензія
    license = MIT
    # В якому файлі знаходиться повний опис?
    description-file =
        README.rst
    # Де можна знайти проект?
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Які версії Python підтримуються?
    python_requires = >=3.6
    # В які категорії можна віднести ваш додаток? https://pypi.python.org/pypi?%3Aaction=list_classifiers
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

    # Автоматично знаходити кореневий пакет
    [options]
    packages = find:

    # Які файли даних треба включити додатково?
    [files]
    data_files =
        some/example = some/example/*

    # Як запускається ваш додаток?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 Трошки змін і готово! 🎉

`✔️Step 2: Вмикаємо Travis!`
******************************

Тут описані два способи увімкнення Travis. Один за допомогою `Travis CLI`_ і один без.

`За допомогою travis CLI`
------------------------

Виконайте :code:`travis login` і увійдіть в travis.

Тепер ви можете виконати :code:`travis init`.

Якщо поточний каталог є git репозиторієм, то Travis автоматично визначить це і спитає, чи коректно це.

Інакше, ви побачите повідомлення, що не вдалося визначити репозиторій.

Як тільки ви натиснете :code:`Enter`, Travis попросить вказати мову вашого проекту. В нашому випадку це :code:`Python`.

Тепер у вашому репозиторії з'явиться новий файл :code:`.travis.yml`. В додаток, Travis увімкнеться для вашого репозиторію.

Ми розглянемо цей файл пізніше.

`Ручний спосіб`
-----------------

- Перейдіть на `головну сторінку Travis`_.
- Зареєструйтесь або увійдіть.
- Перейдіть на сторінку зі своїм профілем і синхронізуйте свій акаунт.
- Теперь ваші публічні Github репозиторії відобразяться вище.
- Увімкніть для потрібного вам проекту.

`.travis.yml`
-------------

Теперь давайте напишемо наш файл налаштувань.

Документація дуже добре написана, і я пропоную вам спочатку прочитати її, так як я не буду пояснювати все. Ви можете знайти її тут https://docs.travis-ci.com/user/getting-started/ .

Однак, я поясню налаштуваня, які я зазвичай використовую.

.. code:: yaml

    # правда треба пояснювати цей рядок?
    language: python

    # ви можете використовувати кеш для прискорення збірки
    cache: pip

    # версія Python. Ви можете вказати декілька, якщо хочете тестувати на різних
    python:
      - '3.6'

    # ваш скрипт або список для установки
    install: pip install rstcheck

    # ваш скрипт з тестами
    script: rstcheck --recursive .

    # налаштуваня сповіщень, мені особисто не подобається спам в пошті
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # цікава частина!
    deploy:
      # Якщо вам потрібно розповсюджувати файли, зібрані Travis, то використовуйте це
      skip_cleanup: true
      # В даному випадку ми хочемо викласти в pypi
      provider: pypi
      # Який дистрибутив ми хочемо викласти
      distributions: sdist bdist_wheel
      # Коли ми хочемо відправляти?
      on:
        # В даному випадку я хочу викласти, тільки якщо вказаний тег...
        tags: true
        # ... що вказує на гілку master з назвою виду "v0.0.0"
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # Ваш логін в pypi
      user: 73VW
      # Ваш пароль в Pypi, зашифрований за допомогою Travis (якщо ви ставили Travis CLI)
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Використовуйте цей рядрк, якщо у вас немає Travis CLI
      password : ${PYPI_PASSWORD}

`Пароль`
----------

Якщо вы не встановлювали `Travis CLI`_, використовуйте другий варіант у прикладі вище і зробіть наступне:

- Знайдіть свій проект на сторінці профілю і натисніть на маленьку шестерню ⚙️. Так ви потрапите в налаштування.
- Перейдіть в розділ :code:`Environment Variables` і додайте нову змінну.
- Якщо ви взяли мій приклад, то її назвою має бути PYPI_PASSWORD, а значенням - ваш пароль.

.. image:: Add_pypi_password.PNG
    :width: 100%
    :alt: Як додати ваш пароль в Travis

Если у вас встановлений `Travis CLI`_, то далі варіант для вас.

- Залиште поле password пустою, як нижче.

.. code:: yaml

    user: 73VW
    # Ваш пароль в Pypi
    password:

- Тепер давайте зашифруємо його! Просто виконайте :code:`travis encrypt --add deploy.password` - Travis спитає ваш пароль, зашифрує його і додасть в файл.

🎉 Тепер все готово! 🎉

`Отже, що тепер?!`
***********************

Хорошо, давайте спробуємо запушити все в репозиторій і перевіримо, чи всё добре і тести проходять!

Відкрийте `головну сторінку Travis`_ и перевірте, чи все пройшло успішно!

Как ви пам'ятаєте, ми не додали тег, так що даний комміт не запустить збірку дистрибутиву.

Travis також повідомить про це:

:code:`Skipping a deployment with the pypi provider because this is not a tagged commit`

`✔️Давайте додамо тег!`
*************************

Тепер додамо тег. Це дуже просто з git. Документацію по git tag можна знайти `тут <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

Зазначу, що для :code:`git tag` параметр :code:`-a` дозволяє вам вказати версію, а :code:`-m` - повідомлення.

Тож ваша команда буде схожа на:

:code:`git tag -a 0.0.1 -m "First pypi deployment"`

Тепер ви можете перевірити, чи створився тег, виконавши :code:`git tag`.
Вивід буде приблизно таким:

.. code:: bash

    $ git tag
    v0.0.1

Тепер запуште и перевірте Travis і pypi - ваш пакет повинен бути викладений!

PSA: Не забудьте додати :code:`--tags` до вашої команди `git push`, інакше теги залишуться тілько у вашому локальному репозиторію.

**✔️Викладено!**

`⚠️Глобальні нотатки`
************************

✔️ Щоб використовувати Travis, ваш проект повинен бути публічним, інакше вам необхідно перейти на Travis pro.

✔️ Ваш email повинен бути верифікованим в pypi, щоб можно було завантажувати нові проекти, інакше завантаження буде скасовано.

✔️Версія вашого тегу **ПОВИННА** бути у форматі [ЧИСЛО.ЧИСЛО.ЧИСЛО]. Більше інформації за посиланням: https://docs.openstack.org/pbr/3.1.0/semver.html .

.. Bibliographie:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`скрипт установки`: ./setup.py
.. _`файлу конфігурації`: ./setup.cfg
.. _`головну сторінку Travis`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
