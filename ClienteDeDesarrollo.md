# CLIENTE DE DESARROLLO
|  DAW/DWES Tema2 |
|:-----------:|
|![Alt](images/portada.jpg)|
| INSTALACIÓN, CONFIGURACIÓN Y DOCUMENTACIÓN DEL CLIENTE DE DESARROLLO |


- [CLIENTE DE DESARROLLO](#cliente-de-desarrollo)
    - [2 Windows 11](#2-windows-11)
      - [2.1 **Configuración inicial**](#21-configuración-inicial)
        - [**Nombre y configuración de red**](#nombre-y-configuración-de-red)
        - [**Cuentas administradoras**](#cuentas-administradoras)
      - [2.2 **Navegadores**](#22-navegadores)
      - [2.3 **MovaXterm**](#23-movaxterm)
      - [2.4 **Netbeans**](#24-netbeans)
        - [Creacion de proyectos](#creacion-de-proyectos)
        - [Eliminacion de proyectos](#eliminacion-de-proyectos)
        - [Informacion del IDE](#informacion-del-ide)
      - [2.5 **Visual Studio Code**](#25-visual-studio-code)
        - [Como crear un Workspace](#como-crear-un-workspace)
        - [Conexion SFTP con maquina de desarrollo](#conexion-sftp-con-maquina-de-desarrollo)
        - [Control de versiones](#control-de-versiones)
        - [Debug PHP (Xdebug)](#debug-php-xdebug)
        - [Connexion con la BBDD (MariaDB)](#connexion-con-la-bbdd-mariadb)
        - [Informacion del IDE](#informacion-del-ide-1)

### 2 Windows 11
#### 2.1 **Configuración inicial**
##### **Nombre y configuración de red**
##### **Cuentas administradoras**
#### 2.2 **Navegadores**

#### 2.3 **MovaXterm**

> **Versión MobaXterm**: MobaXTerm Portable versión 25.2

> **Enlance de descargar**: https://mobaxterm.mobatek.net/download-home-edition.html

Para crear una nueva sesion de usuario le damos aqui (Session):

![alt text](/images/menuSessionButton.png)


Se abrira un menu para elegir el tipo de conexion.
Elegiremos SSH y pondremos la IP del servidor, marcamos la casilla y ponemos nuestro uruario.

![alt text](/images/CreateNewSessionSSH.png)

Al darle a Ok nos pedira la contraseña. Se la ponemos y ya estaria creada la sesion para poder administrar el servidor.



#### 2.4 **Netbeans**


##### Creacion de proyectos

Lo primero que tendremos que hacer es crear una carpeta en nuestro almacenamiento local con el nombre del proyecto: 

> D:\ProyectosNB\ProyectoPrueba

Lo segundo que debemos hacer es realizar la creación del directorio en MobaXterm:

![Creacion directorio mobaXterm](./images/CreateProjectMoba.png)

En este mismo directorio creamos el index PHP.

![Creacion directorio mobaXterm](./images/indexPruebaMoba.png)

Clicamos en el boton de crear un nuevo proyecto.

![Creación index PHP MobaXterm](./images/menuNewProjectButton.png)

---

Seleccionamos el tipo de proyecto:

![](./images/menuProjectType.png)

---

Seleccionamos el nombre del proyecto y en la carpeta donde se va a guardar


![alt text](./images/projectNameAndLocation.png)

---

Seleccionamos la direccion de nuestro servidor y a que carpeta se subiran los archivos

![alt text](./images/projectRemoteConnection.png)

---

Confirmamos el mensaje que aparece y le damos a finalizar.

![alt text](./images/projectConfirm.png)

Confirmamos la transferencia de archivos

![alt text](./images/confirmación%20de%20creacion%20de%20proyecto%20nb.PNG)

---

Y ya lo tenemos

![alt text](./images/projectEnd.png)


##### Eliminacion de proyectos

Le damos click derecho al proyecto que queramos eliminar y seleccionamos la opción "Delete".

Una vez le damos, nos preguntara si queremos tambien eliminar todos los archivos (locales). Lo seleccionamos si queremos y le damos a "Yes". Para que se elimine.

![alt text](./images/confirmación%20de%20borrado%20Netbeans.png)



##### Informacion del IDE

> **Pagina Oficial**: https://netbeans.apache.org/ \
> **Version**: 20 \
> **Link Descarga Version**: https://netbeans.apache.org/front/main/download/nb20/ \
> **Modulos Instalados**: 0

#### 2.5 **Visual Studio Code**

***Primero que nada, importante instalar estas extensiones. Mas adelante explicare como usar las obligatorias:***

Extensiones **obligatorias**:
[SFTP](https://marketplace.visualstudio.com/items?itemName=Natizyskunk.sftp),
[PHP Extension Pack (Xdebug & Autocompletado avanzado)](https://marketplace.visualstudio.com/items?itemName=xdebug.php-pack),
[PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client),
[SQLTools](https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools),
[MySQL/MariaDB Support for SQLTools](https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools-driver-mysql).

Extensiones **opcionales** pero bastante utiles:
[Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer),
[VirtualBox](https://marketplace.visualstudio.com/items?itemName=acherkashin.virtualbox-extension),
[JsDoc](https://marketplace.visualstudio.com/items?itemName=lllllllqw.jsdoc),
[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
[HTML CSS Intellisense](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css).

##### Como crear un Workspace

Un workspace es una colección de una o más carpetas abiertas en una sola ventana del editor. \
Sirve para organizar proyectos y aplicar configuraciones específicas a cada uno. \
Puedes guardar estas colecciones como un archivo ``.code-workspace`` con formato **JSON** para editar y compartir fácilmente la configuracion. (Muy recomendado para despues)

Para crearlo, primero abrimos una ventana vacia sin ninguna carpeta abierta con ``Ctrl + Shift + N``. \
Una vez abierta, arriba del todo a la izquierda. Le damos a ***File > Add Folder to Workspace...*** \
Nos abrira un explorador. Yo recomiendo que; en el directorio que prefieras, crees una carpeta para el workspace, y dentro se cree una carpeta para cada proyecto. Algo asi:
```
D:\
└── ProyectosWebDAW -> (Carpeta Workspace)
    ├── xxxDAWProyectoDAW
    ├── xxxDWECProyectoDWEC -> (Carpeta proyecto)
    ├── xxxDWESProyectoDWES
    ├── xxxProyectoDAW
    └── ...
```
> *Si ya estan los proyectos creados del netbeans, la carpeta del workspace seria "D:\Proyectos_NetBeans" (O como se llame la carpeta).*

Una vez creadas las carpetas. Seleccionamos las carpetas **DENTRO** de ***ProyectosWebDAW***, no la carpeta ***ProyectosWebDAW*** en si. (Se pueden seleccionar todas a la vez)

Una vez hecho. En el explorador del editor apareceran los diferentes proyectos. 

> **IMPORTANTE**: Ir al ***File > Save Workspace As...*** y guardarlo *(Preferiblemente en la carpeta del workspace)*. Es necesario para algunas configuraciones mas tarde.

##### Conexion SFTP con maquina de desarrollo

En cada proyecto creamos una carpeta llamada ``.vscode/`` y que dentro tenga el archivo ``sftp.json``. (O se hace una vez y copiamos y pegamos la carpeta mas rapido).

El archivo tiene que tener este formato (modificar ``name``, ``host`` y ``remotePath``, y borrar los comentarios para que funcione):
```json
{
  "name": "NombreConexion",
  "context": ".", # Carpeta donde se suben/descargan los archivos en local ('.' = carpeta proyecto)
  "host": "10.199.8.95",
  "username": "operadorweb",
  "password": "paso", # Si no se pone te pregunta cada vez que cierres y abres el editor
  "remotePath": "/var/www/html/PROYECTO", # Carpeta donde se suben/descargan los archivos en el servidor
  "uploadOnSave": true, # Sube archivos automaticamente al modificar
  "ignore": [
      ".git",
      ".DS_Store"
  ],
  "remoteExplorer": {
    "filesExclude": [
      ".git",
      ".DS_Store",
      ".cache",
      ".local",
      ".cache",
      ".bash_history",
      ".bashrc"
    ]
  }
}
```

Para subir/descargar archivos manualmente, podemos seleccionar el archivo en cuestion, la carpeta o el proyecto entero y darle a:
- ``Sync Local -> Remote``: Para pasar lo del local a remoto.
- ``Sync Remote -> Local``: Para pasar lo del remoto a local.
- ``Sync Both Directions``: Para pasar hacer ambos a la vez.

*(Para pasar imagenes o documentos externos se le tiene que dar manualmente siempre)*

##### Control de versiones

Para abir el panel para el control de versiones, en la barra lateral buscamos un icono con un círculo dividido con ramas; o hacemos ``Ctrl + Shift + G``.

Desde hay podemos controlar todos los git de cada proyecto (Hacer commits, cambiar ramas, crear y añadir tags, gestionar stashes, ...)

Estaria bien incluir esto en los ``.gitignore`` del proyecto:
```
.bash_history
.bashrc
.wget-hsts
.cache/
.dotnet/
.local/
.ssh/
.vscode-server/
.vscode/

*.code-workspace
nbproject/
```

##### Debug PHP (Xdebug)

Para debuggear PHP con Xdebug, primero, ir a la configuracion y buscar "**php.debug.idekey**" y ponerle el valor "**netbeans-xdebug**".

Y despues en el archivo ``.code-workspace`` al nivel de **"folders"** poner este launch *(cambiando la ruta remota y nombre del directorio local por la que corresponda)*:
```json
{
  "folders": [
    ...
  ],
  "launch": {
		"version": "0.2.0",
		"configurations": [
			{
				"name": "Listen for Xdebug",
				"type": "php",
				"request": "launch",
				"port": 9003,
				"stopOnEntry": false,
				"pathMappings": {
					"/var/www/html": "${workspaceFolder:xxxProyectoDAW}",
					"/var/www/html/xxxDAWProyectoDAW":    "${workspaceFolder:xxxDAWProyectoDAW}",
					"/var/www/html/xxxDWECProyectoDWEC":  "${workspaceFolder:xxxDWECProyectoDWEC}",
					"/var/www/html/xxxDWESProyectoDWES":  "${workspaceFolder:xxxDWESProyectoDWES}",
					"/var/www/html/xxxDWESProyectoTema3": "${workspaceFolder:xxxDWESProyectoTema3}",
					"/var/www/html/xxxDWESProyectoTema4": "${workspaceFolder:xxxDWESProyectoTema4}",
					"/var/www/html/xxxCIBProyectoCiberseguridad": "${workspaceFolder:xxxCIBProyectoCiberseguridad}"
				},
				"xdebugSettings": {
					"max_data": 2048,
				}
			}
		]
	},
}
```

Una vez hecho esto, vamos al apartado de debug de vscode buscando el icono con el triangulo de play con un insecto al lado; o presionando ``Ctrl + Shift + D``. \
Si le damos al Run **Listen for Xdebug** 

##### Connexion con la BBDD (MariaDB)

Buscamos en la barra lateral el icono de un cilindro.

Una vez en el, en el apartado ***CONNECTIONS*** le damos a ***Add New Connection***, se nos abrira un apartado para elegir la DB que queramos usar. Seleccionaremos **MariaDB**.

Se abrira aun formulario par completar la configuracion de la connexion. Lo rellenamos con los datos necesarios. (Para el usuario administrador de la base de datos, en el `Database` ponemos **mysql** para que tenga acceso a todas las BBDD)

Para ejecutar consultas, primero verificamos cual es la connexion acctiva con la que haremos la consulta hacemos clic aqui y seleccionamos la que queramos:
![alt text](./images/vscode/DBStatusBar.png)

( ⚠️ Sin terminar ⚠️ )


##### Informacion del IDE

> **Pagina Oficial**: https://code.visualstudio.com/ \
> **Version**: Ultima version (Actualizada automaticamente) \
> **Link Descarga**: https://code.visualstudio.com/Download \
> **Extensiones Instaladas**: Las de arriba
