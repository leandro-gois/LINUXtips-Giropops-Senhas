# Giropops-Senhas

Desafio prático do [PICK](https://linuxtips.io/pick-2024/) proposto pelo  [badtuxx](https://github.com/badtuxx/).


![giropops-senhas-git](https://github.com/user-attachments/assets/7fd1ae53-bea3-4dc8-a95d-9486a1419a45)

## Objetivo

Colocar o App Python [giropops-senhas](https://github.com/badtuxx/giropops-senhas/) em uma imagem de container para ser executado a partir de um arquivo [Dockefile](https://docs.docker.com/reference/dockerfile/).


![giropops-senhas](https://github.com/user-attachments/assets/f56d3791-2185-4ee1-a33d-1aafd10ba3a6)

### Pré Requisitos

- **Docker**

```sh 
apt-get update

apt-get install curl -y

curl -fsSL https://get.docker.com/ | bash
```

- **Clone do projeto**

```
git clone https://github.com/badtuxx/giropops-senhas.git
```

- **Dockerfile-(usado para a criação da imagem -leandroag/linuxtips-giropops-senhas:1.0)**

```
FROM cgr.dev/chainguard/python:latest-dev AS python-builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --user -r requirements.txt \
    && chmod 777 /home/nonroot/.local/lib/python3.12/site-packages/flask

FROM cgr.dev/chainguard/python:latest
WORKDIR /opt/app
ENV REDIS_HOST=localhost
COPY app.py .
COPY static/ static/
COPY templates/ templates/
COPY --from=python-builder /home/nonroot/.local/lib/python3.12/site-packages /home/nonroot/.local/lib/python3.12/site-packages
COPY --from=python-builder /home/nonroot/.local/bin  /home/nonroot/.local/bin
ENV PATH=$PATH:/home/nonroot/.local/bin
EXPOSE 5000
ENTRYPOINT ["flask", "run", "--host=0.0.0.0"]

```

- **Container Python**
```
docker container run  -d --name giropops-senhas -p 5000:5000 -e REDIS_HOST=172.25.185.99  leandroag/linuxtips-giropops-senhas:1.0
```

- **Container Redis**
```
docker container run --name redis -d -p 6379:6379 redis:latest

```

- **Executando o App a partir do endereço http://172.25.185.99:5000/**

  
![giropops-senhas-endpoint](https://github.com/user-attachments/assets/3c985519-fd5b-47c1-948c-372bc7c37485)



[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/leandro-gois/) [![Gmail](https://img.shields.io/badge/Gmail-333333?style=for-the-badge&logo=gmail&logoColor=red)](mailto:leandroag2007@gmail.com) 
