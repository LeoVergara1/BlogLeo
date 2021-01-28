---
title: "Comparativa entre Ruby, Groovy y Java"
date: 2021-01-01
categories:
- learn
tags:
- Makingdevs
- Experience
- Ruby
- Groovy
- Java
keywords:
- disqus
- learning
- gravatar
- Ruby
- groovy
autoThumbnailImage: false
thumbnailImagePosition: "left"
thumbnailImage: /images/comparative/strong.png
coverImage: /images/comparative/strong.png
metaAlignment: center
---
**Comparativa entre Ruby, Groovy y Java**

La verdad es que esto empieza de una simple curiosidad, actualmente desarrollo un proyecto, en el cual he hecho uso de la concurrencia usando groovy con spring boot, con el paso del desarrollo
el logrado mejorar los resultados sin embargo no me sentí satisfecho con ello en algunos casos, por ejemplo, logró trabajar 2.6 millones de datos en 5 minutos, sin embargo, si los datos son de un tamaño
un poco mayor, el proceso haciende exponencialmente hasta las 2 horas para 1.6 millones de datos, y creo que esto se debe al manejo de los recursos por la JVM.

**Java**

La verdad ya a todos les da flojera porque usa mucha ceremonia para su sintaxis es cierto que con java 8, acelera el desarrollo gracias a los lamdas, aunque he notado que muy pocos desarrolladores, le sacan provecho a eso por lo cual viene a mi mente la pregunta:
¿De que sirve tener Java 15 si las personas siguen desarrollando como un java 7?

Me da a pensar que a las personas les de flojera su amado lenguaje.

**Groovy**

Desde mi perspectiva no es el mejor lenguaje de la historia, pero sí puedo asegurar que es mejor alternativa cuando estas obligado a usar la JVM, ya que hace el desarrollo mas ameno y productivo, resolviendo muchas cosas que JAVA dejo a la deriva.

**Ruby**

Sin lugar a dudas un lenguaje para aprender a programar, hacer cosas muy simples y rápidas, pero también robustas y productivas, si tuviera la libertad de escoger un lenguaje orientado a objetos para un nuevo proyecto lo elijaría.

**Jugando con los lenguajes**

Pruebas de parelelismo:

**JAVA**

<script id="asciicast-LhCN88Hc2YIQ0giy81lRMcZRQ" src="https://asciinema.org/a/LhCN88Hc2YIQ0giy81lRMcZRQ.js" async data-size="small"></script>

**GROOVY**

<script id="asciicast-387674" src="https://asciinema.org/a/387674.js" async></script>

**RUBY**

<script id="asciicast-UC6U5iyBXHO21w3sdOJ9LuolI" src="https://asciinema.org/a/UC6U5iyBXHO21w3sdOJ9LuolI.js" async></script>

**Conclusión**

Aquí podemos ver como claramente ruby (2.7) tiene un mejor manejo del procesamiento y la memoria, en un futuro haré la prueba en ruby(3.0) ya que promete ser 3 veces más eficiente aún así deja de atrás la JVM, por otro lado entre java y groovy notamos que si hay un poco de mejor rendimiento en java sin embargo a lo largo de mi experiencia, groovy resulta ser más rápido en búsquedas e iteraciones que java, además de que con groovy no tendrás problemas en perdidas de hilos y que el lenguaje lo maneja por si sólo, caso que no ocurre con java, basta a ver el código anterior para darse cuenta como java sigue siendo de mucha ceremonia.

¿Que deseas usar un lenguaje elegante (Ruby) o un lenguaje convencional y estricto (Java)?

Recuerda que Groovy trabaja sobre la JVM pero con la elegancia de Ruby!!!
