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


