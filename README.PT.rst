`⚙️Automate your publishing to PyPI with PBR and Travis`
==========================================================

Para a documentação completa:

Vá para `ReadTheDocs <https://publishing-to-pypi-with-pbr-and-travis.readthedocs.io>`_


`✔️Passo 1: escreva o setup para seu projeto python`
******************************************************

Se vbocÊ quiser publicar seu projeto no PyPi, primeiro você deve fazer um arquivo setup.

Nesse tutotirla, eu vou cobrir o use do PBR_ que simplifica o processo.

`Setup.py`
----------

Como você pode ver no arquivo `python setup`_ incluso neste repositório, a sintaxe é bem básica.

.. code:: python

    """Setup example."""

    from setuptools import setup


    setup(
        setup_requires=['pbr'],
        pbr=True
    )

🎉 You simplify define that you want to use PBR_ and that's it! 🎉

`Setup.cfg`
-----------

Como vocÊ pode ver no arquivo `config setup`_ incluso neste repositório, a sintaxe não é mais complicada do que a do último arquivo.

Vamos passar por cada seção juntos.

Primeiro de tudo, a seção de metadata:

.. code:: yaml

    # Tipo da distribuição Python
    [bdist_wheel]
    universal=0

    [metadata]
    # Nome do app
    name = Publishing to PyPI with pbr and Travis
    # Quem fez o app?
    author = Maël Pedretti
    # Eu realmente preciso explicar?
    author_email = mael.pedretti@he-arc.ch
    # Uma pequena descrição do seu app
    summary = Publishing to Pypi with PBR and Travis.
    # Tipo de licença
    license = MIT
    # Que arquivo contém a descrição longa?
    description-file =
        README.rst
    # Onde posso acessar o projeto?
    home-page = https://github.com/73VW/Publishing-to-PyPI-with-pbr-and-Travis
    # Qual versão do Python é necessária para rodar?
    python_requires = >=3.6
    # Como você classifica seu aplicativo? https://pypi.python.org/pypi?%3Aaction=list_classifiers
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

    # Encontrar automaticamente pacote root
    [options]
    packages = find:

    # Quais arquivos que não são códigos-fonte vocÊ quer fazer deploy?
    [files]
    data_files =
        some/example = some/example/*

    # Onde seu app inicia?
    [entry_points]
    console_scripts =
        automabot = your_package.__main__:main

🎉 Depois de um pouco de ajustes você está pronto para seguir! 🎉

`✔️Passo 2: Habilitar Travis!`
******************************

Dois modos de habilitar Travis estão presentes aqui. Um usando  `Travis CLI`_ e outro sem ele.

`Usando travis CLI`
---------------------

Run :code:`travis login` and login to travis.

Agora vocÊ pode rodar :code:`travis init`.

Se você está usando um repositório git, Travis vai detectá-lo e perguntar se está correto.

Caso contrário, ele irá dizer que pode detectar o repositório.

Após você enviar :code:`Enter`, Travis pergunta qual a linguagem principal. Nesse caso, digite :code:`Python`.

Agora um novo arquivo chamado :code:`.travis.yml` foi criado e está disponível no seu repositório. E Travis está habilitado neste repositório.

Vamos passar por esse arquivo mais tarde.

`Manualmente`
----------------

- Vá para `Travis home page`_.
- Logue-se ou Registre-se.
- Vá para sua página de perfil e sincronize sua conta.
- Seu repositório público de Github estão agora listados abaixo.
- Selecione o projeto que deseja.

`.travis.yml`
-----------------

Agora vamos escrever nosso arquivo de configuração.

Como a documentação é realmente bem feita, eu sugiro que você cheque ela primeiro, já que não explicarei tudo. Você pode encontrá-la aqui https://docs.travis-ci.com/user/getting-started/.

Entretanto, vou explicar as configurações que geralmente uso.

.. code:: yaml

    # Realmente preciso explicar essa linha?
    language: python

    # Você pode usar cache para um build mais rápido
    cache: pip

    # Versão do python. Você pode definir mais do que uma se você quiser fazer múltiplos testes
    python:
      - '3.6'

    # Seus script de instalação ou sua lista de instalação
    install: pip install rstcheck

    # Seu script de test ou sua lista de instalação
    script: rstcheck --recursive .

    # Configurações para notificações, Eu pessoalmente não gosto de receber spams no e-mail
    notifications:
      email:
        on_failure: never
        on_pull_requests: never

    # A parte interessante!
    deploy:
      # Se você precisar fazer o deploy de arquivos que o Travis construium use a próxima linha
      skip_cleanup: true
      # Nesse caso nós queremos fazer deploy no pypi
      provider: pypi
      # Que distribuição nós queremos fazer deploy
      distributions: sdist bdist_wheel
      # Quando queremos fazer o deploy?
      on:
        # Nesse caso eu quero fazer deploy apenas quando a tag é "presente"...
        tags: true
        # ... e quando a tag está no master e respeita a forma "v0.0.0"
        branch:
          - master
          - /v?(\d+\.)?(\d+\.)?(\*|\d+)$/
      # Seu nome de usuário pypi
      user: 73VW
      # Sua senha pypi assegurada pelo Travis se você tiver o Travis CLI instalado
      password:
        secure: cGJz+vETnxwWAZQvzveJKOyn3rWy3/tcVmJvTVuflrgKgwMRm+sfQZB3vo39LzDcDbMzlzxLO4SUsqDpCxlPPM1pCjqHeUkke76pXA3HGTqfSS5VBic979pBDBqzFe8SLxery0ND7uPAam2xtZQcMRjIzMZFS+ZBD3tD9pWFnFqQOaw6Mwnfj2dWuA7BeNEBEeG+EErAJTqWHlwodjLsDBBilrvYEMPha049JWSz9TE1SMUKWZszCpo2hda8edvcB7WrNWJCYO+Pmc56aUHGlqiyRUowec9ZQplhmD7HWriRvda4n+1WqUB8tdACqBSBo6t39dis/yiLDv/qZpi6cooxJBtlK184AZvCIfjiu8ua5JqJ/SBghzrwLf7b5VbWg/WOtS8NEB+TYhZhpmkYLPXnOoJLYbbrOYA/sz/QfwXke2NCTp7apZFAtU1lFN2gVWsmff7ysRWwwHW/iidCAcu9BXlwMt2x2dv5PqSSqN1QdwCQ+cGcewlIPInHwCpXwI4sJXPEHeax0J5c206Yf4PMkzgrUj1+UmpB2AKJkMF0+kGd+MOj9SXYbNE1Lc456CuvKUflVry12mVQCgqqL6lZQadQ+aNKy0LoK4o4CN6JTUMpIn6JIOapLc9hzOGZgVuFzZ5YAs6l8VraMzZuAzOEv79UB92B3Iq2Vxki8vo=
      # Use o seguinte se não tiver Travis CLI
      password : ${PYPI_PASSWORD}

`Senha`
----------

Se você não tiver `Travis CLI`_ instalado, use a segunda opção que mencionei acima e faça o seguinte:

- Na sua página de perfil, encontre seu projeto e clique na pequena engrenagem ⚙️. Isso vai abrir as configurações
- Vá para a seção :code:`Environment Variables` e adicione uma nova variável
- Se você pegar o meu exemplo, o nome da váriavel vai ser  PYPI_PASSWORD e o valor vai ser sua senha

.. image:: ./docs/_static/Add_pypi_password.PNG
    :width: 100%
    :alt: Como adicionar sua senha ao Travis

Se você tem `Travis CLI`_, este é pra você.

- Deixe em branco a seção de senha, como abaixo.

.. code:: yaml

    user: 73VW
    # Sua senha Pypi
    password:

- Agora vamos encriptá-la! Simplesmente digite :code:`travis encrypt --add deploy.password` e Travis vai perguntar sua senha, encriptá-lo e colar no arquivo.

🎉 Agora você está pronto para seguir! 🎉

`✔️E agora?!`
******************

Bem, vamos tentar subir tudo para o repositório para checar se tudo está certo e se os testes passam!

Vá para `Travis home page`_ e cheque se tudo correu bem!

Como você se lembra, nós não configuramos nenhuma tag no github então esse commit não deveria ser "deployado".

Travis também vai te dizer isso:

:code:`Skipping a deployment with the pypi provider because this is not a tagged commit`

`✔️Vmos colocar uma tag!`
****************************

Agora, crie uma tag. Isso é muito fácil com o git. A documentação das Tags Git podem ser encontradas  `aqui <https://git-scm.com/book/en/v2/Git-Basics-Tagging>`_.

Note que com  :code:`git tag` a opção :code:`-a` permite que você especifique a versão e :code:`-m` a mensagem.

Então seu comando vai ser o seguinte:

:code:`git tag -a 0.0.1 -m "First pypi deployment"`

Agora você pode checar se ela foi criada com o comando :code:`git tag`.
O resultado deve parecer com o seguinte.

.. code:: bash

    $ git tag
    v0.0.1

E agora use o comando push e cheque Travis de novo e então pypi e seu pacote deverão ser deployados!

Obs: Não se esqueça de adicionar :code:`--tags` ao seu comando push ou eles vão ficar no seu repositório local.

**✔️Deployed!**

`⚠️Notas Globais`
*****************

✔️ Seu projeto precisa ser público para usar Travis. Caso contrário, você vai ter que usar Travis pro.

✔️ Seu endereço de e-mail precisa ser verificado no pypi para fazer o upload de um novo projeto. Caso contrário o upload vai ser rejeitado.

✔️Sua versão de tag  **PRECISA** estar no formato [DIGITO.DIGITO.DIGITO]. Cheque https://docs.openstack.org/pbr/3.1.0/semver.html for more infos.

.. Bibliografia:

.. _PBR: https://docs.openstack.org/pbr/latest/index.html

.. _`python setup`: ./setup.py
.. _`config setup`: ./setup.cfg
.. _`Travis home page`: https://travis-ci.org
.. _`Travis CLI`: https://github.com/travis-ci/travis.rb
