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