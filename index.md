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


![2](https://user-images.githubusercontent.com/104896936/170863508-c7d7f5d7-d90e-4a11-8c78-6745efa09321.jpg)


![1](https://user-images.githubusercontent.com/104896936/170863485-10c95d59-bc02-4a78-a4e4-15a1eb2242f3.jpg)



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


Tenemos ahora que instalar Nagios NRPE que va a jugar un papel importante en nuestro entorno de monitorizacion.

Nagios NRPE nos permite ejectutar plugings/scripts en maquinas remotas.

## Instalacion de Nagios NRPE

````
sudo apt install -y monitoring-plugins nagios-nrpe-plugin
````
Lo que consulta Nagios NRPE:
- CPU
- Memoria
- Disco
- Servicios
- Usuarios
- Procesos

Una vez tenemos instalado NRPE, nos crea un directorio en la ruta que esta abajo. con una serie de plugins/chequeos para poder configurarlos.

````
sudo ls /etc/nagios-plugins/config/
````
![5](https://user-images.githubusercontent.com/104896936/170863213-b596345e-78ad-4c68-bac1-4a29088caefc.jpg)

<<<<<<< HEAD:index.md


=======
## Agregar Cliente
### En este caso agregaremos una maquina windows para poder configurarla.

para ello entramos en la siguiente ruta y fichero

````
/usr/local/nagios/etc/objects 
````
y Hacemos un sudo nano:
````
sudo nano windows.cfg
````

**Cuando entremos tendremos que asignarle la ip de nuestro ordenador cliente**.

como en la siguiente imagen:
![6](https://user-images.githubusercontent.com/104896936/170864446-4e1995ad-db00-455c-ae4d-5a4c330be546.jpg)

Luego entramos en el fichero de "*nagios.cfg"*

Para descomentarlo como en la siguiente imagen:
![7](https://user-images.githubusercontent.com/104896936/170864680-686cb775-1e6b-4253-bd9b-82fe388c5daf.jpg)

Ahora hay que asignarle el fichero de configuracion para que se aplique a nagios con el siguiente comando:
````
sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
````

y deberian aparacer dos host como en la siguiente imagen:


![8](https://user-images.githubusercontent.com/104896936/170865065-0bd2ca34-ec69-4d35-a22a-919cff337322.jpg)

Luego comprobamos en nuestra web:

![9](https://user-images.githubusercontent.com/104896936/170865182-c7126550-0ae7-463d-807b-370bc0cc1a8e.jpg)
 
 **Podemos ver que ya hay dos Hosts** :smiley:
 
 ## Instalacion de ***NSCLient++***
Esto lo instalaremos en nuestra maquina cliente. Solo buscamos ***NSCLient++*** en un buscador y lo descargamos de la pagina web
>>>>>>> 2588e2ce2a9ee1b10d03e3bc9f42ef879179d099:prueba.md



![10](https://user-images.githubusercontent.com/104896936/170866257-d9fbc49c-2098-4073-8cbd-6112cdee202d.jpg)

Luego de la Instalacion tenemos podemos ya monitorizar nuestra maquina cliente Windows. :bowtie:


![11](https://user-images.githubusercontent.com/104896936/170866755-9da8a4fe-e36d-40da-b1fa-6d70b632e2be.jpg)

