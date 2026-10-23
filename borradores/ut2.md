# UT2. Redes

[:material-arrow-left: Volver al índice de todas las unidades](index.md)

!!! tip "Duración"
    10 horas

!!! abstract "Resultado de aprendizaje que se trabaja"
    **RA5.** Interconecta sistemas en red configurando dispositivos y protocolos.

## 2.1. Fundamentos de redes: tipos, topologías y componentes

**¿Qué es una red informática?**

Una **red informática** es un conjunto de dispositivos conectados que pueden intercambiar datos y compartir recursos mediante protocolos de comunicación.

Las redes permiten compartir archivos, utilizar servicios comunes y acceder a aplicaciones que se ejecutan en otros equipos. Por ejemplo, cuando visitas una página web, tu ordenador se comunica a través de una red con un servidor que proporciona los recursos necesarios para mostrarla.

En el desarrollo de aplicaciones web, las redes son fundamentales para comprender cómo se comunican el navegador del usuario, el servidor web y otros servicios, como una API o una base de datos.

**Tipos de redes según su alcance**

Las redes se pueden clasificar según el área que cubren y la tecnología utilizada.

| Tipo | Nombre | Características y ejemplos |
| --- | --- | --- |
| PAN | Personal Area Network | Red de alcance personal, como la conexión Bluetooth entre un móvil y unos auriculares. |
| LAN | Local Area Network | Red de área local que conecta dispositivos en una vivienda, aula, oficina o edificio. |
| WLAN | Wireless Local Area Network | Red local inalámbrica, normalmente basada en Wi-Fi. Es un tipo de LAN. |
| MAN | Metropolitan Area Network | Red que conecta equipos o redes en un área metropolitana. |
| WAN | Wide Area Network | Red de área extensa que conecta redes situadas en lugares geográficos distantes. Internet es el ejemplo más conocido. |

**Ejemplo:** los ordenadores de un aula conectados a un switch forman parte de una LAN. Si se conectan mediante Wi-Fi a través de un punto de acceso, utilizan una WLAN.

**Topologías de red**

La topología de red describe cómo se organizan y conectan los dispositivos. Podemos distinguir entre:

- **Topología física**: describe las conexiones reales entre los dispositivos, el cableado y los equipos de interconexión.
- **Topología lógica**: describe cómo se comunican los dispositivos y cómo circulan los datos, independientemente de la disposición física del cableado.

Las principales topologías son:

| Topología | Descripción | Características |
| --- | --- | --- |
| Bus | Los dispositivos comparten un medio de transmisión común. | Es sencilla, pero un fallo en el cable principal puede afectar a toda la red. Se asocia principalmente a redes antiguas. |
| Estrella | Todos los dispositivos se conectan a un nodo central, habitualmente un switch. | Es habitual en las redes Ethernet actuales. Un fallo en un cable suele afectar solo al dispositivo conectado, pero un fallo del switch puede interrumpir varias conexiones. |
| Anillo | Los dispositivos se conectan formando un circuito cerrado. | Los datos siguen un recorrido determinado. Se encuentra en algunas tecnologías y arquitecturas específicas. |
| Árbol | Combina varias estrellas en una estructura jerárquica. | Facilita organizar redes grandes en diferentes niveles. |
| Malla | Los nodos disponen de varias conexiones entre sí. | Puede proporcionar redundancia y tolerancia a fallos, aunque aumenta la complejidad y el coste. |

En las redes locales Ethernet actuales es frecuente encontrar una topología física en estrella o una estructura jerárquica de estrellas. Esto no significa que todas las redes utilicen la misma topología lógica.

![Topologías de red: bus, estrella, anillo, árbol y malla](img/ut2_topologias_red.png)

**Componentes principales de una red informática:**

- **Tarjeta de red (NIC)**: interfaz que conecta un equipo a la red, con una dirección MAC única.
- **Switch (conmutador)**: interconecta equipos dentro de una misma LAN, dirigiendo el tráfico según la dirección MAC de destino.
- **Router (encaminador)**: interconecta redes distintas y decide la ruta que deben seguir los paquetes entre ellas.
- **Punto de acceso (AP)**: da conectividad inalámbrica a los dispositivos de una WLAN.
- **Hub (concentrador)**: dispositivo antiguo que repite el tráfico a todos los puertos por igual (en desuso, sustituido por el switch).
- **Medio de transmisión**: cable de cobre, fibra óptica u ondas de radio, según el caso.

!!! note "Idea clave"
    Este apartado da una visión general de los componentes de red; el punto 2.6 profundiza en el funcionamiento de switch y router (tablas de conmutación y de encaminamiento, NAT).

## 2.2. Modelos de referencia: OSI y TCP/IP

Un **modelo de referencia** es una representación teórica, organizada en **capas**, de cómo se comunican los dispositivos en una red. Cada capa cumple una función concreta y se apoya en los servicios que le ofrece la capa inferior, sin necesidad de conocer los detalles de su funcionamiento interno (**encapsulado**). Esta organización en capas permite que fabricantes distintos desarrollen hardware y software compatibles entre sí, y facilita además localizar en qué punto se produce un fallo cuando algo no funciona.

Los dos modelos de referencia más utilizados son el **modelo OSI** (de carácter teórico y académico) y el **modelo TCP/IP** (el que realmente implementan los sistemas y se usa en Internet).

**Modelo OSI (Open Systems Interconnection)**

Definido por la ISO, organiza la comunicación en **7 capas**, de la más cercana al usuario a la más cercana al medio físico:

| Capa | Nombre | Función | Ejemplos |
| --- | --- | --- | --- |
| 7 | Aplicación | Proporciona los servicios de red directamente a los programas del usuario | HTTP, DNS, SMTP, FTP |
| 6 | Presentación | Traduce, cifra/descifra y comprime los datos a un formato común entre sistemas | TLS/SSL, JPEG, ASCII |
| 5 | Sesión | Establece, mantiene y finaliza la comunicación entre aplicaciones | NetBIOS, RPC |
| 4 | Transporte | Comunicación extremo a extremo entre procesos, fiable o no | TCP, UDP |
| 3 | Red | Direccionamiento lógico y encaminamiento de paquetes entre redes distintas | IP, ICMP |
| 2 | Enlace de datos | Direccionamiento físico (MAC) y acceso al medio dentro de una misma red | Ethernet, Wi-Fi (802.11) |
| 1 | Física | Transmisión de los bits como señales eléctricas, ópticas o de radio | Cables, conectores, radiofrecuencia |

**Modelo TCP/IP**

Es el modelo que realmente implementan los sistemas operativos y los dispositivos de red, y en el que se basa Internet. Es más simple que el OSI: agrupa sus funciones en solo **4 capas**:

| Capa | Función | Ejemplos de protocolo |
| --- | --- | --- |
| Aplicación | Servicios usados directamente por los programas | HTTP, DNS, SMTP, FTP |
| Transporte | Comunicación extremo a extremo | TCP (fiable), UDP (sin conexión) |
| Internet | Direccionamiento y encaminamiento entre redes | IP, ICMP |
| Acceso a la red | Transmisión física de los datos | Ethernet, Wi-Fi |

**Comparación entre el modelo OSI y el modelo TCP/IP**

| Modelo OSI (7 capas) | Modelo TCP/IP (4 capas) |
| --- | --- |
| 7. Aplicación | Aplicación |
| 6. Presentación | Aplicación |
| 5. Sesión | Aplicación |
| 4. Transporte | Transporte |
| 3. Red | Internet |
| 2. Enlace de datos | Acceso a la red |
| 1. Física | Acceso a la red |

!!! note "Idea clave"
    El modelo TCP/IP agrupa en una única capa de **Aplicación** lo que el modelo OSI reparte entre Aplicación, Presentación y Sesión; y agrupa en **Acceso a la red** las capas de Enlace de datos y Física. El modelo OSI, más detallado, se usa sobre todo como referencia didáctica y para diagnosticar problemas de red capa por capa ("¿falla el cable? ¿falla el direccionamiento IP? ¿falla la aplicación?"); el modelo TCP/IP es el que realmente funciona en la práctica.

## 2.3. Medios de transmisión, cableado y mapa físico/lógico de una red local

**Medios de transmisión**

El **medio de transmisión** es el canal físico por el que viaja la señal (eléctrica, óptica o electromagnética) entre los dispositivos de una red. Se clasifican en dos grandes grupos:

- **Medios guiados (o cableados)**: la señal viaja confinada dentro de un soporte físico. Incluyen el par trenzado, el cable coaxial y la fibra óptica.
- **Medios no guiados (o inalámbricos)**: la señal se propaga libremente por el espacio, sin un soporte físico que la contenga. Incluyen las ondas de radio (Wi-Fi, Bluetooth), las microondas y los infrarrojos.

| Característica | Medios guiados | Medios no guiados |
| --- | --- | --- |
| Movilidad | Nula; el dispositivo debe permanecer conectado al cable | Alta; permite desplazarse dentro del área de cobertura |
| Velocidad y fiabilidad | Generalmente mayores y más estables | Más variables; dependen de interferencias y distancia |
| Interferencias | Poco afectados (según el blindaje) | Más sensibles (otros dispositivos, obstáculos, otras redes cercanas) |
| Seguridad | Mayor; requiere acceso físico al cable | Menor; la señal puede interceptarse en el aire sin acceso físico |
| Coste de despliegue | Requiere tender cableado e infraestructura física | Menor coste de instalación; no requiere tender cable |

**Medios guiados: tipos de cableado más habituales en redes LAN:**

| Cableado | Descripción | Uso |
| --- | --- | --- |
| UTP (par trenzado no apantallado) | 4 pares trenzados, sin blindaje | Redes de oficina y hogar |
| STP/FTP (par trenzado apantallado) | Igual que UTP pero con blindaje | Entornos con interferencias electromagnéticas |
| Coaxial | Conductor central + malla | Prácticamente en desuso en LAN |
| Fibra óptica | Transmisión por pulsos de luz | Backbones, largas distancias, alta velocidad |

**Categorías de cable UTP/FTP** (a mayor categoría, mayor ancho de banda soportado): Cat 5e (hasta 1 Gbit/s), Cat 6 (hasta 10 Gbit/s a distancias cortas), Cat 6A y Cat 7/8 (10 Gbit/s y más, mayor apantallamiento).

**Conectores más comunes:**

- **RJ-45**: conector del cableado de par trenzado (Ethernet).
- **SC / LC**: conectores típicos de fibra óptica.

**Tipos de cable UTP según el cableado interno:**

- **Cable directo (straight-through)**: los pines siguen el mismo orden en ambos extremos. Se usa para conectar dispositivos de distinto nivel (equipo-switch, switch-router).
- **Cable cruzado (crossover)**: los pares de transmisión y recepción están cruzados. Se usa tradicionalmente para conectar dispositivos del mismo nivel (switch-switch, PC-PC), aunque la mayoría de equipos actuales incorporan detección automática (*Auto-MDI-X*) y ya no lo requieren.

**Medios no guiados:**

- **Ondas de radio**: utilizadas por Wi-Fi y Bluetooth; atraviesan obstáculos (paredes, tabiques) con cierta atenuación y no requieren visión directa entre emisor y receptor.
- **Microondas**: requieren visión directa entre antenas (enlaces punto a punto); se usan en enlaces de larga distancia entre edificios o en repetidores de telecomunicaciones.
- **Infrarrojos**: requieren visión directa y tienen un alcance muy corto; prácticamente en desuso en redes de datos, aunque siguen presentes en mandos a distancia.

!!! note "Idea clave"
    La configuración práctica de una red Wi-Fi (SSID, cifrado WPA2/WPA3, puntos de acceso) se desarrolla en el apartado 2.5.

**Mapa físico vs. mapa lógico de una red:**

- **Mapa físico**: representa la ubicación real de los dispositivos, el cableado y su disposición en el espacio (racks, plantas, salas).
- **Mapa lógico**: representa cómo fluye la información entre los dispositivos con independencia de su ubicación física: direccionamiento IP, segmentación en subredes/VLAN, rutas.

**Mapa físico** (disposición real de los equipos y el cableado):

```mermaid
graph LR
    Internet((Internet)) --- R[Router]
    R --- SW[Switch de aula]
    SW --- PC1[PC1]
    SW --- PC2[PC2]
    SW --- PC3[PC3]
```

**Mapa lógico** (segmentación en subredes/VLAN, con independencia de la ubicación física):

```mermaid
graph TD
    R[Router]
    subgraph VLAN10["VLAN 10 · 192.168.10.0/24"]
        PC1["PC1 · 192.168.10.11"]
        PC2["PC2 · 192.168.10.12"]
    end
    subgraph VLAN20["VLAN 20 · 192.168.20.0/24"]
        PC3["PC3 · 192.168.20.11"]
    end
    R --- PC1
    R --- PC2
    R --- PC3
```

!!! note "Idea clave"
    Dos equipos pueden estar físicamente muy cerca (mismo armario de comunicaciones) y pertenecer a segmentos lógicos completamente distintos (VLAN distintas), y viceversa.

## 2.4. Protocolo TCP/IP: direccionamiento IPv4/IPv6, DNS

Este apartado se centra en la capa **Internet** del modelo TCP/IP (ver 2.2), responsable del direccionamiento y el encaminamiento de los paquetes entre redes.

**Direccionamiento IPv4:** dirección de 32 bits, representada en 4 grupos decimales (0-255) separados por puntos (p. ej. `192.168.1.10`).

- **Máscara de subred**: determina qué parte de la dirección identifica la red y cuál el equipo (host). Se puede expresar en notación decimal (`255.255.255.0`) o **CIDR** (`/24`).
- **Direcciones privadas** (uso interno, no enrutables en Internet): `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.
- **Puerta de enlace (gateway)**: dirección del dispositivo (router) por el que un equipo envía el tráfico destinado a otras redes.

**IPv6:** dirección de 128 bits, representada en 8 grupos hexadecimales (p. ej. `2001:0db8:0000:0000:0000:ff00:0042:8329`, simplificable a `2001:db8::ff00:42:8329`), diseñada para resolver el agotamiento de direcciones de IPv4 y simplificar el direccionamiento.

**DNS (Domain Name System):** servicio distribuido y jerárquico que traduce nombres de dominio legibles (`www.ejemplo.com`) a direcciones IP. En el proceso de **resolución**, el equipo consulta a un servidor DNS (a menudo el del proveedor de Internet o uno público como `8.8.8.8`), que responde directamente si conoce la respuesta o la reenvía a otros servidores DNS hasta obtenerla.

## 2.5. Configuración de redes cableadas e inalámbricas

**Configuración de la dirección IP de un equipo:**

- **Estática**: se asigna manualmente (IP, máscara, puerta de enlace, DNS). Habitual en servidores e impresoras, donde interesa que la dirección no cambie.
- **Dinámica (DHCP)**: un servidor **DHCP** asigna automáticamente la configuración de red a cada equipo que se conecta, durante un tiempo determinado (concesión o *lease*). Es la opción por defecto en la mayoría de redes domésticas y de oficina.

| SO | Configuración gráfica | Comandos / archivos |
| --- | --- | --- |
| Windows | Configuración > Red e Internet | `netsh`, PowerShell `Get-NetIPConfiguration` |
| Linux | NetworkManager (GUI) | `nmcli`, `/etc/netplan/*.yaml` (Ubuntu), `ip` |

**Redes inalámbricas (Wi-Fi):**

- **SSID**: nombre que identifica la red inalámbrica.
- **Estándares 802.11**: sucesivas generaciones (a/b/g/n/ac/ax...) con mejoras de velocidad y alcance; los nombres comerciales actuales son Wi-Fi 4 (802.11n), Wi-Fi 5 (802.11ac) y Wi-Fi 6 (802.11ax).
- **Seguridad**: protocolos de cifrado del tráfico inalámbrico, de menor a mayor seguridad: WEP (obsoleto e inseguro) → WPA → **WPA2** (estándar durante años) → **WPA3** (actual).
- **Ampliación de cobertura**: un único punto de acceso no siempre cubre todo el espacio necesario. Un **repetidor** (o un AP en modo repetidor, mediante el estándar **WDS**, *Wireless Distribution System*) retransmite la señal para ampliar el alcance, a costa de reducir el ancho de banda disponible en cada salto adicional. Las **redes mesh** modernas (varios puntos que se coordinan entre sí, con el mismo SSID e itinerancia automática entre ellos) resuelven el mismo problema de forma más eficiente y con gestión centralizada.

## 2.6. Dispositivos de interconexión y encaminamiento

- **Switch (conmutador)**: opera a nivel de enlace, interconectando equipos de una misma LAN. Aprende qué dirección **MAC** está conectada a cada puerto (tabla de conmutación) y reenvía el tráfico solo al puerto correspondiente, en lugar de a todos (a diferencia del antiguo hub).
- **Router (encaminador)**: opera a nivel de red, interconectando redes distintas (por ejemplo, la LAN doméstica con Internet). Mantiene una **tabla de encaminamiento (routing table)** que indica por qué interfaz/siguiente salto se debe enviar el tráfico según la red de destino.

**Tipos de encaminamiento:**

- **Estático**: las rutas se configuran manualmente; sencillo pero poco escalable.
- **Dinámico**: los routers intercambian información de rutas automáticamente mediante protocolos de encaminamiento (p. ej. OSPF, RIP), adaptándose a cambios en la red.

**NAT (Network Address Translation):** técnica, habitual en routers domésticos, que traduce las direcciones IP privadas de la LAN a una única IP pública para acceder a Internet, permitiendo que muchos equipos compartan una sola dirección pública.

**VLAN (Virtual LAN):** segmenta lógicamente una misma red física en varias redes independientes, sin necesidad de cablear switches separados. Requiere un **switch gestionable** (o switch L3), en cuya configuración se asignan los puertos a una VLAN u otra.

- **Puerto de acceso (access)**: pertenece a una única VLAN; es el modo habitual para conectar equipos finales (PC, impresora).
- **Puerto troncal (trunk)**: transporta el tráfico de varias VLAN a la vez, etiquetando cada trama con el identificador de VLAN correspondiente (estándar **802.1Q**); se usa para enlazar switches entre sí o conectar un router que hace de puerta de enlace de varias VLAN.

!!! note "Idea clave"
    Dos equipos conectados al mismo switch físico, pero en VLAN distintas, no pueden comunicarse entre sí sin pasar por un router (o una función de encaminamiento entre VLAN) — exactamente igual que si estuvieran en dos redes físicamente separadas.

## 2.7. Acceso a redes de área extensa y seguridad en comunicaciones

**Tecnologías de acceso a redes WAN / Internet:**

| Tecnología | Medio | Características |
| --- | --- | --- |
| ADSL | Par de cobre telefónico | Velocidad asimétrica, en desuso |
| Cable (DOCSIS) | Coaxial | Compartido por zona, buena velocidad de bajada |
| Fibra óptica (FTTH) | Fibra hasta el hogar | Alta velocidad simétrica, tecnología actual de referencia |
| Redes móviles | Radiofrecuencia | 4G/5G, cobertura amplia, uso también fijo (FWA) |

**VPN (Virtual Private Network):** crea un túnel cifrado a través de una red no confiable (como Internet), permitiendo acceder de forma segura a una red privada remota como si el equipo estuviera físicamente conectado a ella. Muy usada para el teletrabajo y para interconectar sedes de una empresa.

**Servidor proxy:** actúa como intermediario entre los equipos de una red local e Internet, reenviando las peticiones en su nombre. Es habitual en redes de empresas o centros educativos, con dos funciones principales:

- **Caché de contenidos**: guarda copia de los recursos más solicitados, reduciendo el consumo de ancho de banda y acelerando el acceso a lo que ya han consultado otros equipos de la red.
- **Filtrado de contenidos**: permite bloquear el acceso a determinados sitios o categorías (redes sociales, contenido no apropiado...) de forma centralizada, sin configurar cada equipo por separado.

**Squid** es la implementación libre de proxy más extendida en entornos Linux.

!!! note "Idea clave"
    Un proxy actúa en nombre del cliente hacia Internet (*forward proxy*); no debe confundirse con un **proxy inverso** (*reverse proxy*), que actúa en nombre de un servidor hacia sus clientes y que se estudia en otros módulos relacionados con el despliegue de aplicaciones web.

**Protocolos seguros de comunicación:**

| Protocolo inseguro | Alternativa segura |
| --- | --- |
| HTTP | **HTTPS** (HTTP sobre TLS/SSL) |
| Telnet | **SSH** |
| FTP | **FTPS** / **SFTP** |

**TLS/SSL:** protocolos que cifran la comunicación entre cliente y servidor, garantizando confidencialidad (nadie puede leer los datos), integridad (no se pueden modificar sin detectarlo) y autenticidad (mediante certificados digitales).

## Actividades

**Actividad 2.1 — Mapa físico y lógico de una red**
{: .actividad-titulo}

A partir del plano de un aula con 15 puestos, un switch y un router con salida a Internet, dibuja el **mapa físico** (ubicación de equipos y cableado) y el **mapa lógico** (direccionamiento IP, segmento de red) de esa red.

**Actividad 2.2 — Direccionamiento IP**
{: .actividad-titulo}

Dada la red `192.168.10.0/24`, calcula la dirección de red, la de broadcast y el rango de direcciones válidas para host. Configura después esa dirección de forma estática en una máquina virtual y verifica la conectividad con `ping` hacia otro equipo de la misma red.

**Actividad 2.3 — DHCP vs. IP estática**
{: .actividad-titulo}

En un entorno con dos máquinas virtuales y un servicio DHCP, observa qué configuración de red recibe automáticamente un cliente. Cambia después esa misma máquina a configuración estática y razona en qué casos (servidor, impresora, PC de usuario) conviene cada modalidad.
