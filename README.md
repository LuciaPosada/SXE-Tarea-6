# Taera 6

>explicacion

## Instalación de Docker Compose 

>nota aquí

<details>
 <summary>Descarga e Instalacion</summary>
<br>

```bash
# Instalación mediante repositorio
sudo apt install docker-compose

# Instalación manual
DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
mkdir -p $DOCKER_CONFIG/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.29.6/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
```
 
</details>

<details>
 <summary>Comprobación</summary>
<br>

```bash
docker compose version
```

>imagen aqui

>Salida por consola esperada ↑

</details>

</details>

## Creación de archivo compose.yml

<details>
 <summary>Comprobación</summary>
<br>

```bash
services:

 db:
   image: mariadb
   restart: always
   environment:
     MYSQL_ROOT_PASSWORD: admin
     MYSQL_DATABASE: prestashop
     MYSQL_USER: userPS
     MYSQL_PASSWORD: pwdPS
   ports:
     - "3306:3306"

 prestashop:
   depends_on:
     - db
   image: prestashop/prestashop:8-apache
   ports:
     - "7080:80"
   restart: always
   environment:
     DB_HOST: db
     DB_USER: userPS
     DB_PASSWORD: pwdPS
     DB_NAME: prestashop
```

</details>
