# 🌍 Reverse Proxy com Docker Compose + Nginx

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)
![Docker Compose](https://img.shields.io/badge/Docker_Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MIT License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

Projeto desenvolvido com o objetivo de estudar **Docker Compose**, **Nginx Reverse Proxy**, **Virtual Hosts** e a comunicação entre containers utilizando uma única porta de acesso.

Neste projeto, um container Nginx atua como **Reverse Proxy**, recebendo todas as requisições HTTP e encaminhando-as para diferentes containers conforme o subdomínio informado na URL.

---

# 📚 Tecnologias utilizadas

- Docker
- Docker Compose
- Nginx
- Reverse Proxy
- Virtual Hosts
- Bind Mounts
- nip.io

---

# 🏗 Arquitetura

```
                        Cliente
                           │
                           │
        http://subdominio.IP.nip.io:8090
                           │
                           ▼
                 +--------------------+
                 |    WEB_PROXY       |
                 |       Nginx        |
                 +--------------------+
                   │      │      │
        ┌──────────┘      │      └──────────┐
        ▼                 ▼                 ▼
+----------------+ +----------------+ +----------------+
| america_index  | | africa_index   | | europa_index   |
|     Nginx      | |     Nginx      | |     Nginx      |
+----------------+ +----------------+ +----------------+
                           │
                           ▼
                   +----------------+
                   |   asia_index   |
                   |     Nginx      |
                   +----------------+
```

---

# 📁 Estrutura do projeto

```
proxy_reverso_app_copa/

├── africa_index/
├── america_index/
├── asia_index/
├── europa_index/
├── geral_page/
├── proxy_conf/
│   └── default.conf
│
├── docker-compose.yaml
├── README.md
├── LICENSE
└── .gitignore
```

---

# ⚙ Como funciona

O projeto é composto por **cinco containers**:

| Container | Função |
|------------|---------|
| web_proxy | Reverse Proxy responsável por receber todas as requisições |
| america_index | Página da América |
| africa_index | Página da África |
| europa_index | Página da Europa |
| asia_index | Página da Ásia |

O container **web_proxy** publica apenas a porta **8090**.

Todos os demais containers permanecem acessíveis somente pela rede interna do Docker.

---

# 🌐 Utilizando o nip.io

Para simular um ambiente com DNS sem a necessidade de configurar um servidor DNS real, foi utilizado o serviço **nip.io**.

O **nip.io** é um serviço gratuito que resolve automaticamente um domínio para um endereço IP informado na própria URL.

Exemplo:

```
192.168.0.100.nip.io
```

é resolvido automaticamente para:

```
192.168.0.100
```

Isso permite criar subdomínios dinamicamente para testes locais.

Exemplos utilizados neste projeto:

```
http://192.168.0.100.nip.io:8090
```

Página principal.

```
http://am.192.168.0.100.nip.io:8090
```

Redireciona para o container da América.

```
http://af.192.168.0.100.nip.io:8090
```

Redireciona para o container da África.

```
http://eu.192.168.0.100.nip.io:8090
```

Redireciona para o container da Europa.

```
http://as.192.168.0.100.nip.io:8090
```

Redireciona para o container da Ásia.

---

# 🚀 Executando o projeto

Clone o repositório:

```bash
git clone https://github.com/SEU_USUARIO/proxy_reverso_app_copa.git
```

Acesse a pasta:

```bash
cd proxy_reverso_app_copa
```

Suba os containers:

```bash
docker compose up -d
```

Verifique se todos os containers estão em execução:

```bash
docker ps
```

---

# 🔍 Testes

Página principal:

```
http://SEU_IP.nip.io:8090
```

América

```
http://am.SEU_IP.nip.io:8090
```

África

```
http://af.SEU_IP.nip.io:8090
```

Europa

```
http://eu.SEU_IP.nip.io:8090
```

Ásia

```
http://as.SEU_IP.nip.io:8090
```

---

# 📂 Volumes utilizados

O projeto utiliza **Bind Mounts** para disponibilizar as configurações e páginas HTML aos containers.

Exemplo:

```
./proxy_conf/default.conf:/etc/nginx/conf.d/default.conf:ro
```

e

```
./geral_page:/usr/share/nginx/html:ro
```

A opção **`:ro` (Read Only)** garante que os arquivos sejam montados apenas para leitura dentro do container.

---

# 📖 Conceitos estudados

- Docker
- Docker Compose
- Containers
- Redes Docker
- Reverse Proxy
- Nginx
- Virtual Hosts
- Bind Mounts
- DNS Dinâmico com nip.io



---

# 👨‍💻 Autor

Desenvolvido por **Gustavo Barbosa** como projeto de estudos sobre Docker Compose e Reverse Proxy com Nginx.

---

# 📄 Licença

Este projeto está licenciado sob a licença **MIT**.