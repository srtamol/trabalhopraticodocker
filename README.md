# Jogo de Adivinhação com Flask

Este é um simples jogo de adivinhação desenvolvido utilizando o framework Flask. O jogador deve adivinhar uma senha criada aleatoriamente, e o sistema fornecerá feedback sobre o número de letras corretas e suas respectivas posições.

## Estrutura do Repositório do GIT:

Docker Compose File (docker-compose.yml, na pasta raiz do repositorio): Para definir e orquestrar os serviços.
Dockerfile do Backend (pasta raiz do repositorio: Para configurar o container que executa a aplicação Flask.
Dockerfile do Frontend (dentro da pasta frontend): Para configurar o container que serve a aplicação frontend via NGINX.
Configuração do NGINX: Para definir o proxy reverso e o balanceamento de carga entre múltiplas instâncias do backend.

## Requisitos

- Docker
- Docker Compose
- GIT
   
## Funcionalidades

- Criação de um novo jogo com uma senha fornecida pelo usuário.
- Adivinhe a senha e receba feedback se as letras estão corretas e/ou em posições corretas.
- As senhas são armazenadas utilizando base64.
- As adivinhações incorretas retornam uma mensagem com dicas.
- Utilizar o  **Docker Compose** para criar múltiplos containers de backend para balanceamento com o Nginx.

## Instalação

Siga os passos abaixo para instalar e rodar o projeto em sua máquina:

### 1. Clone o repositório

```bash
git clone https://github.com/srtamol/trabalhopraticodocker.git
cd trabalhopraticodocker
```

### 2. Verifique e ajuste as configurações e escale o backend, se achar necessário

_ O arquivo `docker-compose.yml` e o arquivo de configuração do Nginx (`nginx.conf`) já estão configurados para o balanceamento de carga. 
- O `docker-compose.yml` define dois containers de backend rodando a mesma aplicação. Se quiser editar o total de containers basta editar o docker-compose.yml, o numero de replicas, conforme linha abaixo:

  ```
       deploy:
    replicas: 2
  ```
- O arquivo `nginx.conf` define a configuração do proxy reverso para distribuir as requisições entre os containers.

### 3. Rodar o Jogo

Para rodar o projeto, utilize o seguinte comando:

```bash
docker-compose up --build
```

Isso irá construir as imagens e subir os containers do jogo. A aplicação estará disponível em [http://localhost](http://localhost).


## Como Jogar

1. Abra o navegador e acesse [http://localhost](http://localhost). Digite uma frase secreta, envie, e salve o game-id.
2. Para adivinhar a senha, entre com o game_id que foi gerado no passo acima e tente adivinhar.
3. Toda vez que você interagir com o jogo, o Nginx irá redirecionar sua requisição para um dos containers de backend rodando no ambiente.

## Como Limpar o Projeto
Para limpar o projeto e remover os containers, volumes e redes criados pelo Docker Compose, siga os passos abaixo:

Parar e remover os containers

```bash
docker-compose down
```

Remover volumes associados

```
docker-compose down -v
```

## Pratica 02 ##

1- FrontEnd Deployment: Define um Deployment para o FrontEnd com 2 réplicas.
2- FrontEnd Service: Cria um Service do tipo NodePort para expor o FrontEnd na porta 30000.
3- BackEnd Deployment: Define um Deployment para o BackEnd com 2 réplicas.
4- BackEnd Service: Cria um Service do tipo ClusterIP para o BackEnd.
5- PostgreSQL Deployment: Define um Deployment para o PostgreSQL com 1 réplica.
6- PostgreSQL Service: Cria um Service do tipo ClusterIP para o PostgreSQL.
5- HorizontalPodAutoscaler (HPA): Configura o HPA para o BackEnd, escalando entre 2 e 10 réplicas com base na utilização de CPU.
