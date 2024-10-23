# Jogo de Adivinhação com Flask

Este é um simples jogo de adivinhação desenvolvido utilizando o framework Flask. O jogador deve adivinhar uma senha criada aleatoriamente, e o sistema fornecerá feedback sobre o número de letras corretas e suas respectivas posições.

## Estrutura do Repositório do GIT:

Docker Compose File (docker-compose.yml): Para definir e orquestrar os serviços.
Dockerfile do Backend Python: Para configurar o container que executa a aplicação Flask.
Dockerfile do Frontend React: Para configurar o container que serve a aplicação frontend via NGINX.
Configuração do NGINX: Para definir o proxy reverso e o balanceamento de carga entre múltiplas instâncias do backend.

## Requisitos

- Docker
- Docker Compose
   
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
git clone https://github.com/srtamol/trabalhounidade1.git
cd trabalhounidade1
```

### 2. Verifique e ajuste as configurações

_ O arquivo `docker-compose.yml` e o arquivo de configuração do Nginx (`nginx.conf`) já estão configurados para o balanceamento de carga. 
- O `docker-compose.yml` define dois containers de backend (`backend1` e `backend2`) rodando a mesma aplicação. 
- O arquivo `nginx.conf` define a configuração do proxy reverso para distribuir as requisições entre os containers.

### 3. Rodar o Jogo

Para rodar o projeto, utilize o seguinte comando:

```bash
docker-compose up --build
```

Isso irá construir as imagens e subir os containers do jogo. A aplicação estará disponível em [http://localhost](http://localhost).


### 4. Escalar o backend

Para aumentar o número de containers backend rodando o jogo, pode usar o seguinte comando para escalar:

```bash
docker-compose up --scale backend1=3 --scale backend2=3
```

## Como Jogar

1. Abra o navegador e acesse [http://localhost](http://localhost). Digite uma frase secreta, envie, e salve o game-id.
2. Para adivinhar a senha, entre com o game_id que foi gerado no passo acima e tente adivinhar.
3. Toda vez que você interagir com o jogo, o Nginx irá redirecionar sua requisição para um dos containers de backend rodando no ambiente.


  
  
