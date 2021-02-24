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



## 4 - El fichero Makefile

### 4.1 - Variables

#### 4.1.1 - Variables predefinidas

#### 4.1.2 - Formas de difinir variables

### 4.2 - Reglas

#### 4.2.1 - Objetivo de las reglas

#### 4.2.2 - Dependencias de las reglas

#### 4.2.3 - Comandos de las reglas

### 4.3 - Funciones

### 4.4 - Objetivos especiales

### 4.5 - Secciones condicionales

### 4.6 - Otras características y herramientas de interés

## 5 - Como ejecutar Make




## Bibliografía

[Página principal del proyecto GNU Make](https://www.gnu.org/software/make/)

[Manual oficial de GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)
