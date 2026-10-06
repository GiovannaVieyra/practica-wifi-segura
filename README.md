Práctica Wi-Fi Segura — Auditoría de Red Pública
Introducción

Como parte del análisis de seguridad en redes inalámbricas, este informe documenta los riesgos reales de conectarse a una red Wi-Fi pública. Cuando compartimos el "aire" con desconocidos en una cafetería o aeropuerto, cualquier persona con las herramientas adecuadas puede interceptar el tráfico de red que no viaja cifrado. A continuación se analiza el comportamiento de un sitio web que utiliza HTTP (sin cifrado) y se explican los riesgos asociados, junto con la forma en que una VPN mitiga esos riesgos.

Sitio analizado

URL: http://neverssl.com

Este sitio fue elegido deliberadamente porque no utiliza HTTPS, lo que permite observar en texto plano cómo viaja la información entre el navegador y el servidor.

Evidencia observada

Al abrir las herramientas de desarrollador del navegador (F12), seleccionar la pestaña Network y recargar la página, se observó la siguiente información en la primera solicitud:

Dato	Valor
URL solicitada	http://neverssl.com/
Método HTTP	GET
Host	neverssl.com
Protocolo	HTTP (sin cifrado TLS)
Código de respuesta	200 OK
Headers enviados	Host, User-Agent, Accept, Accept-Language, Connection, entre otros

El sitio utiliza HTTP, no HTTPS. Esto significa que no existe ninguna capa de cifrado (TLS/SSL) entre el navegador y el servidor: toda la solicitud —incluyendo el método, la URL, el host y los headers— viaja en texto plano por la red.

Entre la información visible en la solicitud se pueden mencionar al menos estos elementos:

Host: el dominio exacto que se está visitando (neverssl.com).
URL completa: la ruta solicitada dentro del sitio.
Método GET: la acción realizada contra el servidor.
Headers: incluyendo el User-Agent, que revela el navegador, sistema operativo y versión del dispositivo usado.
Riesgos encontrados

Navegar por HTTP desde una red Wi-Fi pública expone múltiples riesgos:

Intercepción de datos (sniffing): cualquier persona conectada a la misma red puede usar herramientas como Wireshark para capturar los paquetes y leer el contenido completo de la solicitud (host, URL, headers, y si hubiera, credenciales o datos de formularios).
Ataques de intermediario (Man-in-the-Middle): un atacante puede posicionarse entre el dispositivo y el router real, interceptando y potencialmente modificando el tráfico antes de que llegue a su destino.
Redes gemelas malignas (Evil Twins): un atacante puede crear una red con un nombre similar al de un comercio (ej. "Starbucks_Gratis") para que los usuarios se conecten sin saber que todo su tráfico pasa por un punto de acceso hostil.
Falsa sensación de seguridad: el hecho de que una red pida contraseña no la hace segura, ya que si esa contraseña se comparte con todos los clientes, cualquiera de ellos puede intentar leer el tráfico de los demás.

En resumen, un atacante en la misma red podría ver exactamente qué sitios visitás, qué buscás, y en sitios sin HTTPS, incluso contraseñas o datos de sesión enviados en texto plano.

Cómo ayuda una VPN

Una VPN (Virtual Private Network) protege el tráfico creando un túnel cifrado entre el dispositivo y un servidor remoto de confianza. Todo el tráfico que sale del dispositivo —incluido el tráfico HTTP que por sí mismo no tiene cifrado— se encapsula dentro de ese túnel y viaja protegido mediante cifrado de extremo a extremo hasta el servidor VPN.

Esto significa que:

Si un atacante intercepta los paquetes dentro de la red pública, solo va a ver datos cifrados e ilegibles, sin poder determinar qué sitio se está visitando ni qué información se está enviando.
El encapsulamiento del tráfico original dentro del túnel VPN oculta tanto el contenido como, en gran medida, el destino real de la conexión frente a cualquiera que esté "escuchando" en la misma red local.
Se preserva la privacidad del usuario, ya que el proveedor de la red pública (y cualquier atacante en ella) deja de tener visibilidad sobre la actividad de navegación.
La protección del tráfico aplica incluso a sitios que todavía usan HTTP, porque la VPN no depende del cifrado propio del sitio web, sino del túnel que ella misma crea.
3 Reglas de Oro para redes Wi-Fi públicas
Usá siempre una VPN al conectarte a una red pública. Así, aunque el sitio que visites use HTTP, tu tráfico viaja cifrado dentro del túnel y es ilegible para terceros en la misma red.
Verificá que los sitios usen HTTPS (el candado en la barra de direcciones) antes de ingresar cualquier dato sensible, como contraseñas o información de pago.
Desconfiá de redes abiertas o con contraseñas compartidas públicamente. Que una red pida contraseña no la hace segura: evitá realizar operaciones sensibles (home banking, compras, login a cuentas importantes) sin protección adicional (VPN) en este tipo de redes.
