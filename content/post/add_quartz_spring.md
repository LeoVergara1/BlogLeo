---
title: "Quartz Scheduler  con spring (Java)"
date: 2017-10-05
categories:
- projects
- releases
- Help
- Quartz
- Java
tags:
- Makingdevs
- Experience
- Java
- Spring
keywords:
- disqus
- google
- gravatar
- Java
- Quartz
- Spring
- Hepl
autoThumbnailImage: false
thumbnailImagePosition: "left"
thumbnailImage: /images/Quartz/quartz.png
coverImage: /images/Quartz/java.png
metaAlignment: center
---

Tarea que se ejecutan automáticamente en algún momento del día.


<!--more-->
Quartz nos sirve para cuando queremos ejecutar ciertas tareas en algún momento de manera automática, en este caso se usaran las expresiones cron, para darle seguimiento.

Al final del post dejo algunas referencias para agregar la dependencia en caso de que aún no la tengan instalada

Al nivel del proyecto en el que estamos trabajando deberíamos crear dentro de la carpeta de config una clase que administre los jobs a ejecutar, por lo cual tendríamos que tener algo como lo siguiente:

<img src="/images/Quartz/Manu.PNG">

En la cual hemos creado un archivo con el nombre JobConfig.java para tener la configuración de nuestro timer el código de nuestra clase debería quedar de la siguiente manera.
Es importante tener en cuanta que importamos las librerías correspondientes a cada anotación de spring


``` java
package mx.edu.inee.oic.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.stereotype.Component;
import org.springframework.context.annotation.ComponentScan;

@Configuration
@EnableAsync
@EnableScheduling

public class JobConfig {

}package mx.edu.inee.oic.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.stereotype.Component;
import org.springframework.context.annotation.ComponentScan;

@Configuration
@EnableAsync
@EnableScheduling

public class JobConfig {

}
```

Posterior a esto crearemos un paquete el nivel de nuestro proyecto como se puede observar en la imagen anterior, en este caso la clase la nombraremos AlerJob.java, para ser mas especifico dentro de la carpeta config crearemos un directorio con el nombre job y dentro de él la clase con el nombre ya mencionado antes.
Se deberá ver como lo siguiente.

``` java
package mx.edu.inee.oic.job;

import org.springframework.stereotype.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.annotation.Async;
import org.springframework.scheduling.annotation.Scheduled;

@Component
public class AlertJob {


    private final Logger log = LoggerFactory.getLogger(AlertJob.class);

    @Async
    @Scheduled(cron="20 0/1 * * * ?") // (cron= "0 0 12 1/1 * ? *")  para cada día a las 12
    public void heavyWorkSync1() {
        log.debug("Timer Ejecutado");
    }

}

```


Finalmente esto lleva de una expresión cron donde le estamos indicando que se ejecute cada 20 segundo y así mismo podremos verlo en nuestro log.

<img src="/images/Quartz/logTimer.PNG">


Es importante mencionar que nuestro proyecto deberá contener la siguiente dependencia.

``` java
<dependency>
<groupId>org.quartz-scheduler</groupId>
<artifactId>quartz</artifactId>
<version>2.3.0</version>
</dependency>
```

https://mvnrepository.com/artifact/org.quartz-scheduler/quartz/2.3.0

De igual manera trabaja para cualquier otro manejador de dependencias

Aplicación web para realizar expresiones cron http://www.cronmaker.com/

