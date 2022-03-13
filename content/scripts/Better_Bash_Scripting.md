---
title: "Better Bash Scripting"
date: 2021-12-08T19:31:12+01:00
lastmod: 2021-12-08T19:31:12+01:00
publishdate: 2021-12-08T19:31:12+01:00
description: ""
weight: 10
---

## Secuencias de comandos más

Empiezo cada script de bash con el siguiente prólogo:

```bash
#!/bin/bash
set -o nounset
set -o errexit
```

Esto se encargará de dos errores muy comunes:

+ Hacer referencia a variables indefinidas (cuyo valor predeterminado es "")
+ Ignorar los comandos fallidos

Las dos configuraciones también tienen abreviaturas (“-u” y “-e”) pero las versiones más largas son más legibles. Si se tolera un comando fallido, use este modismo:

```bash
if ! <possible failing command> ; then
    echo "failure ignored"
fi
```

Tenga en cuenta que algunos comandos de Linux tienen opciones que, como efecto secundario, suprimen algunos fallos, por ejemplo,
“ mkdir -p ” y “ rm -f ”. También tenga en cuenta que el modo "errexit", aunque es una valiosa primera línea de defensa , no detecta todos los fallos, es decir, en determinadas circunstancias, los comandos que fallan no se detectarán. (Para obtener más información, eche un vistazo a este hilo ).
Un lector sugirió el uso adicional de "set -o pipefail"

## Funciones

Bash le permite definir funciones que se comportan como otros comandos; utilícelas libremente; le dará a sus scripts de bash un impulso muy necesario en legibilidad

```bash
ExtractBashComments() {
    egrep "^#"
} 
cat myscript.sh | ExtractBashComments | wc 
comments=$(ExtractBashComments < myscript.sh)
```

Algunos ejemplos más instructivos:

```bash
SumLines() {  # iterating over stdin - similar to awk      
    local sum=0
    local line=””
    while read line ; do
        sum=$((${sum} + ${line}))
    done
    echo ${sum}
} 
SumLines < data_one_number_per_line.txt 
log() {  # classic logger
   local prefix="[$(date +%Y/%m/%d\ %H:%M:%S)]: "
   echo "${prefix} $@" >&2
} 
log "INFO" "a message""

```

Intente mover todo el código bash a funciones dejando  solo definiciones de variable / constante global y una llamada a "main" en el nivel superior.

## Anotaciones variables

Bash permite una forma limitada de anotaciones variables. Los más importantes son:

+ local (para variables locales dentro de una función)
+ readonly (para variables de solo lectura)

```bash
# a useful idiom: DEFAULT_VAL can be overwritten
#       with an environment variable of the same name
readonly DEFAULT_VAL=${DEFAULT_VAL:-7} 
myfunc() {
   # initialize a local variable with the global default
   local some_var=${DEFAULT_VAL}
   ...
}
```

Tenga en cuenta que es posible hacer una variable  de solo lectura que no era antes:

```bash
x = 5
x = 6
readonly x
x = 7 # falla
```

Esfuércese por anotar casi todas las variables en un script bash con local o readonly.

## Favorecer   $ () sobre comillas invertidas ( ` )

Las comillas son difíciles de leer y en algunas fuentes se confunden fácilmente con comillas simples.
$ () también permite anidar sin los dolores de cabeza de las comillas.

```bash
#Los dos comandos sirven para imprimir por pantalla: A-B-C-D
echo "A-`echo B-\`echo C-\\\`echo D\\\`\``"
echo "A-$(echo B-$(echo C-$(echo D)))"
```

## Favorece  [[ ]]  (corchetes dobles) sobre []

[[ ]] evita problemas como la expansión inesperada del nombre de ruta, ofrece algunas mejoras sintácticas
y agrega nuevas funciones:

```bash
Operador        Significado
||              lógica o (solo corchetes dobles)
&&              lógica y (solo corchetes dobles)
<               comparación de cadenas (no es necesario escapar entre corchetes dobles)
-lt             comparación numérica
=               coincidencia de cadenas con globbing
==              coincidencia de cadenas con globbing (solo corchetes dobles, ver más abajo)
= ~             cadena que coincide con expresiones regulares (solo corchetes dobles, ver más abajo)
-n              cadena no está vacía        
-z              cadena está vacía
-eq             igualdad numérica
-ne             desigualdad numérica
```

### Corchete simple

```bash
["$ {nombre}" \> "a" -o $ {nombre} \ <"m"]
```

### Corchetes dobles

```bash
 [["$ {nombre}"> "a" && "$ {nombre}" <"m"]]
```

## Expresiones regulares / Globbing

Estas nuevas capacidades entre corchetes dobles se ilustran mejor con ejemplos:

```bash
t = "abc123"
[[ "$t" == abc* ]] # verdadero (globbing)
[[ "$t" == "abc*" ]] # falso (coincidencia literal)
[[ "$t" = ~ [abc]+[123]+ ]] # verdadero (expresión regular)
[[ "$t" = ~ "abc*" ]] # falso (coincidencia literal)
```

Tenga en cuenta que, a partir de la versión 3.2 de bash, la expresión regular o globbing
no debe estar entre comillas. Si su expresión contiene espacios en blanco, puede almacenarla en una variable:

```bash
r="ab+"
[["a bbb" =~ $r]] # verdadero
```

La coincidencia de cadenas basada en globos también está disponible a través de la declaración de caso:

```bash
case $t in
abc*)  <action> ;;
esac
```

## Manipulación de cadenas

Bash tiene varias formas (subestimadas) de manipular cadenas.

Lo basico

```bash
f="path1/path2/file.ext"  
len="${#f}" # = 20 (string length) 
# slicing: ${<var>:<start>} or ${<var>:<start>:<length>}
slice1="${f:6}" # = "path2/file.ext"
slice2="${f:6:5}" # = "path2"
slice3="${f: -8}" # = "file.ext"(Note: space before "-")
pos=6
len=5
slice4="${f:${pos}:${len}}" # = "path2"
```

Sustitución (con globbing)

```bash
f="path1/path2/file.ext"  
single_subst="${f/path?/x}"   # = "x/path2/file.ext"
global_subst="${f//path?/x}"  # = "x/x/file.ext" 
# string splitting
readonly DIR_SEP="/"
array=(${f//${DIR_SEP}/ })
second_dir="${array[1]}"     # = path2
```

Eliminación al principio / final (con globbing)

```bash
f="path1/path2/file.ext" 
# eliminación al comienzo de la cadena  extension = "$ {f # *.}" # = "ext" 
# eliminación codiciosa al comienzo de la cadena
filename="${f##*/}"  # = "file.ext" 
# eliminación al final de la cadena
dirname="${f%/*}"    # = "path1/path2" 
# eliminación codiciosa al final
root="${f%%/*}"      # = "path1"
```

## Evitar archivos temporales

Algunos comandos esperan nombres de archivo como parámetros, por lo que la canalización sencilla no funciona.
Aquí es donde el  operador <() resulta útil, ya que toma un comando y lo transforma en algo
que se puede usar como un nombre de archivo:

```bash
# descargar y diferenciar dos páginas web
diff <(wget -O - url1) <(wget -O - url2)
```

También son útiles los "documentos aquí" que permiten pasar cadenas arbitrarias de varias líneas
en stdin. Las dos apariciones de 'MARKER' entre corchetes el documento.
'MARCADOR' puede ser cualquier texto.

```bash
# DELIMITER es un comando de cadena arbitrario 
command << MARKER
...
$ {var}
$ (cmd)
...
MARKER
```

Si la sustitución de parámetros no es deseable, simplemente coloque comillas alrededor de la primera aparición de MARKER:

```bash
command << 'MARCADOR'
...
no se está realizando ninguna sustitución aquí.
$ (signo de dólar) se transmite literalmente.
...
MARCADOR
```

## Variables integradas

### Para referencia

```bash
$0  #nombre del script
$n  #parámetros posicionales al script / función
$$  #PID del script
$!  #PID del último comando ejecutado (y ejecutado en segundo plano)
$?  #estado de salida del último comando ($ {PIPESTATUS} para comandos canalizados)
$#  #número de parámetros del script / función
$@  #todos los parámetros del script / función (ve los argumentos como palabras separadas)
$*  #todos los parámetros del script / función (ve argumentos como una sola palabra)
#-----Nota------
$*  #rara vez es la opción correcta.
$@  #maneja la lista de parámetros vacía y el espacio en blanco dentro de los parámetros correctamente.
$@  #generalmente debe estar entrecomillado como "$@"
```

## Depuración

Para realizar una verificación de sintaxis / ejecución en seco de su script bash, ejecute:

```bash
bash -n myscript.sh
```

Para producir un rastro de cada comando ejecutado, ejecute:

```bash
bash -v myscripts.sh
```

Para producir un rastro del comando expandido use:

```bash
bash -x myscript .sh
```

-v y -x también se pueden hacer permanentes agregando
set -o verbose y set -o xtrace al prólogo del script.
Esto puede ser útil si el script se ejecuta en una máquina remota, por ejemplo,
un build-bot y está registrando la salida para una inspección remota.

## Señales de que no debería usar un script bash

+ Su secuencia de comandos tiene más de unos pocos cientos de líneas de código
+ Necesita estructuras de datos más allá de simples arreglos
+ Tiene dificultades para solucionar problemas de cotización
+ Haces mucha manipulación de cadenas
+ No tiene mucha necesidad de invocar otros programas o de incluirlos
+ Te preocupas por el rendimiento

En su lugar, considere lenguajes de secuencias de comandos como Python o Ruby.