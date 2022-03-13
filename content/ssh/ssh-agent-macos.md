---
title: "Ssh Agent Macos"
date: 2021-12-08T18:12:12+01:00
lastmod: 2021-12-08T18:12:12+01:00
publishdate: 2021-12-08T18:12:12+01:00
description: ""
weight: 10
---


# ¿Como añadir el ssh Agent a mac?

- Para añadir nuestra llave ssh al user-agent de nuestra terminal debemos de realizar los siguientes pasos.
    
1.  Lo primero es generar una  llave ssh (si no tenemos)
      ```bash
      ssh-keygen -t rsa -f (nombre que queramos que tenga)
      ```  
     Podemos omitir la parte desde el -f
2.  Es necesario que hagamos la siguiente configuración en el fichero que se encuentra en la siguiente ruta `~/.ssh/config` , si no existe siempre se puede crear.

    Una Vez tenemos creado el fichero tenemos que configurarlo de la siguiente manera:
    ```bash
    Host *
      AddKeysToAgent yes
      IdentityFile ~/.ssh/id_rsa --> es nuestra llave si le hemos puesto otro nombre pondrémos ese.
    ```
Una vez añadamos esto en el fichero debemos realizar el último paso antes de poder conectarnos con ssh -A para que pase  nuestro agente.

3.  El último paso es para añadir nuestra llave al llavero de mac.
       ```bash
       ssh-add -k ~/.ssh/id_rsa o el nombre de la llave que tengamos
       ```
    Esto nos solicitará la contraseña de nuestro mac y una vez lo pongamos, ya lo tendremos listo para funcionar.
    Para comprobarlo podemos realizar el siguiente comando:`ssh-add -l`

Y podemos usarlo con `ssh -A Servidor -l usuario` o `ssh -A usuario@servidor`