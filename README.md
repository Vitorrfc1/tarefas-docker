# Trabalho Prático - Ciclo 02: Containerização de Aplicação

## Disciplina: Sistemas Distribuídos (2026.1)  
## Curso: Engenharia de Software  
## Professor: Álvaro Lopes Bastos  

---

# Integrantes do Grupo

| Nome | Matrícula |
|------|----------|
| Vitor Rodrigues Fernandes Carneiro | 2320595 |
| Layssa Fernandes da Silva | 2320852 |
| Marcel de Morais Carvalho Filho | 2321221 |

---

# Descrição do Projeto

Este projeto consiste na **containerização de uma aplicação de Lista de Tarefas**, utilizando Docker para isolar os serviços:

- Frontend (React + Vite + Nginx)  
- Backend (Node.js + Express)  
- Banco de Dados (PostgreSQL)  

A comunicação entre os serviços foi configurada manualmente utilizando uma **rede Docker do tipo bridge**, sem uso de Docker Compose.

---

# Arquitetura do Sistema

Frontend → Backend → PostgreSQL
(React)   (API)     (Banco de dados)
Frontend: Nginx (porta 80)
Backend: Node.js API (porta 3001)
PostgreSQL: banco de dados (porta 5432)
Comandos de Execução

# 1. Criar rede
docker network create tarefas-network

# 2. Build Backend
docker build -t backend ./backend

# 3. Build Frontend
docker build --build-arg VITE_API_URL=http://localhost:3001 -t frontend ./frontend

# 4. Banco de Dados (PostgreSQL)
docker run -d --name postgres --network tarefas-network -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=tarefasdb -v pgdata:/var/lib/postgresql/data postgres:16-alpine

# 5. Backend
docker run -d --name backend --network tarefas-network -p 3001:3001 -e DB_HOST=postgres -e DB_PORT=5432 -e DB_USER=postgres -e DB_PASSWORD=postgres -e DB_NAME=tarefasdb -e PORT=3001 backend

# 6. Frontend
docker run -d --name frontend --network tarefas-network -p 80:80 frontend

# Verificação
docker ps

# Acesso à Aplicação
Frontend: http://localhost

Backend API: http://localhost:3001

# Funcionalidades
Criar tarefas

Listar tarefas

Persistência em PostgreSQL


# Tecnologias Utilizadas
Docker

Node.js

Express

React + Vite

Nginx

PostgreSQL

# Observações:
Não foi utilizado Docker Compose

Comunicação via rede Docker customizada

Backend conecta ao banco via hostname postgres
