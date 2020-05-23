# MES
Message Enterprise Server

# Prerequisitos

## Sistema Operativo
* Ubuntu 18.04

## Servidores y herramientas
* Tomcat8
* Postgres10
* openjdk version "1.8.0_242"

## Componentes. 
Debido a que las fuentes no estan almacenadas como proyecto maven en git, no es posible descargar las fuentes desde git y construir los ejecutables con los comandos maven. Los siguientes componentes son provistos por el desarrollador:
### MES Aplicación web (front y backend): 
* /var/lib/tomcat8/webapps/MES.war : empaquetado de la aplicación
* /home/ubuntu/mes/conf/main_mes_conf.properties
* /home/ubuntu/mes/images
### Aplicación de carga de datos
* /home/ubuntu/mes/MESFilesloader.jar : ejecutable de la aplicación
* /home/ubuntu/mes/lib/<librerias de __MESFilesloader__> : librerias
* /home/ubuntu/mes/MESfilesloader.properties 
### Pagina de inicio
* /var/lib/tomcat8/webapps/ROOT/index.xml 

# Pasos a seguir
## Instalar los prerequisitos


### Servidores
```
sudo apt-get install openjdk-8-jre-headless
sudo apt-get install tomcat8
sudo apt-get install Postgres10
```


## Carpetas mandatorias
```
mkdir /home/ubuntu/mes
mkdir /home/ubuntu/mes/loader
mkdir /home/ubuntu/mes/reports
mkdir /home/ubuntu/mes/logs
mkdir /home/ubuntu/mes/conf
mkdir /home/ubuntu/mes/images
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
```
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
```
## Configuraciones
### Sincronizar la hora del servidor con la hora local
```
# Seleccionar 2. America, 84. Lima.
sudo dpkg-reconfigure tzdata

```
### Configuracion de dominios internos
* Las Aplicaciónes estan apuntando a la base de datos __smsdbserver__. Este nombre debe tener su equivalente con la ip del servidor de base de datos. Ejemplo: 

```
sudo echo "smsdbserver 127.0.0.1" >> /etc/hosts
tail /etc/hosts
smsdbserver 127.0.0.1
```
### **Aplicación de carga de datos**
* /home/ubuntu/mes/MESfilesloader.properties : Archivo externo a la Aplicación. Datos de conexion a base de datos __smsdbserver__.

### Aplicación web (front y backend)
* /home/ubuntu/mes/conf/main_mes_conf.properties : Datos de conexion a base de datos __smsdbserver__.

## Configuracion de cron tab
* Programar **Aplicación de carga de datos** para ejecutarse periodicamente cada 9min. Ejemplo:
```
sudo cron tab
*/9 * * * * java -jar /home/ubuntu/mes/MESFilesLoader.jar
```



## Prueba
```
curl -v http://localhost:80/MES
```
Validar que la configuracion de tomcat carga automaticamente el archivo war en la ruta estandar /var/lib/tomcat8/webapps
