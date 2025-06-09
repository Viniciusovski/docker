# 📦 Guia Rápido: Enviando Imagens para o Docker Hub

## ✅ Pré-requisitos
- Ter o Docker instalado.
- Ter criado e testado uma imagem local com `docker build`.

---

## 1. Nomeando corretamente uma imagem local
Para que uma imagem possa ser enviada ao Docker Hub, ela precisa estar nomeada com o seu nome de usuário:

```bash
docker tag <imagem_original> <seu_usuario_docker>/<nome_imagem>:<tag>
```

**Exemplo:**
```bash
docker tag app-node aluradocker/app-node:1.0
```

---

## 2. Criando conta no Docker Hub
- Acesse [Docker Hub](https://hub.docker.com).
- Cadastre-se com nome de usuário, e-mail e senha.
- Confirme o e-mail e faça login.

---

## 3. Autenticando no terminal
Antes de fazer o push, autentique-se:

```bash
docker login -u <seu_usuario_docker>
```

Você será solicitado a digitar a senha (invisível por segurança).

---

## 4. Enviando a imagem (push)
Após nomear e autenticar, use o comando:

```bash
docker push <seu_usuario_docker>/<nome_imagem>:<tag>
```

**Exemplo:**
```bash
docker push aluradocker/app-node:1.0
```

Se você usar um repositório que não é seu, receberá o erro:
```
denied: requested access to the resource is denied
```

---

## 5. Subindo novas versões
Para novas versões:
```bash
docker tag app-node:1.2 aluradocker/app-node:1.2

docker push aluradocker/app-node:1.2
```

O Docker reaproveita camadas já enviadas, tornando o envio mais rápido.

---

## ✅ Conclusão
- Imagens precisam ter o repositório com seu nome de usuário para serem enviadas.
- Use `docker login`, `docker tag` e `docker push` para gerenciar uploads.
- O Docker Hub identifica camadas já existentes, otimizando os uploads.

Agora você já sabe como versionar e publicar suas imagens no Docker Hub! 🚀
