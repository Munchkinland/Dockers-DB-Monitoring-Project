## Docker Compose Setup for Data Management and Monitoring
# Project Description

This project sets up a comprehensive development environment using Docker Compose, integrating multiple services essential for data management, visualization, and monitoring. The environment includes:

PostgreSQL: A powerful, open-source relational database management system for storing and managing data.
Portainer: A user-friendly graphical interface for managing Docker containers, allowing you to monitor and control your containers and images.
Grafana: A versatile platform for creating, sharing, and visualizing metrics and data through dashboards and graphs.
Metabase: A tool for exploring and analyzing data with ease, and generating reports.
Prometheus: A robust monitoring and alerting toolkit designed for collecting and storing metrics, particularly from containerized applications.
Node Exporter: A Prometheus exporter that collects system-level metrics, such as CPU and memory usage, from the host system.
pgAdmin: A graphical interface for managing PostgreSQL databases, simplifying tasks such as database creation, management, and query execution.
This setup helps in monitoring and managing the health and performance of your applications and infrastructure, ensuring data is well-organized and insights are easily accessible.

#Importance of Monitoring
Effective monitoring is crucial in modern development and operations for several reasons:

Performance Optimization: Monitoring allows you to track the performance of your applications and systems, helping to identify bottlenecks and optimize resource usage.
Early Issue Detection: By continuously observing system metrics, you can detect and address issues before they escalate into serious problems.
Reliability and Stability: Monitoring helps ensure that your systems remain reliable and stable, reducing downtime and improving overall system health.
Data-Driven Decisions: With access to accurate and timely data, you can make informed decisions to enhance system performance and efficiency.
Compliance and Auditing: Monitoring provides a record of system activity, which is useful for compliance and auditing purposes.

#Prerequisites
Before running this Docker Compose setup, ensure you have the following installed:

Docker Desktop: Docker Desktop is necessary to manage Docker containers on your local machine. It provides the Docker engine, Docker CLI, and Docker Compose. Download and install Docker Desktop from the official Docker website.

Docker Compose: Docker Compose is included with Docker Desktop, so installing Docker Desktop will also install Docker Compose. It allows you to define and run multi-container Docker applications.

requirements.txt
For the services defined in this docker-compose.yml, you don't need a requirements.txt file as it is used for Python projects to list dependencies. However, if you were to list the Docker images and their versions used in the project, it would look something like this:


postgres:latest
portainer/portainer-ce:latest
grafana/grafana:latest
metabase/metabase:latest
prom/prometheus:latest
prom/node-exporter:latest
dpage/pgadmin4:latest
This file is not strictly necessary for this Docker Compose setup but might be useful if you want to keep track of the versions of the Docker images used.

Version
version: '3.9'
Services
PostgreSQL
Image: postgres:latest
Container Name: postgres
Environment Variables:
POSTGRES_USER: admin
POSTGRES_PASSWORD: admin123
POSTGRES_DB: mydb
Volumes:
./data/postgres_data:/var/lib/postgresql/data
Ports:
5432:5432
Networks: españa_vaciada_interna
Portainer
Image: portainer/portainer-ce:latest
Container Name: portainer
Command: -H unix:///var/run/docker.sock
Volumes:
/var/run/docker.sock:/var/run/docker.sock
./data/portainer_data:/data
Ports:
9000:9000
Networks: españa_vaciada_interna
Grafana
Image: grafana/grafana:latest
Container Name: grafana
Environment Variables:
GF_SECURITY_ADMIN_USER: admin
GF_SECURITY_ADMIN_PASSWORD: admin123
Volumes:
./data/grafana_data:/var/lib/grafana
./grafana/provisioning:/etc/grafana/provisioning
Ports:
3000:3000
Networks: españa_vaciada_interna
Metabase
Image: metabase/metabase:latest
Container Name: metabase
Environment Variables:
MB_DB_TYPE: postgres
MB_DB_DBNAME: mydb
MB_DB_PORT: 5432
MB_DB_USER: admin
MB_DB_PASS: admin123
MB_DB_HOST: postgres
Ports:
3001:3000
Depends On:
postgres
Networks: españa_vaciada_interna
Prometheus
Image: prom/prometheus:latest
Container Name: prometheus
Volumes:
./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
Ports:
9090:9090
Networks: españa_vaciada_interna
Node Exporter
Image: prom/node-exporter:latest
Container Name: node-exporter
Ports:
9100:9100
Networks: españa_vaciada_interna
PID: host
pgAdmin
Image: dpage/pgadmin4:latest
Container Name: pgadmin
Environment Variables:
PGADMIN_DEFAULT_EMAIL: admin@admin.com
PGADMIN_DEFAULT_PASSWORD: admin123
Ports:
8080:80
Depends On:
postgres
Networks: españa_vaciada_interna
Volumes
postgres_data: For PostgreSQL data
portainer_data: For Portainer data
grafana_data: For Grafana data
Networks
españa_vaciada_interna
Driver: bridge
IPAM Config:
Subnet: 192.168.10.0/24
españa_vaciada_externa
External: true
Usage
Start the environment:

# Commands
docker-compose up -d
Stop the environment:

docker-compose down
Access Services:

PostgreSQL: Port 5432
Portainer: http://localhost:9000
Grafana: http://localhost:3000
Metabase: http://localhost:3001
Prometheus: http://localhost:9090
Node Exporter: http://localhost:9100
pgAdmin: http://localhost:8080

# Notes

Ensure Docker and Docker Compose are installed on your machine before running these commands.
Adjust environment variables and volume paths as needed for your specific setup.
Feel free to modify this configuration to better fit your needs or to include additional services!