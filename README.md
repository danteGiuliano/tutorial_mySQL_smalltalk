### MySQL_Pharo

## Pequeño tutorial de como usar una base de datos relacional en Pharo

Acerca de mi.

>Hola, que tal. mi nombre es Dante R. Giuliano, soy un novato programador de 23 años de Argentina y adoro Smalltalk con Pharo, eh decidido hacer este tutorial para la comunidad en español, todos sabemos lo dificil que es encontrar informacion de calidad cuando uno no es un programador avanzado. personalmente eh sufrido mucho eso, y lo sigo padeciendo. Pienso que lo mejor de la programacion y de las personas es el compartir conocimiento, por que es lo unico que nos hace mejores personas en el  dia a dia.... me hubiera encantado encontrar este tipo de tutoriales en mi camino de aprendizaje seguramente  hubiera llegado al mismo punto de ahora, pero sin tantos tropiezos... Asi que bueno, espero este tutorial te sea de utilidad :smile: 


## Entorno de trabajo.

deberemos iniciar Pharo en la version 8 , 9 ++. la idea sera usar el driver oficial de **pharo-rdbms** 
este es un repositorio de github donde se encuentran los drivers oficiales para las diferentes bases de datos soportadas en Pharo. si quieres obtener mas informacion puedes acceder aqui <https://github.com/pharo-rdbms>

## Instalar MySQL en Pharo 

Lo que debemos hacer una vez iniciado Pharo  es abrir un playground en Pharo (Ctrl + o + w) y ejecutar el siguiente script Metacello.

```smalltalk
Metacello new
	repository: 'github://pharo-rdbms/Pharo-MySQL';
	baseline: 'MySQL';
	load
```

esto lo que hara, sera clonar el repositorio de github del driver de MySQL, una vez termine de instalar las dependencias necesarias, podemos proseguir con el siguiente paso, mas informacion en <https://github.com/pharo-rdbms/Pharo-MySQL>

## Realizar nuestra primera conexion 

Para realizar nuestra conexion se requiere previamente, tener una instancia de MySQL activa, puede ser local o externa, debemos obtener las credenciales para poder realizar la conexion. en mi caso. use la pagina <https://www.clever-cloud.com/en/> que me ofrece una instancia de MySQL con PhPMyAdmin  y 10 mb de capacidad  al dia 20/06/2021. las credenciales necesarias son

- Host (Direccion ip ej: 198.1.168.0 o amazingShopcleverCloud.com)
- Database Name (Nombre de la base de datos Ej: AmazingShop)
- User (Usuario administrador de la base de datos)
- Password (password )
- Port (Puerto de conexion)

Una vez que tenemos estos datos solamente basta con generar el pequeño script para conectarlo.

### Funcionamiento del  package Pharo-MySQL

Este package se basa en una conexion de dos pasos utilizando dos clases en particular.
MySQLDriverSpec y MySQLDriver

 la clase MySQLDriver es la encargada de generar el objeto que se comunica con la base de datos y ejecutar los querys necesarias  en tanto MySQLDriverSpec  actua como un dependiente o colaborador interno de dicha clase . puesto que ella es la encargada de mantener los **Credenciales** 

### Como seria el script correspondiente? 

bueno, deberemos iniciar un nuevo PlayGround (Ctrl + o + w) y escribir el siguiente script.

```smalltalk
|dt cd|
cd :=MySQLDriverSpec new 
				database:'XXXXXXXXXXXX';
				host:'XXXXXXXXXXXXXXX';
				user:'XXXXXXXXXXX';
				password:'XXXXXXX';
				port:7070.
				
dt  :=MySQLDriver new.
dt  connect:cd.
```

una ves realizado esto,  al ejecutar el codigo debe figurarnos en el inspeccionador "a MySQLOkay" esto nos devolvera informacion relacionada con nuestra base de datos,  como el estado, columnas y filas afectadas etc.

## Como realizar consultas?

Bueno, ahora que ya sabemos como conectar la base de datos. nosotros debemos usar a la clase MySQLDriver para realizar las querys correspondientes. dicha clase contiene en su protocolo de mensajes el mensaje ```query:aQueryString``` este mensaje recibe por parametro un simple string usando el formato de consultas  SQL, si quieres saber mas sobre este lenguajes de Querys puedes acceder aqui <https://www.w3schools.com/sql/default.asp>. 

>  te recomiendo crear una tabla de test en tu base de datos , y ejecutar la query describe tuTabla esto hara un select all de la misma y podras ver como esta compuesta.

### Recomendaciones mias 

Estas recomendaciones las hago desde mis ojos de novato en programacion, si no sabes como tener un acceso sencillo a dichos objetos en tu proyecto te recomiendo considerar una clase llamada DataBase, que mantengan 2 variables de clase. ```database``` y ```credentials``` . Luego crear un mensaje de clase llamado ```runDataBase``` donde haga la inicializacion y mantenga la referencia de los objetos.

```smalltalk
runDataBase
self initializeCredentials.
database := MySQLDriver new connect:credentials.

```

```smalltalk
initializeCredentials
credentials:= MySQLDriverSpec new 
				database:'XXXXXXXXXXXX';
				host:'XXXXXXXXXXXXXXX';
				user:'XXXXXXXXXXX';
				password:'XXXXXXX';
				port:7070.

```

de esta forma cuando apliquemos el mensaje ```DataBase runDataBase``` haremos una conexion y mantendremos la sesion por un periodo de tiempo para las querys. no te olvides de aplicar el mensaje 
```disconect``` para no forzar multiples conexiones en simultaneo, dejo a tu criterio de uso dicha implementacion.



ante cualquier duda, puedes preguntar en la comunidad oficial de Pharo  <https://discord.com/invite/QewZMZa>  



