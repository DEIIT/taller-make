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

Además de esto también existen funciones con las que podremos ejecutar código en la terminal asociada.

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

```make
$(mi_variable)
```

#### 4.1.1 - Formas de difinir variables

Existen dos formas de definir una variable, dependiendo de como queramos que se realice la expansión de variables, además de una declaración condicional.

##### 4.1.1.1 - Variables expandidas de forma recursiva

Para definir este tipo de variables utilizaremos el operador `=`, por ejemplo:

```make
src_main = main.cpp
```

La ventaja de utilizar este tipo de variables es que el orden de declaración no importa, es decir, si queremos utilizar la variable `foo` dentro de otra variable, es indiferente el orden de definición, por ejemplo:

```make
foo = $(bar)
bar = $(ugh)
ugh = Huh?
```

Aunque `bar` y `ugh` se declaran y definen despues de `foo`, en el momento que para `foo` consultemos el valor de `bar`, Make expanderá su valor y declarará esta variable, realizará el mismo proceso con `ugh` y por lo tanto `foo` tendrá como valor `Huh?`.

##### 4.1.1.2 - Variables expandidas de forma simple

Para definir este tipo de variables utilizaremos el operador `:=`, por ejemplo:

```make
src_main := main.cpp
```

En este caso las variables se expanden de forma simple según se han escrito en el Makefile, por ejemplo, estas dos secciones de declaraciones serían equivalentes:

```make
x := foo
y := $(x) bar
x := later
```

```make
y := foo bar
x := later
```

Por lo tanto, si utilizamos el operador `:=` y utilizamos una variable definida más adelante, su valor será vacio, no el definido más adelante.


##### 4.1.1.3 - Variables declaradas condicionalmente

Tambien podremos utilizar el operador `?=`, este operador solo tiene efecto si la variable no ha sido definida todavía, por ejemplo:

```make
FOO ?= bar
```

Si la variable `FOO` no se ha definido hasta ahora, tendrá el valor `bar`, pero si se ha definido antes (o se ha definido como parámetro al ejecutar el fichero `Makefile` como veremos más adelante), tendrá el valor anterior y no se realizará esta asignación.

#### 4.1.2 - Variables predefinidas

Las variables ya definidas en Make son variables que permiten utilizar Make 


### 4.2 - Reglas

#### 4.2.1 - Objetivo de las reglas

#### 4.2.2 - Dependencias de las reglas

#### 4.2.3 - Comandos de las reglas


### 4.3 - Ejemplos de ficheros Makefile sencillos

### 4.4 - Funciones

### 4.5 - Objetivos especiales

### 4.6 - Secciones condicionales

### 4.7 - Otras características y herramientas de interés

## 5 - Como ejecutar Make

## Bibliografía

[Página principal del proyecto GNU Make](https://www.gnu.org/software/make/)

[Manual oficial de GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

[Variables predefinidas en Make](https://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html)
