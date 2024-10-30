# Tarea 6

En este documento  se especifican los pasos a seguir para la instalación y uso de Docker Compose para la implementación de Prestashop.

## Instalación de Docker Compose 

<details>
 <summary>Descarga e Instalación</summary>
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
    image: prestashop/prestashop:latest         # Especificación de version para la imagen de Prestashop
    environment:
      - PS_DEV_MODE="1"                         # Activación de el modo de desarrollo
      - PS_INSTALL_AUTO="1"                     # Instalación automatica
      - DB_SERVER=mysql                         # Especificación de la base de datos a usar
    ports:
      - "8080:80"                               # Especificación de puerto para el servicio
    depends_on:
      - mysql                                   # Establecimiento de dependencia a la base de datos
    restart: unless-stopped                     # Especificación de reinicio (automático, a menos que el contenedor se detenga a mano)
    networks:
      - prestashop-network                      # Especificación de red interna

  mysql:
    image: mysql:5.7                            # Especificación de version para la imagen de MySQL
    environment:
      - MYSQL_ROOT_PASSWORD=admin               # Contraseña del usuario root
      - MYSQL_DATABASE=prestashop               # Nombre de la base de datos 
      - MYSQL_USER=userPS                       # Nombre de usuario de la base de datos 
      - MYSQL_PASSWORD=pwdPS                    # Contraseña para el usuario
    volumes:
      - db_datos:/var/lib/mysql                 # Especificación de un volumen 
    restart: unless-stopped
    ports:
      - "8000:3306"                             # Especificación de puerto para el servicio DB
    networks:
      - prestashop-network                      # Especificación de red interna

volumes:
  db_datos:                                     # Creación de un volumen persistente

networks:
  prestashop-network:                           # Creación de un red de Docker
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

## Instalación de Prestashop

<details>
 <summary>Acceder a Prestashop</summary>
<br>

```bash
http://<ip>:<puerto_prestashop>
```

> La página resultante deberia ser las siguiente ↓

![Instalación_1](/img/Install_PrestaShop_1.png)

</details>

<details>
 <summary>Configurar parametros de instalación</summary>
<br>

- Rellenar la información pertinente
 
![Instalación_2](/img/Install_PrestaShop_2.png)

- Configurar la base de datos
 
![Instalación_1](/img/Install_PrestaShop_3.png)

> Alternativamente se puede utilizar la ip y el puerto del contenedor que contenga la base de datos ↓
  
![Configuracion_Alt](/img/Install_PrestaShop_3_Aux_1.png)

</details>

<details>
 <summary>Obtener acceso al BackOffice la tienda</summary>
<br>
 
Para poder acceder al "BackOffice" (la interfaz usada por el comercial) de Prestashop es necesario realizar los siguiente pasos por consola para garantizar la seguridad:

```bash
# Eliminar la carpeta install 
docker exec -it <nombre|id contenedorPrestashop> rm -rf /var/www/html/install

# Renombrar la carpeta admin
docker exec -it <nombre|id contenedorPrestashop> mv /var/www/html/admin /var/www/html/<nuevoNombre>
```

> Ejemplo de uso ↓

![Instalación_2](/img/Terminar_Backend.png)

</details>

<details>
 <summary>Comprobación Final</summary>
<br>

```bash
http://<ip>:<puerto_prestashop>
```
 
> Comprobación FrontIffice ↓

![Comprobacion_FrontIffice](/img/Comprobacion_PrestaShop_1.png)

```bash
http://<ip>:<puerto_prestashop>/<nuevoNombreCarpetaAdmin>
```

> Comprobación BackIffice ↓

![Comprobacion_BackOffice](/img/Comprobacion_PrestaShop_2.png)

</details>
