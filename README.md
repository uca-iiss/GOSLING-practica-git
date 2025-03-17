# Entregable: Docker Compose para Drupal y WordPress

## **Parte 1: Drupal + MySQL**

Este `docker-compose.yml` configura **Drupal** con una base de datos **MySQL**. Ambos servicios se comunican mediante una red personalizada.

### **📌 Servicios Configurados**
- **MySQL**: Se usa la imagen oficial `mysql:5.7`.
- **Drupal**: Se usa la imagen oficial `drupal:latest` y se expone en el puerto `81`.
- **Red `drupal_network`** para la comunicación interna.
- **Volúmenes** para persistencia de datos.

### **📌 Código `docker-compose.yml`**
```yaml
services:
  mysql:
    image: mysql:5.7
    container_name: mysql_drupal
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: drupal_db
      MYSQL_USER: drupal_user
      MYSQL_PASSWORD: drupal_password
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - drupal_network

  drupal:
    image: drupal:latest
    container_name: drupal_server
    restart: always
    ports:
      - "81:80"
    environment:
      DRUPAL_DB_HOST: mysql
      DRUPAL_DB_USER: drupal_user
      DRUPAL_DB_PASSWORD: drupal_password
      DRUPAL_DB_NAME: drupal_db
    volumes:
      - drupal_data:/var/www/html
    networks:
      - drupal_network

volumes:
  mysql_data:
  drupal_data:

networks:
  drupal_network:
```

### **📌 Pasos para ejecutar**
1. Verificar la configuración:
   ```bash
   docker compose config
   ```
2. Iniciar los servicios:
   ```bash
   docker compose up -d
   ```
3. Verificar los contenedores en ejecución:
   ```bash
   docker ps
   ```
4. Acceder a **Drupal** en el navegador:
   ```
   http://localhost:81
   ```

---

## **Parte 2: WordPress + MariaDB**

En esta sección, configuramos **WordPress** con **MariaDB** y los conectamos a una red llamada `redDocker`.

### **📌 Servicios Configurados**
- **MariaDB**: Se usa la imagen oficial `mariadb:latest`.
- **WordPress**: Se usa la imagen oficial `wordpress:latest` y se expone en el puerto `82`.
- **Red `redDocker`** para la comunicación interna.
- **Volúmenes** para persistencia de datos.

### **📌 Código `docker-compose.yml`**
```yaml
services:
  mariadb:
    image: mariadb:latest
    container_name: mariadb_wp
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress_db
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: wp_password
    volumes:
      - mariadb_data:/var/lib/mysql
    networks:
      - redDocker

  wordpress:
    image: wordpress:latest
    container_name: wordpress_server
    restart: always
    ports:
      - "82:80"
    environment:
      WORDPRESS_DB_HOST: mariadb
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wp_password
      WORDPRESS_DB_NAME: wordpress_db
    volumes:
      - wordpress_data:/var/www/html
    networks:
      - redDocker

volumes:
  mariadb_data:
  wordpress_data:

networks:
  redDocker:
```

### **📌 Pasos para ejecutar**
1. Verificar la configuración:
   ```bash
   docker compose config
   ```
2. Iniciar los servicios:
   ```bash
   docker compose up -d
   ```
3. Verificar los contenedores en ejecución:
   ```bash
   docker ps
   ```
4. Acceder a **WordPress** en el navegador:
   ```
   http://localhost:82
   ```

---
