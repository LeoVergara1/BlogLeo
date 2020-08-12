---
title: "Timer with elixir"
date: 2018-05-03
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
thumbnailImagePosition: "left"
thumbnailImage: /images/elixir.png
coverImage: /images/elixir.png
metaAlignment: center
---

**How do a simple timer with proccess?**

<!--more-->
This timer should have to GenServer to take away the management of the timer, In this timer we will have a counter to know how many seconds we want during the timer.

```elixir
defmodule ExTyperacer.TimerServer do

  use GenServer
  require Kernel
  alias ExTyperacer.Logic.{Game, Player}
  
  def start_link() do
    GenServer.start_link(__MODULE__, %{})
  end
  
  def init(state) do
    IO.puts "Init timer"
    #timer = Process.send_after(self(), {:work, counter, uuid}, 1_000)
    {:ok, state}
  end

  def handle_call({:reset_timer, counter, uuid}, _from, %{timer: timer}) do
    :timer.cancel(timer)
    timer = Process.send_after(self(), {:work, counter, uuid}, 1_000)
    {:reply, :ok, %{timer: timer}}
  end

  def handle_info({:work, 0, uuid}, state) do
    IO.puts "Finished proccess"
    {:noreply, state}
  end

  def handle_info({:work, counter, uuid}, state) do
    IO.puts "Counting..."
    IO.inspect counter
    counter = counter - 1
    timer = Process.send_after(self(), {:work, counter, uuid},1_000)
    {:noreply, %{timer: timer}}
  end


  def handle_info({:start_timer, counter, uuid}, state) do
    timer = Process.send_after(self(), {:work, counter, uuid}, 1_000)
    {:noreply, %{timer: timer}}
  end
    # So that unhanded messages don't error
  def handle_info(_, state) do
      {:ok, state}
  end

end
```

**Demo**

<script src="https://asciinema.org/a/nNmUebs1toWoPjybvhVWSljwV.js" id="asciicast-nNmUebs1toWoPjybvhVWSljwV" async></script>


If you want to add one communicate for broadcast to channel, you should have to like this:

  * Subscribe timer server to channel
  ```elixir
  ExTyperacerWeb.Endpoint.subscribe "timer:start", []
  ```
  * Created a private function to use broadcast

  ```elixir
  defp broadcast(time, response) do
    IO.inspect response
    ExTyperacerWeb.Endpoint.broadcast! "timer:update", "new_time_#{response.uuid}", %{ response: response.message, time: time}
  end
  ```

  For example:


  ```elixir
  defmodule ExTyperacer.TimerServer do

  use GenServer
  require Kernel
  alias ExTyperacer.Logic.{Game, Player}
  
  def start_link() do
    GenServer.start_link(__MODULE__, %{})
  end
  
  def init(state) do
    IO.puts "Init timer"
    ExTyperacerWeb.Endpoint.subscribe "timer:start", []
    #timer = Process.send_after(self(), {:work, counter, uuid}, 1_000)
    {:ok, state}
  end

  def handle_call({:reset_timer, counter, uuid}, _from, %{timer: timer}) do
    :timer.cancel(timer)
    timer = Process.send_after(self(), {:work, counter, uuid}, 1_000)
    {:reply, :ok, %{timer: timer}}
  end

  def handle_info({:work, 0, uuid}, state) do
    IO.puts "Finished proccess"
    broadcast 0, %{message: "Counting", uuid: uuid}
    {:noreply, state}
  end

  def handle_info({:work, counter, uuid}, state) do
    IO.puts "Counting..."
    IO.inspect uuid
    IO.inspect counter
    broadcast counter, %{message: "Counting", uuid: uuid}
    counter = counter - 1
    timer = Process.send_after(self(), {:work, counter, uuid},1_000)
    {:noreply, %{timer: timer}}
  end


  def handle_info({:start_timer, counter, uuid}, state) do
    timer = Process.send_after(self(), {:work, counter, uuid}, 1_000)
    {:noreply, %{timer: timer}}
  end
    # So that unhanded messages don't error
  def handle_info(_, state) do
      {:ok, state}
  end

  defp broadcast(time, response) do
    IO.inspect response
    ExTyperacerWeb.Endpoint.broadcast! "timer:update","new_time", %{ time: time, response: response.message}
  end

end
  ```