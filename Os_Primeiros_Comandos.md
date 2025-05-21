
🧱 Principais Comandos do Docker
🔍 Imagens
docker pull <imagem>
→ Baixa uma imagem do Docker Hub (ex: docker pull nginx)

docker images
→ Lista as imagens disponíveis localmente

docker rmi <imagem>
→ Remove uma imagem local

🚀 Containers
docker run <opções> <imagem>
→ Cria e executa um container (ex: docker run nginx)

Opções mais usadas:

-d → modo desanexado (em segundo plano)

--name <nome> → dá um nome ao container

-p <porta_host>:<porta_container> → mapeia as portas

-v <diretório_host>:<diretório_container> → mapeia volumes (persistência de dados)

docker ps
→ Lista containers em execução

docker ps -a
→ Lista todos os containers (ativos e parados)

docker stop <nome ou id>
→ Para um container

docker start <nome ou id>
→ Inicia um container parado

docker restart <nome ou id>
→ Reinicia um container

docker rm <nome ou id>
→ Remove um container

docker exec -it <container> <comando>
→ Executa um comando dentro do container (ex: bash)

docker logs <nome ou id>
→ Mostra os logs do container

🛠️ Dockerfile & Build
docker build -t <nome>:<tag> .
→ Cria uma imagem a partir de um Dockerfile

📦 Volumes
docker volume create <nome>
→ Cria um volume

docker volume ls
→ Lista volumes

docker volume rm <nome>
→ Remove volume

🧭 Fluxo de Criação de Containers no Docker
Criação da imagem

Pode ser baixada do Docker Hub (docker pull)

Ou criada com um Dockerfile (docker build)

Execução do container

Usando docker run com as opções desejadas

Mapeamento de portas

Feito com -p <porta_host>:<porta_container>

Exemplo: -p 8080:80
Isso significa que a porta 80 do container será acessível na porta 8080 da máquina host

Persistência de dados (se necessário)

Usar volumes com -v para mapear diretórios entre o host e o container

Gerenciamento

Iniciar, parar, reiniciar, ver logs e executar comandos dentro do container

🌐 Sobre Mapeamento de Portas no Docker
Quando um container expõe uma porta (como a 80 para um servidor web), ela não é acessível externamente por padrão. Para tornar isso possível, usamos o mapeamento com -p:

bash
Copiar
Editar
docker run -d -p 8080:80 nginx
80: porta do container

8080: porta da sua máquina (host)

🔁 Assim, quando você acessa localhost:8080, o tráfego vai para a porta 80 dentro do container.