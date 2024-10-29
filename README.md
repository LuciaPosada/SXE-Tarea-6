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

