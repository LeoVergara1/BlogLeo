+++
date = "2017-01-26T09:18:40-06:00"
title = "Inicio en Android"
Categories = ["Development","GoLang"]
Tags = ["Development","golang"]
Description = ""
menu = "main"

+++

En este tema hablare sobre como realice y primer aplicación en android, la cual fue iniciada en java como convencionalmente se hace, para posteriormente ser trasladada a groovy.
Programa necesarios para desarrollar en Android:

- Android Studio
- Genymotion
- Virtual Box

Una vez instaladas las aplicaciones, continue a realizar una maquina virtual en Genymotion emulando android 6, ya que tuve este paso, procedí a realizar mi aplicación en Android studio.

### Para esto tenemos 2 apartados importantes.

MainActivity.xml -> En el cual realizaremos toda la parte de diseño para la aplicación.
MainActivity. java -> Se realiza toda la parte del controlador (controller).

Al iniciar el proyecto en Android Studio realice un análisis de los componentes y opciones que tenia en frente para poco a poco ir entendiendo el funcionamiento del IDE y para iniciarme en este lenguaje se realizara una pequeña aplicación donde se podrá ver el uso de botones, colores, diseños, metodos, multimedia.

Finalmente explicaré el código realizado en XML para obtener una retroalimentación de lo adquirido

### Vista del XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout android:orientation="vertical"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@mipmap/ic_launcher"
        android:id="@+id/photo"/>
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:id="@+id/photoName"/>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:text="false"
            android:id="@+id/button_false"
            android:textColor="@color/colorAccent"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:text="True"
            android:id="@+id/button_true"
            android:textColor="@color/colorPrimaryDark"/>

    </LinearLayout>
</LinearLayout>

```
**<*LinearLayout*>** Diseño de la vista a implementar.

**<*XMLS*>** Dirección de Android y sus funciones.

Android:.

**<*ImageView*>** TAG para definir componentes de una imagen determinada

**<*TextView*>** TAG para definir componentes de un texto.

**<*Button*>** TAG para definir los componentes de un botón.