![n8n_self_hosting](https://github.com/user-attachments/assets/2373fb4b-6a48-476c-9500-9f4a53507c50)

# n8n Self-Hosted with PostgreSQL & pgAdmin (Docker Setup)

This repository contains a **self-hosted setup of [n8n](https://n8n.io/)** with **PostgreSQL** as the database and **pgAdmin** as the database management GUI.  
It uses **Docker Compose** for orchestration, Docker **volumes** for persistent data, and Docker **networks** for internal communication.

---

## 📌 Features
- **n8n**: Workflow automation platform (self-hosted, no SaaS required).  
- **PostgreSQL**: Relational database to persist n8n workflows and execution data.  
- **pgAdmin**: Web-based GUI to manage and visualize PostgreSQL.  
- **Docker Compose**: Clean orchestration of services.  
- **.env file**: Centralized environment variable management.  
- **Persistent storage**: Using Docker volumes.  
- **Custom network**: Isolated Docker network for inter-service communication.  

---

## 🗂️ Project Structure
n8n-docker-setup/
│── compose.yaml # Docker Compose configuration
│── .env # Environment variables (ignored by Git)
│── .env.example # Template for environment variables
│── .gitignore # Ignore secrets and volumes
│── README.md # Documentation

yaml


---

## ⚙️ Setup Instructions

### 1. Clone the repo
```bash
git clone https://github.com/<your-username>/n8n-docker-setup.git
cd n8n-docker-setup
2. Configure environment variables
Copy .env.example to .env and fill in your values:

bash

cp .env.example .env
Edit .env:

ini
Copy code
# Example
POSTGRES_USER=admin
POSTGRES_PASSWORD=securepassword
POSTGRES_DB=n8n

N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=changeme

PGADMIN_EMAIL=your@email.com
PGADMIN_PASSWORD=pgadminpass
GENERIC_TIMEZONE=Asia/Kolkata
3. Start services
bash

docker compose up -d
4. Access services
n8n → http://localhost:5678

pgAdmin → http://localhost:8080

###
---

🐳 Docker Concepts Learned
While building this project, I learned and practiced:

Docker Volumes:
Used for persistent storage (postgres_data, n8n_data, pgadmin_data).
Ensures data survives container restarts.

Docker Networks:
Created a custom bridge network (n8n_network) for communication between containers.
Services communicate using their container names (postgres, n8n, pgadmin) instead of IPs.

Environment Variables with .env:
Centralized sensitive configs (users, passwords, DB names).
Used .env.example for safe sharing without exposing real secrets.

Service Dependencies:
Used depends_on to ensure postgres starts before n8n and pgadmin.

🛠️ Common Issues & Fixes
1. Error: services.pgadmin.environment must be a mapping
Cause: Wrong syntax in compose.yaml (: used instead of = for env).

Fix: Corrected to:

yaml

environment:
  PGADMIN_DEFAULT_EMAIL: ${PGADMIN_EMAIL}
  PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_PASSWORD}
2. pgAdmin: "Unable to connect to server: Name does not resolve"
Cause: Tried connecting with localhost instead of the Docker service name.

Fix: Use postgres as the hostname inside pgAdmin since services share the same Docker network.

3. CSRF Token Missing in pgAdmin
Cause: Session issue after container restart.

Fix: Refresh browser or clear pgAdmin session cookies.

4. Where is my data stored?
Postgres data → postgres_data volume

n8n config → n8n_data volume

pgAdmin settings → pgadmin_data volume


Deploy this setup to a cloud server (VPS) and expose securely via reverse proxy + SSL (Traefik/Nginx).

📜 License
