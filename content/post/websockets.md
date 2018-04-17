---
title: "WebScokets con Vertx"
date: 2018-04-16
categories:
- projects
- releases
- Help
- Groovy
- Java
- Vertx
- WebSockets
tags:
- Makingdevs
- Experience
- Groovy
- SocketJS
- Vertx
keywords:
- disqus
- google
- gravatar
- Groovy
- Vertx
- SocketJs
- Hepl
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /images/WebSockets/logo.jpg
coverImage: /images/WebSockets/logo.jpg
metaAlignment: center
---

En este post tratare de dar un ejemplo sencillo de como hacer websockets con vertx y así mismo tratar de que se entienda como es que funcionan los websockets y su gran ventaja.

¿Que es un WebSocket?

Es un API con una tecnología que busca establecer una conexión entre el usuario cliente (Navegador) y el servidor mediante esta funcionalidad se permite el envió de mensajes y respuestas constroladas atravez de un único canal de comunicación ya previamente establecido

Por ejemplo en el caso de las peticiones por AJAX del método HTTP ya sea GET o POST pasa lo siguiente, cada vez que uno de ellos trata de comunicarse necesitan establecer comunicación lo cual vuelve lento la transferencia de información entre ellos, a diferencia de un socket que sólo hace una única vez para establecer la comunicación y el canal permanece abierto pera así manejar la comunicación de la información de una forma rápida y transparente.

Una vez sabido un poco de lo que se refiere con websocket pasaremos a ver un sencillo ejemplo donde aprovecharemos todo el potencial de ello, así mismo lo que nos ofrece la tecnología de vertx para resolver ciertas cosas.

Problema: Simular la comprar de boletos de manera simultanea donde el boleto estará disponible en tiempo real para N clientes, y así mismo la persona que logre comprarlo primero sera el ganador de él, de tal manera que los demás clientes sepan que ocurre con el boleto en tiempo real.

Lo necesario: 

	* Conocimientos previo en vertx
	* Dependencias para js, SocketJs y Vertx-event-bus

Primero que nada explicare un pequeño archivo donde tendremos una pequeña configuración para iniciar nuestra aplicación en vertx.

```ruby
package com.makingdevs.ticket

import io.vertx.ext.web.handler.StaticHandler
import io.vertx.ext.web.Router
import io.vertx.core.Vertx
import io.vertx.ext.web.handler.sockjs.SockJSHandler
import io.vertx.ext.dropwizard.MetricsService
import io.vertx.core.DeploymentOptions
import io.vertx.ext.shell.ShellService
import io.vertx.ext.shell.ShellServiceOptions
import io.vertx.ext.shell.term.TelnetTermOptions
import io.vertx.ext.web.handler.sockjs.BridgeOptions
import io.vertx.ext.web.handler.sockjs.PermittedOptions

println "Webserver ok"

DeploymentOptions opts_1 = new DeploymentOptions()
opts_1.setWorker(true)
opts_1.setInstances(10)
vertx.deployVerticle("TicketOffice.groovy", opts_1)
vertx.deployVerticle("ComunicateVerticle.groovy", opts_1)

def service = MetricsService.create(vertx)
def server = vertx.createHttpServer()
def router = Router.router(vertx)

BridgeOptions bridgeOptions = new BridgeOptions()
bridgeOptions.addInboundPermitted(new PermittedOptions().setAddressRegex("com.makingdevs.comunicate.*"))
.addOutboundPermitted(new PermittedOptions().setAddressRegex("com.makingdevs.comunicate.*")); // Expresión para comunicar todo lo que sea después de los Asteriscos 

def ebHandler = SockJSHandler.create(vertx).bridge(bridgeOptions)

router.route("/eventbus/*").handler(ebHandler)
router.route().handler(StaticHandler.create().setCachingEnabled(false))

Integer counter = 0

vertx.setPeriodic(30000){ t ->
println "Trabajando.."
vertx.eventBus().send("com.makingdevs.web.monitor", counter)
counter++
}

def serviceShell = ShellService.create(vertx,
new ShellServiceOptions().setTelnetOptions(
new TelnetTermOptions().
setHost("localhost").
setPort(4000)
)
);
serviceShell.start();

server.requestHandler(router.&accept).listen(8080)
```

En esta configuración solo estamos desplegandon una pequeña apliación para escuchar nuestro router en un puerto.

BridgeOtions 

Clase encarga de iniciar la comunicación con un cliente con cierto parámetros como las rutas permitidas para entablar comunicación ya sean por un string o una expresión regular, así mismo es una de las partes esenciales del Event Bus.

 SockJSHandler

Nos proporciona encabezado para las llamadas que tendremos desde el cliente en JS, este mismo necesitara la configuración del BridgeOptios.

Ahora realizaremos la parte del JS para entablar la comunicación.

Como recomendación se realizara un js de la forma de una clase para que está nos entregue una instancia de vertx y así lograr realizar un "Send" o "Consumer" según sea el caso 

<img src="/images/WebSockets/Image.png" style="width: 80%; height: 80%">

Constructor: 
 Inicia nuestro Eventbus js con la url y el puerto de nuestra aplicación, así mismo genera los métodos que harán referencia a nuestro objeto principal.

Send: 
Recibe una dirección la cual será la que deberá hacer match con el consumer creado del lado del servidor, el segundó mensaje que mandaremos sera nuestro mensaje, siempre debe de ser un string.

Consumer: 
Serán las funciones que estarán disponibles del lado del cliente para que el servidor se comunique con ellos.

Esta instancia podremo crearla en otro archivo de la siguiente manera:

```
varticleManagerSend = VerticleManager.getInstance();
```

Ahora crearemos un botón en el html para ser escuchado y que este envié un mensaje de prueba a nuestro cliente.

Para el ejemplo estará bien de la siguiente forma:


<img src="/images/WebSockets/buttonClick.PNG" style="width: 80%; height: 80%">

usaremos nuestra instancia para mandar un mensaje al servidor

<img src="/images/WebSockets/buttonSend.PNG" style="width: 80%; height: 80%">


Para que nuestro mensaje sea escuchado deberemos tener un consumer del lado del servidor como lo siguiente:

<img src="/images/WebSockets/consumer.PNG" style="width: 80%; height: 80%">

De tal manera que cuando demos click a el botón pasara lo siguiente:

<img src="/images/WebSockets/servidorMsg.PNG" style="width: 80%; height: 80%">

Por ultimo nos falta ver como mandar un mensaje del servidor al cliente y usaremos el mismo ejemplo.
Primero declararemos un consumer del lado del cliente:

<img src="/images/WebSockets/consumerVer.PNG" style="width: 80%; height: 80%">

Este lleva dos parámetros el primero es la dirección y el segundó es una función donde en ella se ejecutara todo lo deseado una vez que se recibe un mensaje.

Finalmente regresaremos al consumer creado en el servidor para agregar lo siguiente:


<img src="/images/WebSockets/consumerReturn.PNG" style="width: 80%; height: 80%">

Y se mostrara lo siguiente en la consola de nuestro navegador:

<img src="/images/WebSockets/ChromeCOnsole.PNG" style="width: 80%; height: 80%">



Cosas importantes que se deben saber:

* Para un consumer en js siempre se deberá crear una nueva instancia ya que si se usa la misma tendrá conflictos con los demás consumers ya que los sobrescribe 
* Siempre se deberán mandar mensajes, si deseas un objeto mándalo en forma de JSON y transformalo del lado del cliente.
* Este ejemplo es una parta de un mini proyecto, y se deja una demostración del mismo así como el repo.



<img src="/images/WebSockets/exampleVertx.gif" style="width: 80%; height: 80%">

GitHub del Proyecto: https://github.com/LeoVergara1/tickets/tree/feature/TestBlog

