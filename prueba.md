# NagiosGuide
## Pasos para instalar Nagios y Monotorizacion.
primero tenemos que saber que tenemos algunos Pre-requisitos:
- PHP
- LAMP
- Wget
- Apache2
- PHP repo
- Crear un Usuario y Grupo

### PHP Instalacion:
añadimos la repo con el siguiente comando:

``` 
sudo add-apt-repository ppa:ondrej/php
```
Ahora toca todos los componentes de PHP Y MIBs con el siguiente comando:

````
sudo add-apt-repository ppa:ondrej/php
````
## Instalacion de Nagios
### **Descargaremos Nagios 4.4.5**

empezaremos con el siguiente comando:
`````
cd /tmp/
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.5.tar.gz
tar xzf nagios-4.4.5.tar.gz
``````
vamos a la carpeta de *tmp/nagios-4.4.5/*
````
cd /tmp/nagios-4.4.5/
````
y metemos el siguiente comando para compilar nagios: 

````
sudo ./configure --with-httpd-conf=/etc/apache2/sites-enabled
````
luego este comando:
````
sudo make all
````

Para crear Usuarios y grupos lanzamos los siguiente comandos:

````
sudo make install-groups-users
sudo usermod -a -G nagios www-data
````
Instalamos los Binarios:
````
make install
````
### luego instalamos el servicio Daemon:
````
sudo make install-daemoninit
````
Luego de hacer todo esto pasamos a instalar ficheros de configuraciones.

En este paso vamos a instalar los ficheros con configuraciones a modo ejemplo.
***Estos son necesarios para que podemos iniciar Nagios.***
````
sudo make install-config
````
## Instalamos los ficheros de configuracion de Apache:
En este paso se instalan los ficheros de configuracion del servidor web Apache y lo configura.
````
sudo make install-webconf
sudo a2enmod rewrite
sudo a2enmod cgi
````
Configuramos el firewall: 
````
sudo ufw allow Apache
sudo ufw reload
````
Crear el usuario "nagiosadmin" en mi caso yo en la contraseña puse lo mismo que en el usuario...

Tiramos el siguiente comando y luego ponemos nuestro usuario y contraseña:

````
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
````
Iniciamos los servicios   Apache y Nagios:

````
sudo systemctl restart apache2.service
sudo systemctl start nagios.service
````
Ya podriamos acceder a nuestro nagios a traves de cualquier navegador web poniendo la IP de nuestro ordenador :smiley:

![imagen pero no se como



## Nagios plugins
### **Este paso es para arreglar los problemas que teneos con los hosts.**
Necesitamos algunos Prerequisitos:
- autoconf
- gcc
- libmcrypt-dev
- wget
- bc
- dc 
- gawk
- build essential
- gettext
- libnet-snmp-perl

Con el siguiente comando instalamos las dependencias:
````
sudo apt install libssl-dev gcc libc6 libmcrypt-dev wget bc gawk dc build-essential snmp gettext libnet-snmp-perl -y
````

nos metemos en la carpeta de ***"tmp"***

y clonamos el nagios plugins para descargarlo con el siguiente comando dentro de la la carpeta ***tmp***.

````
git clone https://github.com/nagios-plugins/nagios-plugins/
cd nagios-plugins
````
Dejemos que se descargue y luego utilizamos esta serie de comandos para que se instale:
````
./tools/setup
./configure --prefix=/usr/local/nagios --with-cgiurl=/nagios/cgi-bin
sudo make
sudo make install
sudo make install-root
````
**Ya deberiamos de tener todo listo para empezar la parte de monitorizacion**.



