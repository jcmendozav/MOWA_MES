# MES
Message Enterprise Server

# Prerequisitos

## Sistema Operativo
* Ubuntu 18

## Servidores y herramientas
* Tomcat8
* Postgres10
* openjdk version "1.8.0_242"
* Docker en caso de realizar la instalacion en un contenedor

## Componentes
* MES Aplicacion web (front y backend): /var/lib/tomcat8/webapps/MES.war 
* Pagina de inicio: /var/lib/tomcat8/webapps/ROOT/index.xml 
* **Aplicacion de carga de datos** MESFilesloader: /home/ubuntu/mes/MESFilesloader.jar
* Librerias para **aplicacion de carga de datos**: /home/ubuntu/mes/lib/<librerias de __MESFilesloader__>

# Pasos a seguir
## Instalar los prerequisitos

### En caso de usar docker, instalar 
```
sudo docker pull ubuntu:18.04
```
### Servidores
```
sudo apt-get install openjdk8
sudo apt-get install Tomcat8
sudo apt-get install Postgres10
```


## Carpetas mandatorias
```
mkdir /home/ubuntu/mes
mkdir /home/ubuntu/mes/loader
mkdir /home/ubuntu/mes/reports
mkdir /home/ubuntu/mes/logs
export home_mes=/home/ubuntu/mes
export home_mes_loader=$home_mes/loader
export home_mes_reports=$home_mes/reports
export home_mes_logs=$home_mes/logs
```

## Permisos
Dar permisos a las siguientes carpetas:
```
sudo chmod -R 777 /home/ubuntu/mes
sudo chmod -R 777 /var/lib/tomcat8/webapps
```

## Redireccion de puertos
* 80->8080 : Realizarlo en contenedor

## Configuraciones
### Sincronizar la hora del servidor con la hora local
```
sudo timedatectl set-timezone America/New_York
```
### Configuracion de dominios internos
* Las aplicaciones estan apuntando a la base de datos __smsdbserver__. Este nombre debe tener su equivalente con la ip del servidor de base de datos. Ejemplo: 

```
sudo echo "smsdbserver 127.0.0.1" >> /etc/hosts
tail /etc/hosts
smsdbserver 127.0.0.1
```
### **Aplicacion de carga de datos**
* /home/ubuntu/mes/MESFilesloader.properties : Archivo externo a la aplicacion. Datos de conexion a base de datos __smsdbserver__.

### Aplicacion web (front y backend)
* hibernate.cfg: Archivo dentro de la aplicacion. Datos de conexion a base de datos __smsdbserver__.
* conexion.java: Archivo dentro de la aplicacion. Datos de conexion a base de datos __smsdbserver__. 

## Configuracion de cron tab
* Programar **aplicacion de carga de datos** para ejecutarse periodicamente cada 9min. Ejemplo:
```
sudo cron tab
*/9 * * * * java -jar /home/ubuntu/mes/MESFilesloader.jar
```

## Prueba
* http://localhost/MES

Validar que la configuracion de tomcat carga automaticamente el archivo war en la ruta estandar /var/lib/tomcat8/webapps
