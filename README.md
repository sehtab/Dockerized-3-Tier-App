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
<img width="1136" height="809" alt="3-tier-build-1" src="https://github.com/user-attachments/assets/b483eca0-cc7e-459f-a2cb-9be397419132" />

![image](https://github.com/sehtab/Dockerized-3-Tier-App/blob/a38d3a1a40378f1463a47a805a4dcbb26f28a543/3-tier-build-1.png)
![image alt](https://github.com/sehtab/Dockerized-3-Tier-App/blob/a38d3a1a40378f1463a47a805a4dcbb26f28a543/3-tier-build-2.png)
![image alt](https://github.com/sehtab/Dockerized-3-Tier-App/blob/a38d3a1a40378f1463a47a805a4dcbb26f28a543/docker%20ps-test.png)


Accessing the Application

    Web Interface: Open http://localhost:8080 (or http://localhost:8081 if you changed the host port mapping).

    Backend API Health Endpoint: Open http://localhost:8080/api/health in your browser or test via curl

curl http://localhost:8080/api/health
![image alt](https://github.com/sehtab/Dockerized-3-Tier-App/blob/a38d3a1a40378f1463a47a805a4dcbb26f28a543/Dockerized-3%20tier%20app.png)


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

![image alt](https://github.com/sehtab/Dockerized-3-Tier-App/blob/a38d3a1a40378f1463a47a805a4dcbb26f28a543/data%20example.png)


Troubleshooting

    1.Port Already in Use: If port 8080 is taken, edit docker-compose.yml under frontend.ports and map to another host port (e.g., "8081:80").

    2.Database Table Missing: If init.sql was added after your first run, PostgreSQL's initialization entrypoint won't re-execute automatically. Run docker compose down -v to reset the database volume and run docker compose up -d again.

3. If build is not recognized, delete your previous version of docker and reinstall.

![image alt](https://github.com/sehtab/Dockerized-3-Tier-App/blob/a38d3a1a40378f1463a47a805a4dcbb26f28a543/docker%20com.png)
