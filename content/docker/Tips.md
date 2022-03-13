---
title: "Tips para Docker"
date: 2021-11-01T21:08:40+01:00
lastmod: 2021-11-01T21:08:40+01:00
publishdate: 2021-11-01T21:08:40+01:00
description: ""
weight: 10
---


1.- Docker stats
     este comando nos permite ver las estadisticas por contenedor

2.- docker system prune
    Este comando borra todo lo que no estemos usando (networks imagenes contenedores...)

3 docker inspect +containerid

4 docker cp containerid:/tmp

5 docker-compose restart always
on-failure:10

6 docker logs --tail=10 -t containerid

7 docker run --it  --entrypoint=bash imagen.
