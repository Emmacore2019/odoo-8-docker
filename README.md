# odoo-8-docker
Install odoo 8 on docker 
version: '2'
services:
  
  db:
    container_name: odoo8_db
    image: postgres:9.6-alpine
    ports:
      - "5070:5432"
    environment:
      - POSTGRES_PASSWORD=odoo
      - POSTGRES_USER=odoo
      - POSTGRES_DB=postgres
      - PGDATA=/var/lib/postgresql/data/pgdata 
    volumes:
      - db-data:/var/lib/postgresql/data/pgdata  
    restart: always

  web:
    container_name: odoo8_web
    image: odoo:8.0
    depends_on:
      - db
    ports:
      - "8069:8069"
    tty: true
    command: -- --dev=reload
    volumes:
      - ./addons:/mnt/extra-addons
      - ./etc:/etc/odoo
      - web-data:/var/lib/odoo
    restart: always

volumes:
  db-data:
  web-data:

