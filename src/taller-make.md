## 1 - Introducción: Qué es y para qué sirve GNU Make

GNU Make es un software desarrollado por el proyecto GNU que controla la generación de binarios así como la ejecución de tareas necesarias para compilar y trabajar sobre un proyecto software.

### 1.1 - Qué es capaz de hacer Make

- Make te permite compilar e instalar un paquete con un par de instrucciones, sin que el usuario final sepa como funciona el proceso de compilación. De la misma forma, utilizando Make los usuarios que sepan utilizar esta herramienta podrán modificar la forma en la que se compila el proyecto para adaptarlo a sus necesidades.

- Cuando utilizamos Make no es necesario generar todo el proyecto si ya lo hemos generado anteriormente, Make generará solo las partes del proyecto que se vean afectadas por los cambios realizados después de la última compilación.

- Make no se limita a compilar proyectos de un lenguaje concreto, como veremos más adelante, utiliza comandos de terminal, por lo que podremos procesar cualquier lenguaje o tarea que se pueda realizar ejecutando comandos en terminal.

- Aunque normalmente se utiliza para compilar y construir proyectos software, Make también se puede utilizar para automatizar cualquier tarea que queráis realizar.

### 1.2 - Cómo funciona Make

Make utiliza un archivo llamado `Makefile`. En dicho archivo encontraremos principalmente dos tipos de estructuras, variables y reglas, que comentaremos más adelante.

Las variables serán un recurso auxiliar para las distintas tareas que realizará el `Makefile`, mientras las reglas serán las estructuras que generarán el proyecto o ejecutarán las tareas necesarias. Make utilizará estas reglas para saber que partes del proyecto son necesarias volver a compilar ya que se han actualizado y de esta forma evitar tener que generar todo desde cero.

Nosotros le diremos a Make que queremos que construya un objetivo, dicho objetivo tendrá asociada una regla, y Make se encargará de generar dicho objetivo automaticamente.

Además de esto también existen funciones con las que podremos ejecutar código en la terminal asociada.

Todas las tareas que realiza Make (tanto en las funciones, como en la ejecución de reglas) se ejecutarán como script en shell.

## 2 - Por qué deberías usar Make

Utilizar Make es una opción mucho más recomendable que escribir scripts de compilación e instalación propios. Make realiza las tareas más tediosas que puedes encontrar a la hora de construir un proyecto, como generar en el orden correcto las dependencias, además de solo volver a generar las dependencias que sean necesarias.

El principal objetivo de Make es que el construir un proyecto sea sencillo tanto para el usuario final como para el desarrollador, y en especial que este último no gaste más tiempo en la forma de construir su proyecto que en desarrollar el proyecto.

Además, Make se trata de un software multiplataforma, disponible en cualquier sistema operativo al ser software libre, por lo tanto podremos ejecutar un `Makefile` tanto en sistemas Unix (cualquier distribución GNU/Linux, macOS, *BSD, etc) como en entornos Windows (si el fichero `Makefile` es escrito de forma correcta).

## 3 - Instalación de Make

### 3.1 - Instalación en GNU/Linux

Para GNU/Linux la opción recomendada es instalar Make utilizando el gestor de paquetes de la distribución instalada:

- Para Arch Linux (y derivadas, como Manjaro):

```sh
sudo pacman -S make
```

- Para Debian (y derivadas, como Ubuntu):

```sh
sudo apt install build-essential
```

- Para RedHat (y derivadas, como Fedora):

```sh
sudo dnf install make
```

- Para openSUSE:

```sh
sudo zypper install make
```

### 3.2 - Instalación en macOS

Para instalar Make en macOS será necesario compilarlo desde el código fuente o bien instalarlo utilizando XCode:

```sh
xcode-select --install
```

### 3.3 - Instalación en Windows

Para instalar Make en Windows será necesario compilarlo desde el código fuente o bien instalarlo utilizando [Chocolatey](https://chocolatey.org/install):

```sh
choco install make
```

### 3.4 - Instalación desde el código fuente

Para instalarlo desde el código fuente tenemos que descargarnos la última versión disponible desde el servidor [FTP de GNU Make](https://ftp.gnu.org/gnu/make/) y utilizar el fichero `build.sh`:

```sh
./build.sh
```

También podremos compilarlo utilizando Make, aunque nos proporcionan este script de instalación en el caso de que no lo tengamos compilado.

## 4 - El fichero Makefile

Como hemos comentado, el fichero `Makefile` se compone a grandes rasgos de una serie de variables que utilizaremos para la construcción del proyecto, las reglas encargadas de construir el proyecto y funciones auxiliares para ayudar a realizar las tareas que creamos necesarias.

### 4.1 - Variables

En Make encontraremos algunas variables del sistema ya predefinidas, además de distintas formas de definir variables.

En todo caso, para referirnos a una variable una vez está declarada, utilizaremos un dolar seguido de el nombre de la variable entre parentesis:

```Makefile
$(mi_variable)
```

#### 4.1.1 - Formas de difinir variables

Existen dos formas de definir una variable, dependiendo de como queramos que se realice la expansión de variables, además de una declaración condicional.

##### 4.1.1.1 - Variables expandidas de forma recursiva

\

Para definir este tipo de variables utilizaremos el operador `=`, por ejemplo:

```Makefile
src_main = main.cpp
```

La ventaja de utilizar este tipo de variables es que el orden de declaración no importa, es decir, si queremos utilizar la variable `foo` dentro de otra variable, es indiferente el orden de definición, por ejemplo:

```Makefile
foo = $(bar)
bar = $(ugh)
ugh = Huh?
```

Aunque `bar` y `ugh` se declaran y definen despues de `foo`, en el momento que para `foo` consultemos el valor de `bar`, Make expanderá su valor y declarará esta variable, realizará el mismo proceso con `ugh` y por lo tanto `foo` tendrá como valor `Huh?`.

##### 4.1.1.2 - Variables expandidas de forma simple

\

Para definir este tipo de variables utilizaremos el operador `:=`, por ejemplo:

```Makefile
src_main := main.cpp
```

En este caso las variables se expanden de forma simple según se han escrito en el Makefile, por ejemplo, estas dos secciones de declaraciones serían equivalentes:

```Makefile
x := foo
y := $(x) bar
x := later
```

```Makefile
y := foo bar
x := later
```

Por lo tanto, si utilizamos el operador `:=` y utilizamos una variable definida más adelante, su valor será vacio, no el definido más adelante.


##### 4.1.1.3 - Variables declaradas condicionalmente

\

Tambien podremos utilizar el operador `?=`, este operador solo tiene efecto si la variable no ha sido definida todavía, por ejemplo:

```Makefilefile
FOO ?= bar
```

Si la variable `FOO` no se ha definido hasta ahora, tendrá el valor `bar`, pero si se ha definido antes (o se ha definido como parámetro al ejecutar el fichero `Makefile` como veremos más adelante), tendrá el valor anterior y no se realizará esta asignación.

#### 4.1.2 - Variables predefinidas

\

Las variables ya definidas en Make son variables comunmente utilizadas en la mayoría de entornos de programación, de forma que sus valores por defecto permiten crear ficheros de Make de forma más cómoda. Algunas de estas son:

- `CC`: Compilador de C, por defecto `cc`.
- `CFLAGS`: Opciones de compilación para el compilador de C.
- `CXX`: Compilador de C++, por defecto `g++`.
- `CXXFLAGS`: Opciones de compilación para el compilador de C++.
- `RM`: Comando para eliminar un fichero, por defecto `rm -rf`.
- `SHELL`: Terminal donde se ejecutarán los comandos, por defecto `/bin/sh` en entornos Unix.

### 4.2 - Reglas

Las reglas son la estructura principal de Make. Tienen la siguiente forma, en la que podemos distinguir tres partes:

```Makefile
objetivo/s: dependencias
	receta
```

Las dependencias pueden ser un conjunto tanto de objetivos como de ficheros, la receta es un conjunto de órdenes de shell y el objetivo será un conjunto de nombres separados por espacios.

Es importante destacar que la linea del objetivo y dependencias se escribe sin ningún nivel de indentación (si tenemos muchas dependencias podemos escapar el salto de linea), mientras que los comandos que formarán la receta han de estar indentados un nivel con el caracter `'\t'` y no espacios.

Cuando Make tenga que construir un objetivo, comprobará la regla de dicho objetivo, si no se cumple alguna de las dependencias las añadirá como objetivo y las construirá con sus respectivas reglas o comprobará si existen en el caso de ser ficheros, y una vez satisfechas todas las dependencias, ejecutará la receta para conseguir el objetivo. 

#### 4.2.1 - Objetivo de las reglas

\

El objetivo de una regla se trata de una lista de nombres separados por espacios. Cuando finaliza la ejecución de la receta de la regla asociada a un objetivo Make marcará dicho objetivo como cumplido.

Estos objetivos pueden ser ficheros o bien nombres que no serán ficheros y nos servirán como objetivos auxiliares para realizar una tarea.

#### 4.2.2 - Dependencias de las reglas

\

Las dependencias se tratan de requisitos que el desarrollador impone para que la receta de una regla sea generada. Por ejemplo:

```Makefile
main: main.o foo.o
	g++ main.o foo.o -o main

main.o: main.cpp
	g++ main.cpp -o main.o

foo.o: foo.cpp
	g++ foo.cpp -o foo.o
```

Si Make tiene como objetivo la regla `main`, hasta que no esté generado `main.o` y `foo.o`, no podrá ejecutar la receta de la regla `main`, de forma que Make se encargará de ejecutar antes las reglas necesarias para cumplir las dependencias.

Además, Make se encargará de decidir si es necesario volver a ejecutar una receta para una regla, por ejemplo, si generamos una vez el objetivo `main`, modificamos el fichero `main.cpp`, y volvemos a generar con Make el objetivo `main`, Make comprobará la dependencia de `main.o`, encontrará el fichero en el sistema, pero como la fecha de modificación de `main.o` es anterior a la fecha de modificación de `main.cpp` ejecutará la receta de esta regla, sin embargo, en el caso de `foo.o`, el fichero será más reciente a su dependencia `foo.cpp`, luego no ejecutará su receta ya que el objetivo está actualizado.

De esta forma Make solo ejecutará las recetas necesarias y que hayan sufrido cambios desde la última compilación.

#### 4.2.3 - Receta de las reglas

\

La receta de una regla se trata de una serie de instrucciones escritas en script de shell que Make ejecutará.

Todas las lineas que compongan la receta deberían tener un nivel de tabulación utilizando el caracter `\t`, y nunca indentación con espacios.

En principio, si una de estas instrucciones falla (devuelve un código de error distinto de cero) Make parará el proceso de construcción avisando de un error al generar un objetivo y detrendrá la ejecución del `Makefile`. Una forma de evitar esto es añadir `-` al inicio de la linea de la regla en la que permitimos que el comando falle, un ejemplo de la utilidad de este modificador es si tenemos una regla que elimina los binarios construidos:

```Makefile
clean:
	rm -f *.o
```

Esta regla, cuyo objetivo es `clean` y no tiene ninguna dependencia, tiene como receta eliminar todos los ficheros con extensión `.o` del directorio de trabajo, sin embargo, si esta regla es ejecutada por Make y no existe ningún fichero con dicha extensión el comando `rm` nos devolverá un estado de error y Make detendrá el proceso de compilación con un estado de error. En este caso, aunque queramos eliminar los ficheros objetos, también nos vale que estos no existan, luego si modificamos la regla de la siguiente forma, Make tan solo mostrará un aviso pero seguirá ejecutandose.

```Makefile
clean:
	-rm -f *.o
```

Otro detalle a tener en cuenta es que cuando Make ejecuta una regla el comando a ejecutar en el shell se mostrará por pantalla, esto puede ser de utilidad a la hora de ver que comandos se ejecutan y con que parámetros (por ejemplo al ejecutar `gcc`), sin embargo puede ser molesto para algunos casos como el siguiente:

```Makefile
saludar:
	echo "Hola"
```

En este caso, cuando se ejecute la regla con el objetivo `saludar`, obtendremos la siguiente salida:

```sh
echo "Hola"
Hola
```

Una forma de evitar esto es añadir el modificador `@` al principio de la linea de shell, evitando que se muestre el comando que se está ejecutando.

```Makefile
saludar:
	@echo "Hola"
```

Y con esto obtendríamos la siguiente salida:

```sh
Hola
```


##### 4.2.3.1 - Variables especiales dentro de las recetas

\

### 4.3 - Ejemplos de ficheros Makefile sencillos

### 4.4 - Funciones de Make

### 4.5 - Declaración de nuevas funciones

### 4.6 - Objetivos especiales

### 4.7 - Secciones condicionales

### 4.8 - Otras características y herramientas de interés

## 5 - Como ejecutar Make

## Bibliografía y enlaces de interés

[Página principal del proyecto GNU Make](https://www.gnu.org/software/make/)

[Manual oficial de GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

[Variables predefinidas en Make](https://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html)

[Convenciones al escribir Makefiles](https://www.gnu.org/software/make/manual/html_node/Makefile-Conventions.html#Makefile-Conventions)
