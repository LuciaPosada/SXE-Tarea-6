# Tarea 6

>explicacion

## Instalación de Docker Compose 

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

>Salida por consola esperada ↓

![Comprobación de Compose](/img/Comprobación_Compose.png)

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
 <summary>Ejemplos de archivos</summary>
<br>

<details>
 <summary>Basico</summary>
<br>

```bash
services:

 db:
   image: mariadb
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
    restart: unless-stopped
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
    restart: unless-stopped
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
 <summary>Crear</summary>
<br>

```bash
# Ejecución en segundo plano (deja libre la terminal)
docker compose up -d

# Ejecución en primer plano  (muestra el log de los contenedores)
docker compose up 
```

> Ejemplos de salida por consola tras ejecutar el comando ↓

![Composer_Up_Ejemplo](/img/Composer_Up_Basico.png)

![Composer_Up_Ejemplo](/img/Composer_Up_Completo.png)

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

---
</details>

> [!NOTE]
> Es necesario situarse en el directorio donde se encuentra el archivo docker-compose.yml para ejecutar los comandos

