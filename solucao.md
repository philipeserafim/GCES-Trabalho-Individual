# GCES - Trabalho Individual 2022.1

# 1 - Containerização do banco

## .env
Utilizado para configurar as variáveis de ambiente que serão utilizadas na criação do container.
Aqui não foi necessário a especificação de um Dockerfile, pois aplicando apenas as variáveis de ambiente no ````docker-compose.yml```` já é suficiente.

```
POSTGRES_DB=library_db
POSTGRES_USER=postgres
POSTGRES_PASSWORD=password
POSTGRES_HOST=library_db
POSTGRES_PORT=5432
```

A utilização de um ```.env``` genérico possibilita a sua disponibilização para fins de correção e testes.

## docker-compose.yml
É criado um container com nome ```library_db``` e imagem ```postgres```, é passado as variáveis de ambiente utilizadas para configuração do banco de dados.

```
environment:
  POSTGRES_USER: ${POSTGRES_USER}
  POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
  POSTGRES_DB: ${POSTGRES_DB}
  POSTGRES_PORT: ${POSTGRES_PORT}
  POSTGRES_HOST: ${POSTGRES_HOST}
```

E, por último, é especificada qual porta do host irá 'refletir' a porta do container. O que permite o acesso ao banco de dados por fora do container.

## Como executar

Para realizar o build do container, basta executar o comando 

```
docker-compose up --build
```
E caso deseje testar a conexão com o banco de dados, basta executar o comando

```
pg_isready -d library_db -h 0.0.0.0 -p 5432 -U postgres
```

# 2 - Containerização do app

## Dockerfile

Usado para configurar um container com configurações/arquivos específicos, nele é definido o ambiente e sua versão(````python:latests````), variáveis de ambiente próprias do Django, a pasta de trabalho dentro do container e os comandos que serão executados apenas na montage da imagem do container.

### Instalar dependências necessárias ao projeto
```
COPY requirements.txt /library_back/requirements.txt
RUN pip install -r requirements.txt
```

### Copiar projeto para dentro do container
```
COPY . /library_back
```

## docker-compose.yml
Foi adicionada uma ```network``` com intuito de isolar as conexões entre os containers, adicionado um ```healthcheck``` ao serviço ```library_db```, que é responsável por verificar se o banco de dados está disponível e aceitando conexões,  um ```depends_on``` ao serviço ```library_app``` em relação ao ``````library_db``````, que só permite a completa inicialização do ```library_app``` após a ```condition: service_healthy``` ser cumprida e especificação da porta utilizada para acesso ao ```library_app``` e seus endpoints.

No compose também é especificado os comandos que devem ser executados toda vez que o container for iniciado.

```
command: sh -c "python manage.py makemigrations && python manage.py migrate && python manage.py runserver 0.0.0.0:8000"
```
````makemigrations````/````migrate```` e o ```runserver 0.0.0.0:8000``` ques inicializa o app e especifica o endereço e porta para conexão.

## Como executar
```
docker-compose up --build
```

Para testar, basta acessar o endereço indicado nos ```command``` do ```docker-compose```: http://localhost:8000.

# 3 - Containerização do front