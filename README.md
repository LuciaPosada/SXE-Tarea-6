# Tarea 6

>explicacion

## Instalación de Docker Compose 

>nota aquí

<details>
 <summary>Descarga e Instalacion</summary>
<br>

- Instalación mediante repositorio
  
```bash
sudo apt install docker-compose
```

- Instalación manual
  
```bash
# Define la ubicación de la configuracion de Docker
DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}

# Crea la carpeta cli-plugins en el directorio de configuración de Docker
mkdir -p $DOCKER_CONFIG/cli-plugins

# Descarga la versión mas reciente de compose
curl -SL https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
```
---
</details>

> [!NOTE]
> Para poder instalar el plugin mediante los repositorios de Ubuntu, tambien es necesario haber instalado Docker a través de los repositorios oficiales

<details>
 <summary>Comprobación</summary>
<br>

```bash
docker compose version
```

![Comprobación de Compose](/img/Comprobación_Compose.png)

>Salida por consola esperada ↑

</details>

## Creación de archivo compose.yml

<details>
 <summary>Crear carpeta y archivo</summary>
<br>

```bash
# Montar una carpeta para almacenar el archivo compose.yml
mkdir composePS

# Colocarse en la carpeta recien creada
cd composePS

# Creación del archivo compose.yml
nano docker-compose.yml
```
 
</details>

<details>
 <summary>Archivos</summary>
<br>

<details>
 <summary>Basico</summary>
<br>

```bash
services:

 db:
   image: mariadb
   restart: no
   environment:
     MYSQL_ROOT_PASSWORD: admin
     MYSQL_DATABASE: prestashop
     MYSQL_USER: userPS
     MYSQL_PASSWORD: pwdPS

 prestashop:
   depends_on:
     - db
   image: prestashop/prestashop:8-apache
   ports:
     - "7080:80"
   restart: no
```

</details>

<details>
 <summary>Completo</summary>
<br>

```bash                                                                
services:
  prestashop:
    image: prestashop/prestashop:latest
    environment:
      - PS_DEV_MODE="1"
      - PS_INSTALL_AUTO="1"
      - DB_SERVER=mysql
    ports:
      - "8080:80"
    depends_on:
      - mysql
    restart: no
    networks:
      - prestashop-network

  mysql:
    image: mysql:5.7
    environment:
      - MYSQL_ROOT_PASSWORD=admin
      - MYSQL_DATABASE=prestashop
      - MYSQL_USER=userPS
      - MYSQL_PASSWORD=pwdPS
    volumes:
      - db_datos:/var/lib/mysql
    restart: no
    ports:
      - "8000:3306"
    networks:
      - prestashop-network

volumes:
  db_datos:

networks:
  prestashop-network:
```

</details>

---
</details>

## Manejo de los contenedores 

<details>
 <summary>Creación</summary>
<br>

```bash
docker compose up -d
```

</details>

<details>
 <summary>Detener</summary>
<br>

```bash
docker compose stop
```

</details>

<details>
 <summary>Eliminar</summary>
<br>

```bash
docker compose down
```

</details>
