
## Jogo Multiplayer de senha

Este projeto é um jogo multiplayer simples rodando em containers Docker. O jogador deve adivinhar uma senha criada aleatoriamente, e o sistema fornecerá feedback sobre o número de letras corretas e suas respectivas posições.

## Requisitos

Antes de começar, certifique-se de que você tem as seguintes ferramentas instaladas:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Funcionalidades

Utilizamos **Docker Compose** para criar múltiplos containers de backend para balanceamento. O objetivo é distribuir as requisições entre diferentes instâncias do backend.

## Instalação

Siga os passos abaixo para instalar e rodar o projeto em sua máquina:

### 1. Clone o repositório

```bash
git clone https://github.com/maurizb/trabalhodocker2.git
cd trabalhodocker2
```

### 2. Verifique e ajuste as configurações

O arquivo `docker-compose.yml` e o arquivo de configuração do Nginx (`nginx.conf`) já estão configurados para o balanceamento de carga (**NÃO IMPLEMENTADO**).

- O `docker-compose.yml` define dois containers de backend (`backend1` e `backend2`) rodando a mesma aplicação. (**NO MOMENTO APENAS UM**).
- O arquivo `nginx.conf` define a configuração do proxy reverso para distribuir as requisições entre os containers.

Se quiser, ajuste as configurações de portas, número de containers, ou outras variáveis.

### 3. Rodar o projeto

Para rodar o projeto, utilize o seguinte comando:

```bash
docker-compose up --build
```

Isso irá construir as imagens e subir os containers do jogo. A aplicação estará disponível em [http://localhost](http://localhost).

### 4. Escalar o backend (opcional)

Se você quiser aumentar o número de containers backend rodando o jogo, pode usar o seguinte comando para escalar:

```bash
docker-compose up --scale backend1=3 --scale backend2=3
```

Isso criará mais instâncias dos containers de backend, e o Nginx irá balancear a carga entre eles.

## Como Jogar

1. Abra o navegador e acesse [http://localhost](http://localhost). Digite uma frase secreta, envie, e salve o game-id.

2. Para adivinhar a senha, entre com o game_id que foi gerado no passo acima e tente adivinhar.
3. Toda vez que você interagir com o jogo, o Nginx irá redirecionar sua requisição para um dos containers de backend rodando no ambiente.
   
## Encerrando o Jogo

Para parar todos os containers e excluir os volumes, executar:

```bash
docker compose down --volumes
```

Isso irá desligar e remover todos os containers criados.


# Decisões de Design da Aplicação

O foco foi em criar uma aplicação escalável, eficiente e fácil de manter, utilizando Docker, Docker Compose e Nginx para balanceamento de carga.

- **Isolamento**: Cada serviço (backend e proxy) é isolado em seu próprio container, evitando conflitos de dependências e facilitando o gerenciamento. As placas de rede também são isoladas (fora o necessário para comunicação).
- **Portabilidade**: Os containers garantem que a aplicação rodará da mesma maneira em qualquer ambiente, seja de desenvolvimento, testes ou produção, sem dependências externas além do Docker.
- **Escalabilidade**: Com o Docker, é fácil aumentar o número de instâncias de qualquer serviço, permitindo escalar a aplicação horizontalmente conforme a demanda. Foram usados 2 backends por padrão no docker-compose.yml (backend1 e backend2).

Utilizamos o Docker Compose para **orquestrar** os containers. Ele facilita a definição, deploy e gerenciamento dos serviços da aplicação em um único arquivo (`docker-compose.yml`).

- **Multicontainers**: A aplicação utiliza múltiplos containers de backend, e o Docker Compose coordena a criação e conexão desses containers com outros serviços, como o Nginx.
- **Redundância**: O uso de múltiplos containers backend permite redundância, melhorando a resiliência e disponibilidade.

O **Nginx** foi escolhido para desempenhar o papel de proxy reverso e balanceador de carga.

- **Proxy reverso**: O Nginx recebe todas as requisições e as redireciona para os backends, mantendo a arquitetura modular e protegendo os serviços internos.
- **Balanceamento de carga**: Distribui as requisições entre múltiplos containers backend, garantindo que nenhum deles seja sobrecarregado, o que melhora a performance e resiliência.

Utilizei o bloco `upstream` no arquivo `default.conf` do nginx para definir múltiplos servidores backend, facilitando o balanceamento de carga entre eles. O Nginx distribui as requisições de maneira circular por padrão entre os servidores backend (fonte: https://medium.com/@aedemirsen/load-balancing-with-docker-compose-and-nginx-b9077696f624).

A aplicação foi projetada para ser **escalável horizontalmente**, adicionando mais containers de backend conforme necessário. Isso é feito através do Docker Compose com um comando simples:

```bash
docker-compose up --scale backend1=3 --scale backend2=3
```

Essa flexibilidade permite que a aplicação lide com aumentos de tráfego sem comprometer a performance.