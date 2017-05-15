---
title: "Experiencia con Wonder Code"
date: 2017-05-14
categories:
- projects
- releases
tags:
- hexo theme
- responsive
- gravatar
- disqus
- google analytics
keywords:
- disqus
- google
- gravatar
autoThumbnailImage: false
thumbnailImagePosition: "top"
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
