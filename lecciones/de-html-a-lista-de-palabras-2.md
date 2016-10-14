---
title: De HTML a lista de palabras (parte 2)
authors:
- William J. Turkel
- Adam Crymble
date: 2012-07-17
reviewers:
- Miriam Posner
- Jim Clifford
translator:
- Víctor Gayol
layout: default
next: normalizing-data
previous: de-html-a-lista-de-palabras-1
---

## Objetivos de la lección

En esa lección aprenderás los comandos de Python que son necesarios para implementar la segunda parte del algoritmo que comenzamos en [De HTML a lista de palabras (parte 1)][]. La primera parte del algoritmo obtiene el contenido de una página HTML y guarda solamente el contenido que se encuentra entre la primera etiqueta `<p>` y la última etiqueta `<br/>`. La segunda mitad del algoritmo hace lo siguiente:

- Revisar cada caracter de la cadena de texto *contenidoPagina*, uno por uno.
- Si el caracter es un corchete angular izquierdo (\<) entonces estamos dentro de una etiqueta así que ignora cada uno de los caracteres siguientes.
-  Si el caracter es un corchete angular derecho (\>) entonces estamos saliendo de una etiqueta; ignora el caracter actual, pero mira cada uno de los caracteres siguientes.
- Si no estamos dentro de una etiqueta, añade añade el caracter actual a una nueva variable: *texto*.
- Secciona la cadena de caracteres *texto* en una lista de palabras individuales que puedan ser manipuladas después.

### Archivos requeridos para esta lección

- *obo.py*
- *contenido-juicio*

Si no tienes estos archivos puedes descargar el archivo comprimido python-lessons2.zip ([zip][]) de la lección anterior.

## Repetir y probar en Python

El siguiente escalón es implementar el algoritmo que busca cada uno de los caracteres en la cadema *contenidoPagina*, uno a la vez, y decide si el caracter pertenece a una marca de HTML o al contenido de la transcripción del juicio. Antes de que puedas hacer esto tienes que aprender algunas cuantas técnicas para la repetición de tareas y condiciones de prueba.

### Bucles (*Looping*)

Como muchos lenguajes de programación Python incluye un número de mecanismos de bucle . El que necesitarás usar en este caso es un *bucle for*. La versión debajo le dice al intérprete que haga algo en cada caracter de una cadena llamada *contenidoPagina*. La variable *caract* contendrá cada caracter de *contenidoPagina* en sucesión. le dimos el nombre *caract* porque no tiene porque no tiene un significado especial y podríamos haberlo llamado *tintineo* o *k* si nos hubiéramos sentido tentados. Puedes utilizar la codificación a colores en Komodo Edit como una guía para decidir si una palabra es una variable con un nombre dado por el usuario (como *caract*) o se trata de un nombre definido para Python que sirve para un propósito específico (como '`for`'). Generalmente es buena idea darle a las variables nombres que provean información acerca de lo que contienen. Esto hará mucho más fácil entender un programa que no has revisado desde hace tiempo. Con esto en mente, *tintineo* no es seguramente una buena elección para el nombre de la variable en este caso.

``` python
for caract in contenidoPagina:
	# haz algo con caract
```

### Salto (*Branching*)

Enseguida necesitarás una manera de comprobar los contenidos de una cadena y escoger la acción a seguir basada en esa prueba. De nuevo, como muchos lenguajes de programación, Python incluye un número de mecanismos de salto (o estructuras de control). La que vamos a utilizar aquí es la *sentencia condicional if*. La versión debajo hace una prueba para ver si la cadena llamada *caract* consiste en un corchete angula izquierdo. Como mencionamos anteriormente, la sangría o indentación en Python es importante. Si el código está indentado, Python lo ejecutará cuando la condición sea verdadera.

Toma en cuanta que Python utiliza el signo de igual (=) para *asignación*, es decir, para ajustar que una cosa sea equivalente a otra. Con el fin de comprobar la igualdad, utiliza dos signos de igual (==) en lugar de uno. Los programadores principiantes suelen confundir ambos.

```python
if caract == '<':
    # haz algo
```

Una forma más general de la sentencia condicional if te permite especificar qué hacer ante un evento en el la condición de prueba es falsa.

```python
if caract == '<':
    # haz algo
else:
    # haz algo distinto
```

En Python tienes la opción de hacer pruebas adicionales después de la primera mediante la utilización de la sentencia condicional *elif* (abreviatura de *else if*).

```python
if caract == '<':
    # haz algo
elif caract == '>':
    # haz otra cosa
else:
    # haz algo completamente diferente
```

## Utiliza el algoritmo para retirar el marcado en HTML

Ahora sabes lo suficiente para implementar la segunda parte del algoritmo: retirar todas las etiquetas HTML. En esta parte el algoritmo queremos:

- Buscar en cada caracter de la cadena *contenidoPagina*, un caracter a la vez
- Si el caracter es un corchete angular izquierdo (\<) estamos dentro de una etiqueta así que ignora el caracter
- Si el caracter es un corchete angular derecho (\>) estamos saliendo de una etiqueta, ignora el caracter
- Si no estamos al interior de una etiqueta, anexa el caracter actual a una nueva variable: texto

Para hacer esto, usarás un bucle para buscar cada caracter sucesivo en la cadena. Usarás entonces una sentencia condicional if / elif para determinar si el caracter es parte de una marca de HTML o parte del contenido, después anexar los caracteres de contenido a la cadena *texto*. ¿Cómo haremos el seguimiento de si nos encontramos dentro o fuera de una etiqueta? Podemos utilizar una variable entera que podrá ser 1 (verdadero) si el caracter correspondiente está dentro de una etiqueta y 0 (falso) si  no lo está (en el siguiente ejemplo hemos llamado a la variable "adentro").

### La rutina de *stripTags*

Poniendo todo junto, la versión final de la rutina se muestra a continuación. Toma en cuenta que hemos expandido la función *stripTags* que creamos anteriormente. Asegúrate de mantener la sangría o indentación como se muestra cuando remplaces la anterior rutina *stripTags* de *obo.py* con esta nueva.

Yu rutina debe versa ligeramente diferente y, mientras que funcione, todo está bien. Si estás inclinado a experimentar, probablemente es mejor que pruebes nuestra versión para asegurarte que tu programa hace lo que hace el nuestro.

``` python
# obo.py
def stripTags(contenidoPagina):
    startLoc = contenidoPagina.find("<p>")
    endLoc = contenidoPagina.rfind("<br/>")

    contenidoPagina = contenidoPagina[startLoc:endLoc]

    inside = 0
    text = ''

    for caract in contenidoPagina:
        if caract == '<':
            inside = 1
        elif (inside == 1 and caract == '>'):
            inside = 0
        elif inside == 1:
            continue
        else:
            text += caract

    return text
```

Hay dos nuevos conceptos de Python en este nuevo código: *continue* y *return*.

La declaración de Python *continue* le ordena al intérprete regresar al principio del bucle. Así que si estamos procesando caracteres dentro de un par de corchetes angulares, queremos ir al siguiente caracter en la cadena de texto *contenidoPagina* sin añadir nada a nuestra variable *texto*.

En los ejemplos anteriores hemos utilizado `print` extensamente. Éste da salida al resultado de nuestro programa en la pantalla para que lo lea el usuario. Sin embargo, a menudo queremos que una parte del programa envíe información a otra parte. Cuando termina de ejecutarse una función, puede regresar un valor al código que la ha invocado.  Si vamos a llamar a *stripTags* utilizando otro programa, deberemos hacerlo de esta manera:


``` python
#understanding the Return statement

import obo

myText = "This is my <h1>HTML</h1> message"

theResult = obo.stripTags(myText)
```

Al utilizar `return`, hemos sido capaces de guerdar la salida de datos de la función *stripTags* directamente en una variable que hemos denominado 'theResult', cuyo proceso podemos reanudar según sea necesario mediante código adicional.

Toma en cuenta que en el ejemplo *stripTags* desde el inicio de esta subsección, el valor qu equeremos recuperar no es *contenidoPagina* sino el contenido qu eha sido despojado de las etiquetas HTML.

Para comprobar nuestra nueva rutina de *stripTags* puedes ejecutar el programa *contenido-juicio.py* de nuevo. Dado que hemos redefinido *stripTags*, el programa *contenido-juicio.py* ahora hace algo diferente (y más cercano a lo que nosotros queremos). Antes de que continúes, asegúrate de comprender por qué cambia el comportamiento de *contenido-juicio.py* si solamente hemos editado *obo.py*.

## Listas en Python

Ahora que tienes la habilidad para estraer texto en crudo de páginas Web, querrás tener ese texto en una forma que sea fácil de procesar. HAsta ahora, cuando has necesitado guardar información en tus programas de Python lo has hecho utilizando cadenas de texto. Sin embargo, hay un par de excepciones. En la rutina de *stripTags* también hiciste uso de un [entero][] llamado *inside* para guardar un 1 cuando estabas procesando una etiqueta y un 0 cuando no. Puedes hacer operaciones matemáticas con los enteros pero no puedes guardar fracciones o números decimales en una variable de entero.

``` python
inside = 1
```

Y cada vez que has necesitado leer o escribir a un archivo, has utilizado un controlador de archivo especial como *f* en el ejemplo siguiente:

``` python
f = open('helloworld.txt','w')
f.write('hello world')
f.close()
```

Sin embargo, uno de los [tipos][] de objetos que provee Python es *list* (o *listas*), una colección ordenada de otros objetos (incluyendo, potencialmente, otras listas). Convertir una cadena de texto a una lista de caracteres o palabras es muy sencillo. Escribe o copia el siguiente programa en tu editor de texto para ver dos maneras de lograrlo. Guarda el archivo como *string-to-list.py* y ejecútalo. Compara las dos listas que se imprimen en el panel de comandos de salida y ve si puedes imaginarte cómo funciona este código.


``` python
# string-to-list.py

# some strings
s1 = 'hello world'
s2 = 'howdy world'

# list of characters
charlist = []
for char in s1:
    charlist.append(char)
print(charlist)

# list of 'words'
wordlist = s2.split()
print(wordlist)
```

La primera rutina utiliza un bucle "for" para pasar por cada caracter en la cadena de texto *s1*, y añade el caracter al final de *charlist*. La segunda rutina utiliza la operación dividir para romper la cadena *s2* en fragmentos cada vez que encuentre espacios en blanco (espacios, tabulaciones, retornos de caroo y caracteres similares). En realidad, es simplificar un poco las cosasreferirse a los objetos de la segunda lista como palabras. Prueba a cambiar el contenido de *s2* del programa anterior por "howdy wordld!"  y ejecútalo de nuevo. ¿Qué sucedió con el signo de exclamación? Ten en cuenta que deberás alvar los cambios antes de utilizar Ejecutar Python de nuevo.

Tomando en cuenta lo que has aprendido hasta ahora, ya puedes abrir un URL, descargar la página Web en una cadena de texto, despojarla de las etiquetas HTML y luego cortar el texto en una lsita de palabras. Intenta ejecutar el siguiente programa:

``` python
#html-to-list1.py
import urllib2, obo

url = 'http://www.oldbaileyonline.org/print.jsp?div=t17800628-33'

response = urllib2.urlopen(url)
html = response.read()
text = obo.stripTags(html)
wordlist = text.split()

print(wordlist[0:120])
```

Debes obtener algo como lo siguiente:

`` python
['324.', '\xc2\xa0', 'BENJAMIN', 'BOWSEY', '(a', 'blackmoor', ')', 'was', 
'indicted', 'for', 'that', 'he', 'together', 'with', 'five', 'hundred', 
'other', 'persons', 'and', 'more,', 'did,', 'unlawfully,', 'riotously,', 
'and', 'tumultuously', 'assemble', 'on', 'the', '6th', 'of', 'June', 'to', 
'the', 'disturbance', 'of', 'the', 'public', 'peace', 'and', 'did', 'begin', 
'to', 'demolish', 'and', 'pull', 'down', 'the', 'dwelling', 'house', 'of', 
'\xc2\xa0', 'Richard', 'Akerman', ',', 'against', 'the', 'form', 'of', 
'the', 'statute,', '&amp;c.', '\xc2\xa0', 'ROSE', 'JENNINGS', ',', 'Esq.', 
'sworn.', 'Had', 'you', 'any', 'occasion', 'to', 'be', 'in', 'this', 'part', 
'of', 'the', 'town,', 'on', 'the', '6th', 'of', 'June', 'in', 'the', 
'evening?', '-', 'I', 'dined', 'with', 'my', 'brother', 'who', 'lives', 
'opposite', 'Mr.', "Akerman's", 'house.', 'They', 'attacked', 'Mr.', 
"Akerman's", 'house', 'precisely', 'at', 'seven', "o'clock;", 'they', 
'were', 'preceded', 'by', 'a', 'man', 'better', 'dressed', 'than', 'the', 
'rest,', 'who']
```

Tener simplemente una lista de palabras no es realmente significativo. Como seres humanos tenemos la habilidad de leer; sin embargo, te estás acercando a tener una idea de lo que tus programas pueden procesar.

## Lecturas sugeridas

## Suggested Reading

-   Lutz, *Learning Python*
    -   Ch. 7: Strings
    -   Ch. 8: Lists and Dictionaries
    -   Ch. 10: Introducing Python Statements
    -   Ch. 15: Function Basics

### Sincronización de código

Para seguir a lo largo de las lecciones futuras es importante que tengas los archivos correctos y programas en el directorio "programming-historian" de tu disco duro. Al final de cada lección puedes descargar el archivo zip "programming-historian" para asegurarte que tienes el código correcto.

-   python-lessons3.zip ([zip sync][])

  [De HTML a lista de palabras (parte 1)]: ../lecciones/de-html-a-lista-de-palabras-1
  [entero]: http://docs.python.org/2.4/lib/typesnumeric.html
  [tipos]: http://docs.python.org/3/library/types.html
  [zip]: ../assets/python-lessons2.zip
  [zip sync]: ../assets/python-lessons3.zip