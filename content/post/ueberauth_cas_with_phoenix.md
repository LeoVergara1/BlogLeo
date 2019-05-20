---
title: "Ueberauth_cas with Phoenix (Elixir)"
date: 2018-07-29
categories:
- projects
- Help
- Elixir
- Erlang
- cas
- Ueberauth_cas
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
thumbnailImage: /images/cas/authotification_cover.jpg
coverImage: /images/cas/authotification.jpg
metaAlignment: center
---

**How do a connection between cas and phoenix?**

This post I want to talk about :ueberauth_cas, since I haven't found a example to be used.

**ueberauth_cas**


Is a central Authentication for a ticket from any cas authentication
View github: https://github.com/marceldegraaf/ueberauth_cas

**Install dependencies**

You should know, this dependency needs to ueberauth to its functio, than you have a little information about this dependency (ueberauth): https://github.com/ueberauth/ueberauth

**First Step**

Add dependencies


```elixir
## Application

def application do
[
mod: {ExampleUeberauthCas.Application, []},
extra_applications: [:logger, :runtime_tools, :httpoison, :ueberauth, :ueberauth_cas]
]
end

## Your deps
defp deps do
[
{:phoenix, "~> 1.3.2"},
{:phoenix_pubsub, "~> 1.0"},
{:phoenix_ecto, "~> 3.2"},
{:postgrex, ">= 0.0.0"},
{:phoenix_html, "~> 2.10"},
{:phoenix_live_reload, "~> 1.0", only: :dev},
{:gettext, "~> 0.11"},
{:cowboy, "~> 1.0"},
{:httpoison, "~> 1.2", override: true},
{:ueberauth, "~> 0.4"},
{:ueberauth_cas, "~> 1.1.0", override: true}
]
end
```
We have to add override in true, because cas dependency needs to httpoison in version 0.3 but in this test was able to check with as correct functionally in its current version.

**Second Step**

We add the configuration in your config, dev or prod, according to the case

```elixir
config :ueberauth, Ueberauth,
providers: [
cas: {Ueberauth.Strategy.CAS, [
base_url: "https://apr-test.ebc.edu.mx/casad",
callback: "http://localhost:4000/",
callback_url: "http://localhost:4000/auth/cas/callback"
]}]
```

**Third Step**

We crate a controller like to auth_controller.ex

```elixir
defmodule ExampleUeberauthCasWeb.AuthController do
@moduledoc """
Auth controller responsible for handling Ueberauth responses
"""

use ExampleUeberauthCasWeb, :controller
plug Ueberauth

alias Floki

Plug.Conn

def login(conn, _params, _current_user, _claims) do
conn
|> Ueberauth.Strategy.CAS.handle_request!
end

def callback(%{assigns: %{ueberauth_failure: _fails}} = conn, _params) do
conn
|> put_flash(:error, "Failed to authenticate.")
|> redirect(to: "/")
end

def callback(%{assigns: %{ueberauth_auth: auth}} = conn, _params) do
conn
|> put_flash(:info, "Successfully authenticated.")
|> put_session(:current_user, auth)
|> redirect(to: "/")
end

def delete(conn, _params) do
conn
|> put_flash(:info, "You have been logged out!")
|> configure_session(drop: true)
|> redirect(external: "https://apr-test.ebc.edu.mx/casad/logout")
end


end
```
In my case I needed to add Floki dependency, because in producción, phoenix was not compiling.

**Fourth Step**

We are near 🙂, so we create our scope in your router.

```elixir
scope "/auth", ExampleUeberauthCasWeb do
pipe_through [:browser]

get "/:provider", AuthController, :request
get "/:provider/callback", AuthController, :callback
delete "/:provider/logout", AuthController, :delete
get "/:provider/logout", AuthController, :delete
end
```

**Method Request**

it Is who goes to make petition to cas authentication

**Method callback**

It is where returned the information from cas authentication

**Method delete**

This case we create both methods, delete and get, because in some cases the form in html5 not supported method delete.
is need to realize this, Ueberauth.Strategy.CAS saves in cache the token from cas authentication, so we need to delete session that is saved in phoenix.

This is enough to function, we can see in the url: http://localhost:4000/auth/cas

**Plus:**

you wan to everyone calls to controllers has to pass for this auth, then you should create a plug like this

In your plug custom

```elixir
defmodule ExampleUeberauthCasWeb.Plug.Auth do

import Plug.Conn
import Phoenix.Controller
require Logger

def init(_opts) do
end

def call(%{private: %{plug_session: current_user}} = conn, _options) do
case current_user["current_user"] do
nil ->
Logger.info ":: Not Logger ::: "
conn
|> put_flash(:error, "You need to sign in or sign up before continuing.")
|> redirect(to: "/auth/cas")
|> halt()
_ ->
conn
|> put_flash(:ok, "You are sign on")
|> redirect(to: "/auth/cas")
|> halt()
end
end

def call(conn, _params) do
IO.inspect conn
Logger.info ":: Error ::: "
halt(conn)
conn
end

end
```

In your router you had to add pipeline like this:


```elixir
pipeline :auth do
plug ExampleUeberauthCasWeb.Plug.Auth
end
```
Now in your scope is enough add the pipeline

```elixir
scope "/", ExampleUeberauthCasWeb do
pipe_through [:browser, :auth] # Use the default browser stack

get "/", PageController, :index
end
```

This way every to call some controller, will be check the session.


<img src="/images/cas/exampleCas.png" style="width: 80%; height: 80%">

**Conclusion**

You remember, phoenix saves the session in chache of the broswer for a limit time, if you want to make a correct logout, you should to do in both platforms (Your application and cas application ), As I said previously