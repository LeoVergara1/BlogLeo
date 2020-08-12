---
title: "Experiencia con Wonder Code"
date: 2017-05-14
categories:
- projects
- releases
tags:
- responsive
- Experience
keywords:
- disqus
- google
- gravatar
autoThumbnailImage: false
thumbnailImagePosition: "left"
thumbnailImage: images/wonder2.png
coverImage: /images/wonder.png
metaAlignment: center
---

<!--more-->
Me gustaría mencionar, que al inicio de esta experiencia estaba un tanto emocionado, aunque con un poco de preocupación por la complejidad que pudiera tener la aplicación para ser desarrollada o en dado caso los retos generados en el transcurso de la aplicación.

Una vez que se confirma el desarrollo de la aplicación, había un gran reto ya que teníamos que realizarla en menos de 3 semanas. La aplicación consta de los siguiente:  Hacer una página con el diseño de la mujer maravilla, así como sus poster, trailer de la película etc. pero el verdadero reto estaba en desarrollar una sección llamada  "Wonder code" la cual se encargaría de impulsar la tecnología en las niñas con un promedio de edad, 9 a 13 años, ya que actualmente menos del 20 % de de los graduados en esta área son mujeres, dicha aplicación realizaría la simulación de la estructura básica para crear una funcionalidad, que son:

-  Objeto
-  Diseño
-  Variables (Tamaño, Color)
-  Figuras

La primera intención es que las niñas tomaran un Objeto como normalmente lo hacemos en un lenguaje orientado a objetos y así mismo este consta de atributos, propiedades y metodos para ir moldeando su funcionalidad.
<img src="/images/wonder/estructura.png" style="width: 50%; height: 50%">

Una ves arrastrado dicho elemento se haría la creación del objeto, en este caso un brazalete de la mujer maravilla.


<img src="/images/wonder/obetoNew.png" style="width: 70%; height: 70%">

Posterior a esto tendremos las opciones de agregar una etiqueta con el nombre de diseño en el cual podremos insertar variables y es aquí cuando los cambios se verán en tiempo real, de esta manera la niña lograra ver como , modificando los atributos de estas variables, cambian las propiedades del objeto, viéndose así reflejadas en la proyección del brazalete.


<img src="/images/wonder/objetoVar.png" style="width: 80%; height: 80%">

Como se logra apreciar en esta imagen, el objeto a sido modificado según los gustos y los cambios se ven reflejados al instante.
Por ultimo las niñas tendrán la oportunidad de elegir una figura para su brazalete y así hacerlo única, a según las propiedades que ellas eligieron para crearla.


<img src="/images/wonder/shapes.png" style="width: 50%; height: 50%">

Para esta sección, hay algo en particular que consta de lo siguientes, cualquiera de las figuras será plasmada en el brazalete, pero para el caso de la letra tendrá una segunda funcionalidad donde podrán elegir una letra en especifico y es aquí cuando se observa la  función de un método.


<img src="/images/wonder/brazaFull.png" style="width: 70%; height: 70%">

De igual manera se puede eliminar todas las etiquetas y estas se verán reflejadas de la siguiente manera.


<img src="/images/wonder/gifbrazaleteTrash.gif" style="width: 80%; height: 80%">

Aquí se logra apreciar como, la funcionalidad del boto de basura es regresar todos los elementos a su estado original, también se pueden eliminar uno por uno para modificar las propiedades del brazalete.

Una vez terminado todo este proceso se habilita un botón, el cual nos dará el acceso a una siguiente sección donde veremos las opciones de descargar nuestro brazalete generado para impresión en 3D o en otro caso solo descargar una imagen del mismo, otras de las funciones serán, la de compartir en google+, twitter y Facebook.

# Retos de la aplicación:

- Proyección en 3D de el brazalete (Three.js).
- Drag and Drop para pc, móvil y tablet.
- Función responsiva para todos los tipos de pantalla.
- Programación de cada elemento, para que realicen lo deseado.

# Proyección en 3D.

Dicho proyecto fue realizado haciendo equipo con mi compañero Gamma así mismo dividiendo las tareas del proyecto, en el transcurso de la investigación encontramos un plugin llamado THREE.js el cual tiene un cierto grado de complejidad ya que como bases necesitas tener un buen conocimiento en  geometría del espacio para entender como se harán las proyecciones de los elementos ya que estas se basan en una plano cartesiano de 3 ejes en donde se toma mucho en cuenta el foco, el eje de la figura, y la cámara para la proyección, no profundizare mucho es tema pues ya realizare un post solo para esta tecnología.
En lo personal al principio fue un poco difícil entender todas las características del plugin para hacer un buen uso de él, pero con un poco de dedicación y lectora a su documentación, se logro realizar lo requerido, que era proyectar el brazalete.

# Drag and Drop para pc, móvil y tablet.

Para esta parte nos ayudamos de otro plugin llamado “JQueryUI” él cual nos facilito la funcionalidad para hacer la parte que nos permite arrastrar los elementos a una área en especifico, cabe mencionar que con este plugin no se logra hacer que la aplicación funcione en los dispositivos móviles, y de momento creíamos que resultaría mas difícil cumplir con esta funcionalidad pero basto que buscar un poco en la internet y encontramos nuevamente un plugin llamado “JQueryUI-Touch” la cual se encarga de registrar el evento del touch, en los dispositivos móviles.

# Función responsiva para todos los tipos de pantalla.

Aquíiento satisfecho de lo logrado y feliz de verlo publicado en una página oficial donde todo mundo puede interactuar con la aplicación.lo complicado no era saber como hacerlo si no el tiempo y código que se requería para realizar esta parte, se podría decir que solo fue un momento de muy “talachudo” como diríamos vulgarmente 😃 ya que se tenía que cuidar el correcto funcionamiento de la aplicación en cualquier pantalla.

# Programación de cada elemento, para que realicen lo deseado.

Lo nombro así, para hablar un poco sobre las complicaciones que se tuvo para realizar dicha aplicación, como por ejemplo:

- Que las figuras no se salieran de los limites.
- Los elementos reconocieran donde están ubicados y en función a ello supieran interactuar con los demás.
- Cambiar de nuevo sus ambientes en el momento que se eliminan.
- Crear una area DRAG, DROP y una PREVIW (proyección).
- Que todos los elementos se conozcan, sepan de su existencia y en función a eso tengan ciertas propiedades.
- La proyección trabaja en efecto a los elementos que están en el área DROP y así mismo cuando no estén o sean cambiados
- Despliegue del menu para ocultar los elementos que están contenidos.

La verdad es que varias de estas tarea son las mas destacables entre muchas otras y algunas ocasionaron dolores de cabeza, pero con tiempo y dedicación de puede lograr.

Por ultimo quiero mencionar que en general este proyecto me gusto mucho ya que me deja un gran aprendizaje y una experiencia en nuevas tecnologías, de igual manera el trato que se lleva con el cliente y como este puede ir cambiando los requerimientos al momento que se va realizando la aplicación, es curioso a veces el concepto que ellos llegan a tener sobre la complejidad de realizar una aplicación ya que pareciera que solo es decirle a la computadora “Haz esto” y magiacamiente lo haga, pero así entendí que también es importante saber transmitir al cliente  el proceso por el que tenemos que pasar para lograr cumplir con sus requerimientos.

Me siento satisfecho de lo logrado y feliz de verlo publicado en una página oficial donde todo mundo puede interactuar con la aplicación.

Link de la aplicación: http://www.wondercode.com.mx
