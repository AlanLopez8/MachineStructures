# Lab 1 - Punteros en C y GDB

## Objetivos

* Aprender cómo compilar y ejecutar un programa en C.
* Examinar diferentes tipos de control de flujo en C.
* Introducirlos al depurador de C
* Conseguir experiencia práctica utilizando GBD para depurar programas en C.
* Ganar más confianza al trabajar con punteros.

## Preparación
Visiten este [link](https://classroom.github.com/a/R92ylAB1). Aquí encontrarán todos los archivos necesarios para completar este lab. En esta página, encontrarán un botón que dice "Accept assignment". Al presionar este botón, se creará automáticamente un repositorio en Github llamado www.github.com/cc-3/lab1-c-gdb-<USUARIO>. Noten que el "dueño" de este repositorio es un usuario llamado `cc-3`, y el usuario de ustedes es únicamente el sufijo del nombre del repo. De esta forma, nos encargamos de tener acceso siempre a su código, en caso existan copias o cualquier otro tipo de trampa. Sepan de una vez que, si encontramos plagio o cualquier otro tipo de trampa en sus laboratorios, su nota será AUTOMÁTICAMENTE 0,  sin posibilidad de cambiarla. De repetirse nuevamente este acontecimiento, el staff del curso organizará una reunión con ustedes y sus directores de carrera para contarles lo ocurrido.

Después de realizar esto, en la máquina virtual (o sus propias computadoras) abran una terminal en el directorio que prefieran, y ejecuten el siguiente comando:
```shell
git clone https://github.com/cc-3/lab1-c-gdb-<SU USUARIO DE GITHUB>
```
esto descargará en el directorio que escogieron todos los archivos base para este laboratorio.

## Compilando y ejecutando un programa de C
En este laboratorio, estaremos usando el programa `gcc` para compilar programas en c. La manera más sencilla de ejecutar `gcc` es la siguiente:
```shell
gcc program.c
```
Esto compila el archivo `program.c` y crea un archivo ejecutable llamado `a.out`. Si tienen experiencia en Java, pueden más o menos considerar a `gcc` como el equivalente en C de `javac`. Este archivo se puede ejecutar con el siguiente comando:
```shell
./a.out
```
El archivo ejecutable es `a.out`, así que, ¿Qué rayos es eso de punto y diagonal? La respuesta: cuando quieren ejecutar un ejecutable, es necesario preponer una ruta de archivo para distinguirlo de un comando como `python`. El punto se refiere al "directorio actual". De paso, dos puntos (..) se referirían al directorio que está un nivel arriba.

`gcc` tiene varias opciones (o argumentos) de línea de comandos, los cuales les recomendamos explorar. En este laboratorio, vamos a estar usando solamente -o,  que se usa para especificar el nombre del ejecutable que `gcc` genera. Usando -o, se utilizarían estos comandos para compilar `program.c` en un archivo llamado `program`, y ejecutarlo. Eso nos sirve si no queremos que todos nuestros archivos ejecutables se llamen `a.out`.

```shell
gcc -o program program.c
./program
```
## Ejercicio 1: Programa simple de C
En este ejercicio, veremos un ejemplo de definiciones macro de preprocesador. Las macros pueden ser un tema complicado, pero en general, la forma en que funcionan es que, antes de que un archivo en C es compilado, las constantes macro son reemplazadas exactamente por el valor al que se refieren.
En este ejercicio, estaremos usando macros exctulivamente como constantes globales. Aqui definimos `CONSTANT_NAME` como un `literal_value` (una literal entera). Noten que solo hay 1 espacio separando el nombre del valor.
```shell
#define CONSTANT_NAME LITERAL_VALUE
```
Ahora, vean el código en eccentric.c (en el repo). **Noten** los cuatro diferentes ejemplos de control básico de flujo. (¿Cuáles son?)
Compilen y ejecuten el programa para ver lo que hace. Jueguen con las constantes de las cuatro macros: `v0` a `v3`. Vean cómo cambiar **cada uno** de estos cambia el output del programa.
Su tarea: Modificando solo estos cuatro valores, hagan que el programa produzca el siguiente mensaje:
```shell
$ gcc -o eccentric eccentric.c
$ ./eccentric
Berkeley eccentrics:
====================
Happy Happy Happy
Yoshua

Go BEARS!
```

Hay múltiples combinaciones de valores en las macros que consiguen este resultado, El reto para ustedes en este ejercicio es: Consideren el **mínimo** número de distintos valores que las constantes `v0` a `v3` puedan tener que aún den el mismo resultado correcto. Como ejemplo, el máximo teórico es cuatro (cuando todos son diferentes uno de otro).

## Ejercicio 2: _Debugger_ (depurador)
### ¿Qué es un _debugger_?
Esta sección es para los estudiantes que no están familiaricados con los _debuggers_. Un **debugger**, como sugiere el nombre, es un programa específicamente diseñado para ayudarlos a encontrar _bugs_, o errores lógicos, u otros errores en el código (nota: si quieren saber por qué se les llama _bugs_ a los errores, vean [aquí](https://www.quora.com/Why-are-errors-in-software-codes-called-bugs)). Distintos debuggers tienen distintas características, pero es normal que todos los debuggers sean capaces de hacer las siguientes cosas:
  1. Poner un **breakpoint** en el programa. Un Breakpoint es una línea específica en su código en donde quisieran que se detenga la ejecución del programa, para que puedan ver lo que está pasando alrededor.
  2. **Step** línea a línea por el programa. El código siempre se ejecuta línea a línea, pero pasa muy rápido como para que sepamos qué línea produce algún error. Ser capaces de ejecutar línea a línea el programa les permite observar **exactamente** qué esta causando un bug en el programa.
Para este ejercicio, necesitarán la [GDB reference card](http://inst.eecs.berkeley.edu/~cs61c/resources/gdb5-refcard.pdf). GDB quiere decir "GNU De-Bugger". Compilen hello.c con la bandera "-g":
```shell
gcc -g -o hello hello.c
```
Esto hará que gcc guarde información en el archivo ejecutable para que gdb lo interprete. Ahora ejecuten el debugger, (c)gdb:
```shell
cgdb hello
```
Vean lo que hace este comando. Están ejecutando el programa `cgdb` en el **ejecutable** `hello` generado por `gcc`. No intenten ejecutar cgdb en el archivo fuente en `hello.c`! Eso no va a funcionar.
Si cgdb no funciona, gdb se puede usar para completar los ejercicios (utilicen `gdb hello`).
**su tarea: ejecuten el programa varias veces haciendo esto:**
  1. poniendo un breakpoint en el main.
  2. usando el comando run de gdb.
  3. usando el comanndo single-step de gdb.

Escriban `help` adentro de gdb para averiguar cómo hacer estas cosas, o usen la reference card.

**Si encuentran un mensaje de error que dice:** `printf.c: No such file or directory`. Probablemente entraron a una función `printf`. Si siguen ejecutando paso a paso, pareciera que nunca avanzaran en el código. CGDB está dando el error porque no tienen el archivo en el que se define la función `printf`. Esto es algo molesto, y para librarse de esto, usen el comando `finish` para ejecutar el programa hasta que termine la función printf. Y la **próxima vez**, utilicen el comando `next` para saltar sobre la linea que usa `printf`.
**Nota: CGDB vs GDB**.

en este ejercicio, usamos cgdb para depurar nuestros programas. cgdb es idéntico a gdb, excepto que tiene unas características extra que hacen más cómodo el trabajo. Todos los comandos de la hoja de referencia funcionan también en gdb.
En cgdb, pueden presionar `ESC`para ir a la ventana del código (arriba), y usar `i` para regresar a la ventana de comandos (abajo), similar a `vim`. La ventana de comandos es donde se introducen los comandos de gdb.
  
**Preguntas**

  1. 
  
## Ejercicio 3: Depurando un problema con fallas usando GDB
Ahora, usarán su nuevo conocimiento para depurar un pequeño programa. Vean el programa `ll_equal.c`. Compilen y ejecuten el programa, y analicen un poco lo que hace. Así como está, preoducirá un resultado como el siguiente:
```shell
$ gcc -g -o ll_equal ll_equal.c
$ ./ll_equal
equal test 1 result = 1
Segmentation fault
```
**Averigüen qué produce la falla de segmentación**.

Ejecuten gdb en el programa, siguiendo las instrucciones aprendidas en los ejercicios anteriores. Les recomendamos añadir un breakpoint en la función `ll_equal()`. Cuando el debugger pare en el breakpoint, ejecuten paso a paso el programa, para que puedan descifrar qué es lo que provoca el error.

Pista: Analicen el valor de los punteros `a` y `b` in la función (despliegenlos!). ¿están siempre apuntando a la dirección correcta?

pista 2: vean el código fuente en `main` para ver la estructura de los nodos, y ver exactamente qué está pasando como argumento a `ll_equal`.

Después de corregir el problema, compilen nuevamente y ejecuten el código. ¿Notan la diferencia?

## Ejercicio 4: "Debuggeando" un programa en C que requiere interacción del usuario
Veamos qué pasa cuando, a un programa que requiere interacción del usuario, lo ejecutamos con gdb. Primero, ejecuten el programa en `interactive_hello.c` para hablar con un programa muy amigable :).
```shell
$ gcc -g -o int_hello interactive_hello.c
$ ./int_hello
```
Ahora, traten de depurarlo (aunque no haya ningún problema realmente):
```shell
$ cgdb int_hello
```
¿Qué pasa cuando intentar ejecutar el programa hasta el final?
Vamos a aprender acerca de una herramienta que nos ayudará a evitar este problema. El propósito de este ejercicio es que no tengan miedo de usar un debugger incluso cuando el programa requiera de interacción con el usuario.

Resulta que es posible enviar texto a [stdin](https://en.wikipedia.org/wiki/Standard_streams#Standard_input_.28stdin.29), el flujo de datos que es leído por la función `fgets` en este programa, con unos caracteres especiales desde la línea de comandos. Echen un vistazo a la "redirección" en [esta página](https://www.cs.bu.edu/teaching/c/file-io/intro), y vean si pueden descifrar cómo enviar texto al programa sin escribirlo textualmente mientras el programa está en ejecución (lo cual, como ya saben, no funciona bien en CGDB).

Pueden ver [esta discusión de stackoverflow](https://stackoverflow.com/questions/19467865/how-to-use-redirection-in-c-for-file-input) para más inspiración.

(Pista: si están creando un archivo de texto que contiene su input, van bien!)

(pista 2: Recuerden que es posible ejecutar programas con **argumentos (incluyendo símbolos de redirección) desde CGDB!**
Este ejercicio no vale puntos :-) pero es importante conocer sobre estas cosas para que puedan utilizarlo en el futuro (los siguientes laboratorios y proyectos podrían necesitar de este conocimiento)

## Ejercicio 5: Punteros y estructuras en C

----fin-----
