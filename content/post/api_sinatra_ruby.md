---
title: "Api con Sinatra y ruby"
date: 2020-04-01
categories:
- projects
- releases
- Api
- Sinatra
- Ruby
tags:
- Makingdevs
- Experience
- Ruby
- Spring
keywords:
- disqus
- learning
- gravatar
- Ruby
- Sinatra
autoThumbnailImage: false
thumbnailImagePosition: "top"
thumbnailImage: /images/apiSinatra/Sinatra_tiny.png
coverImage: /images/apiSinatra/Sinatra_tiny.png
metaAlignment: center
---
**Api simple con sinatra y ruby**

## ¿Que es Sinatra?

Es un DSL de desarrollo para servicios web por medio de ruby usando protocolos HTTP.

En resumen: **Es simple, pero poderoso**. =) Y esta apoyado y motivado por Heroku y Github

Requerimientos
- Sólo ruby >= 2.0

## Crear proyecto

1.  mkdir api_sinatra
2.  touch server.rb

Si, así de sencillo.

## Instalar Sinatra

1.  gem install sinatra

Si, otro paso muy sencillo

## Correr aplicación
Posterior a eso en el archivo colocamos lo siguiente:
```
# server.rb
require 'sinatra'

get '/' do
  'Welcome to BookList!'
end
```
Ahora en la terminal colocar

````bash
ruby server.rb
````
Listo, podremos verificar nuestra aplicación en el navegador

## Mysql

Usar código sólo si es necesario, ya que la aplicación conecta y crea la tabla con un usuario si no existe.

```mysql
CREATE TABLE users ( id smallint unsigned not null auto_increment, name varchar(20) not null, username varchar(20) not null, constraint pk_example primary key (id) );

INSERT INTO users ( id, name, username ) VALUES ( null, 'Sample data' , "brandonVergara");
```
## Creando usuario y validado JSON

```ruby
  post '/users' do
    user = User.new(json_params)
    if user.save
      status 201
    else
      status 422
      body UserSerializer.new(user).to_json
    end
  end
```

Ejemplo: borrar un usuario desde la consola.

```bash
curl -i -H "Content-Type: application/json" -X DELETE http://localhost:4567/api/v1/users/2
```

### Json no valido
```bash
curl -H "Content-Type: application/json" -X POST http://localhost:4567/api/v1/users -d '{"name": "brandon", "username": "postman"'
```
En seguida deberemos recibir la siguiente respuesta

```
{"message":"Invalid JSON"}
```

## Helpers

Son funciones desginadas por sinatra para ser utilizadas.

Estos son para el manejo de errores en las solicitudes que llegan, por ejemplo un json corrupto.



```ruby
  helpers do
    def base_url
      @base_url ||= "#{request.env['rack.url_scheme']}://{request.env['HTTP_HOST']}"
    end

    def json_params
      begin
        JSON.parse(request.body.read)
      rescue
        halt 400, { message:'Invalid JSON' }.to_json
      end
    end
  end
```

## Serializable

Serializamos el objeto para que pueda ser leído por el error
http://sinatrarb.com/extensions.html


## Borrando un usuario

```ruby
  delete '/users/:id' do |id|
    user = User.where(id).first
    user.destroy if user
    status 204
  end
```
## Como regalo por llegar al final del post 🤭

Agregar a nuestro Gemfile la siguiente dependencia:

```ruby
gem 'sinatra-reloader'
```
Y finalmente a nuestra aplicación añadir:

```ruby
#Code..
require 'sinatra/reloader'

#Code..



```
