+++
date = "2017-01-12T09:37:34-06:00"
draft = true
title = "Problemas con temas de goHugo"

+++

Problemas con temas de hugo

Al iniciar mi viaje con el framework hugo,  no tuve problemas para crear el sitio con sus carpetas predeterminadas y con sus correspondientes post, pero a la hora de levantar el servidor para ver un ejemplo de lo que estaba creando, levantaba correctamente pero sin cargar el tema.

El comando ejecutado era el siguiente:
$ hugo server --theme=”hurock” --buildDrafs
Dicho problema ocasiona que los elementos no se mostrarán  correctamente o simplemente no se veían. La verdad fue que este inconveniente me demoro un par de horas y el problema solo radicaba en la configuración del archivo config.toml en la línea correspondiente:

    1. basurl = "https://localhost:1313/"

Bastaba con arreglar un simple detalle, el cual era el siguiente:

    1. https: -> http: 
Así es solo quitamos la letra “s” y el problema se habrá solucionado.
