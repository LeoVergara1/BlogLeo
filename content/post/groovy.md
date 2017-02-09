+++
date = "2017-01-18T13:38:40-06:00"
draft = true
title = "Groovy Programming"

+++

Primero que nada daré una breve descripción de lo que es groovy. Es un lenguaje ágil y dinámico para la máquina virtual de java, *“Construido sobre las fortalezas de Java pero tiene características adicionales de poder inspiradas por lenguajes como Python, Ruby y Smalltalk” (MakingDevs)*

La verdad es que jamás había escuchado sobre este lenguaje, y me doy cuenta que eso es realmente triste ya que cuenta con un gran potencial al ser un lenguaje dinamico y ademas cuenta con closures, builders y tipado dinámico.

Realmente es increíble la simplificación que se logra hacer al lenguaje java, agilizando nuestros proyectos y dando un mejor entendimiento al problema para así realizar una óptima solución.

Al principio estuve un poco desconcertado, por lo complejo que pudiera ser aprender este nuevo lenguaje, sin embargo con un poco de explicación entendí que prácticamente es lo mismo que java pero suprimiendo partes del código innecesario.

Un claro ejemplo es el siguiente.
## Hola Mundo en JAVA


```java
    public class HolaMundo {
  private String nombre;
  public String getNombre() {
    return nombre;
  }
  public void setNombre(String nombre) {
    this.nombre = nombre;
  }
  public String saluda() {
    return "Hola " + this.nombre + " !!!";
  }
  public static void main(String[] args) {
    HolaMundo objeto = new HolaMundo();
    objeto.setNombre("@grailsmx");
    System.out.println(objeto.saluda());
  }
}

```
## Hola Mundo en GROOVY
```groovy
class HolaMundo {
  String nombre
  def saluda() { "Hola  ${this.nombre} !!!" }
}

def objeto = new HolaMundo(nombre:"@grailsmx")
println(objeto.saluda())

```

Realmente después de una series de pasos es impresionante la cantidad de código que nos logramos ahorrar con el uso de este lenguaje.

Para finalizar es importante mencionar las siguientes características del lenguaje.

* Admite el lenguaje tal cual de java dentro del código en caso de que no sepamos cómo escribirlo en groovy.
* Groovy es capaz de emigrar todo el proyecto al código de java junto consus “.class” necesarios para correr desde la consola.
* Con el uso de los cluseres dinámicos, definitivamente nuestra vida programando sera mas facil y obteniendo un nivel de programació con mejor nivel de tecnología del momento.