---
title: "Spring con vertx"
date: 2018-02-03
categories:
- projects
- releases
- Help
- Groovy
- Java
- Vertx
tags:
- Makingdevs
- Experience
- Groovy
- Spring
- Vertx
keywords:
- disqus
- google
- gravatar
- Groovy
- Vertx
- Spring
- Hepl
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /images/Vertx/vextxLogoMin.png
coverImage: /images/Vertx/vextxLogo.png
metaAlignment: center
---
Si quires hacer tus aplicaciones con procesos en múlti-hilo, esté es el primer paso....
(Spring 4.3, Vertx 3.5)

<!--more-->

Primero quisiera decir que no es un tema fácil de entender así que daré por hecho ciertos conocimientos y me centraré únicamente en la implementación.

Puntos importantes:

- Configuración Boostrap de Spring
- Uso de Spring Factory
- Configuración de puertos
- Anotaciones importantes @Component, @Aoutowire,@PostConstructor, @Scope("SCOPE_PROTOTYPE")

Primero que nada empezaremos con la configuración de Spring Factory. Si te preguntas porque la necesitamos es porque ciertamente necesitamos los servicios de Spring dentro de Vertx y como ya debemos saber Vertx crea múltiples instancias a contrario de Spring que crea singletons para sus servicios y es por eso que muchas veces tenemos conflictos a la hora de implementar Vertx en Spring, Un vez mencionado esto entendemos que necesitamos el "ApplicationContext" de Spring dentro de las clases de que usará Vertx para tener un deploy.

``` groovy
package mx.edu.ebc.core.init

import io.vertx.core.Verticle
import io.vertx.core.spi.VerticleFactory
import org.springframework.beans.BeansException
import org.springframework.context.ApplicationContext
import org.springframework.context.ApplicationContextAware
import org.springframework.stereotype.Component

@Component
class SpringVerticleFactory implements VerticleFactory, ApplicationContextAware {

  ApplicationContext applicationContext;

  @Override
  public boolean blockingCreate() {
    return true;
  }

  @Override
  public String prefix() {
    return "ebc"; // Nombre para tomar la referencia de tus clases
  }

  @Override
  public Verticle createVerticle(String verticleName, ClassLoader classLoader) throws Exception {
    String clazz = VerticleFactory.removePrefix(verticleName);
    return (Verticle) applicationContext.getBean(Class.forName(clazz));
  }

  @Override
  public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
    this.applicationContext = applicationContext;
  }
}
``` 
Posterior a esto pasaremos a la creación de la clase boostrap la cual en donde lograremos definir los verticles que ocuparemos a lo largo de nuestra aplicación, así como sus opciones de despliegue. Lo importante de esta clase es que la haremos como un componente de spring el cual se ejecutara justo de haber levantado la aplicacion junto con el contexto ya que es requerido.

**Shell**

Si queremos utilizar la shell de vertx basta con añadir el siguiente código a la clase Bootstrap:
``` groovy
  ShellService service = ShellService.create(vertx,
  new ShellServiceOptions().setTelnetOptions(
    new TelnetTermOptions().
      setHost("localhost").
      setPort(4000)
    )
  )
  service.start()
``` 
En la clase mencionada se debe de hacer la anotación  @PostConstruct al metodo que tiene que construir en este caso el encargado de hacer nuestros despliegues del verticle mientras de igual manera se inyecta el VerticleFactory el cual correspopnde a la interfaz implementada en nuestra clase SpringVerticleFactory.

Despliegue de un verticle

Para hacer un despliegue de un verticle se deberá realizar de la siguiente manera.


``` groovy
vertx.deployVerticle("${verticleFactory.prefix()}:${CourseVerticle.class.name}")
``` 
A continuación dejo el código completo de la clase Bootstrap.

``` groovy
package mx.edu.ebc.core.init

import javax.annotation.PostConstruct
import mx.edu.ebc.core.verticle.*
import org.springframework.context.ApplicationContext
import io.vertx.core.Vertx
import io.vertx.core.spi.VerticleFactory
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.stereotype.Component
import org.springframework.context.annotation.Scope
import io.vertx.core.DeploymentOptions
import io.vertx.ext.web.handler.StaticHandler
import io.vertx.ext.web.Router
import io.vertx.ext.web.handler.sockjs.SockJSHandler
import io.vertx.ext.web.handler.sockjs.BridgeOptions
import io.vertx.ext.web.handler.sockjs.PermittedOptions

import io.vertx.ext.shell.*
import io.vertx.ext.shell.term.*

@Component
class Bootstrap {

  @Autowired
  VerticleFactory verticleFactory

  @PostConstruct
  void deployVerticles(){
    println "*"*100
    Vertx vertx = Vertx.vertx()

    ShellService service = ShellService.create(vertx,
        new ShellServiceOptions().setTelnetOptions(
          new TelnetTermOptions().
          setHost("localhost").
          setPort(4000)
        )
      );
    service.start();

    vertx.registerVerticleFactory(verticleFactory)

    DeploymentOptions opts_1 = new DeploymentOptions()
    opts_1.setWorker(true)
    opts_1.setInstances(5)

    DeploymentOptions opts_2 = new DeploymentOptions()
    opts_2.setWorker(true)
    opts_2.setInstances(50)

    vertx.deployVerticle("${verticleFactory.prefix()}:${CourseVerticle.class.name}", opts_1)
    vertx.deployVerticle("${verticleFactory.prefix()}:${ProcessVerticle.class.name}", opts_1)
    vertx.deployVerticle("${verticleFactory.prefix()}:${EnrollmentCourseVerticle.class.name}", opts_2)
    //vertx.deployVerticle(new CourseVerticle(applicationContext), options)
    //vertx.deployVerticle(new MainVerticle(applicationContext))
    //vertx.deployVerticle(new ProcessVerticle(applicationContext))
    //vertx.deployVerticle(new PingVerticle(applicationContext))
    //vertx.deployVerticle(new PongVerticle(applicationContext))
    def verticles = vertx.deploymentIDs()
    println "Verticles: ${verticles.dump()}"

    def server = vertx.createHttpServer()
    def router = Router.router(vertx)

    BridgeOptions bridgeOptions = new BridgeOptions()
    bridgeOptions.addOutboundPermitted(new PermittedOptions().setAddress("mx.edu.ebc.moodle.create.course"))
    .addOutboundPermitted(new PermittedOptions().setAddress("mx.edu.ebc.moodle.list.course"))

    bridgeOptions.addInboundPermitted(new PermittedOptions().setAddress("mx.edu.ebc.moodle.create.course"))
    .addInboundPermitted(new PermittedOptions().setAddress("mx.edu.ebc.moodle.list.course"))

    bridgeOptions.addInboundPermitted(new PermittedOptions().setAddress("mx.edu.ebc.moodle.warn.course"))
    bridgeOptions.addInboundPermitted(new PermittedOptions().setAddress("mx.edu.ebc.moodle.finish"))
    .addOutboundPermitted(new PermittedOptions().setAddress("mx.edu.ebc.moodle.finish"));



    def ebHandler = SockJSHandler.create(vertx).bridge(bridgeOptions)

    router.route("/eventbus/*").handler(ebHandler)

    server.requestHandler(router.&accept).listen(9090)
    println "Vert.x is deployed "
  }

}
``` 
Servidor para web sockets js

Si deseas levantar un servidor para la comunicación de tu front deberás hacerlo en la clase mencionada antes como se muestra en el código.

Ejemplo de clase para desplegar:

Importante mencionar que estas clases deberán llevar la anotación  @Scope(SCOPE_PROTOTYPE)
para que Spring logre hacer múltiples instancias ya que así lo requiere vertx.

``` groovy
package mx.edu.ebc.core.verticle

import io.vertx.core.AbstractVerticle


import org.springframework.beans.factory.annotation.Autowired
import org.springframework.context.annotation.Scope
import org.springframework.stereotype.Component
import groovy.json.JsonSlurper
import mx.edu.ebc.api.service.EnrollmentService
import mx.edu.ebc.moodle.model.MoodleCourse

import static org.springframework.beans.factory.config.ConfigurableBeanFactory.*

import io.vertx.core.DeploymentOptions

@Component
@Scope(SCOPE_PROTOTYPE)
class EnrollmentCourseVerticle extends AbstractVerticle {

  @Autowired
  EnrollmentService enrollmentService

  @Override
  void start() throws Exception {
    super.start()
    vertx.eventBus().consumer("mx.edu.ebc.moodle.enrollment.course") { message ->
      println message.body().shortname
      List<MoodleCourse> moodleCourse = []
        moodleCourse << new MoodleCourse(
          shortname: "${message.body().shortname}+T10",
          idnumber:"${message.body().shortname}+T10",
          category: "Vertx-test",
          fullname: message.body().subject,
          summary:"",
          format:"",
          startdate:"",
          groupmode:"",
          enablecompletion:"",
          coursetemplate:"MER23432-P"
        )
       enrollmentService.createCoursesInMoodle(moodleCourse)
       vertx.eventBus().send("mx.edu.ebc.moodle.bulid","Termine el curso ${message.body().shortname} ")
    }
  }

}
``` 
**Conclusión** 

La verdad es que esta implementan me agrado bastante ya que al principio tuve muchas complicaciones para hacer las instancias de vertx ya que todo se ejecutaba de manera secuencial, lo cual no era el punto de la tecnología. Pero finalmente una vez ya trabajando me sorprendió su funcionamiento.

Pd: Si usas java, ya dejalo!!! usa groovy y descubre una forma más divertida de programar 😃

