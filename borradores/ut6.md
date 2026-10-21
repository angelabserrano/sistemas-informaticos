# UT6. Sistemas operativos en red

[:material-arrow-left: Volver al índice de todas las unidades](index.md)

!!! tip "Duración"
    25 horas

!!! abstract "Resultado de aprendizaje que se trabaja"
    **RA6.** Opera sistemas en red gestionando sus recursos e identificando restricciones de seguridad.

    *Transversal:* **RA7** (parcial) — uso de sistemas de correo y mensajería y de servicios de transferencia de ficheros.

## 6.1. Recursos compartidos, permisos de red y directivas

Compartir un recurso (carpeta, impresora) en red implica dos niveles de permisos que se combinan:

- **Permisos de recurso compartido (permisos de red)**: se aplican solo al acceder desde la red (por ejemplo, "Lectura" o "Lectura/Escritura" al compartir una carpeta en Windows).
- **Permisos locales del sistema de archivos** (NTFS o los permisos Linux vistos en la UT5): se aplican siempre, se acceda local o remotamente.

!!! note "Idea clave"
    Cuando ambos niveles de permisos existen (típico en Windows), se aplica el **más restrictivo** de los dos. Por ejemplo, si el recurso compartido permite "control total" pero el permiso NTFS de la carpeta es "solo lectura", el resultado final es solo lectura.

**Modelos de administración de una red:**

- **Grupo de trabajo (workgroup)**: cada equipo gestiona sus propios usuarios y permisos de forma independiente; adecuado para redes muy pequeñas.
- **Dominio**: la gestión de usuarios, equipos y directivas se centraliza en un servidor (ver 6.4); adecuado a partir de cierto número de equipos.

**Unidades de red:** una carpeta compartida remota puede **mapearse** como si fuera una unidad más del propio equipo, facilitando su uso habitual.

## 6.2. Servidores de archivos, impresión y aplicaciones

- **Servidor de archivos**: centraliza el almacenamiento y el acceso a carpetas y documentos compartidos por varios usuarios, con sus correspondientes permisos. Puede implementarse con Windows Server, con **Samba** (que permite a un servidor Linux compartir recursos usando el protocolo SMB de Windows), o mediante dispositivos **NAS** dedicados.
- **Servidor de impresión**: centraliza la gestión de una o varias impresoras, permitiendo que los equipos de la red impriman sin necesidad de tener el dispositivo conectado directamente ni el driver instalado en cada equipo. Gestiona colas de impresión y prioridades.
- **Servidor de aplicaciones**: ejecuta la lógica de una aplicación y la sirve a los clientes de la red (por ejemplo, un ERP o una aplicación web), de forma que los equipos cliente no necesitan tener instalada ni ejecutar la aplicación localmente.

## 6.3. Conexión remota y cortafuegos

**Técnicas de conexión remota**, para administrar un equipo sin acceso físico a él:

| Protocolo/herramienta | Tipo de acceso | SO típico |
| --- | --- | --- |
| **RDP** (Remote Desktop Protocol) | Escritorio remoto gráfico | Windows |
| **SSH** (Secure Shell) | Línea de comandos cifrada | Linux (y disponible en Windows) |
| **VNC** | Escritorio remoto gráfico | Multiplataforma |
| TeamViewer / AnyDesk | Escritorio remoto gráfico, suele atravesar NAT sin configuración | Multiplataforma |

**Cortafuegos (firewall):** sistema (hardware o software) que filtra el tráfico de red entrante y saliente según un conjunto de reglas, permitiendo o bloqueando conexiones según criterios como el puerto, el protocolo o la dirección IP de origen/destino.

| SO | Herramienta |
| --- | --- |
| Windows | Firewall de Windows Defender |
| Linux | `iptables` / `nftables`, con interfaces simplificadas como `ufw` (Ubuntu) o `firewalld` (Fedora/RHEL) |

!!! warning "Puertos abiertos = superficie de ataque"
    Cada puerto abierto en un cortafuegos es un posible punto de entrada. La buena práctica es aplicar el principio de mínimo privilegio también en red: abrir únicamente los puertos estrictamente necesarios para los servicios que se ofrecen.

## 6.4. Dominios: implantación y explotación

Un **dominio** (en el ecosistema Windows, típicamente **Active Directory**) centraliza en uno o varios servidores llamados **controladores de dominio** la gestión de usuarios, equipos, grupos y directivas de seguridad de toda una organización.

**Ventajas frente al modelo de grupo de trabajo:**

- Inicio de sesión único (el mismo usuario y contraseña sirve en cualquier equipo del dominio).
- Aplicación centralizada de directivas de grupo (GPO): configuración de seguridad, restricciones, instalación de software.
- Gestión centralizada de permisos sobre recursos compartidos.
- Escalable a cientos o miles de equipos.

**Componentes principales:**

- **Controlador de dominio (DC)**: servidor que almacena la base de datos del dominio y autentica a los usuarios.
- **Servicio de directorio**: base de datos jerárquica de objetos (usuarios, equipos, grupos, unidades organizativas), basada en el protocolo estándar **LDAP**.
- **DNS integrado**: un dominio Active Directory depende de un servicio DNS correctamente configurado para que los equipos puedan localizar al controlador de dominio.

**Unirse a un dominio:** proceso por el que un equipo pasa de gestionarse de forma local (grupo de trabajo) a que sus usuarios y configuración puedan ser gestionados desde el controlador de dominio.

???+ tip "Alternativa libre: Samba AD DC / Zentyal"
    En el mundo Linux, **Samba** (ya mencionado en 6.2 para compartir archivos) puede configurarse también como **controlador de dominio compatible con Active Directory** (modo *Samba AD DC*), autenticando por igual a clientes Windows y Linux. **Zentyal** es una distribución basada en esta tecnología que ofrece esta función mediante un panel de administración gráfico, como alternativa libre a Windows Server para pymes.

## 6.5. Correo y mensajería electrónica

El correo electrónico se apoya en varios protocolos con funciones distintas:

| Protocolo | Función | Puerto habitual |
| --- | --- | --- |
| **SMTP** | Envío de correo (entre servidores, y del cliente al servidor) | 25 / 587 |
| **POP3** | Descarga del correo al cliente (habitualmente eliminándolo del servidor) | 110 |
| **IMAP** | Sincronización del correo con el servidor (el correo permanece en el servidor, accesible desde varios dispositivos) | 143 |

Todos ellos cuentan con versión cifrada mediante TLS/SSL (SMTPS, POP3S, IMAPS), sobre puertos distintos.

**Formas de acceder al correo:**

- **Cliente de correo** (Outlook, Thunderbird, apps móviles): requiere configurar los servidores de entrada (IMAP/POP3) y salida (SMTP).
- **Webmail**: acceso a través del navegador, sin necesidad de configuración local (Gmail, Outlook Web).

**Mensajería instantánea:** herramientas de comunicación en tiempo real de uso habitual en el entorno profesional (Microsoft Teams, Slack), complementarias al correo para comunicaciones más ágiles.

## 6.6. Servicios de transferencia de ficheros

**FTP (File Transfer Protocol):** protocolo clásico para transferir archivos entre un cliente y un servidor. Usa dos canales: uno de control (puerto 21) y otro de datos.

- **FTP**: transmite las credenciales y los datos **sin cifrar** — desaconsejado salvo en entornos controlados.
- **FTPS**: FTP sobre TLS/SSL.
- **SFTP**: protocolo distinto (no es FTP cifrado, sino un protocolo de transferencia de archivos que funciona sobre **SSH**, puerto 22); es la opción más extendida hoy en día por combinar seguridad y sencillez de configuración (un único puerto).

**Clientes de transferencia de archivos habituales:** FileZilla (gráfico, multiplataforma), `scp` y `sftp` (línea de comandos, incluidos con SSH), y clientes integrados en los propios exploradores de archivos.

**Transferencia de archivos en la nube:** servicios como WeTransfer, o el propio almacenamiento en la nube (Drive, OneDrive) con enlaces compartidos, como alternativa a FTP para transferencias puntuales sin necesidad de configurar un servidor.

!!! note "Idea clave"
    Ante la duda de qué protocolo usar para transferir archivos de forma segura entre dos equipos, **SFTP** es hoy la opción recomendada por defecto: reutiliza la seguridad y la configuración de SSH, sin necesidad de abrir puertos ni certificados adicionales.

## Actividades

**Actividad 6.1 — Carpeta compartida y combinación de permisos**
{: .actividad-titulo}

Comparte una carpeta en red con permiso de "solo lectura" a nivel de recurso compartido, pero con permiso NTFS de "control total" a nivel local. Accede desde otro equipo y comprueba qué permiso prevalece, relacionándolo con la idea clave del punto 6.1.

**Actividad 6.2 — SSH y transferencia segura**
{: .actividad-titulo}

Configura el servicio SSH en una máquina virtual Linux, conéctate desde otro equipo con un cliente SSH y transfiere un archivo mediante `scp` o `sftp`. Documenta los comandos utilizados y los puertos implicados.

**Actividad 6.3 — Grupo de trabajo vs. dominio**
{: .actividad-titulo}

Plantea el caso de una pyme que pasa de 5 a 60 empleados. Redacta un informe breve razonando si conviene mantener el modelo de grupo de trabajo o migrar a un dominio con Active Directory, describiendo al menos tres ventajas concretas de la opción elegida para ese escenario.
