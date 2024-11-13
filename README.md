# Jogo de Adivinhação com Flask - Pratica 01 - Docker Compose

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

# Jogo de Adivinhação com Flask - Pratica 02 - Kubernetes

Este é um simples jogo de adivinhação desenvolvido utilizando o framework Flask. O jogador deve adivinhar uma senha criada aleatoriamente, e o sistema fornecerá feedback sobre o número de letras corretas e suas respectivas posições.

## Estrutura do Repositório do GIT:

Docker Compose File (docker-compose.yml, na pasta raiz do repositorio): Para definir e orquestrar os serviços.
Dockerfile do Backend (pasta raiz do repositorio: Para configurar o container que executa a aplicação Flask.
Dockerfile do Frontend (dentro da pasta frontend): Para configurar o container que serve a aplicação frontend via NGINX.

## Requisitos

- Docker
- Kubectl instalado local ou um cluster Kubernetes
- GIT
   
## Funcionalidades

- Criação de um novo jogo com uma senha fornecida pelo usuário.
- Adivinhe a senha e receba feedback se as letras estão corretas e/ou em posições corretas.
- As senhas são armazenadas utilizando base64.
- As adivinhações incorretas retornam uma mensagem com dicas.
- Utilizar o  **manifesto jogoadivinhacao.yaml** para criar a estrutura do jogo.

## Instalação

Siga os passos abaixo para instalar e rodar o projeto em sua máquina:

### 1. Clone o repositório

```bash
git clone https://github.com/srtamol/trabalhopraticodocker.git
cd trabalhopraticodocker
```

### 2. Verifique e ajuste as configurações e escale o backend, se achar necessário

FrontEnd Deployment: Define um Deployment para o FrontEnd com 2 réplicas usando uma imagem do Docker Hub.
FrontEnd Service: Cria um Service do tipo NodePort para expor o FrontEnd na porta 30000.
BackEnd Deployment: Define um Deployment para o BackEnd com 2 réplicas usando uma imagem do Docker Hub.
BackEnd Service: Cria um Service do tipo ClusterIP para o BackEnd.
PostgreSQL Deployment: Define um Deployment para o PostgreSQL com 1 réplica.
PostgreSQL Service: Cria um Service do tipo ClusterIP para o PostgreSQL.
HorizontalPodAutoscaler (HPA): Configura o HPA para o BackEnd, escalando entre 2 e 10 réplicas com base na utilização de CPU.

### 3. Rodar o Jogo

Para rodar o projeto, utilize o seguinte comando:

Detalhamento do arquivo jogoadivinhacao.yaml:

1 -Aplique o manifesto:

'''
kubectl apply -f jogoadivinhacao.yaml
'''

Verifique os recursos:

'''
kubectl get deployments
'''

'''
kubectl get services
'''

'''
kubectl get pods
'''

'''
kubectl get hpa
'''

Dicas Adicionais:
Logs: Se precisar verificar os logs de um pod específico, use:

'''
kubectl logs nome-do-pod
'''

Descrever Recursos: Para obter mais detalhes sobre um recurso específico, use:

'''
kubectl describe deployment nome-do-deployment
'''

'''
kubectl describe service nome-do-service
'''


## Como Jogar

1. Abra o navegador e acesse [http://localhost](http://localhost). Digite uma frase secreta, envie, e salve o game-id.
2. Para adivinhar a senha, entre com o game_id que foi gerado no passo acima e tente adivinhar.



