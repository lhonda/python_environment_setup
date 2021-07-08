# Python environment setup

Este documento ilustra alguma ferramentas que facilitam o uso do Python no seu projeto.

## Ferramentas

* [Pyenv](https://github.com/pyenv/pyenv)
* Python [docker](https://www.docker.com/) container + [docker compose](https://docs.docker.com/compose/)
* [Virtualenv](https://virtualenv.pypa.io/en/latest/) e [Virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/)
* [Pip](https://pip.pypa.io/en/stable/)
* [Pytest](https://docs.pytest.org/en/6.2.x/)
* [Httpie](https://httpie.io/)
* [Ipython](https://ipython.org/)
* [Ipdb](https://github.com/gotcha/ipdb)/[pdbpp](https://github.com/pdbpp/pdbpp)
* [autoenv](https://github.com/inishchith/autoenv)/[direnv](https://direnv.net/)


Pyenv
=====

O Pyenv é uma ferramenta que gerencia a instalação de versões do Python.

Desta forma você pode usufruir de diferentes versões do Python sem mexer na instalação padrão do Python do seu sistema operacional.

O Pyenv permite que seja elegido uma versão global de Python e versões locais por diretório, ideal para uso em projetos.

Para tanto, basta instalar, configurar o Pyenv, executar um novo shell e instalar a versão de Python requerida.

O truque que o Pyenv usa é baseado na manipulação da variável de ambiente PATH, de modo que o ao digitar `python` no shell, o primeiro caminho que encontra o comando python é o caminho definido pelo Pyenv.

Caso queira usar o Python do sistema, basta usar o caminho completo:

`/usr/bin/python`


Virtualenv
==========

O virtualenv provisiona uma instalação isolada da versão de Python corrente. O objetivo é usar esta versão isolada do Python para o seu projeto de forma que qualquer dependência seja contida nesta instalação.

Podemos criar um virtualenv da seguinte forma:

`virtualenv venv`

ou

`python -m venv venv`

Em ambos os casos um diretório venv será criado no diretório corrente e no diretório venv/bin temos os scripts para ativar o virtualenv, o python e pip desse virtualenv.

Para ativar o virtualenv basta executar:

`source venv/bin/activate` 

Para desativar o virtualenv:

`deactivate`

Ao ativar um virtualenv a variável de ambiente PS1 é alterada, identificando o nome do virtualenv ativado.

Uma dica é configurar algum powerline como o https://github.com/justjanne/powerline-go para ajudar na identificação do virtualenv, branches de git,etc.

Virtualenvwrapper
=================

O Virtualenvwrapper tem a mesma funcionalidade do Virtualenv contudo ele cria o virtualenv abaixo do diretório $HOME/.virtualenvs. Deste modo, se você usa alguma IDE como o Pycharm, a busca fica mais limpa.

Os comandos mais usados do Virtualenwrapper são:

- mkvirtualenv <nome>
- rmvirtualenv <nome>
- lsvirtualenv
- workon <nome>
- deactivate
- llsitepackages


Pip
===

O Pip é o gerenciador de pacotes de Python.

Podemos instalar, remover,atualizar pacotes, listar pacotes antigos, listar pacotes instalados.

Os pacotes externos estão cadastrados na [Loja de queijo](https://cheeseshop.python.org)

Exemplos:

- pip install requests
- pip install -U requests  # atualizar pacotes  
- pip uninstall requests
- pip list -o  # lista pacotes antigos
- pip freeze  # lista pacotes instalados

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


httpie
======

Com frequência usamos clientes HTTP e o httpie é um cliente que tem características interessantes tais como:

* suporte a JSON

* sessões persistentes

* saída formatada e colorida

* plugins

* suporte a uploads, formulários

A lista de características é extensa. Um ponto forte do httpie é o formato dos parâmetros que é intuitivo.

Vamos ver alguns exemplos:

1. https httpie.io/hello

Neste exemplo usamos o `https` que executa um método GET no endpoint.

2. http PUT pie.dev/put X-API-Token:123 name=John

Neste exemplo usamos o método PUT passando um HTTP header X-API-Token com valor 123 e um payload com chave name e valor John.

Identificamos o header por `:` e payload por `=`.

3. http -f POST pie.dev/post hello=World

Neste exemplo o payload é enviado com Content-type=application/x-www-form-urlencoded, pois usamos o parâmetro `-f`, formulário.

4. http -v pie.dev/get

Neste exemplo o parâmetro `-v` habilita o modo verboso e mostra detalhes da requisição e resposta HTTP.

5. http pie.dev/post < files/data.json

Neste exemplo um POST é executado usando um arquivo JSON.

6. http pie.dev/image/png > image.png

Neste exemplo é feito o download de uma imagem e a saída é direcionada para um arquivo png.

Para mais informações acesse [httpie.io](https://httpie.io/)

ipython
=======

O Ipython é um interpretador de Python com funções que otimizam a produtividade. Ele tem várias funcionalidades tais como:

1. carregamento de script: `ipython -i meuscript.py`.

2. acesso ao docstring.

3. salvar trechos da sessão do ipython em arquivo.

4. acesso direto ao shell: `!ls -l`.

5. persistência de sessão.

O ipython pode ser usado com debugger como o ipdb ou o pdbpp, e roda como um dos kernels do Jupyter.

Para ver a lista de referência dos comando do ipython execute: `%quickref`.

ipdb
====

O Ipdb é um debugger para python e usa comandos tais como continue(c), next(n), step into(s), jump(j), list(l), where(w), etc.

Para ativar o ipdb basta importar no script Python que você quer debugar e ativar o trace:

`import ipdb;ipdb.set_trace()`

Os comandos mais comuns são:

- next(n): executa até a próxima linha
- step into(s):executa a linha corrente;entra dentro da função chamada na linha corrente
- jump(j): pula para a linha especificada
- list(l): lista as linhas do frame atual
- help(h): mostra o manual; pode ser executado passando um comando como:h l (help list) 
- where(w): mostra a pilha de frames.
- print(p): mostra o valor de uma variável: p mylist
- pretty pprint(pp):mostra o valor de uma variável: pp mylist
- quit(q): sai do ipdb

pdbpp
=====

O pdbpp é um debugger muito similar ao Ipdb, a maioria dos comandos são idênticos aos do Ipdb.

O import deve ser feito desta maneira:

`import pdb;pdb.set_trace()`

O cursor do pdbpp tem o seguinte formato:

`(Pdb++)`

O python 3 já tem um módulo pdb embutido mas o ipdb/pdbpp tem melhorias de interface e características.

autoenv
=======

O autoenv é uma ferramenta escrita em shell que executa um arquivo .env assim que você no diretório onde o .env está situado.

O arquivo .env na raiz de um projeto pode configurar variáveis de ambiente e rodar comandos, como no exemplo abaixo:


`
export API_URL=https://httpbin.org
export API_USERNAME=lancelot
export API_CREDENTIAL="Ni!Ni!Ni!"
alias l='ls -latr'
alias t='pytest $@'
`

Evite versionar o arquivo .env e adicione variáveis relacionadas a credenciais, endpoints.   



direnv
======

O direnv é uma ferramenta escrita em Go, similar ao autoenv mas o arquivo utilizado para rodar comandos é o .envrc.

Caso o conteúdo do arquivo .envrc seja modificado, o direnv demanda que o conteúdo seja aprovado antes de executar o arquivo.


