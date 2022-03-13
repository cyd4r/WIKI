---
title: "Wireguard"
date: 2021-12-08T18:23:06+01:00
lastmod: 2021-12-08T18:23:06+01:00
publishdate: 2021-12-08T18:23:06+01:00
description: ""
weight: 10
---

# Indice de contenido
    1 Instalar y congurar WireGuard VPN en Ubuntu
    1.1 Instalar WireGuard en Ubuntu
    1.2 Congurar WireGuard
    1.3 Congurar el cortafuegos
    1.4 Iniciar WireGuard
    1.5 Instalar WireGuard cliente
    1.6 Conectar el cliente con el servidor
## 1. Instalar y congurar WireGuard
La instalación de WireGuard en Ubuntu y derivados es
bastante simple. Tan solo debemos agregar el repositorio e
instalar la herramienta.

```bash
sudo add-apt-repository ppa:wireguard/wireguard
sudo apt install wireguard
```

## 2. Configurar WireGuard
Lo primero que tenemos que hacer es generar una clave
publica.
```bash
umask 077
sudo wg genkey | tee privatekey | wg
pubkey > publickey
```

<!-- Puedes ver las claves generadas con: cat privatekey y  cat
publickey .

En nuestro ejemplo creamos el archivo de conguración
wg0.conf.
Copia y pega lo siguiente (donde «Tu-KEY», agregas tu clave).
SUSCRÍBETE A NUESTRO
BOLETÍN
Correo electrónico *
¡Suscríbete!
5. Se instalarán los siguientes
paquetes adicionales:
6. wireguard-dkms wireguard-tools
7. Se instalarán los siguientes
paquetes NUEVOS:
8. wireguard wireguard-dkms
wireguard-tools
9. 0 actualizados, 3 nuevos se
instalarán, 0 para eliminar y 2 no
actualizados.
10. Se necesita descargar 358 kB de
archivos.
11. Se utilizarán 2.041 kB de espacio de
disco adicional después de esta
operación.
12. ¿Desea continuar? [S/n]
1. umask 077
2.
3. wg genkey | tee privatekey | wg
pubkey > publickey
1. nano /etc/wireguard/wg0.conf
1. [Interface]
2. PrivateKey = <Tu-KEY>
3. Address = 10.0.0.1/24,
fd86:ea04:1115::1/64
4. ListenPort = 51820
5. PostUp = iptables -A FORWARD -i wg0 -
j ACCEPT; iptables -t nat -A
POSTROUTING -o eth0 -j MASQUERADE;
ip6tables -A FORWARD -i wg0 -j

Usamos cookies para garantizar que tengas la mejor experiencia en nuestro sitio. Si continúas en
"sololinux.es" asumiremos que estás conforme.
OK Leer más
3/10/2020 Instalar y configurar WireGuard VPN en Ubuntu y derivados
https://www.sololinux.es/instalar-y-configurar-wireguard-vpn-en-ubuntu-y-derivados/ 4/9
Modica las conguraciones según tus necesidades.
Address: dene las direcciones IPv4 e IPv6 privadas para
el servidor WireGuard.
ListenPort: especica qué puerto utilizará WireGuard
para las conexiones entrantes.
PostUp y PostDown: dene los pasos que se ejecutarán
después de encender o apagar la interfaz con respecto a
las iptables.
SaveCong: ordena al archivo de conguración que se
actualice automáticamente cada vez que se agrega un
nuevo par.
Congurar el cortafuegos
Abrimos el puerto ssh y el de WireGuard.
Vericamos que los puertos están abiertos.
Iniciar WireGuard
Iniciar el servicio (según el nombre que hayas denido
dependiendo de tus interfaces).
ACCEPT; ip6tables -t nat -A
POSTROUTING -o eth0 -j MASQUERADE
6. PostDown = iptables -D FORWARD -i wg0
-j ACCEPT; iptables -t nat -D
POSTROUTING -o eth0 -j MASQUERADE;
ip6tables -D FORWARD -i wg0 -j
ACCEPT; ip6tables -t nat -D
POSTROUTING -o eth0 -j MASQUERADE
7. SaveConfig = true
1. sudo ufw allow 22/tcp
2.
3. sudo ufw allow 51820/udp
4.
5. sudo ufw enable
1. sudo ufw status verbose

Usamos cookies para garantizar que tengas la mejor experiencia en nuestro sitio. Si continúas en
"sololinux.es" asumiremos que estás conforme.
OK Leer más
3/10/2020 Instalar y configurar WireGuard VPN en Ubuntu y derivados
https://www.sololinux.es/instalar-y-configurar-wireguard-vpn-en-ubuntu-y-derivados/ 5/9
Habilitamos el servicio para que inicie con el sistema.
La VPN ya debería estar funcionado, puedes vericarlo con
los siguientes comandos.
Instalar WireGuard cliente
La conguración de un cliente es similar a la del servidor. Si
usas Ubuntu como sistema operativo cliente, la única
diferencia entre el cliente y el servidor es el contenido del
archivo de conguración.
Instalamos la aplicación como mencionamos anteriormente,
después generamos las claves.
Creamos el archivo de conguración.
Copia y pega lo siguiente.
Guarda el archivo y cierra el editor.
1. wg-quick up wg0
1. sudo systemctl enable wg-quick@wg0
1. sudo wg show
1. ifconfig wg0
1. umask 077
2.
3. wg genkey | tee privatekey | wg
pubkey > publickey
1. nano /etc/wireguard/wg0.conf
1. [Interface]
2. PrivateKey = <archivo que contiene
la clave>
3. Address = 10.0.0.2/24,
fd86:ea04:1115::5/64

Usamos cookies para garantizar que tengas la mejor experiencia en nuestro sitio. Si continúas en
"sololinux.es" asumiremos que estás conforme.
OK Leer más
3/10/2020 Instalar y configurar WireGuard VPN en Ubuntu y derivados
https://www.sololinux.es/instalar-y-configurar-wireguard-vpn-en-ubuntu-y-derivados/ 6/9
Conectar el cliente con el servidor
La forma más sencilla de conectarnos al VPN es editar el
archivo…
Ingresa tus datos reales.
Guarda y cierra el editor.
Podemos vericar la conexión con el siguiente comando.
La conguración de la red se agregara automáticamente al
archivo *.conf. Si no te as y quieres forzar el guardado
puedes ejecutar el siguiente comando (con el nombre
correspondiente).
Ahora y como punto nal del articulo puedes vericar la
conexión con…
1. nano /etc/wireguard/wg0.conf
1. [Peer]
2. PublicKey = <Server Public key>
3. Endpoint = <Server Public IP>:51820
4. AllowedIPs = 10.0.0.2/24,
fd86:ea04:1115::5/64
1. sudo wg
1. wg-quick save wg0
1. ping 10.0.0.1
2.
3. sudo wg
Canales de Telegram: Canal SoloLinux –
Canal SoloWordpress

Usamos cookies para garantizar que tengas la mejor experiencia en nuestro sitio. Si continúas en
"sololinux.es" asumiremos que estás conforme.
OK Leer más
3/10/2020 Instalar y configurar WireGuard VPN en Ubuntu y derivados
https://www.sololinux.es/instalar-y-configurar-wireguard-vpn-en-ubuntu-y-derivados/ 8/9
AGREGAR COMENTARIO
b i link b-quote del ins img ul ol li
code more cerrar las etiquetas
Responder
Muchas gracias por el manual, pero tengo un par de
cuestiones…
Este punto donde tendría que editarse, ¿en el server o en
cliente?
[Peer]
PublicKey =
Endpoint = :51820
AllowedIPs = 10.0.0.2/24, fd86:ea04:1115::5/64
Otra pregunta que tengo es:
Yo tengo montado en server ubuntu donde tengo instalado
Wireguard pero tengo congurado un debian server que hace
función de enrutador con dos interfaces de red: 10.33.6.0 y la
192.168.1.0
La ip privada del debian 10.33.6.1 y la interfaz de red que da
salida a internet con la 192.168.1.68
El ubuntu server (wireguard) forma parte de la red privada del
debian y tiene la ip 10.33.6.6
El cliente ubuntu no forma parte de la red privada si no que
tiene otra red 192.168.1.139
Mi cuention es ¿que ip publica debería poner EndPoint?
Si me pudieras ayudar en este punto te lo agradecería
muchísimo…
Gracias!

Usamos cookies para garantizar que tengas la mejor experiencia en nuestro sitio. Si continúas en
"sololinux.es" asumiremos que estás conforme.
OK Leer más
3/10/2020 Instalar y configurar WireGuard VPN en Ubuntu y derivados
https://www.sololinux.es/instalar-y-configurar-wireguard-vpn-en-ubuntu-y-derivados/ 7/9
Espero que este articulo te sea de utilidad, puedes
ayudarnos a mantener el servidor con una donación
(paypal), o también colaborar con el simple gesto de
compartir nuestros artículos en tu sitio web, blo -->