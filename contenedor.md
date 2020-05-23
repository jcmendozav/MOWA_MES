# MOWA_MES



# Imagen
## Generación de contenedor a partir de la última imagen:
* sudo docker pull soportemowa/mowa:mes_env_tomcat_postgres_latest
## Confirmar últimos cambios en la imagen:
* sudo docker commit <container name> soportemowa/mowa:mes_env_tomcat_postgres_latest
* sudo docker push  soportemowa/mowa:mes_env_tomcat_postgres_latest

# Contenedor:
## Construcción
* sudo docker run -it --network=host  --name <container name>   -v $HOME/container_share:/home/ubuntu/container_share   -d soportemowa/mowa:mes_env_tomcat_postgres_latest

## Acceso
* sudo docker exec -it -u ubuntu -w /home/ubuntu <container name> bash

## Usuarios:
* ubuntu/****
* tomcat/****
* postgres/****

## Servidor web tomcat
### Scripts de despliegue de servicios:
* /home/ubuntu/tomcat_start.sh
* /home/ubuntu/tomcat_stop.sh

### Ruta de servidor:
* /usr/share/tomcat8

## Servidor de base de datos postgres
### Scripts de apagado de servicios:
* /home/ubuntu/postgres_start.sh
* /home/ubuntu/postgres_stop.sh



