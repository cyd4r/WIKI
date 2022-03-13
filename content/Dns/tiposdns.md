---
title: "Tipos de Registros"
date: 2021-11-01T21:06:50+01:00
lastmod: 2021-11-01T21:06:50+01:00
publishdate: 2021-11-01T21:06:50+01:00
description: ""
weight: 10
---



# Registros DNS
Cuando buscamos una web por internet, ésta se carga gracias a unas direcciones IP que localizan los dispositivos que contienen dicha página. Hoy en día, casi nos cuesta recordar el número de nuestro teléfono móvil, así que recordar esta serie de números separados por puntos no es muy práctico, así que los servidores utilizan nombres de dominios que resultan más sencillos de recordar. Para traducir estos nombres de dominios en direcciones IP, están las DNS.

Los servidores DNS guían la información de manera que indican hacia qué servidor se debe
encaminar la consulta para poder mostrar la información que se ha pedido, o marcan la ruta de un correo electrónico hacia su destino.
# ¿Qué tipo de registros DNS existen y para qué sirven?

El lugar donde se configuran las entradas DNS para cada dominio son los servidores de nombres. Los
diferentes tipos de entradas de registro son:
### Registro A: 
Este registro se utiliza para convertir nombres de host en direcciones IP.
Se llama también registro de host. Vincula un dominio con la dirección IP física de un ordenador
en     el que se alojan los servicios de ese dominio.   

### Registro CNAME:
 Se utiliza para crear nombres de host adicionales (alias),     y para crear
diferentes servicios bajo una misma dirección IP. Por ejemplo querremos personalizar
direcciones de servicio para Gmail,Google Calendar, ...   
### Registro NS (Name Server):

 Determina los servidores que comunicarán la información del DNS
de un dominio. Por ejemplo se puede configurar para que señalen a los servidores de Google.   

### Registro MX: 
Dirigen el correo electrónico de un dominio a los servidores que alojan las cuentas
de usuario del dominio. Definen  en qué IP se entregan los correos electrónicos de las cuentas
de correo de tu dominio. Por ejemplo, apuntar registros MX a los servidores de correo de
Google. Se les asignan prioridades y cuando se reciba correo se irá dirigiendo el correo a los
Registros
diferentes servidores en orden de prioridad, por si fallan. Si un servidor de correo falla, se irá al
siguiente.   
### Registros TXT: SPF, DKIM, DMARC.
#### SPF:
Puedes configurar un registro SPF para evitar que los spammers envíen correos electrónicos no
autorizados desde tu dominio, práctica que también se denomina spoofing. Algunos destinatarios de
correo requieren utilizar SPF. Si no añades un registro SPF en tu dominio, tus mensajes se pueden
marcar como spam o incluso pueden ser devueltos.
Un registro SPF incluye los servidores de correo que pueden enviar mensajes en nombre de tu
dominio. Si se envía un mensaje a través de un servidor de correo no autorizado, se denuncia y se
puede marcar como spam.

#### DKIM:

Puedes ayudar a evitar el spoofing añadiendo una firma digital a las cabeceras de los mensajes
salientes mediante el estándar DKIM. Para ello, tienes que usar una clave de dominio privada a fin de
cifrar las cabeceras de los mensajes salientes de tu dominio y añadir una versión pública de la clave
en los registros DNS del dominio. Los servidores de los destinatarios podrán recuperar la clave
pública para descifrar las cabeceras entrantes y verificar que el mensaje realmente procede de tu
dominio y que no ha sufrido modificaciones durante la transmisión.
#### DMARC:
Ambos protocolos DKIM y SPF son complementarios y responden a diferentes tipos de ataques. Sin
embargo, tienen la desventaja de no dar instrucciones de acción en caso de un ataque. El protocolo
DMARC supera esta deficiencia y proporciona indicaciones en caso de que haya un ataque: es
particularmente posible ser notificado si alguien intenta robar su identidad (por ejemplo, si el
atacante utiliza una IP no autorizada o si el atacante ha modificado el contenido de su correo
electrónico).
### DIFERENCIA ENTRE CNAME Y ALIAS
La diferencia principal entre un registro CNAME y un registro ALIAS no está en el resultado, ambos
apuntan a otro registro DNS, sino en cómo resuelven el registro DNS objetivo cuando se consulta.
Como resultado de esta diferencia, se puede usar con seguridad en el vértice de la zona (por ejemplo,
dominio desnudo, como example.com) y el otro no.
Comencemos con el tipo de registro CNAME. Simplemente señala un nombre DNS, como
www.example.com, en otro nombre DNS, como lb.example.net. Esto introduce una penalización de
rendimiento, ya que se debe realizar al menos una búsqueda de DNS adicional para resolver el
objetivo (lb.example.net). En el caso de que ninguno de los registros haya sido consultado
anteriormente por su sistema de resolución recursivo, es incluso más costoso en términos de tiempo,
ya que la jerarquía DNS completa debe atravesarse para ambos registros:
- Usted como el cliente DNS (o el servidor de resoluciones) consulta su resolución recursiva para
www.example.com.
- Su resolución recursiva consulta el servidor de nombres raíz para www.example.com.
- El servidor de nombre de raíz refiere su resolución recursiva al servidor autorizado Dominio de
primer nivel .com (TLD).
- Su resolver recursivo consulta el servidor autorizado .com TLD para www.example.com.
- El servidor autorizado .com TLD refiere su servidor recursivo a los servidores autorizados para
example.com.
A PARTIR DE AQUÍ LO HARÍA EL SERVIDOR AUTORIZADO CON ALIAS
- Su resolución recursiva consulta los servidores autorizados para www.example.com y recibe
lb.example.net como respuesta.
- Su resolución recursiva guarda en caché la respuesta y se la devuelve.
- Ahora emite una segunda consulta a su resolver recursivo para lb.example.net.
- Su resolución recursiva consulta el servidor de nombres raíz para lb.example.net.
- El servidor de nombre de raíz refiere su resolución recursiva al servidor autorizado Dominio de nivel
superior (TLD) .net.
- Su resolvedor recursivo consulta el servidor autorizado .net TLD para lb.example.net.
- El servidor autorizado .net TLD refiere su servidor recursivo a los servidores autorizados para
example.net.
- Su resolución recursiva consulta los servidores autorizados
para lb.example.net, y recibe una dirección IP como respuesta.
- Su resolución recursiva guarda en caché la respuesta y se la devuelve.
Cada uno de estos pasos consume al menos varios milisegundos, a menudo más, dependiendo de las
condiciones de la red. Esto puede sumar una considerable cantidad de tiempo que pasa esperando la
respuesta final y procesable de una dirección IP.
En el caso de un registro ALIAS, todas las mismas acciones se toman como con el CNAME, excepto
que el servidor autorizado de example.com realiza los pasos seis a trece para usted y devuelve la
respuesta final de una dirección IP. Esto ofrece dos ventajas y un inconveniente importante:
Ventajas
- Velocidad de resolución de respuesta final más rápida. En la mayoría de los casos, los servidores
autorizados de example.com son más potentes y tienen una conexión a Internet más rápida que su
propia computadora y conexión. Por lo tanto, pueden atravesar la jerarquía DNS y recuperar la
respuesta final mucho más rápido que usted.
- La respuesta parece un registro A. Como un registro ALIAS devuelve la respuesta final que consta de
una o más direcciones IP, se puede usar en cualquier lugar donde se pueda usar un registro A,
incluido el ápice de la zona. Esto lo hace más flexible que un CNAME, que no se puede usar en la zona 
ápice.
Desventajas
- La información de orientación geográfica se pierde. Dado que es el servidor autorizado de
example.com el que está emitiendo las consultas para lb.example.net, cualquier funcionalidad de
enrutamiento inteligente en el registro lb.example.net actuará sobre la ubicación del servidor
autorizado, no en su ubicación. La opción EDNS0 edns-client-subnet no se aplica aquí. Esto significa
que puede estar mal encaminado: por ejemplo, si se encuentra en Nueva York, y el servidor
autorizado de example.com está en California, entonces lb.example.com le creerá que se encuentra
en California y le devolverá un respuesta que claramente no es óptima para usted en Nueva York.
Una cosa importante a tener en cuenta es que NS1 colapsa los registros CNAME siempre que todos
estén dentro del sistema NS1, es decir, los servidores de nombres de NS1 son autorizados tanto para
el CNAME como para el registro de destino. El colapso simplemente significa que el servidor de
nombres NS1 devolverá toda la cadena de registros, desde CNAME hasta la respuesta final, en una
sola respuesta. Esto elimina todos los pasos de búsqueda adicionales y le permite usar registros
CNAME, incluso en una configuración anidada, sin ninguna penalización de rendimiento.
Y aún mejor, NS1 admite un tipo de registro único llamado registro vinculado. Esto es básicamente un
enlace simbólico dentro de nuestra plataforma que actúa como un registro ALIAS podría, excepto con
una velocidad de resolución de sub-microsegundo. Para utilizar un registro vinculado, simplemente
cree el registro objetivo como lo haría normalmente (puede ser de cualquier tipo) y luego cree un
segundo registro para señalarlo, y seleccione la opción de registro vinculado. Tenga en cuenta que
los registros vinculados pueden cruzar límites de dominio (zona) e incluso límites de cuenta dentro de
NS1, y ofrecen una forma eficaz de organizar y optimizar su estructura de registro de DNS.
----
Los registros CNAME y ALIAS parecen muy similares, ya que puede usarlos para señalar un nombre
de host de su dominio a otro destino. Sin embargo, hay algunas diferencias entre estos registros que
debe tener en cuenta.
Se resuelven de manera diferente
Una diferencia entre un CNAME y un registro ALIAS es la forma en que llegan a un destino.
Un uso común para un registro CNAME es señalar www.ralphsdomainname.com a la misma ubicación
que ralphsdomainname.com. Para ello, primero configure un registro A para ralphsdomainname.com
para que apunte a una dirección IP y luego cree un CNAME para www.ralphsdomainname.com que
luego apunte a ese registro A.
Al usar esta configuración de DNS, cuando un visitante ingresa a www.ralphsdomainname.com en su
navegador, obtendrá la respuesta de ralphsdomainname.com. El navegador necesita hacer una
búsqueda DNS adicional para obtener la dirección IP de ralphsdomainname.com pidiendo el registro A.
Un registro ALIAS puede verse exactamente igual, sin embargo, en lugar de regresar a
ralphsdomainname.com para que su navegador vaya y busque, los servidores de nombres usan una
resolución que hace el trabajo por usted y en su lugar devuelve una dirección IP como si fuera una
resolución de tipo A.
Los CNAME tienen algunas limitaciones que los registros de ALIAS no
Los registros CNAME no pueden compartir el mismo host que otros registros
Al configurar un registro CNAME para un host como blog.ralphsdomainname.com, no podrá configurar
ningún otro registro para blog.ralphsdomainname.com, ya que esto puede causar un conflicto entre
los registros y resultados incorrectos.
Por ejemplo; si desea configurar un registro TXT para verificar blog.ralphsdomainname.com con los
servicios de Google, no podrá hacerlo si ya tiene un CNAME en su lugar.
Si pudieras configurar tu DNS de esta manera, a veces obtendrías el resultado para el registro CNAME
y otras veces, el registro TXT. Esto causa aún más problemas cuando el CNAME entra en conflicto con
los registros NS o SOA configurados en la raíz del dominio.
Los registros ALIAS solo redireccionan los registros A y AAA, lo que significa que no entrarán en
conflicto con ningún otro registro como el registro TXT en nuestro ejemplo.
Los registros CNAME no se pueden configurar en la raíz de un dominio
No puede configurar un CNAME en la raíz de un nombre de dominio, lo que significa que no puede
usar un CNAME para desviar un dominio raíz como ralphsdomainname.com a otra ubicación o servicio.
Los registros ALIAS no tienen estas limitaciones ya que devuelven una dirección IP muy parecida a un
registro A. Esto los convierte en una buena opción para los servicios que brindan conjuntos de
direcciones IP, como las Páginas Github.