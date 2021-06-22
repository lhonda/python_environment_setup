# Python environment setup

Versões de Python

Motivos
-------
* [Pyenv](https://github.com/pyenv/pyenv)
* Python [docker](https://www.docker.com/) container + [docker compose](https://docs.docker.com/compose/)
* [Virtualenv](https://virtualenv.pypa.io/en/latest/) e [Virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/)
* [Pip](https://pip.pypa.io/en/stable/)
* [Pytest](https://docs.pytest.org/en/6.2.x/)
* [Httpie](https://httpie.io/)
* [Ipython](https://ipython.org/)
* [Ipdb](https://github.com/gotcha/ipdb)/[pdbpp](https://github.com/pdbpp/pdbpp)
* [autoenv](https://github.com/inishchith/autoenv)/[direnv](https://github.com/andrewp-as-is/direnv.py)


Python docker container + docker compose
========================================
* Um dos principais objetivos de utilizar [docker](https://www.docker.com/) é a reutilização para evitar surpresas no ambiente de QA/DEV/PROD. Muito mais no python2 do que no 3, existem pacotes que no [ubuntu](https://ubuntu.com/) você consegue instalar sem problemas, porém quando chegam na AWS/Azure e você não está usando uma distro igual a sua máquina, as coisas não funcionam tão bem assim.
* Por temos máquinas com diferentes OS WIN/OSX/LINUX, utilizando docker tudo acaba mais homogêneo e sem pegadinhas.
* Ambiente limpo, dependências externas localmente na minha visão fazem com que o processo para ligar com que é negocio seja mais complicado. Ter N versões de banco sql ou nosql não rola.
* Usar docker não descartar ter um canivete suiço como [PYTEST, IPYTHON, IPDB|PDBPP, HTTPIE] nas mãos. O [pip](https://pip.pypa.io/en/stable/installing/) por default já vem com o `Python 3 >= 3.4`, faz parte da baterias incluidas.

Exemplo de um `DockerFile` para python3:
```shell
FROM python:3.7-stretch
WORKDIR /app
COPY requirements.txt /app/requirements.txt
RUN pip install -r requirements.txt
```

`requirements.txt`
```shell
pytest==3.6.2
ipdb==0.11
httpie==2.4.0
```


Pytest
======

O Pytest é um test runner para Python e tem as seguintes características:

- mensagens de erro detalhada em caso de falha nos testes.
- detecção automática de testes.
- fixtures modulares.
- roda unittest/nose tests.
- funciona com Python3+/Pypy 3.
- arquitetura de plugins com mais de 315 módulos disponibilizados pela comunidade.

O Pytest procura por arquivos nomeados seguindo os padrões test_*.py e *_test.py.

## Fixtures

As fixtures são os passos e dados necessários para rodar um teste. Digamos que uma função a ser testada precisa de determinados parâmetros então podemos definir fixtures que retornam esses parâmetros.

As fixtures podem ser usadas em vários testes e usam o decorador @pytest.fixture.

## Parâmetros

O Pytest fornece vários parâmetros para controle de execução dos testes, tais como o -s que desabilita a captura de stdout/stderr, usado em conjunto com debugger.

Um parâmetro interessante do Pytest é o -k que filtra e executa todos os testes que tenham o argumento passado para o -k.

Exemplo:

```
pytest -k test_login_should_fail_with
```

Este comando vai executar todos os testes que contenham a string test_login_should_fail_with no nome.

## Plugins

Existem centenas de plugins para Pytest (https://docs.pytest.org/en/latest/reference/plugin_list.html) mas vamos mencionar o pytest-cov (https://pypi.org/project/pytest-cov/) que provê code coverage, ferramenta essencial para achar código sem teste unitário.

Para conhecer o Pytest em detalhes, consulte https://docs.pytest.org/.
