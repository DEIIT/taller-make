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
objetivos: dependencias
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

En la receta de una regla también podremos utilizar las variables del fichero `Makefile`, además, Make nos da algunas variables especiales que solo podemos utilizar dentro de la receta, ya que estas variables están asociadas a la regla de la receta:

- `$@` : Nombre del archivo del objetivo de la regla. Si la regla tiene multiples objetivos, el nombre del archivo que generó que se ejecutara la receta.
- `$<` : Primer elemento de la lista de dependencias.
- `$?` : Dependencias que son más recientes que el objetivo, separadas por espacios. Si el objetivo no existe, esta variable serán todas las dependencias.
- `$^` : Todas las dependencias, separadas por espacios. Solo contiene archivos, no contendrá objetivos que no son ficheros. Si un fichero aparece más de una vez esta variable solo lo almacenará una única vez.
- `$+` : Misma variable que `$^`, pero si se tienen dependencias repetidas, aparecerán repetidas.
- `$|` : Dependencias que no son ficheros, separadas por espacios.

### 4.3 - Ejemplos de ficheros Makefile sencillos

Con las características comentadas hasta ahora podemos crear ficheros `Makefile` simples, como el siguiente ejemplo:

- Ejemplo de un Makefile de las primeras sesiones de la asignatura Metodología de la Programación:

```Makefile
HOME = .

LIB = $(HOME)/lib
INCLUDE = $(HOME)/include
BIN = $(HOME)/bin
SRC = $(HOME)/src
OBJ = $(HOME)/obj

OBJETIVOS = $(BIN)/I_PosicionPrimerBlanco $(BIN)/I_SaltaPrimeraPalabra $(BIN)/I_DemoCadenasClasicas

CXX = g++

all : $(OBJETIVOS) finalizado

$(BIN)/I_PosicionPrimerBlanco : $(SRC)/I_PosicionPrimerBlanco.cpp
	@echo -e "Generando $@"
	$(CXX) -o $@ $<
	@echo -e "Generado correctamente \n"

$(BIN)/I_SaltaPrimeraPalabra : $(SRC)/I_SaltaPrimeraPalabra.cpp
	@echo "Generando $@"
	$(CXX) -o $@ $<
	@echo -e "Generado correctamente \n"

$(BIN)/I_DemoCadenasClasicas : $(OBJ)/I_DemoCadenasClasicas.o $(OBJ)/MiCadenaClasica.o
	@echo -e "Generando $@"
	$(CXX) -o $@ $^
	@echo -e "Generado correctamente \n"

$(OBJ)/I_DemoCadenasClasicas.o : $(SRC)/I_DemoCadenasClasicas.cpp $(INCLUDE)/MiCadenaClasica.h
	@echo -e "Generando objetos necesarios"
	$(CXX) -c -o $@ $< -I$(INCLUDE)
	@echo

$(OBJ)/MiCadenaClasica.o : $(SRC)/MiCadenaClasica.cpp $(INCLUDE)/MiCadenaClasica.h
	@echo -e "Generando objetos necesarios"
	$(CXX) -c -o $@ $< -I$(INCLUDE)
	@echo

finalizado :
	@echo -e "/***********************************************************/"
	@echo -e "\nTodas las tareas han sido ejecutadas correctamente \n"
	@echo -e "/***********************************************************/\n"

clean :
	-rm $(OBJ)/*

mrproper : clean
	-rm $(BIN)/*
```

## 5 - Como ejecutar Make

### 5.1 - Como lanzar Make de forma básica

Una vez ya sabemos escribir un archivo `Makefile`, para poder obtener resultados tenemos que ejecutar dicho fichero con Make.

Make reconocerá el fichero `Makefile` en la carpeta en la que se ejecuta siempre que este fichero se llame `makefile` o `Makefile`, por lo que podemos simplemente ejecutar lo siguiente en el directorio donde se encuentre dicho fichero:

```sh
make
```

Si el fichero tiene un nombre distinto, podemos utilizar el parámetro `-f`:

```sh
make -f mi_fichero_make
```

### 5.2 - Como especificar el objetivo a Make

Si lanzamos Make sin especificar ningún objetivo, Make tendrá como objetivo la primera regla que encuentre en el fichero `Makefile`. Podemos especificarle un objetivo concreto ejecutandolo de la siguiente forma:

```sh
make <objetivo>
```

Y esto se puede añadir a la opción de utilizar un `Makefile` con un nombre especial:

```sh
make <objetivo> -f mi_fichero_make
```

Por ejemplo, si en el ejemplo de la sección anterior queremos construir solo el binario `$(BIN)/I_PosicionPrimerBlanco`, podríamos ejecutar:

```sh
make ./bin/I_PosicionPrimerBlanco -f mi_fichero_make
```

Y si por ejemplo, queremos limpiar los binarios, cosa que no se realiza de forma normal al lanzar Make, podríamos usar la siguiente orde:

```sh
make mrproper -f mi_fichero_make
```

### 5.3 - Objetivo all

Como hemos comentado, Make tendrá como objetivo la primera regla que se especifique siempre que no se ejecute con un objetivo definido, por este motivo es una práctica bastante estandarizada incluir un objetivo llamado `all` que siempre será la primera regla que aparecerá en el `Makefile`. De esta forma, ejecutar:

```sh
make
```

Ha de ser equivalente a ejecutar:

```sh
make all
```

### 5.4 - Definiendo variables al llamar a Make

Como vimos al principio, en Make existe el operador `?=` para definir una variable, en el que se asigna el valor si la variable no se ha definido antes.

Uno de los casos en los que nos es util este operador es si definimos una variable al llamar a Make, esto lo podemos hacer de la siguiente forma:

```sh
make foo=hola
```

De esta forma, la variable `foo` tendrá el valor `hola` dentro del `Makefile`.

Un ejemplo de usar esta forma de definir variables es el siguiente `Makefile`:

```Makefile
HOME = .
SRC ?= $(HOME)/src
BIN ?= $(HOME)/bin

all : $(BIN)/ejemplo

$(BIN)/ejemplo : $(SRC)/ejemplo.cpp
	g++ -o $@ $^
```

Si lanzamos este `Makefile` utilizando la orden `make`, usará las rutas de `src` y `bin` por defecto, sin embargo si ejecutamos por ejemplo:

```sh
make BIN=../salida SRC=./src/programa1
```

Podemos cambiar la ruta en la que el `Makefile` generará los binarios y en la que obtendrá el código fuente.


### 5.5 - Otros parámetros de Make

Algunos de los parámetros más interesantes que nos ofrece Make son:

- `-s` : Ejecutará todas las recetas sin mostrar su contenido, equivalente a utilizar `@` en todos los comandos de las recetas.
- `-B` : Reconstruir todas las recetas, aunque no sea necesario.
- `-C dir` : Cambiar el directorio antes de leer el `Makefile`, por ejemplo, si el fichero Make se encuentra en la carpeta `build` y queremos ejecutar Make desde la raiz podemos ejecutar `make -C build`
- `-d` : Mostrar información de depuración a nivel de Make, normalmente que archivos se están recompilando,  que reglas se están utilizando, entre otros detalles.
- `-i` : Ignora los errores al volver a construir archivos, sería equivalente a utilizar `-` en todos los comandos de las recetas.
- `-k` : Intenta continuar aunque Make encuentre un error.
- `-j [N]` : Número de comandos a ejecutar de forma simultanea. Si no se especifica número se ejecutarán tantos como sea posible. Opción solo disponible en entornos Unix, en Windows esta opción es ignorada.
- `-o archivo` : No reconstruir el archivo dado, aunque la fecha de modificación del fuente sea más reciente.

## 6 - Aspectos más avanzados de Make

Aunque hemos visto los aspectos básicos de Make con los que se podría trabajar perfectamente, en esta sección cubriremos características que nos ayudarán a escribir nuestro `Makefile` de una forma más rapida.


### 6.1 - Reglas con patrones

Make también soporta escribir reglas con patrones. Para realizar esta tarea, también añade el operador `%`.

Si escribimos la siguiente regla:

```Makefile
*.o : *.cpp
	g++ -o $@ $^
```

Si disponemos de multiples objetivos y ficheros de C++, estamos indicando una regla con multiples objetivos, y como dependencia una gran lista de ficheros de C++, en la que los objetivos se construirán con todos los ficheros de C++, sin embargo, el operador `%` se utiliza para referirse a un único fichero, y si `%` aparece varias veces, el valor siempre ha de coincidir, por ejemplo:

```Makefile
%.o : %.cpp
	g++ -o $@ $^
```

En el primer caso (utilizando asterisco), si tenemos dos ficheros `foo.cpp` y `bar.cpp`, la regla tendría dos objetivos, y dos dependencias, sin embargo, en el segundo caso la regla tendría un único objetivo y una única dependencia, reutilizandose para ambos ficheros, de forma que si en otra regla se tiene como dependencia `foo.o`, esta última regla tomaría el siguiente valor:

```Makefile
foo.o : foo.cpp
	g++ -o $@ $^
```

Y respectivamente si se cambia `foo.o` por `bar.o`. Esto será muy util para reglas repetitivas que suelen tener la misma entrada, por ejemplo, el `Makefile` de ejemplo sencillo quedaría así:



```Makefile
HOME = .

LIB = $(HOME)/lib
INCLUDE = $(HOME)/include
BIN = $(HOME)/bin
SRC = $(HOME)/src
OBJ = $(HOME)/obj

OBJETIVOS = $(BIN)/I_PosicionPrimerBlanco $(BIN)/I_SaltaPrimeraPalabra $(BIN)/I_DemoCadenasClasicas

CXX = g++

all : $(OBJETIVOS) finalizado

$(BIN)/% : $(SRC)/%.cpp
	@echo -e "Generando $@"
	$(CXX) -o $@ $<
	@echo -e "Generado correctamente \n"


$(BIN)/I_DemoCadenasClasicas : $(OBJ)/I_DemoCadenasClasicas.o $(OBJ)/MiCadenaClasica.o
	@echo -e "Generando $@"
	$(CXX) -o $@ $^
	@echo -e "Generado correctamente \n"

$(OBJ)/%.o : $(SRC)/%.cpp $(INCLUDE)/%.h
	@echo -e "Generando objetos necesarios"
	$(CXX) -c -o $@ $< -I$(INCLUDE)
	@echo

finalizado :
	@echo -e "/***********************************************************/"
	@echo -e "\nTodas las tareas han sido ejecutadas correctamente \n"
	@echo -e "/***********************************************************/\n"

clean :
	-rm $(OBJ)/*

mrproper : clean
	-rm $(BIN)/*
```

### 6.2 - Funciones de Make

Make tiene una serie de funciones predefinidas que nos pueden ser muy útiles a la hora de trabajar con nuestros proyectos. La forma de llamar a las funciones es la siguiente:

```Makefile
$(nombre_funcion parametro1...parametroN)
```

Algunas de las funciones más interesantes son:

- `value` : Nos será de utilidad por el comportamiento de las recetas de las distintas reglas. Las recetas se tratan de comandos en shell, por lo tanto, puede darse el siguiente caso:

```Makefile
FOO = $PATH

all:
	@echo $(FOO)
```

En este caso, `FOO` se expandería como `$PATH`, sin embargo Make expandería `$P` como una variable de Make (que estaría vacia) y por lo tanto el comando `echo` tendrá como salida `ATH`, una forma de solucionarlo es utilizando la función `value`:


```Makefile
FOO = $PATH

all:
	@echo $(value FOO)
```

En este caso, Make evaluará la variable `FOO` y sustituirá su contenido por completo, de forma que se ejecutará `echo $PATH` y el resultado será la variable de entorno con el directorio de trabajo actual.


- `shell` : Nos permitirá ejecutar comandos en shell y nos devolverá su salida. Uno ejemplo de su uso es si queremos tener una variable que contenga la ruta absoluta, realice búsquedas de ficheros o busque patrones de expresiones regulares y queremos utilizar alguna herramienta de shell como `find`, `grep`, etc.

```Makefile
DIRECTORIO := $(shell pwd)
FICHEROS_C := $(shell echo *.c)
IMG_PNG := $(shell find img/ -name "*.png")
```

- `subst` : Sustituir texto en una cadena, por ejemplo:

```Makefile
$(subst ol,lo,Hola mundo)
```

Cambiaría las apariciones de `ol` por `lo` en la cadena `Hola`, quedando como `Hloa mundo`.

- `patsubst` : Como la anterior, pero además soporta patrones con `%`, de forma que se pueden hacer sustituciones uno a uno con ficheros con el mismo nombre. Un ejemplo muy usual de esto es cambiar la extensión de una serie de ficheros:

```Makefile
$(patsubst %.c,%.o,foo.c bar.c)
```

Esto daría como resultado `foo.o bar.o`.

- `strip` : Elimina los espacios en blanco repetidos, dejando únicamente un espacio en blanco, por ejemplo:

```Makefile
$(strip  a     b  c  )
```

Daría como resultado `a b c`.

Esta función será util cuando algunas variables tengan espacios al inicio o final, y queramos concatenar valores, por ejemplo:

```Makefile
archivo = a 
extension = .txt

nombre_completo = $(archivo)$(extension)
```

Esto daría como resultado ` a  .txt` debido a los espacios en las variables, sin embargo, podemos escribir lo siguiente:

```Makefile
archivo = a 
extension = .txt

nombre_completo = $(strip $(archivo))$(strip $(extension))
```

Que daría como resultado `a.txt` al eliminar los espacios antes y despues de cada palabra.


- `wildcard` : Nos permite utilizar las expansiones que realiza Make dentro de otras funciones. Por ejemplo, si al declarar una variable utilizamos `*.c`, dicha variable contendrá todos los ficheros que tengan extensión de C, sin embargo, si queremos utilizar esa lista de ficheros en otra función podemos utilizar la función `wildcard`. Por ejemplo, podríamos obtener una lista de todos los ficheros en C, pero cambiando su extensión para tener su nombre como código objeto:

```Makefile
$(patsubst %.c,%.o,$(wildcard *.c))
```


- `foreach` : Nos permite iterar sobre una lista para aplicar alguna operación, por ejemplo, si quisieramos los nombres de los archivos de ciertos directorios:

```Makefile
archivos := $(foreach directorio, src include, $(wildcard $(directorio)/*))
```

Con lo que obtendríamos todos los ficheros en los directorios `src` e `include`.

- `call` : Como veremos en la siguiente sección, nosotros podemos definir funciones propias en nuestro `Makefile`. Esta función nos permite llamar a dichas funciones.

```Makefile
$(call funcion, param1, ..., paramN)
```

### 6.3 - Declaración de nuevas funciones

Las funciones en Make ejecutarán código en shell, y para declarar una función en Make utilizaremos la siguiente estructura:

```Makefile
define funcion
	comandos
endef
```

De esta forma podremos crear funciones con el objetivo de crear tareas concretas y utilizarlas con la función `call`. Nos podemos referir a los parámetros dados con `$(N)` siendo `N` el número del parámetro comenzando desde 1, por ejemplo:

```Makefile
define creadir
	@printf "\033[1;32mCreando directorio\033[0m %s\n" $(1)
	@mkdir -p $(2)
endef

all:
	$(foreach dir,src bin obj, $(call creadir, $(dir)))
```

### 6.4 - Objetivos especiales

Make tiene algunos objetivos especiales, utilizados para el funcionamiento interno de Make, algunos de ellos son:

- `.PHONY`: Este objetivo se utiliza para marcar los objetivos que no se tratan de ficheros, si no de reglas auxiliares para el proceso de compilación, de forma que Make no comprobará su última fecha de modificación para ver si es necesario lanzar la regla, siempre la lanzará. El objetivo `all`, por ejemplo, siempre debería formar parte del objetivo `.PHONY`.

Para utilizarlo basta con asignarle el valor, por ejemplo:

```Makefile
define creadir
	@printf "\033[1;32mCreando directorio\033[0m %s\n" $(1)
	@mkdir -p $(2)
endef

.PHONY = all

all:
	$(foreach dir,src bin obj, $(call creadir, $(dir)))
```

Se puede asignar a todas las reglas que queramos, tanto por separado como de forma individual:

```Makefile
define creadir
	@printf "\033[1;32mCreando directorio\033[0m %s\n" $(1)
	@mkdir -p $(2)
endef

# También podríamos usar:
# .PHONY = all clean

.PHONY = all

all:
	$(foreach dir,src bin obj, $(call creadir, $(dir)))

.PHONY = clean

clean :
	-rm *.o
```


- `.DEFAULT` : La receta asociada a este objetivo se usará todos las dependencias que no tengan asociado un objetivo para ser construidos.

- `.SILENT` : Las dependencias de este objetivo no mostrarán sus recetas al ser ejecutadas (equivalente a utilizar `@` en todos los comandos de la receta). Si se utiliza este objetivo, pero no se asignan dependencias, ninguna regla mostrará su receta (equivalente a ejecutar Make con el parámetro `-s`).

- `.ONESHELL` : Si se menciona como objetivo, cuando se ejecuta la receta de un objetivo todos los comandos se ejecutarán en la misma shell.

### 6.5 - Secciones condicionales

En Make podemos tener secciones condicionales de la siguiente forma:

```Makefile
<condicional>
si-cierto
else <otro-condicional>
si-otro
else
si-ambos-falso
endif
```

Siendo las secciones else opcionales. Las directivas condicionales son las siguientes:

- `ifeq (arg1, arg2)` : Si los argumentos son iguales, se evalua como cierto. Otra sintaxis válida es `ifeq 'arg1' 'arg2'`, sin importar que tipo de comillas utilizar siempre que se usen de forma consistente.
- `ifneq (arg1, arg2)` : Si los argumentos son distintos, se evalua como cierto. Otra sintaxis válida es `ifeq 'arg1' 'arg2'`, sin importar que tipo de comillas utilizar siempre que se usen de forma consistente.
- `ifdef var` : Si una variable está definida se evalua como cierto.
- `ifndef var` : Si una variable no está definida se evalua como cierto.

Un ejemplo sería el siguiente:

```Makefile
ifeq ($(CXX), g++)
CXXFLAGS = -std=gnu++20
else
CXXFLAGS = -std=c++20
endif
```

En este ejemplo comprobamos el compilador a utilizar, si es G++, compilaremos con las extensiones de GNU para C++20, si es otro compilador, utilizaremos la versión estandar de C++20.

## 7 - Un poco más alla de Make: CMake

Aunque con las herramientas mencionadas se podría escribir un Makefile que funcione en distintas plataformas, es un proceso tedioso y a veces tan complicado que es más sencillo escribir `Makefile`s distintos. Por este motivo surgió CMake, cross platform make.

CMake utiliza una única sintaxis para generar un `Makefile` dependiendo del sistema operativo en el que se ejecute CMake, de forma que el programador no se tiene que preocupar por detalles del sistema sobre el que va a compilar, simplemente lanzar CMake y este se encargará de generar todo lo necesario para que funcione en el S.O utilizado.

Si os interesa, CMake tiene publicado en su [página web](https://cmake.org/) un tutoríal para [comenzar a utilizar CMake](https://cmake.org/cmake/help/latest/guide/tutorial/index.html).

## Bibliografía y enlaces de interés

[Página principal del proyecto GNU Make](https://www.gnu.org/software/make/)

[Manual oficial de GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

[Variables predefinidas en Make](https://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html)

[Convenciones al escribir Makefiles](https://www.gnu.org/software/make/manual/html_node/Makefile-Conventions.html#Makefile-Conventions)
