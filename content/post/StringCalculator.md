+++
Categories = ["Development","GoLang"]
Tags = ["Development","golang"]
Description = ""
date = "2017-02-23T13:19:00-06:00"
title = "String Calculator"
menu = "main"

+++

Este es un ejercicio de refactor para las pruebas que realizamos a nuestro código, así mismo prueba nuestros conocimientos en los diferentes strings que nos pueden mandar en una cadena y también prueba nuestra lógica para entender las pruebas y aplicarlas, también nos da una selección que dice "No confies en una prueba que no falla", con esto me refiero a que debemos hacer pruebas en las que debemos fallar.

Empezaré por mostrar el problema planteado:

String Calculator
The following is a TDD Kata- an exercise in coding, refactoring and test-first, that you should apply daily for at least 15 minutes (I do 30).

Before you start:

- Try not to read ahead.
- Do one task at a time. The trick is to learn to work incrementally.
- Make sure you only test for correct inputs. there is no need to test for invalid inputs for this kata

String Calculator

- Create a simple String calculator with a method int Add(string numbers)
    - The method can take 0, 1 or 2 numbers, and will return their sum (for an empty string it will return 0) for example “” or “1” or “1,2”
    - Start with the simplest test case of an empty string and move to 1 and two numbers
    - Remember to solve things as simply as possible so that you force yourself to write tests you did not think about
    - Remember to refactor after each passing test
- Allow the Add method to handle an unknown amount of numbers
- Allow the Add method to handle new lines between numbers (instead of commas).
    - the following input is ok:  “1\n2,3”  (will equal 6)
    - the following input is NOT ok:  “1,\n” (not need to prove it - just clarifying)

    - Support different delimiters
    - to change a delimiter, the beginning of the string will contain a separate line that looks like this:   “//[delimiter]\n[numbers…]” for example “//;\n1;2” should return three where the default delimiter is ‘;’ .
    - the first line is optional. all existing scenarios should still be supported
- Calling Add with a negative number will throw an exception “negatives not allowed” - and the negative that was passed.if there are multiple negatives, show all of them in the exception message
stop here if you are a beginner. Continue if you can finish the steps so far in less than 30 minutes.

- Numbers bigger than 1000 should be ignored, so adding 2 + 1001  = 2
- Delimiters can be of any length with the following format:  “//[delimiter]\n” for example: “//[***]\n1***2***3” should return 6
- Allow multiple delimiters like this:  “//[delim1][delim2]\n” for example “//[*][%]\n1*2%3” should return 6.
- make sure you can also handle multiple delimiters with length longer than one char

Link de referencia = http://osherove.com/tdd-kata-1/

Cuando llegue a la cuarta prueba tuve la siguiente característica:
Lo primero que se me ocurrio para dar solución al problema fue lo siguiente

```groovy
if(numbers.size()>1){
numbers = numbers.replace("\n",",")
Integer e = 0
def lista = numbers.split(",")
for(int i=0; i<lista.size();i++){
e = e + lista[i].toInteger()
numbers = e
}
```
En el cual lograba observar que mi código estaba deficiente ya que era un poco complejo de entender y estaba utilizando un for para iterar la lista.

Posteriormente tuve un segundo acercamiento, buscando una manera diferente de realizarlo
```groovy
if(numbers.size()>1){
numbers = numbers.replace("\n",",")
def lista = numbers.split(",")
int e = 0
numbers.eachLine(){
  for(e=0; e < numbers.split(",").size();e++) {
  lista << it.split(",")[e].toInteger()}
}
numbers = lista.sum()
}
```
Este fragmento de código para ser sincero parecería que fui de mal a peor, sin embargo me dió la pauta para leer bien los conceptos de listas y darme cuenta de que no era necesario el for, dando como resultado el siguiente código.

```groovy
if(numbers.size()>1){
numbers = numbers.replace("\n",",")
def lista = numbers.split(",").collect() {it.toInteger()}
return lista.sum()
}
```

**Y de esta manera se va mas elegante y entendible.**

Es importante saber que un concepto básico para empezar esto es el de decir solo haz lo que pide la prueba y no supongas
En lo personal decidí irme por el lado de las expresiones regulares con el lenguaje groovy y este fue mi primer acercamiento a la solución del problema:

![FirstCode](/images/firstCode.png)

Ahora trataré de mejorar mi código haciendo más entendible y mejor orientado

Una vez visualizado el Código, logré hacer refactor a unas cuantas de linéas y el resultado fue el siguiente:

![SecondCode](/images/secondCode.png)
