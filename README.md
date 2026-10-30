# Seguridad en el aire: cómo sobrevivir a una Wi-Fi pública

## Introducción

Las redes Wi-Fi públicas, como las de cafeterías, aeropuertos y centros comerciales, permiten conectarse a Internet fácilmente, pero también pueden representar riesgos para la privacidad y la seguridad de la información.

Cuando varios dispositivos comparten una misma red, un atacante podría intentar interceptar comunicaciones, capturar paquetes o realizar ataques de intermediario (Man-in-the-Middle).

En esta práctica analicé el tráfico generado al acceder a un sitio HTTP utilizando las herramientas de desarrollo de Chrome. El objetivo fue identificar qué información puede observarse durante una solicitud y comprender cómo una VPN ayuda a proteger la comunicación en una red pública.

## 1. Sitio analizado

Para realizar la práctica utilicé el sitio:

**http://neverssl.com**

Este sitio permite comprobar cómo funciona una navegación web mediante HTTP, sin el cifrado TLS que utiliza HTTPS.

Al ingresar a la página, el navegador mostró el aviso de conexión no segura. Durante la carga también observé una solicitud hacia un subdominio de NeverSSL:

`astoundinglushfreshpathway.neverssl.com`

La solicitud final analizada fue:

`http://astoundinglushfreshpathway.neverssl.com/online/`

## 2. Evidencia observada

Utilicé las herramientas de desarrollador de Google Chrome (F12), en la pestaña Network, para observar las solicitudes realizadas durante la carga de la página.

Los datos registrados fueron los siguientes:

| Campo | Resultado observado |
|---|---|
| URL solicitada | `http://astoundinglushfreshpathway.neverssl.com/online/` |
| Método HTTP | GET |
| Host | `astoundinglushfreshpathway.neverssl.com` |
| Estado de respuesta | 200 OK |
| Protocolo | HTTP/1.1 |
| Dirección remota | `34.223.124.45:80` |
| Referer | `http://neverssl.com/` |

También pude identificar distintos encabezados HTTP:

- **Host:** indica el servidor o dominio al que se dirige la solicitud.
- **User-Agent:** informa características del navegador y de la plataforma declarada.
- **Accept:** indica los formatos de contenido que acepta el navegador.
- **Accept-Encoding:** informa los métodos de compresión admitidos.
- **Accept-Language:** indica los idiomas preferidos.
- **DNT:** expresa una preferencia de no seguimiento.

Estos datos permiten observar información técnica de la comunicación sin necesidad de acceder al contenido interno del navegador.

### Capturas de evidencia

**Solicitud HTTP y encabezados**

![Solicitud HTTP y Headers](evidencias/01-solicitud-http-headers)

**Protocolo HTTP/1.1**

![Protocolo HTTP 1.1](evidencias/02-protocolo-http11)

## 3. Riesgos encontrados en una red Wi-Fi pública

El uso de HTTP en una red pública puede comprometer la confidencialidad y la integridad de la información.

Entre los principales riesgos se encuentran:

**Sniffing o captura de paquetes**

Un atacante conectado a una red podría utilizar herramientas de análisis, como Wireshark, para observar el tráfico que circula por ella. Cuando la comunicación utiliza HTTP sin cifrar, determinados datos de las solicitudes y respuestas pueden quedar expuestos.

**Ataques Man-in-the-Middle (MITM)**

Un atacante puede intentar posicionarse entre el usuario y el servicio al que desea acceder, interceptando o modificando la comunicación. Este tipo de ataque puede facilitar el robo de información cuando no existen mecanismos adecuados de cifrado y autenticación.

**Redes gemelas malignas (Evil Twin)**

Un atacante puede crear un punto de acceso Wi-Fi con un nombre similar al de una red legítima. Si una persona se conecta por error, podría estar utilizando una red controlada por un tercero.

En la práctica observé que HTTP permite visualizar información como la URL, el método GET, el Host y diferentes encabezados. Aunque no ingresé contraseñas ni datos bancarios durante la prueba, estos resultados demuestran por qué no es recomendable transmitir información sensible mediante conexiones sin cifrado.

## 4. ¿Cómo ayuda una VPN?

Una VPN (Virtual Private Network) agrega una capa de protección a la conexión mediante el cifrado y el encapsulamiento del tráfico dentro de un túnel seguro entre el dispositivo y el servidor VPN.

Cuando se utiliza una VPN en una red pública:

- **Cifrado:** los datos que viajan entre el dispositivo y el servidor VPN están protegidos mediante cifrado.
- **Encapsulamiento:** el tráfico se introduce dentro de otro paquete que viaja a través del túnel VPN.
- **Protección del tráfico:** una persona que observe la red Wi-Fi podrá identificar que existe una conexión con un servidor VPN, pero no podrá interpretar directamente el contenido protegido dentro del túnel.
- **Privacidad de la IP:** los sitios web de destino normalmente observan la dirección IP pública del servidor VPN, en lugar de la dirección IP pública habitual del usuario.

### Prueba complementaria con Proton VPN

Como parte de la práctica, realicé una comprobación de mi dirección IP pública antes y después de activar Proton VPN.

Sin VPN observé una dirección IP correspondiente a mi conexión habitual. Después de activar Proton VPN, la dirección IP cambió y la aplicación indicó una conexión a un servidor ubicado en Estados Unidos.

Esta comparación permitió comprobar que los sitios web pueden observar una dirección IP diferente cuando la conexión sale a través de un servidor VPN.

Es importante aclarar que una VPN no garantiza anonimato total. El proveedor VPN puede conocer determinados datos de la conexión y los sitios web todavía pueden identificar al usuario cuando inicia sesión en sus cuentas.

Además, una VPN no reemplaza HTTPS. Si se utiliza HTTP, la comunicación desde el servidor VPN hasta el sitio de destino continúa sin cifrado HTTPS. Por eso, ambas herramientas cumplen funciones complementarias.

## 5. Mis 3 Reglas de Oro para navegar en redes Wi-Fi públicas

**1. Protegé tu conexión.**

Utilizá una VPN confiable cuando te conectes a una red pública y comprobá que el túnel esté activo. Evitá realizar operaciones sensibles si no podés verificar la seguridad de la conexión.

**2. Verificá siempre el sitio web.**

Comprobá que la dirección sea la correcta y que utilice HTTPS. No ingreses contraseñas ni datos bancarios en páginas que indiquen que la conexión no es segura.

**3. Evitá conexiones innecesarias y mantené tus dispositivos protegidos.**

Desactivá la conexión automática a redes Wi-Fi desconocidas, evitá compartir archivos en redes públicas y mantené el sistema operativo, el navegador y las aplicaciones actualizados.

## Conclusión

Esta práctica me permitió observar directamente cómo funciona una solicitud HTTP y reconocer qué información técnica puede quedar visible durante una comunicación sin cifrado.

También pude comprobar la diferencia entre la dirección IP de mi conexión habitual y la que se observa al utilizar una VPN.

Comprendí que la seguridad al navegar por una red pública no depende de una única herramienta. El uso de una VPN confiable, la navegación mediante HTTPS, las actualizaciones y los buenos hábitos digitales permiten reducir los riesgos de intercepción y exposición de información.

La principal conclusión es que conectarse a una Wi-Fi pública no significa que toda comunicación sea segura. Es necesario conocer los riesgos y aplicar distintas medidas de protección antes de compartir información personal o sensible.
