+++
date = "2017-02-20T09:16:40-06:00"
title = "Base de Datos en Android (Java SQLiteHelper)"
menu = "main"
Categories = ["Development","GoLang"]
Tags = ["Development","golang"]
Description = ""

+++

Primero debemos saber que es muy importante realizar ciertas abstracciones de nuestro código, implementando interfaces lo que hará de nuestra aplicación de mejor calidad.

Por buenas practicas yo decidí crear un esquema de nuestra base de datos en una clase a diferencia de los demás sitios que por lo regular no hacen, la cual nombre como "AccountDBSchema"

![Estructura](/images/esquemaBase.png)

Cómo se logra observar es una simple estructura de una base de datos sencilla.

Ahora proseguire a crear la base de datos de la siguiente manera.
El String DATABASE_NAME se indicará el nombre de la base de datos, la versión será la 1 y por ùltimo en la parte que dice String SQL_CREATE_ENTRIES, colocaremos el formato en sql para la creación de la tabla, en la imagen se muestran dos formas de hacerlo pero nosotros ocuparemos la primera, en donde se hace referencia a nuestra clase estructura, falta con ver las dos imágenes y lograremos entender cómo es que está funcionando la referencia a la estructura ya antes creada.
Para dar un poco mas de detalles estamos haciendo referencia a la clase después a  una subclase estática que tiene como nombre un string y posteriormente las columnas.

Por último en el constructor de nuestra clase Helper colocaremos los datos antes definidos que son la, DATABASE_NAME, y la versión.

![Creación](/images/SeCreaBase.png)

No pongamos atención al String que dice llamar "segundo" ya que no  es importante lo use para ciertas pruebas de compilación.
Siguiendo con la explicación como se puede ver en la imagen se realiza una clase para el helper como referencia y le heredamos los atributos de la clase SQLiteOpenHelper en el momento de la herencia nos marcara un error que dá significado a que necesitamos unas clase SQLite para compilar, que son los de "onCreate", "onUpgrade".
No se logra apreciar de manera correcta el constructor así que lo coloco nuevamente:
public AccountBaseHelper(Context context){
    super(context, DATABASE_NAME, null, VERSION);}
En el cual estamos indicando el contexto y así mismo pasando los parámetros anteriores ya definidos que son la DATABASE_NAME y la VERSIÓN

En el siguiente método llamado onCreate mandaremos el String con la instrucción sql para crear la tabla y no hay más detalles.
Ahora en el método de onUpgrade enviaremos una instrucción sql en la cual mencionaremos que si no existe la tabla la creé, así como se muestra en la imagen.

Explicaré rápidamente como se realizaron los siguientes archivos.
Se implementa una interfaz la cual nos manejara los metodos necesarios para manejar nuestra base de datos, de momento solo necesitaremos el método de añadir ya que la finalidad es saber como hacer todo el proceso de conexión, una vez sabiendo eso, el resto es simplemente SQL o en su defecto podemos implementar cursores.

![Creación](/images/Interface.png)

En esta imagen estoy definiendo la interface que voy a implementar. cabe resaltar que solo usare el metodo de "databaseAdd" de momento para la explicación.

Como se puede observar se encuentra en la "Package Services" para una mejor estructura, posteriormente se crea una clase que se encargara de implementar los metodos de la interface que realizamos. La cual lleva el nombre de "ManejaInterface"

![Creación](/images/EstructuraCodigo.png)

Es importante saber que cuando creamos la clase que implementa la interface, nos marcara un error de compilación debido a que debemos implementar todos los metodos de la interface, como se puede apreciar en la imagen. Lo más importante de esto es tener bien en claro cómo va funcionar el contexto de la aplicación ya que como repito lo necesitaremos en todo momento, en la imagen podemos hacer acercaciones para visualizar descripciones por las líneas de código.
Se logra observar que tenemos dos constructores uno encargado de obtener el contexto de la aplicación para posteriormente pasarlo al helper de la base de datos, ya que esta la requiere para su correcto funcionamiento.

![Creación](/images/AnadirBase.png)

En esta imagen se realiza toda la función de insert, tengamos muy en claro cómo se lleva acabo el acceso a la base de datos y es muy importante no perder el hilo, para entender cómo llegamos a los datos de ella. Usamos el COntenValues crear un elemento con el cual logremos enviar los datos capturados a la base de datos por medio de la instrucción "Put" y despues ultilizamos una función del sql llamada Insert la cual se encarga de realizar la inserción en la base de datos indicando la tabla y los elementos del elemento.