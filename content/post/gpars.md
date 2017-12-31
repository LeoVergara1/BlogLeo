
---
title: "Gpars Paralelismo con Groovy (Java)"
date: 2017-12-30
categories:
- Tutorial
- Gpars
- Help
- Groovy
- Java
tags:
- Makingdevs
- Experience
- Java
- Groovy
- Gpars
keywords:
- disqus
- google
- gravatar- Java
- Groovy
- Gpars
- Hepl
- Parallel
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /images/gpars/groovyTitle.png
coverImage: /images/gpars/groovyFos.png 
metaAlignment: center
---

Una forma sencilla de manejar el paralelismo en groovy (Java)

<!--more-->

Como ya sabemos uno de los principales problemas de la programación, es el hecho de que la mayoría de las funciones son ejecutadas con forma de sincronía siempre resolviendo uno por uno, en este post no profundizare sobre ningún tema en especial, pero si buscare causarte la inquietud sobre como una vez más groovy es la mejor alternativa si no te quieres despegar de java jeje.

Ahora imagina que tienes un problema donde necesitas hacer mil peticiones, y piensas que no hay problema porque el servicio web contesta muy rápido, suponiendo que tienes una super maquina que lo resolvería en 5 minutos, ahora plantea 1 millón de peticiones creo que la persona se cansara de esperar a que termine el proceso jeje, lo primero que se vendrá a tu mente sera el manejos de hilos en java, aunque es una soluciones no suele ser muy fácil aplicarlo ya que siempre tendremos un gran problema a la hora de ver los resultados, ya que perdemos el comportamiento de los mismos.

Y parece sencillo entender el comportamiento de los hilos, ya que solo tenemos que verlos como procesos independientes.

Ahora Gpars se basa en el modelo de actores el cual nos dice que cada objeto debe de verse como un actor, el cual sera una entidad que se constituirá por cola de mensajes o buzón y un comportamiento, pero si desean saber mas sobre esto aquí les dejo la referencia: http://www.gpars.org/webapp/quickstart/index.html

Sin mas concepto empiezo con un sencillo ejemplo de uno de los grandes potenciales de GPARS y como así de una manera tan sencilla podemos manejar paralelismo en nuestras aplicaciones.

Primero creamos una lista con los siguientes números

 
``` java
lista = [5000,100,200,300,400,100,900,300,6000]
// Cada elemento de la lista representará un tiempo en milisegundos
```

Posterior a esto crearemos un "whitPool('Instancias o hilos')" para esto debemos tener bien conocido la potencia de nuestro equipo ya que de esta forma trataremos de usar todo su potencial.


``` java
withPool(10){}
```

dentro del withPool necesitamos hacer un eachParallel para que este sepa que puede hacer concurrencia en el método

Para que esto sea interpretado no debemos olvidad incluir la librería de gpars.


``` java
import static groovyx.gpars.GParsPool.withPool
```

Ahora pasemos a los ejemplos:

Forma convencional (Sincronía)


<img src="/images/gpars/simpleExample.PNG">

En este primer ejemplo observamos que el tiempo total fue de 13363 mili segundos - > 13 segundos  ya que todo fue ejecutado de forma sincrónica. ahora pasaremos al primer ejemplo con gpars para mirar los resultados.


<img src="/images/gpars/gparsEachParallel.PNG">

Como podemos ver el tiempo se redujo el doble con este sencillo ejemplo, aunque aquí no sabemos cuantos hilos fueron creados en este ultimo ejemplo indicaremos en numero en el pool que sea el mismo numero de elementos de la lista.


<img src="/images/gpars/gparsEachParallelPool.PNG">

Aquí podemos observar como cada uno de los elementos de la lista fue procesado de manera independiente obteniendo como resultado final 6.3 segundos y como podemos observar hay elementos en la lista que son los que marcan el mayor tiempo como esta el caso del primero en el cual son 5 segundo y el ultimo que son 6 segundos.

Por ultimo mostraré el mismo ejemplo pero viendo el control de un hilo con un sencillo paso mas.

<img src="/images/gpars/gparsEachParallelPool1.PNG">

Sin más les dejo el link del código:

https://github.com/LeoVergara1/exampleGpars

Conclusión

Tal vez este es un caso muy sencillo pero si dejan volar a su imaginación, imaginen a cuantos problemas de concurrencia podemos darle soluciones, leyendo y entendiendo bien lo que es GPARS sin embargo no lo veo como la única solución ya que para ello existe otra forma como lo es vertx el cual también se enfoca en el tema de modelos de actores, pero eso sera otro tema.
Finalmente creo que esto podemos dar un gran impulso a nuestras aplicaciones en pequeños algoritmos que a veces requieran bastante tiempo y hacer la implementación ya que siempre demos pensar en la escala de la aplicación cuando es usada por una infinidad de usuarios.

Agradecimiento

Realmente estoy seguro que el gran impulso que ha tenido mi aprendizaje es gracias al equipo de makingdevs donde nuestro mentor es Juan Reyes Zúñiga @neodevelop

http://makingdevs.com/

