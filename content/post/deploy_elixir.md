---
title: "Deploy elixir"
date: 2018-02-03
categories:
- projects
- releases
- Help
- Elixir
- Erlang
tags:
- Makingdevs
- Experience
keywords:
- disqus
- elixir 
- delivery
- edeliver
- distillery 
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /images/deployElixir/phoenix.png
coverImage: /images/deployElixir/phoenix.png
metaAlignment: center
---

Primero que nada es muy importante saber que elixir, construye sobre la maquina virtual de erlang por lo tanto esta trabja en base al hardware que se usa.

Una vez sambiendo esto lo que debemos hacer es crear una maquina virtual, en este caso la realizaré con vagrant, para realizar los siguiente tenemos que tener instalado:

    Virtual Box
    Vagrant
    Ansible (Manejador de tareas)

Esto lo necesitamos porque tenemos que hacer la construcción del proyecto desde la maquina virtual, la cual debe tener la misma arquitectura que nuestro servidor para este caso usaremos centos 7.

Vagrant

Para la mauina virtual usaremos una que previmente ya ha sido configurada por Felipe Juarez, para ello descargaremos el siguiente repositorío:

https://github.com/sohjiro/ansible_elixir

Entraremos al repositorío y ejecutaremos el siguiente comando:

vagrant up

podemos confimar las mauinas corriendo en VB: ansible_elixir_buid-host y ansible_elixir_test-host


Si todo esta en orden, deberemos porder conectarnos a la maquina mediante lo siguiente:

ssh vagrant@192.168.60.7
Está es la mauina donde construiremos nuestro proyecto para posteriormente mandar el release a producció
ssh vagrant@192.168.60.8
Maquina de prueba en el caso de que querer simular producción

Ya que nos sercioramos de que todo esto está en correcto orden pasaremos a la configuración de nuestro proyecto

En el archivo mix.exs de tu proyecto agregar edeliver and distullery como dependencias

      {:edeliver, "~> 1.5.0"}
      {:distillery, "~> 1.4", runtime: false}


Posterior a ese paso sólo deberemos verificar que el proyecto siga en orden compilando las dependencias:


 mix do deps.get, compile


Ahora continuaremos con el archivo de configuración edelivery

``` java
# .deliver/config

	 

	APP="nombre-de-tu-app"

	 

	BUILD_HOST="192.168.60.7"

	BUILD_USER="vagrant"

	BUILD_AT="/home/vagrant/builds"

	 

	STAGING_HOSTS="192.168.60.8"

	STAGING_USER="vagrant"

	TEST_AT="/home/vagrant/releases"

	 

	pre_erlang_get_and_update_deps() {

	local _prod_secret_path="$WORKSPACE/$APP/config/prod.secret.exs"

	 

	if [ "$TARGET_MIX_ENV" = "prod" ]; then

	status "Copying '$_prod_secret_path' file to build host"

	scp "$_prod_secret_path" "$BUILD_USER@$BUILD_HOST:$BUILD_AT/config/prod.secret.exs"

	 

	fi

	}

	 

	pre_erlang_clean_compile() {

	local _profile_path="$WORKSPACE/$APP/config/profile"

	 

	if [ "$TARGET_MIX_ENV" = "prod" ]; then

	status "Copying '$_profile_path' file to build host"

	scp "$_profile_path" "$BUILD_USER@$BUILD_HOST:$BUILD_AT/../.profile"

	 

	status "Preparing assets with: brunch build and phoenix.digest"

	__sync_remote "

	set -e

	cd '$BUILD_AT/assets'

	 

	npm install

	node_modules/brunch/bin/brunch build --production

	 

	cd '$BUILD_AT'

	APP='$APP' MIX_ENV='$TARGET_MIX_ENV' $BUILD_CMD phoenix.digest $SILENCE

	"

	fi

	}

```

Ahora exportaremos la varibales necesrias para nuestra configuracion
Sustituye por la ruta de tu proyecto

    export WORKSPACE=/Users/brandonvergara/ProjectsMakingdevs
    export APP=ex_typeracer

   
De esta manera nuestro archivo de producción deberá quedar de la siguiente forma "prod.secret.exs"

```java
config :ex_typeracer, ExTyperacer.Repo,
  adapter: Ecto.Adapters.Postgres,
  username: System.get_env("DATABASE_USERNAME"),
  password: System.get_env("DATABASE_PASSWORD"),
  database: System.get_env("DATABASE_NAME"),
  hostname: System.get_env("DATABASE_HOST"),
  port:     System.get_env("DATABASE_PORT"),
  pool_size: 15
```

Dicho archivo obtendra la configuración desde el profile, archivo que debe de estar en "config/profile"

```java
export DATABASE_USERNAME=postgres
export DATABASE_PASSWORD=postgres
export DATABASE_NAME=ex_typeracer_dev
# local machine
export DATABASE_HOST=localhost
export DATABASE_PORT=5432
```

Para esto necesitas actuliaziar tu archivo de "prod.exs"

```java
config :ex_typeracer, ExTyperacerWeb.Endpoint,
  load_from_system_env: true,
  url: [host: {:system, "HOST"}, port: {:system, "PORT"}],
  cache_static_manifest: "priv/static/cache_manifest.json"

# Do not print debug messages in production
config :logger, level: :info

config :phoenix, :serve_endpoints, true # es importante esta linea ya que es quien da acceso para exponer el server
```

Una vez terminado todo esto sólo deberemos ejecutar los siguientes comandos

mix edeliver build release

mix edeliver deploy release to production

mix edeliver start production

Con este comando pueden comprobar el funcionamiento de su servidor
mix edeliver ping production
Asegurense de terner abierto el puerto 4000 en su server ya que es donde por default hace deploy elixir

En caso de tener migraciones de base de datos deberemos ejecutar el siguiente comando

mix edeliver migrate production


Conslusion:

Realmente no parece ser complicado realizar estos pasos, pero si es necesario tener como al menos bien entendidos los conceptos de vagrant y ansible, así como conocimientos basicos de sus configuraciones o comandos. Me sorprede y me agrada mucho que la elixir compile el proyecto basandose en la maquina que se esta ejecutando para así porder aprovechar el hardware al máximo.

Si quisieran implementar continuso delivery por medio de un jeinkins como consejo se debera tener un script que por medio se ssh sea ejecutado a nuestro servidor de despliegue para evitar cualquier tipo de problema en la comunicación de los comandos y esto es más que nada por el hardware entre las dos maquinas.


Referencias:

Está información la logré aprender tomando como guía de Felipe Juárez (https://medium.com/@Sohjiro)
Repositorio: https://medium.com/@Sohjiro/deploying-an-elixir-application-with-edeliver-76a1bb2b4c9e