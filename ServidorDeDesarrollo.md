|  |
|:-----------:|
|![Alt](images/portada.jpg)|
| INSTALACIÓN, CONFIGURACIÓN Y DOCUMENTACIÓN DEL SERVIDOR DE DESARROLLO |

- [1. Entorno de Desarrollo](#1-entorno-de-desarrollo)
  - [1.1 Ubuntu Server 24.04.3 LTS](#11-ubuntu-server-24043-lts)
    - [1.1.1 **Configuración inicial**](#111-configuración-inicial)
      - [Nombre y configuración de red](#nombre-y-configuración-de-red)
      - [**Actualizar el sistema**](#actualizar-el-sistema)
      - [**Configuración fecha y hora**](#configuración-fecha-y-hora)
      - [**Cuentas administradoras**](#cuentas-administradoras)
      - [**Cortafuegos (UFW)**](#cortafuegos-ufw)
        - [Instalacion](#instalacion)
        - [Configuracion](#configuracion)
        - [Monitorizacion](#monitorizacion)
        - [Mantenimiento](#mantenimiento)
      - [**SSH**](#ssh)
        - [Instalacion](#instalacion-1)
        - [Configuracion](#configuracion-1)
        - [Monitorizacion](#monitorizacion-1)
        - [Mantenimiento](#mantenimiento-1)
      - [**Antivirus**](#antivirus)
        - [Instalacion](#instalacion-2)
        - [Configuracion](#configuracion-2)
        - [Monitorizacion](#monitorizacion-2)
        - [Mantenimiento](#mantenimiento-2)
    - [1.1.2 Servidor web (Apache)](#112-servidor-web-apache)
      - [Instalación](#instalación)
      - [Configuracion](#configuracion-3)
      - [Monitorizacion](#monitorizacion-3)
      - [Mantenimiento](#mantenimiento-3)
  - [Verficación del servicio](#verficación-del-servicio)
      - [Virtual Hosts](#virtual-hosts)
      - [Permisos y usuarios](#permisos-y-usuarios)
      - [HTTPS](#https)
    - [1.1.3 PHP-FPM](#113-php-fpm)
      - [Instalacion](#instalacion-3)
      - [Configuracion](#configuracion-4)
  - [**Activarlo para cada virtualhost**](#activarlo-para-cada-virtualhost)
      - [Monitorizacion](#monitorizacion-4)
      - [Mantenimiento](#mantenimiento-4)
    - [1.1.4 MariaDB](#114-mariadb)
      - [Instalacion](#instalacion-4)
      - [Configuracion](#configuracion-5)
      - [Monitorizacion](#monitorizacion-5)
      - [Mantenimiento](#mantenimiento-5)
    - [1.1.5 XDebug](#115-xdebug)
      - [Instalacion](#instalacion-5)
      - [Configuracion](#configuracion-6)
      - [Monitorizacion](#monitorizacion-6)
      - [Mantenimiento](#mantenimiento-6)
    - [1.1.6 DNS](#116-dns)
    - [1.1.7 SFTP](#117-sftp)
    - [1.1.8 Apache Tomcat](#118-apache-tomcat)
    - [1.1.9 LDAP](#119-ldap)


## 1. Entorno de Desarrollo

### 1.1 Ubuntu Server 24.04.3 LTS

Este documento es una guía detallada del proceso de instalación y configuración de un servidor de aplicaciones en Ubuntu Server utilizando Apache, con soporte PHP y MySQL

#### 1.1.1 **Configuración inicial**

##### Nombre y configuración de red

> **Nombre de la máquina**: daw-used\
> **Memoria RAM**: 2G\
> **Particiones**: 150G(/) y resto (/var)\
> **Configuración de red interface**: enp0s3 \
> **Dirección IP** :10.199.10.22/22\
> **GW**: 10.199.8.1/22\
> **DNS**: 10.151.123.21 y 10.151.126.21

Para comprobar esto usamos:
```bash
hostname    # Para ver el nombre de la maquina
ip a        # Para ver la IP y interface
ip r        # Par ver la IP, interface y gateway
resolvectl  # Para ver el DNS
df -h       # Para ver las particiones
fdisk -l    # # Para ver las particiones (fromato mas limpio)
cat /etc/os-release # Ver la verison del SO
```
Para saber que sistema operativo se tiene.
```bash
uname -a
```

Para saber la versión,
```bash
lsb_release -a
```
---

Cambiamos el nombre de la maquina con:
```bash
sudo hostnamectl set-hostname <nombre>
sudo nano /etc/hosts
```

Para que cambie, en el prompt, hay que cerrar sessión.
```bash
exit
```

* Para ver Interfaces de red y sus direcciones IP:
```bash
ip a
```
* Para ver la tabla de enrutamiento.
```bash
ip r
```
* Para saber el DNS del servidor.
```bash
resolvectl status
```

* Para comprobar las particiones: 
  vista jerárquica (disco → particiones → puntos de montaje)
```bash
lsblk
```
o 
```bash
df -h
```
o una vista completa del sistema de archivos + permisos.
```bash
lsblk -fm
```
o mostrar todos los dispositivos, incluso los vacíos o no usados
```bash
lsblk -a
```
o mostrar solo los nombres, sin formato visual
```bash
lsblk -fn
```
o listar particiones con detalles del disco físico
```bash
fdisk -l
```



Editar el fichero de configuración del interface de red  **/etc/netplan**,
* Para configurar la red de interface:
  Se hace una copia de seguridad del archivo de configuración que se encuentra en /etc/netplan. 

```bash
cd /etc/netplan
sudo cp 50-cloud-init.yaml 50-cloud-init.yaml.backup
```

* Para cambiar el nombre del archivo
```bash
sudo mv 50-cloud-init.yaml enp0s3.yaml
```

* Y se edita el fichero /etc/netplan


```bash
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      addresses:
       - 10.199.10.98/22
      nameservers:
         addresses:
         - 10.151.123.21
         - 10.151.126.21
      routes:
          - to: default
             via: 10.199.8.1
         search: [educa.jcyl.es]
  version: 2
````

* Para aplicar la configuración
```bash
sudo netplan apply
```

##### **Actualizar el sistema**

```bash
sudo apt update && sudo apt upgrade -y
```

##### **Configuración fecha y hora**

```bash
# Comprobamos la fecha con:
date
```
En caso de que este mal hacemos esto: [Establecer fecha, hora y zona horaria](https://somebooks.es/establecer-la-fecha-hora-y-zona-horaria-en-la-terminal-de-ubuntu-20-04-lts/ "Cambiar fecha y hora")

```bash
# Ponemos la zona horaria correcta para cambiar la hora
sudo timedatectl set-timezone Europe/Madrid

# Comprobamos que la fecha y/o hora han cambiado con:
date
```

##### **Cuentas administradoras**

> - [X] root(inicio)
> - [X] miadmin/paso
> - [X] miadmin2/paso

La cuenta ``root`` viene por defecto. Las otras dos las creamos con el comando:
```bash
sudo adduser <nombre_usuario>
# Te pedira contraseña, y el resto de cosas serán opcionales (teléfono, correo, etc)

# Para hacerlo administrador usamos:
sudo usermod -aG sudo,adm,cdrom,dip,plugdev,lxd <nombre_usuario>
```

o con:
```bash
sudo useradd -m -d </ruta/del/home> -s </bin/bash> -G <grupo/s> <usuario>
sudo passwd <usuario> # Para poner la contraseña
sudo chown -R <usuario>:<grupo> </ruta/del/home> # Para cambiar el dueño de la carpeta home
```

Para cambiar entre usuarios hacemos:
```bash
sudo su - <nombre_usuario>
```

* Para ver en que grupo está el usuario
```bash
cat /etc/group | grep miadmin
```
* Para ver los usuarios, y saber su carpeta shell (grep es para filtrar)
```bash
cat /etc/passwd | grep nombreUsuario
```

* Para crear un usuario con una shell concreta
```bash
sudo usermod -s /bin/bash miadmin
```

##### **Cortafuegos (UFW)**

###### Instalacion
```bash
sudo apt update
sudo apt install ufw     # Instalamos UFW si no está instalado
```
###### Configuracion
```bash
sudo ufw enable          # Activamos el cortafuegos
sudo ufw allow 22        # Abrimos el puerto 22 (SSH)
```

Para eliminar reglas específicas (por ejemplo IPv6 o cualquier otra):

```bash
sudo ufw status numbered  # Mostramos reglas con número
sudo ufw delete <numero_regla>        # Eliminamos la regla con el número correspondiente
```
###### Monitorizacion
```bash
sudo ufw status verbose    # Mostramos el estado detallado del cortafuegos y las reglas activas
```
###### Mantenimiento
```bash
sudo ufw disable           # Desactivamos el cortafuegos temporalmente
sudo ufw reset             # Reseteamos todas las reglas a la configuración inicial
```

##### **SSH**

###### Instalacion
```bash
sudo apt update
sudo apt install openssh-server   # Instalamos el servidor SSH
```
###### Configuracion
```bash
sudo nano /etc/ssh/sshd_config   # Archivo de configuración SSH
sudo systemctl restart ssh       # Reiniciamos el servicio para aplicar cambios si hacemos
```
###### Monitorizacion
```bash
sudo systemctl status ssh        # Comprobamos el estado del servicio
```
###### Mantenimiento
```bash
sudo systemctl enable ssh         # Habilitamos que SSH se inicie al arrancar
sudo systemctl disable ssh        # Deshabilitamos el inicio automático si se necesita
sudo systemctl restart ssh        # Reiniciamos el servicio si hay problemas
```


##### **Antivirus**

###### Instalacion
Instalaremos el Antivirus `ClamAV`:
```bash
sudo apt update && sudo apt install -y clamav
```

###### Configuracion
Si no quieres cambiar nada, puedes verificar la configuración.
```bash
cat /etc/clamav/clamd.conf      # Mostramos la configuración del demonio
cat /etc/clamav/freshclam.conf  # Mostramos la configuración de actualizaciones
```

Si quieres actualizar la base de datos de los virus:
```bash
sudo systemctl stop clamav-freshclam   # Detenemos el servicio de actualizaciones
sudo freshclam                         # Actualizamos la base de datos de virus
sudo systemctl start clamav-freshclam  # Volvemos a iniciar el servicio
```
###### Monitorizacion
```bash
sudo systemctl status clamav-daemon    # Comprobamos el estado del servicio ClamAV
sudo systemctl status clamav-freshclam # Comprobamos el estado del servicio de actualizaciones
```
###### Mantenimiento
```bash
sudo systemctl enable clamav-daemon     # Habilitamos el demonio al inicio del sistema
sudo systemctl disable clamav-daemon    # Deshabilitamos el inicio automático si se necesita
sudo systemctl restart clamav-daemon    # Reiniciamos el servicio si hay problemas
```

#### 1.1.2 Servidor web (Apache)

Servidor web de código abierto que gestiona y entrega páginas a los usuarios. \
Permite configurar sitios, manejar peticiones HTTP/HTTPS y servir contenido dinámico y estático. \
Es compatible con módulos y lenguajes como PHP, ofreciendo gran flexibilidad y personalización.

##### Instalación
```bash
sudo apt update
sudo apt install apache2 -y   # Instalamos Apache
```

* Verificar el estado del servicio
```bash
sudo systemctl status apache2
```
* Se abre el puerto 80
```bash
sudo ufw allow 80
```
* Se borra el puerto 80 v6
```bash
sudo ufw status numbered
```
```bash
sudo ufw delete numeroproceso
```



##### Configuracion

* Se crea un directorio de errores. 

```bash
sudo mkdir /var/www/html/error
sudo touch /var/www/html/error/error.log
```
* Y hay que indicarlo en el /etc/apache2/sites-available/000-default, ya antes haremos una copia por si surje algún imprevisto. 
```bash
sudo cp 000-default.conf 000-default.conf.backup
```

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```


ErrorLog /var/www/html/error/error.log

![Alt](images/apache2000DefaultError.png)


* Modificar apache2.conf para .htaccess
```bash
sudo nano /etc/apache2/apache2.conf
```
Buscar la sección 
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

Y cambiar a 
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>

![Alt](images/apache2Conf.png)

Para aplicar los cambios despues de editar usamos:
```bash
sudo apache2ctl configtest      # Comprobamos que no de errores = "Syntax OK"
sudo systemctl restart apache2  # Reiniciamos el servicio para aplicar cambios
```
Abrimos el puerto 80:
```bash
sudo ufw allow 80         # Abrimos el puerto 80 (HTTP)
sudo ufw status numbered  # Mostramos reglas con número
sudo ufw delete <numero_regla>  # Eliminamos la regla IPv6
```
##### Monitorizacion
```bash
sudo systemctl status apache2   # Comprobamos si Apache está activo
sudo ufw status | grep "80"     # Verificamos que el puerto 80 está escuchando
```
##### Mantenimiento
```bash
sudo systemctl start apache2     # Iniciamos el servicio si está detenido
sudo systemctl stop apache2      # Detenemos el servicio
sudo systemctl restart apache2   # Reiniciamos el servicio
sudo systemctl enable apache2    # Habilitamos el inicio automático al arrancar
sudo systemctl disable apache2   # Deshabilitamos el inicio automático si se necesita

```

---

<!-- documentar "/etc/apache2/" y subcarpetas -->
Los archivos de configuracion de Apache se encuentran en **``/etc/apache2/``**:
  - **``apache2.conf``**: es el archivo de configuracion inicial. Es el primer fichero que se ejecuta cuando arrancamos el servidor.
  - **``ports.conf``**: donde se definen los puertos en los que Apache escuchará las conexiones
  - **``mods-available/``**: Contiene todos los módulos de Apache que están instalados en el sistema.
  - **``mods-enabled/``**: Contiene enlaces simbólicos a los módulos de mods-available/ que están activos, es decir, cargados y funcionando en el servidor.
  - **``conf-available/``**: Almacena archivos de configuración global mediante enlaces simbólicos influyendo en la configuración general del servidor.
  - **``conf-enabled/``**: Contiene enlaces simbólicos a los archivos de conf-available/ que están activos, aplicando su configuración al servidor.
  - **``sites-available/``**: Guarda archivos de configuración de sitios virtuales mediante enlaces simbólicos, permitiendo configurar diferentes sitios alojados en el mismo servidor
  - **``sites-enabled/``**: Contiene enlaces simbólicos a los archivos de sites-available/ que están activos, habilitando los sitios virtuales correspondientes.

---

Para comprobar que los cambios en la documentacion funcionnan correctamente usamos:
```bash
sudo apache2ctl configtest
```


### Verficación del servicio
* Comprobar si se puede ver el index de Apache2
```bash
sudo nano /var/www/html/index.html
```
En el navegador se puede con la URL:http//IPServidor/index.html


##### Virtual Hosts
##### Permisos y usuarios

> - [X] operadorweb/paso
> - [X] operadorweb2/paso
> - [X] operadorweb3/paso

Creamos un usuario llamado `operadorweb` que tenga el grupo `www-data`.

```bash
# Creamos el usuario
sudo useradd -m -d /var/www/html/ -s /bin/bash -g www-data operadorweb

# Le ponemos contraseña
sudo passwd operadorweb


# Cambiamos el propietario de su carpeta home con:
sudo chown -R operadorweb:www-data /var/www/html/

# Y los permisos con:
sudo chmod -R 775 /var/www/html
```
* Información de los usuarios
```bash
id operadorweb
```
o
```bash
cat /etc/passwd | grep operador
```

Para cambiar de contraseña
```bash
sudo passwd operadorweb
```

Para cambiar el grupo del propietario (www-data (para web))
```bash
sudo chown -R operadorweb:www-data /var/www/html
```
Para borrar un usuario de un grupo
```bash
sudo gpasswd -d nombreusuario nombregrupo
```
Para cambiar permisos
```bash
sudo chmod -R 775 /var/www/html
```
* Para borrar un usuario
```bash
sudo deluser nombreusuario
```
Y habilitamos el puerto 80 en el UFW si no esta ya.

##### HTTPS

Creación de los certificados SSL en Apache.

![alt text](/images/imagenModeloHTTPS.png)


Generamos un certificado autofirmado y su clave privada (válido 1 año), y rellenamos la info que nos pide.
```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ahf-used.key -out /etc/ssl/certs/ahf-used.crt
```
Rellenamos la información solicitada

```bash
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:ZAMORA
Locality Name (eg, city) []:BENAVENTE
Organization Name (eg, company) [Internet Widgits Pty Ltd]:IES LOS SAUCES
Organizational Unit Name (eg, section) []:INFORMATICA
Common Name (e.g. server FQDN or YOUR name) []:ahf-used
Email Address []:alejandro.huefer@educa.jcyl.es
```

Comprobamos que se han creado correctamente los certificados:
```bash
sudo ls -la /etc/ssl/certs/   | grep ahf-used
sudo ls -la /etc/ssl/private/ | grep ahf-used
```

Habilitamnos el modulo ssh, y configuramos un sitio para que lo use.

```bash
sudo a2enmod ssl
sudo systemctl restart apache2
cd /etc/apache2/sites-available/
sudo cp default-ssl.conf ahf-used.conf
sudo nano ahf-used.conf
```


![alt text](/images/apacheSiteConfgHTTPS.png)

Habilitamos el sitio y reiniciamos apache para aplicar cambios.
```bash
sudo a2ensite ahf-used.conf
sudo systemctl reload apache2
```

Para terminar, abrimos el puerto 443 para permitir HTTPS.
```bash
sudo ufw allow 443
```
Borramos la regla 443 (V6) que se ha creado.

```bash
sudo ufw status numbered # Mostramos reglas numeradas

sudo ufw delete <numero_regla> # Borramos la regla
```

#### 1.1.3 PHP-FPM

Gestor de procesos para ejecutar PHP de forma eficiente. \
Permite procesar múltiples peticiones simultáneamente, mejorando el rendimiento de servidores web. \
Se integra con servidores como Nginx o Apache para servir páginas PHP de manera rápida y estable.

https://www.php.net/manual/es/install.fpm.php

##### Instalacion

```bash
# --- Actualizar paquetes y preparar el sistema ---
# Se asegura que todos los paquetes estén actualizados y se instalan
# herramientas necesarias para añadir modulos externos.
```
```bash
sudo apt install php8.3-fpm php8.3
```
```bash
# Reiniciamos el servicio php8.3-fpm
```
```bash
sudo systemctl restart php8.3-fpm

```

##### Configuracion
Para habilitar los modulos Proxy_FCGI y SetEnvif
```bash
sudo a2enmod proxy_fcgi setenvif
```

### **Activarlo para cada virtualhost**

Para que se comunique entre php y el apache
 
Se pone esta expresion en el archivo /etc/apache2/sites-available/000-default.conf

```bash
  ProxyPassMatch ^/(.*\.php)$ unix:/run/php/php8.3-fpm.sock|fcgi://127.0.0.1/var/www/html
```
![Alt](images/apache2000DefaultPHP.png)

  
Por último activamos (o comprobamos que esta activado):

```bash
sudo a2enconf php8.3-fpm
```
El archivo principal de configuración de PHP-FPM se encuentra en ``/etc/php/8.3/fpm/php.ini``.

Hacemos una copia de seguridad de `php.ini` y despues lo editamos cambiando estos valores:
```bash
cp php.ini php.ini.backup
```

![](/images/php.ini_errors.png)
![](/images/php.ini_memory.png)

Y reiniciamos el servicio para aplicar los cambios a la configuracion con:
```bash
sudo systemctl restart php8.3-fpm
```

##### Monitorizacion

```bash
sudo systemctl status php8.3-fpm   # Verifica el estado de PHP-FPM
php -v    # Comprobamos la versión de PHP
php -m    # Lista los módulos activos
```

##### Mantenimiento

```bash
sudo systemctl start php8.3-fpm      # Inicia el servicio
sudo systemctl stop php8.3-fpm       # Detiene el servicio
sudo systemctl restart php8.3-fpm    # Reinicia el servicio
sudo systemctl enable php8.3-fpm     # Habilita inicio automático al arrancar
sudo systemctl disable php8.3-fpm    # Deshabilita inicio automático
```

#### 1.1.4 MariaDB

Sistema de gestión de bases de datos relacional y de código abierto, compatible con MySQL. \
Permite almacenar, consultar y gestionar datos de forma segura y eficiente. \
Se puede acceder desde aplicaciones y IDEs mediante conexión local o remota usando el puerto **3306**.

##### Instalacion
Instalamos con:
```bash
sudo apt update
sudo apt install mariadb-server -y  # Instalamos el servidor MariaDB
```

##### Configuracion
El archivo principal de configuración se encuentra en:
```
/etc/mysql/mariadb.conf.d/50-server.cnf
```
Configuración del acceso remoto:

Esto permitirá conectarse a la base de datos MariaDB desde otros equipos. Para habilitar el acceso remoto al servidor mariadb desde otros equipos, debes modificar el fichero de configuración.

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Editamos la línea del ``bind-address`` para permitir conexiones desde cualquier IP (por defecto solo permite localhost), cambiandolo de ``127.0.0.1`` por ``0.0.0.0`` para permitir todas las conexiones externas.

Reiniciamos el servidor MariaDB:


```bash
sudo systemctl restart mariadb
```

Entramos en MariaDB para crear un nuevo usuario administrador con ``sudo mariadb`` \
Y creamos el usuario con:
```sql
GRANT ALL ON *.* TO 'adminsql'@'%' IDENTIFIED BY 'paso' WITH GRANT OPTION;
```
Comprobamos el puerto usado por el servidor Mariadb (es tcp/3306 por defecto).

1. Usando comandos del sistema:

```bash
sudo ss -punta |grep mariadb

tcp   LISTEN  0  80  127.0.0.1:3306   0.0.0.0:*   users:(("mariadbd",pid=1234,fd=10))

```
2. Usando consola de Mariadb:

Entrar al cliente:

```bash
sudo mariadb
```

Luego ejecuta:

```bash
SHOW VARIABLES LIKE 'port';
```

Nos muestra como resultado:

| Variable_name | Value |
| --------------- | ------ |
| port          | 3306  |

Ejecutamos el asistente de seguridad:
```bash
sudo mysql_secure_installation   # Configuramos contraseña root y opciones de seguridad
```

* En el primer paso preguntará por la contraseña de `root` para MariaDB, pulsa la tecla `Enter` ya que no hay contraseña definida.
* La siguiente, preguntará si quieres asignar una contraseña para el usuario “root”. Es recomendable usar una contraseña.
* En el tercer paso preguntará si quieres eliminar `usuario anónimo`, aquí indica que `Sí` quieres borrar los datos.
* Después preguntará si quieres desactivar el acceso remoto del usuario “root”, aquí indica que `Sí` quieres desactivar acceso remoto para usuario por seguridad.
* De nuevo preguntará si quieres eliminar la base de datos `test`, aquí indica de nuevo que Sí quieres borrar las base de datos de prueba.
* Por último, preguntará si quieres recargar privilegios, aquí indica que `Sí`.

Reiniciamos el servicio para aplicar los cambios:
```bash
sudo systemctl restart mariadb
```

Para permitir que el servidor web pueda conectarse con la DB tenemos que instalar este mudulo php y reiniciarlo:
```bash
sudo apt install php8.3-mysql
sudo systemctl restart php8.3-fpm
```

##### Monitorizacion

Verificamos la IP y el puerto que está utilizando MariaDB:
```bash
sudo ss -punta | grep mariadb   # Muestra conexiones activas y puertos usados por MariaDB
```

Comprobamos el estado del servicio:
```bash
sudo systemctl status mariadb
```

##### Mantenimiento
| **Acción**                         | **Comando**                      | **Descripción**                                              |
| ---------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| **Iniciar el servicio**            | `sudo systemctl start mariadb`   | Inicia el servidor MariaDB.                                  |
| **Detener el servicio**            | `sudo systemctl stop mariadb`    | Detiene el servidor MariaDB.                                 |
| **Reiniciar el servicio**          | `sudo systemctl restart mariadb` | Reinicia el servidor.                                        |
| **Ver estado del servicio**        | `sudo systemctl status mariadb`  | Muestra si el servidor está activo o inactivo.               |
| **Habilitar inicio automático**    | `sudo systemctl enable mariadb`  | Configura el servicio para iniciarse al arrancar el sistema. |
| **Deshabilitar inicio automático** | `sudo systemctl disable mariadb` | Evita que el servicio se inicie automáticamente.             |
| **Ver versión instalada**          | `mariadb --version`              | Muestra la versión actual de MariaDB instalada.              |

Comprobamos si PHP detecta los módulos de MySQL/MariaDB:
```bash
sudo php -m | grep mysql
```

#### 1.1.5 XDebug

Es un módulo de PHP que permite depurar y analizar el código de forma más sencilla. \
Permite inspeccionar variables, pausar la ejecución y seguir el flujo del programa paso a paso. \
Se integra con IDEs mediante el puerto **9003** para depuración remota.

##### Instalacion

```bash
sudo apt update
sudo apt install php8.3-xdebug -y   # Instalamos XDebug para PHP 8.3
```

Verificamos que XDebug está activo:
```bash
sudo php -v | grep Xdebug   # Con la X mayuscula; sino no aparece
```

##### Configuracion

El archivo principal de configuración se encuentra en:
```bash
/etc/php/8.3/fpm/conf.d/20-xdebug.ini:
```

Y tiene que tener esto:
```bash
zend_extension=xdebug.so
xdebug.mode=develop,debug
xdebug.start_with_request=yes
xdebug.client_port=9003
xdebug.log=/tmp/xdebug.log
xdebug.log_level=7
xdebug.idekey="netbeans-xdebug"
xdebug.discover_client_host=1
```

Reiniciamos PHP-FPM para aplicar los cambios:
```bash
sudo systemctl restart php8.3-fpm
```

##### Monitorizacion

Verificamos que XDebug está cargado:
```bash
php -m | grep xdebug
```

Podemos revisar el log para errores o avisos:
```bash
cat /tmp/xdebug.log
```

##### Mantenimiento

```bash
sudo systemctl start php8.3-fpm      # Inicia el servicio PHP-FPM
sudo systemctl stop php8.3-fpm       # Detiene el servicio
sudo systemctl restart php8.3-fpm    # Reinicia el servicio
sudo systemctl enable php8.3-fpm     # Habilita inicio automático
sudo systemctl disable php8.3-fpm    # Deshabilita inicio automático
```

#### 1.1.6 DNS
#### 1.1.7 SFTP
#### 1.1.8 Apache Tomcat
#### 1.1.9 LDAP