+++
date = "2017-01-12T15:07:41-06:00"
draft = true
title = "Inicio en goHugo"
+++
![Hugo](/images/hugo1.png)

Prácticamente es un framework que sirve para realizar sitios estáticos.  A diferencia de otros sistemas que construyen dinámicamente una página cada vez que un visitante solicita una. Dado que los sitios web son vistos con mucha más frecuencia de lo que se editan, Hugo está optimizado para la visualización de página web al tiempo que proporciona una gran experiencia de la escritura.

Primero que nada visite la siguiente página para realizar la instalación y de igual manera familiarizarme con el framework ya que en ella se encuentra un pequeño tutorial de inicio rápido.

![Hugo](/images/hugo2_opt.jpg)

De la siguiente manera seguí los pasos correspondientes.

1. Crear el proyecto en hugo con el siguiente comando **$hugo new site BlogLeo**
2. Despues Agregar un pos de la siguiente manera **$hugo new post/primero.md**.

 *En este apartado debo comentar que en momentos tenía problemas a la hora de crear los post, ya que siempre se debe estar en la carpeta raíz del proyecto.*

3.Agregar un tema al proyecto para después ser publicado, esté se debe guardar en la carpeta “themes” o de lo contrario no funcionara.

*Aquí fue un de los momentos donde tuve mayores problema ya que a la hora de tratar de llamar al tema había que hacer ciertas configuración en el config.tom para que funcionara.*

4.Finalmente se nos menciona como levantar un servidor local para visualizar nuestro sitio en la aplicación del navegador 

![Captura](/images/captura1.png)

En lo personal, pienso que al principio fue complicado de entender, principalmente la parte de los temas ya que si me tomo mucho tiempo lograr configurarlos a mi gusto, pero ya una vez hecho esto logre, hacer uso de ciertas herramientas de manera más sencilla.

Otro problema con el que me encontré fue con el de subir el sitio ya de manera pública, ya que tuve muchos problemas con las configuraciones para lograr hacer este proceso. 

primero que nada tuve que realizar un proyecto con control de versiones mediante Git para así dejar una versión funcionando de manera local y en otra rama realizar las modificaciones correspondientes para subir el sitio a la red. Los problemas que encontré fueron:

1. **Cargar el tema** 
2. **Cargar imágenes**
3. **Modificar los vínculos de los post que el tema tenía predeterminados**  
4. **Usar un tema correspondiente a la versión de hugo instalada** 
5. **Conceptos básicos de Git para la conexión remota**

En esta parte mi problema fue que confundí el origen del repositorio lo que no me permitia hacer un “git push” de manera correcta, posteriormente después de unos ejemplos con mis compañeros esto quedó de manera más clara así como las 3 formas de hacer el acceso remoto.

1. Clonar el repositorio y bajarlo a la pc
2. Subir al repositorio una carpeta ya creada 
3. Crear un repositorio nuevo




