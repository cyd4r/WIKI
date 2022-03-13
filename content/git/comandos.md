---
title: "Comandos"
date: 2021-11-01T21:10:12+01:00
lastmod: 2021-11-01T21:10:12+01:00
publishdate: 2021-11-01T21:10:12+01:00
description: ""
weight: 10
---


# ¿ Qué es GIT?

Git es un conocido sistema de control de versiones distribuido de código abierto.

Fue creado para manejar desde pequeños proyectos, hasta grandes proyectos empresariales, pero con algo en común… siempre con la misma rapidez y eficacia.

## Comandos GIT imprescindibles
    
**git config**

Aplicar una configuración especifica al usuario, algoritmo, nombre, email, etc…

ejemplo

``` git config --global user.email nombre@domain.com ```

 
**git init**

Crear / generar un repositorio nuevo.

```git init```
 
**git add**

Agregar archivos al index principal. Como ejemplo añadimos el archivo «ejemplo.txt».

```git add ejemplo.txt```

 
**git clone**

Con este comando puedes clonar un repositorio remoto a local, o simplemente supervisarlo.

ejemplo de supervisión del repositorio…

```git clone nombre@normalmente-la-ip:/path/to/repository```

ejemplo de «clonar repositorio»…

```git clone /path/to/repository```

 
**git commit**

Editar el contenido actual del indice.

ejemplo…

``` git commit –m “ Mi mensaje” ```

 
**git push**

Obviamente este es el comando más usado. Se usa para enviar ampliaciones, ediciones, etc…, a un repositorio remoto.

```git push  origen-master```

 
**git status**

Imprime en pantalla los archivos que han sido modificados, junto a los que van a ser añadidos.

```git status```

 
**git checkout**

Crear nuevas ramas o intercambiar entre ellas (que son las ramas de git).

ejemplo crear nueva rama y cambiarse a la nueva

``` git checkout -b <nueva-rama>```

ejemplo intercambiar rama…
 
```git checkout <otra-rama>```

 
**git remote**

Conecta con opciones a un repositorio remoto.

Conectar desde un repositorio local a un server remoto.

```git remote add origin <IP>```

Saber los repositorios que están activos.

```git remote -v```

 
**git branch**

Borrar o listar ramas.

ejemplo de borrar ramas…

``` git branch -d <otra-rama>```

Listar ramas.
```git branch```

 
**git pull**

Aplicar todas las modificaciones realizadas en tu repositorio local.

```git pull```

 
**git merge**

Fusionar una rama con otra (debe estar activa).

```git merge <segunda-rama>```

 
**git diff**

Listar todos los conflictos.

```git diff```

Listar los conflictos de un archivo en particular.
```git diff --base <archivo-demo>```

Ver los conflictos existentes entre dos ramas, es muy recomendable ejecutar este comando antes de fusionar.
``` git diff <rama-uno> <rama-dos>```

 
**git reset**

Resetear el directorio e index, en el cual estas trabajando actualmente.

```git reset - -hard HEAD```

 
**git rm**

Borrar archivos o directorios.

```git rm archivo.txt```

 
**git show**

Imprime información de cualquier objeto GIT.

```git show```

 
**git fetch**

Muestra los objetos de un repositorio remoto, que no están en el repositorio local.

```git fetch origen```

 
**git grep**

Buscar frases o palabras en todo el contenido.

```git grep “frase a buscar”```

 
**gitk**

Invocar una interfaz gráfica para el repositorio local.

```gitk```

 
**git instaweb**

Con este comando puedes hacer que un repositorio local, este interconectado con un servidor web.

```
git instaweb –http=...
```
 
**git gc**

Optimiza el repositorio (limpia basura, optimiza archivos, etc…).

```git gc```

 
**git archive**

Crear tar o zip de los archivos de un árbol del repositorio.

```
git archive – -format=tar master
```
 
**git prune**

Eliminar todos los objetos que no tengan ningún puntero de entrada.

```
git prune
```
 
**git fsck**

Chequear la integridad del sistema de archivos GIT.
```
git fsck
```
 
**git rebase**

Aplicar los mismos compromisos en otra rama.

```
git rebase master
```
 

 

