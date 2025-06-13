# Docker: Um Guia Didático para Iniciantes 🐳

Seja bem-vindo ao mundo do **Docker**!  
Vamos explorar essa ferramenta incrível que revolucionou a maneira como desenvolvemos, empacotamos e executamos aplicações.

---

## 1. O que é o Docker?

O **Docker** é uma plataforma que permite criar, executar e gerenciar aplicações dentro de **containers**.  
Um container é como uma "caixinha" isolada que contém tudo o que sua aplicação precisa para funcionar: código, bibliotecas, configurações e dependências.

### 🔹 Vantagens do Docker:
- ✔ **Isolamento**: Cada container roda de forma independente.
- ✔ **Portabilidade**: Funciona igual em qualquer máquina (Windows, Linux, macOS).
- ✔ **Eficiência**: Consome menos recursos que máquinas virtuais (VMs).

---

## 2. Suba um Container Docker

### 🔧 Instale o Docker
👉 [Download do Docker](https://www.docker.com/get-started)

### ▶ Rodando seu primeiro container
```bash
docker run hello-world
```

📌 **Explicação**:
- `docker run`: Baixa a imagem (se não existir) e inicia o container.
- `hello-world`: Imagem de exemplo que mostra uma mensagem de boas-vindas.

### 🌐 Subindo um servidor web (Nginx)
```bash
docker run -d -p 80:80 nginx
```
- `-d`: Roda em segundo plano (detached).
- `-p 80:80`: Mapeia a porta 80 do container para a 80 do seu PC.

Acesse: [http://localhost](http://localhost)

---

## 3. Crie e Personalize Imagens

Imagens são como **receitas** para criar containers.  
Você pode criar as suas usando um `Dockerfile`.

### 📄 Exemplo: Dockerfile para uma app Node.js
```dockerfile
# Usa a imagem base do Node.js
FROM node:14

# Cria um diretório para a aplicação
WORKDIR /app

# Copia os arquivos necessários
COPY package.json .
COPY . .

# Instala as dependências
RUN npm install

# Define o comando de inicialização
CMD ["node", "app.js"]
```

### ▶ Construindo e executando sua imagem
```bash
docker build -t minha-app-node .     # Cria a imagem
docker run -p 3000:3000 minha-app-node  # Executa o container
```
- `-t`: Define um nome para a imagem (`minha-app-node`).
- `.`: Indica que o Dockerfile está no diretório atual.

---

## 4. Persistência de Dados com Volumes

Containers são **efêmeros** (tudo é perdido ao reiniciar).  
Para persistir dados, usamos **volumes**.

### 📦 Tipos de volumes

#### ➤ Volumes nomeados (gerenciados pelo Docker)
```bash
docker volume create meu-volume
docker run -v meu-volume:/dados nginx
```

#### ➤ Bind mounts (mapeia uma pasta do PC para o container)
```bash
docker run -v /caminho/do/pc:/dados nginx
```

### 🛢 Exemplo com banco de dados (MySQL)
```bash
docker run -d -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=senha mysql
```

📌 Os dados ficam seguros mesmo se o container for removido!

---

## 5. Comunicação entre Containers com Redes

Por padrão, containers **não se comunicam**.  
Para isso, criamos redes Docker:

### 🌐 Criando uma rede e conectando containers
```bash
docker network create minha-rede
docker run -d --network minha-rede --name meu-mongo mongo
docker run -d --network minha-rede --name minha-app node-app
```

📌 Agora, `minha-app` pode acessar o MongoDB usando `meu-mongo` como hostname!

---

## 6. Docker Compose: Gerenciando Múltiplos Containers

Em vez de rodar vários comandos `docker run`, usamos o **Docker Compose** para orquestrar containers com um único arquivo (`docker-compose.yml`).

### 🧩 Exemplo: App Node.js + MongoDB
```yaml
version: "3.8"
services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - mongo

  mongo:
    image: mongo
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

### Comandos úteis:
```bash
docker-compose up -d   # Inicia os containers
docker-compose down    # Para e remove tudo
```

---

## 🎉 Conclusão

Agora você já sabe:
✅ Subir containers (`docker run`)  
✅ Criar imagens personalizadas (`Dockerfile`)  
✅ Persistir dados (volumes)  
✅ Conectar containers (redes)  
✅ Gerenciar múltiplos serviços (Docker Compose)

O Docker facilita o desenvolvimento e o deploy de aplicações.  
**Pratique e explore mais!** 🚀

---

📚 **Quer mergulhar fundo?**  
Consulte a [documentação oficial do Docker](https://docs.docker.com/)

**Happy Dockering!** 🐋

# Introdução ao Amazon ECS

Se você está começando com a AWS e quer aprender a executar aplicações em contêineres, o Amazon Elastic Container Service (ECS) é um ótimo serviço para começar. Neste guia, vamos explicar de forma simples:

✅ O que é o ECS e como ele funciona  
✅ Como armazenar imagens de contêineres no Amazon ECR  
✅ As três camadas do ECS: Cluster, Serviço e Tarefa  
✅ Como fazer deploy de uma aplicação com Load Balancer  
✅ Automatizar deploys usando GitHub Actions  

Vamos lá!

---

## 1. O que é o Amazon ECS?

O Amazon ECS (Elastic Container Service) é um serviço gerenciado da AWS para executar aplicações em contêineres usando o Docker. Ele facilita a implantação, o gerenciamento e a escalabilidade de aplicações conteinerizadas sem precisar configurar servidores manualmente.

### Por que usar o ECS?
✔ Escalabilidade automática: Ajusta o número de contêineres conforme a demanda.  
✔ Integração com a AWS: Funciona bem com outros serviços como Load Balancer, RDS, CloudWatch, etc.  
✔ Custo eficiente: Você paga apenas pelos recursos que usar.

---

## 2. O Amazon ECR: Repositório de Imagens de Contêiner

Antes de executar um contêiner no ECS, você precisa armazenar sua imagem Docker em um lugar seguro e acessível. É aí que entra o Amazon ECR (Elastic Container Registry).

### Como funciona?
1. Você cria um repositório no ECR.  
2. Constrói sua imagem Docker localmente.  
3. Faz o upload (push) da imagem para o ECR.  
4. O ECS puxa (pull) essa imagem quando for executar sua aplicação.

### Comandos básicos para usar o ECR
```bash
# Autenticar no ECR  
aws ecr get-login-password | docker login --username AWS --password-stdin SUA_CONTA.dkr.ecr.REGIAO.amazonaws.com  

# Criar uma imagem Docker  
docker build -t minha-aplicacao .  

# Taggear a imagem para o ECR  
docker tag minha-aplicacao:latest SUA_CONTA.dkr.ecr.REGIAO.amazonaws.com/meu-repositorio:latest  

# Fazer upload da imagem  
docker push SUA_CONTA.dkr.ecr.REGIAO.amazonaws.com/meu-repositorio:latest  
```

---

## 3. As Camadas do ECS: Cluster, Serviço e Tarefa

O ECS é organizado em três camadas principais:

### 🔹 Cluster
Um cluster é um grupo de servidores (máquinas virtuais ou instâncias EC2) onde seus contêineres serão executados. Você pode ter vários clusters para diferentes ambientes (ex: produção, teste).

### 🔹 Tarefa (Task)
Uma tarefa é a menor unidade do ECS. Ela define:
- Qual imagem Docker será executada  
- Quanta CPU e memória o contêiner precisa  
- Variáveis de ambiente e portas expostas  

### 🔹 Serviço (Service)
Um serviço mantém sua aplicação rodando continuamente. Ele:
- Gerencia quantas tarefas devem estar em execução  
- Lida com falhas (reinicia contêineres que quebram)  
- Pode ser integrado a um Load Balancer para distribuir tráfego  

---

## 4. Fazendo Deploy de uma Aplicação com Load Balancer

Vamos ver um exemplo prático de como implantar uma aplicação web no ECS com um Application Load Balancer (ALB) para balancear o tráfego.

### Passos básicos:
1. Criar um cluster ECS (pode ser com instâncias EC2 ou serverless com Fargate).  
2. Definir uma tarefa (Task Definition) especificando a imagem do ECR, CPU, memória e portas.  
3. Criar um serviço (Service) que execute essa tarefa e se conecte a um ALB.  
4. Configurar o Load Balancer para rotear tráfego para os contêineres.  

Pronto! Sua aplicação estará no ar, escalável e com alta disponibilidade.

---

## 5. Automatizando Deploys com GitHub Actions

Para evitar fazer tudo manualmente, você pode automatizar o deploy usando GitHub Actions.

### Exemplo de fluxo:
1. Sempre que um código novo for enviado (push) para o GitHub, um workflow é acionado.  
2. O workflow faz o build da imagem Docker e envia para o ECR.  
3. Atualiza a definição de tarefa (Task Definition) no ECS.  
4. Reinicia o serviço no ECS para aplicar as mudanças.  

### Exemplo de arquivo `.github/workflows/deploy.yml`:
```yaml
name: Deploy to ECS  

on:  
  push:  
    branches: [ main ]  

jobs:  
  deploy:  
    runs-on: ubuntu-latest  
    steps:  
      - name: Checkout code  
        uses: actions/checkout@v2  

      - name: Login to Amazon ECR  
        id: login-ecr  
        uses: aws-actions/amazon-ecr-login@v1  

      - name: Build and push Docker image  
        run: |  
          docker build -t minha-aplicacao .  
          docker tag minha-aplicacao:latest ${{ secrets.ECR_REPOSITORY }}:latest  
          docker push ${{ secrets.ECR_REPOSITORY }}:latest  

      - name: Deploy to ECS  
        uses: aws-actions/amazon-ecs-deploy-task-definition@v1  
        with:  
          task-definition: minha-task-definition.json  
          service: meu-servico-ecs  
          cluster: meu-cluster  
          wait-for-service-stability: true  
```

---

## Conclusão

Neste guia, você aprendeu:
✔ O que é o Amazon ECS e como ele gerencia contêineres.  
✔ Como armazenar imagens no ECR.  
✔ As camadas do ECS: Cluster, Serviço e Tarefa.  
✔ Como implantar uma aplicação com Load Balancer.  
✔ Como automatizar deploys com GitHub Actions.  

Agora você está pronto para começar a usar o ECS na AWS! 🚀

> Quer se aprofundar? Consulte a [documentação oficial da AWS](https://docs.aws.amazon.com/ecs).
