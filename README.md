# 🚀 Lista de exercicio sobre Docker!

**Objetivo**: Realizar a lista de exercício disponibilizada e colocar prints e comandos para evidenciar a realizacao da mesma.

## 📋 Sumário
- [Fácil](#-facil)
  - [1. Rodando um container básico](#1-facil)
  - [2. Criando e rodando um container interativo](#2-facil)
  - [3. Listando e removendo containers](#3-facil)
  - [4. Criando um Dockerfile para uma aplicação simples em Python](#4-facil)
-[Médio](#-medio)
  - [5. Criando e utilizando volumes para persistência de dados](#5-medio)

## 🔧 Exercícios

### 1. Rodando um container básico
**Execute um container usando a imagem do Nginx e acesse a página padrão no 
navegador. Use a landing page do TailwindCSS como site estático dentro do 
container.**:  

- Primeiramente vamos pegar imagem do Nginx do DockerHub ![nginx](img/exe1/dockerHub.png) ![docker pull](img/exe1/dockerPull.png)
- Depois iremos executar o comando `docker run --name meu-nginx -v [seudiretorio]:/usr/share/nginx/html -d -p 81:80 nginx` que sobe o container do Nginx e copia o diretorio da landing page do TailwindCSS pro diretorio onde o Nginx busca o arquivo index.html e faz a conexao entre a porta do host e do container.![running](img/exe1/containerRunning.png)![Texto alternativo](img/exe1/pageDocker.png)

### 2. Criando e rodando um container interativo
**Inicie um container Ubuntu e interaja com o terminal dele. Teste um script Bash que 
imprime logs do sistema ou instala pacotes de forma interativa.**:  
- Primeiramente vamos pegar imagem do Nginx do DockerHub

### 3. Listando e removendo containers
**Inicie um container Ubuntu e interaja com o terminal dele. Teste um script Bash que 
imprime logs do sistema ou instala pacotes de forma interativa.**: 

### 4. Criando um Dockerfile para uma aplicação simples em Python
**Inicie um container Ubuntu e interaja com o terminal dele. Teste um script Bash que 
imprime logs do sistema ou instala pacotes de forma interativa.**: 

### 5. Criando e utilizando volumes para persistência de dados
```yaml
services:
  backend:
    build: ./backend
    ports:
      - "80:80"
    environment:
      - DATABASE_DB=example
      - DATABASE_USER=root
      - DATABASE_PASSWORD=/run/secrets/db-password  
      - DATABASE_HOST=db
    depends_on:
      - db
    secrets:
      - db-password

  db:
    image: mysql:8.0.27
    environment:
      - MYSQL_DATABASE=example
      - MYSQL_ROOT_PASSWORD_FILE=/run/secrets/db-password  
    volumes:
      - db-data:/var/lib/mysql  
    secrets:
      - db-password

  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend

volumes:
  db-data:
secrets:
  db-password:
    file: db/password.txt
```
### 6. Criando e rodando um container multi-stage
```Dockerfile
FROM golang:1.19 AS build-stage

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY *.go ./

RUN CGO_ENABLED=0 GOOS=linux go build -o /docker-gs-ping

FROM gcr.io/distroless/base-debian11 AS build-release-stage

WORKDIR /

COPY --from=build-stage /docker-gs-ping /docker-gs-ping

EXPOSE 8080

USER nonroot:nonroot

ENTRYPOINT ["/docker-gs-ping"]
```
### 7. Construindo uma rede Docker para comunicação entre containers

```yaml
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/usr/src/app
      - /usr/src/app/node_modules
    depends_on:
      - backend
    networks:
      - app-network

  backend:
    build: ./backend
    volumes:
      - ./backend:/usr/src/app
      - /usr/src/app/node_modules
    environment:
      - MONGO_URL=mongodb://mongo:27017/mydb
    depends_on:
      - mongo
    networks:
      - app-network

  mongo:
    image: mongo:4.2.0
    volumes:
      - mongo_data:/data/db
    networks:
      - app-network

networks:
  app-network:

volumes:
  mongo_data:
```
### 8. Criando um compose file para rodar uma aplicação com banco de dados
```yaml
services:
  postgres:
    image: postgres:latest
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=pw23
      - POSTGRES_DB=db_base
    ports:
      - "5432:5432"

  pgadmin:
    image: dpage/pgadmin4:latest
    environment:
      - PGADMIN_DEFAULT_EMAIL=teste@gmail.com
      - PGADMIN_DEFAULT_PASSWORD=teste123
    ports:
      - "5050:80"
```
### 9. Criando uma imagem personalizada com um servidor web e arquivos estáticos
```Dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```
### 10. Evitar execução como root
```Dockerfile
FROM nginx:alpine

COPY . /usr/share/nginx/html

EXPOSE 80

RUN adduser -D appuser && \
    chown -R appuser:appuser /usr/share/nginx/html

USER appuser

CMD ["sh", "-c", "nginx -g 'daemon off;' || sleep infinity"]
```
### 11. Analisar imagem Docker com Trivy

Bibliotecas do sistema (Debian):
libaom3
libbluetooth-dev
libbluetooth3
libexpat1
libexpat1-dev
libharfbuzz0b
libldap-2.5-0
libopenexr-3-1-30
libopenexr-dev
libperl5.36
libtiff-dev
libtiff6
libtiffxx6
libxml2
libxml2-dev
linux-libc-dev
perl
perl-base
perl-modules-5.36
zlib1g
zlib1g-dev

Bibliotecas Python:
setuptools (python-pkg)


atualizar as bibliotecas e dependências do sistema operacional pode ajudar a reduzir vulnerabilidades, outra solucao é trocar para um sistema operacional mais leve, como o Alpine, que tem menos dependências e, portanto, menos vulnerabilidades potenciais.
