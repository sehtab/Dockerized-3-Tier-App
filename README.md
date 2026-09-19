# Dockerized-3-Tier-App
# Dockerized Three-Tier Web Application

A full-stack, containerized web application built with **Nginx (Frontend Proxy)**, **Node.js/Express (Backend API)**, and **PostgreSQL (Database)** orchestrated using Docker Compose.

---

## Architecture Overview

```text
[ Browser / Client ]
         │
    (Port 8080 / 8081)
         ▼
 ┌──────────────────────┐
 │    Frontend Tier     │  --> Nginx static web server & reverse proxy
 └──────────┬───────────┘
            │ Internal network (app-network)
            ▼
 ┌──────────────────────┐
 │     Backend Tier     │  --> Node.js / Express REST API
 └──────────┬───────────┘
            │ Internal network (app-network)
            ▼
 ┌──────────────────────┐
 │    Database Tier     │  --> PostgreSQL with persistent volume
 └──────────────────────┘

Tech Stack

    Frontend: HTML5, JavaScript, Nginx (Reverse Proxy)

    Backend: Node.js, Express.js, pg (Node Postgres client)

    Database: PostgreSQL 16

    Containerization: Docker, Docker Compose

three-tier-app/
├── docker-compose.yml     # Orchestrates all 3 tiers
├── init.sql               # Database schema & initial seed data
├── backend/
│   ├── Dockerfile         # Node.js container setup
│   ├── package.json       # App dependencies
│   └── server.js          # Express API server
└── frontend/
    ├── Dockerfile         # Nginx container setup
    ├── nginx.conf         # Nginx reverse proxy configuration
    └── index.html         # Frontend interface

Prerequisites

    Docker Desktop (includes Docker Engine and Docker Compose V2) installed and running on your machine


1. Clone the Repository

git clone [https://github.com/your-username/three-tier-docker-app.git](https://github.com/your-username/three-tier-docker-app.git)
cd three-tier-docker-app

2. Launch the Application

Start all three tiers (Database, Backend, Frontend) in detached mode:

docker compose up --build -d

Note for Docker V1 users: If using legacy compose, use docker-compose up --build -d.
<img width="1136" height="809" alt="3-tier-build-1" src="https://github.com/user-attachments/assets/484c065b-fff5-474f-a1e1-a23973c61f01" />
<img width="1380" height="652" alt="Dockerized-3 tier app" src="https://github.com/user-attachments/assets/1fcce8a2-1eeb-4859-9356-fe4685fd3b09" />
<img width="1136" height="264" alt="docker ps-test" src="https://github.com/user-attachments/assets/48a3a2b7-c382-48ea-98ee-95a88b9e55fb" />
<img width="1136" height="809" alt="3-tier-build-2" src="https://github.com/user-attachments/assets/c72626d9-53f2-4aeb-a8fb-ad7df3b09832" />



Accessing the Application

    Web Interface: Open http://localhost:8080 (or http://localhost:8081 if you changed the host port mapping).

    Backend API Health Endpoint: Open http://localhost:8080/api/health in your browser or test via curl

curl http://localhost:8080/api/health

<img width="1159" height="407" alt="data example" src="https://github.com/user-attachments/assets/a7a92542-227b-4569-93fd-6ef00eeb3b24" />
<img width="1380" height="652" alt="Dockerized-3 tier app" src="https://github.com/user-attachments/assets/69a47e08-f7dc-49bf-9f25-c5718bf435c8" />


Managing the Database

The database is automatically initialized with the init.sql script on the first container build.
Run Inline SQL Queries

You can execute queries inside the running database container without installing PostgreSQL locally:
# Query the users table
docker exec -it app_db psql -U admin -d myapp -c "SELECT * FROM users;"

Interactive Database Terminal

To enter an interactive psql shell inside the running database container:

docker exec -it app_db psql -U admin -d myapp
(Type \q to exit)
<img width="1159" height="407" alt="data example" src="https://github.com/user-attachments/assets/ec4f006f-b790-4ac7-b0ac-6fbfddc6f168" />


<img width="755" height="426" alt="docker com" src="https://github.com/user-attachments/assets/58388f99-1033-42eb-84ce-77d2daa46a88" />

Troubleshooting

    1.Port Already in Use: If port 8080 is taken, edit docker-compose.yml under frontend.ports and map to another host port (e.g., "8081:80").

    2.Database Table Missing: If init.sql was added after your first run, PostgreSQL's initialization entrypoint won't re-execute automatically. Run docker compose down -v to reset the database volume and run docker compose up -d again.

3. If build is not recognized, delete your previous version of docker and reinstall.

