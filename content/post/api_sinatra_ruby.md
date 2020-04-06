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
thumbnailImage: /images/Quartz/quartz.png
coverImage: /images/Quartz/java.png
metaAlignment: center
---
**Api simple con sinatra y ruby**

## ¿Que es Sinatra?

Es un framework de desarrollo para servicios web por medio de ruby usando protocoles HTTP.

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
Listo podremos verificar nuestra aplicación en el navegador

## Mysql
```mysql
CREATE TABLE users ( id smallint unsigned not null auto_increment, name varchar(20) not null, username varchar(20) not null, constraint pk_example primary key (id) );

INSERT INTO users ( id, name, username ) VALUES ( null, 'Sample data' , "brandonVergara");
```
## Helpers

Estos son para el manejo de errores en las slicitudes que llegan por ejemplo un json corrupto

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

Serializamos el objeto para que pueda ser leido por el error
http://sinatrarb.com/extensions.html

## Creando usuario y validado JSON

### Json no valido
```bash
curl -H "Content-Type: application/json" -X POST http://localhost:4567/api/v1/users -d '{"name": "brandon", "username": "postman"'
```
En seguida deberemos recibir la siguiente respuesta

```
{"message":"Invalid JSON"}
```

## Borrando un usuario

```ruby
  delete '/users/:id' do |id|
    user = User.where(id).first
    user.destroy if user
    status 204
  end
```
Ejemplo

```bash
curl -i -H "Content-Type: application/json" -X DELETE http://localhost:4567/api/v1/users/2
```