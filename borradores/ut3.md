# UT3. Sistemas operativos: Instalación y primeros pasos

[:material-arrow-left: Volver al índice de todas las unidades](index.md)

!!! tip "Duración"
    45 horas

!!! abstract "Resultado de aprendizaje que se trabaja"
    **RA2.** Instala sistemas operativos planificando el proceso e interpretando documentación técnica.

    *Transversal:* **RA7** (parcial) — instalación y uso de aplicaciones informáticas de propósito general (ofimática, trabajo colaborativo y utilidades).

## 2.1. Evolución, clasificación y funciones de los SO

Un **sistema operativo (SO)** es el software que gestiona los recursos hardware de un equipo (procesador, memoria, almacenamiento, periféricos) y ofrece a los programas y al usuario una interfaz para utilizarlos, sin necesidad de conocer los detalles técnicos del hardware subyacente.

**Evolución histórica (resumen):**

- **Sistemas por lotes (batch, años 50-60)**: los trabajos se agrupaban y ejecutaban secuencialmente sin interacción del usuario.
- **Sistemas de tiempo compartido (timesharing, años 60-70)**: varios usuarios comparten el mismo equipo mediante turnos muy cortos de CPU, dando sensación de simultaneidad.
- **Sistemas multiusuario/multitarea (años 80-90)**: consolidación de Unix y, más adelante, de sistemas personales como MS-DOS y Windows.
- **Sistemas gráficos modernos**: Windows, macOS, distribuciones Linux con entorno de escritorio.
- **Sistemas móviles y embebidos**: Android, iOS, sistemas orientados a un único dispositivo o tarea concreta.

**Funciones principales de un sistema operativo:**

- **Gestión de procesos**: crear, planificar y finalizar los programas en ejecución, repartiendo el tiempo de CPU entre ellos.
- **Gestión de memoria**: asignar y liberar espacio en RAM a cada proceso, y gestionar la memoria virtual.
- **Gestión de almacenamiento y archivos**: organizar la información en sistemas de archivos (carpetas, permisos, metadatos).
- **Gestión de dispositivos de E/S**: comunicarse con los periféricos a través de controladores (drivers).
- **Interfaz con el usuario**: línea de comandos (CLI) y/o interfaz gráfica (GUI).
- **Seguridad y control de acceso**: autenticación de usuarios y gestión de permisos.

**Clasificación de los sistemas operativos:**

| Criterio | Categorías |
| --- | --- |
| Número de usuarios | Monousuario / Multiusuario |
| Número de tareas | Monotarea / Multitarea |
| Número de procesadores atendidos | Monoprocesador / Multiprocesador |
| Tipo de interfaz | Texto (CLI) / Gráfica (GUI) |
| Arquitectura del núcleo | Monolítico, microkernel, híbrido |
| Ámbito de uso | Escritorio, servidor, móvil, embebido |

**Grandes familias actuales:** Windows, distribuciones **GNU/Linux** (Ubuntu, Debian, Fedora, etc.), **macOS**, y sistemas móviles **Android** e **iOS**.

## 2.2. Tipos de aplicaciones y licencias de software

**Software de sistema vs. software de aplicación:**

- **Software de sistema**: el propio SO y las herramientas que gestionan el hardware (drivers, utilidades de sistema).
- **Software de aplicación**: programas orientados a que el usuario realice una tarea concreta (ofimática, navegación, edición, etc.).

**Clasificación según el tipo de licencia:**

| Tipo de licencia | Código fuente | Coste | Ejemplo |
| --- | --- | --- | --- |
| Software libre | Disponible, se puede modificar y redistribuir | Puede ser gratuito o de pago | LibreOffice, GNU/Linux |
| Software propietario | Cerrado | Habitualmente de pago | Microsoft Windows, Adobe Photoshop |
| Freeware | No necesariamente disponible | Gratuito, sin acceso al código | Adobe Acrobat Reader |
| Shareware | No necesariamente disponible | Gratuito por tiempo/funciones limitadas, después de pago | Versiones de prueba (*trial*) |
| Dominio público | Puede estar disponible | Gratuito, sin restricciones de autor | Software con copyright expirado |
| OEM | — | Vinculado a la venta de hardware | Windows preinstalado en un equipo nuevo |

**Licencias de software libre más comunes:** GPL (copyleft: las modificaciones deben mantenerse libres), MIT y BSD (permisivas: permiten uso en proyectos propietarios), Apache 2.0.

**Modalidades de licenciamiento comercial:** licencia individual (*retail*), licencia por volumen (para empresas/centros educativos), licencia OEM (atada a un equipo), suscripción (SaaS, pago periódico, p. ej. Microsoft 365).

!!! note "Idea clave"
    "Gratuito" (freeware) y "libre" (open source) no son sinónimos: un programa puede ser gratuito y a la vez cerrado (no se puede ver ni modificar su código).

## 2.3. Planificación y procedimiento de instalación de un SO

Antes de instalar un sistema operativo conviene planificar el proceso:

1. **Comprobar requisitos hardware** (procesador, RAM, espacio en disco, controladores compatibles).
2. **Elegir el tipo de instalación**:
    - **Instalación limpia**: formatea el disco/partición e instala el SO desde cero.
    - **Actualización (upgrade)**: instala una versión nueva conservando aplicaciones y datos.
    - **Arranque dual / multiarranque (dual boot)**: instala varios sistemas operativos en particiones distintas del mismo equipo.
3. **Realizar copia de seguridad** de los datos importantes antes de empezar.
4. **Preparar el medio de instalación**: USB o DVD de arranque, o instalación por red (PXE) en entornos con muchos equipos.
5. **Planificar el particionado** del disco (tamaño y sistema de archivos de cada partición), incluyendo el espacio para memoria de intercambio si procede (ver más abajo).

???+ tip "Medios de arranque con varios sistemas"
    Herramientas como **Rufus** o **balenaEtcher** graban una única imagen ISO en un USB. Para llevar varias ISO de instalación distintas en el mismo pendrive (varias distribuciones Linux, herramientas de recuperación...) sin tener que reformatearlo cada vez, se usan herramientas de "USB multiboot" como **Ventoy**: se instala una sola vez en el USB y, a partir de ahí, basta con copiar o borrar archivos ISO con el explorador de archivos para añadir o quitar sistemas arrancables.

**Memoria de intercambio (swap / archivo de paginación):** además de la RAM física, el sistema operativo puede usar espacio en disco como memoria virtual cuando la RAM se agota, a costa de una velocidad mucho menor.

- **Windows** usa un **archivo de paginación** (`pagefile.sys`), ubicado en la propia partición del sistema y gestionado automáticamente por defecto.
- **GNU/Linux** suele reservar una **partición o un archivo swap** independiente, configurable durante el particionado.

| RAM instalada | Swap orientativa |
| --- | --- |
| ≤ 2 GB | El doble de la RAM |
| 2-8 GB | Igual a la RAM |
| > 8 GB | Entre 4 y 8 GB suele bastar |

Conviene aumentar esta orientación si se va a usar **hibernación** (necesita una swap al menos igual a la RAM instalada, para volcar en ella todo su contenido) o en equipos con cargas de trabajo que consumen mucha memoria. En Linux, el parámetro `vm.swappiness` (0-100) regula cuánto tiende el sistema a usar la swap en lugar de liberar RAM: un valor bajo (5-10) es preferible en un equipo de escritorio con SSD, para minimizar tanto el impacto en el rendimiento como el desgaste de la unidad.

**Fases típicas del proceso de instalación:**

1. Arranque desde el medio de instalación (configurando el orden de arranque en la BIOS/UEFI).
2. Selección de idioma, zona horaria y distribución de teclado.
3. Particionado y selección del sistema de archivos.
4. Copia de archivos e instalación del sistema base.
5. Configuración inicial: usuario administrador, contraseña, nombre del equipo, red.
6. Instalación de controladores y actualizaciones posteriores a la instalación.

## 2.4. Gestores de arranque: configuración y reparación

Al encender un equipo, el firmware (**BIOS** o **UEFI**) localiza un dispositivo de arranque y cede el control al **gestor de arranque (bootloader)**, el programa encargado de cargar el sistema operativo.

**Estilo de partición del disco:**

- **MBR (Master Boot Record)**: esquema clásico, máximo 4 particiones primarias, discos de hasta 2 TiB.
- **GPT (GUID Partition Table)**: esquema moderno, sin ese límite de particiones ni de tamaño de disco; requiere arranque en modo UEFI.

**Gestores de arranque habituales:**

- **GRUB2**: gestor de arranque estándar en la mayoría de distribuciones Linux; permite arrancar varios sistemas operativos (multiarranque) mostrando un menú de selección.
- **Windows Boot Manager (bootmgr)**: gestor de arranque de los sistemas Windows actuales.

**Reparación del gestor de arranque:** situaciones típicas en las que deja de funcionar (tras instalar otro SO que lo sobrescribe, tras un fallo de disco) y herramientas para repararlo:

- En Windows: entorno de recuperación, comandos como `bootrec /fixmbr`, `bootrec /fixboot` y `bootrec /rebuildbcd`.
- En Linux: reinstalación de GRUB desde un *live CD* (`grub-install`, `update-grub`).

!!! warning "Multiarranque"
    En un sistema con arranque dual, es recomendable instalar primero Windows y después Linux, ya que GRUB puede detectar e incluir automáticamente las instalaciones de Windows en su menú; en el orden inverso, el instalador de Windows suele sobrescribir el gestor de arranque de Linux.

## 2.5. Virtualización: tipos y herramientas

La **virtualización** permite ejecutar uno o varios sistemas operativos "invitados" (guest) de forma aislada sobre un mismo equipo físico "anfitrión" (host), compartiendo sus recursos hardware.

**Tipos de hipervisor** (software que crea y gestiona las máquinas virtuales):

| Tipo | Descripción | Ejemplos |
| --- | --- | --- |
| Tipo 1 (bare metal) | Se ejecuta directamente sobre el hardware, sin SO anfitrión | VMware ESXi, Microsoft Hyper-V, Proxmox VE |
| Tipo 2 (hosted) | Se ejecuta como una aplicación sobre un SO anfitrión ya instalado | VirtualBox, VMware Workstation, Parallels |

**Ventajas de la virtualización:** aislamiento entre sistemas, aprovechamiento del hardware, facilidad para probar sistemas operativos sin arriesgar el equipo real, uso de **instantáneas (snapshots)** para volver a un estado anterior, portabilidad de las máquinas virtuales entre equipos.

**Conceptos relacionados:**

- **Máquina virtual (VM)**: equipo completo simulado por software, con su propio SO, disco virtual y recursos asignados.
- **Contenedores** (Docker, LXC): virtualización a nivel de sistema operativo, más ligera que una VM completa porque comparten el kernel del host; se estudian en detalle en otros módulos, pero conviene conocer la diferencia con una VM tradicional.

???+ example "Ampliación: contenedor vs. máquina virtual con Docker"
    Si ya has terminado las actividades básicas de esta unidad, puedes comprobar por ti mismo/a la diferencia entre contenedor y VM:

    1. Instala Docker Desktop (o Docker Engine en Linux) en tu equipo o en una VM.
    2. Arranca el mismo servicio de dos formas: como contenedor (`docker run -d -p 8080:80 nginx`) y como máquina virtual completa con ese mismo servicio instalado.
    3. Compara el tiempo desde que lo lanzas hasta que responde, el espacio en disco ocupado y la memoria RAM consumida (`docker stats` frente al Administrador de tareas/`htop` de la VM).
    4. Con `docker exec -it <contenedor> bash` y luego `uname -a`, comprueba que el kernel que ve el contenedor es el mismo que el de tu equipo anfitrión — a diferencia de la VM, que tiene el suyo propio.

    No es una actividad evaluada: es una forma de comprobar de primera mano por qué un contenedor no sirve para las actividades de esta unidad (no tiene BIOS/UEFI, gestor de arranque ni disco que particionar) y, en cambio, resulta mucho más ligero y rápido para desplegar un servicio ya construido — algo que se estudiará en detalle en módulos posteriores de despliegue.

**Discos virtuales:** cada VM almacena su disco como uno o varios archivos en el sistema de archivos del anfitrión, en formatos como **VDI** (nativo de VirtualBox), **VMDK** (VMware, el más extendido para el intercambio entre plataformas) o **VHD/VHDX** (Hyper-V). Al crear el disco se elige entre:

- **Tamaño dinámico**: el archivo ocupa en el anfitrión solo el espacio realmente usado por la VM, y va creciendo hasta el máximo definido. Es la opción recomendada en la mayoría de los casos.
- **Tamaño fijo**: reserva de entrada todo el espacio máximo en el anfitrión. Ofrece un rendimiento algo más constante, a costa de ocupar desde el principio todo el espacio, tanto se use como si no.

!!! note "Idea clave"
    Con un disco de tamaño dinámico, el sistema operativo invitado "cree" que dispone de todo el espacio del disco virtual, aunque el anfitrión no le haya asignado aún ese espacio real; si el disco del anfitrión se llena antes de alcanzar ese máximo, la máquina virtual puede fallar de forma abrupta.

**Guest Additions / VMware Tools:** paquete de controladores y utilidades que se instala *dentro* del sistema operativo invitado (no en el anfitrión) y mejora la integración entre ambos: resolución de pantalla ajustable, portapapeles compartido, arrastrar y soltar archivos, carpetas compartidas y mejor rendimiento gráfico y de red. Sin este paquete instalado, la máquina virtual funciona, pero con prestaciones e integración limitadas.

**Exportación e importación de máquinas virtuales:** para trasladar una VM completa a otro equipo o hipervisor se usa el estándar **OVF** (*Open Virtualization Format*), admitido por VirtualBox, VMware y otros fabricantes. Un paquete OVF consta de un archivo de configuración y uno o varios discos virtuales (normalmente convertidos a VMDK); empaquetado todo junto en un único archivo comprimido, se usa la extensión **OVA**. Esto permite migrar una VM entre plataformas de virtualización distintas, aunque no siempre se conservan todas sus características si el hipervisor de destino no las soporta.

## 2.6. Instalación, desinstalación y actualización de aplicaciones

**Formas de instalar aplicaciones según el sistema operativo:**

- **Windows**: instaladores ejecutables (`.exe`, `.msi`), tiendas de aplicaciones (Microsoft Store), instalación silenciosa mediante parámetros de línea de comandos (útil para despliegues masivos).
- **GNU/Linux**: gestores de paquetes que resuelven automáticamente las dependencias:
    - `apt` / `dpkg` (Debian, Ubuntu)
    - `dnf` / `rpm` (Fedora, RHEL)
    - `pacman` (Arch Linux)
- **macOS**: paquetes `.pkg`, aplicaciones `.app`, App Store.

**Desinstalación:** debe eliminar tanto los archivos del programa como sus entradas de configuración asociadas (registro en Windows, archivos de configuración en `/etc` o el directorio personal en Linux) para no dejar residuos.

**Actualización de aplicaciones:** puede hacerse de forma manual, mediante el propio gestor de paquetes, o mediante mecanismos de autoactualización integrados en la aplicación.

**Verificación de la integridad del software descargado:**

- **Sumas de comprobación (checksums)**: valores hash (MD5, SHA-256) que permiten comprobar que el archivo descargado no está corrupto ni manipulado.
- **Firma digital**: garantiza la autenticidad del editor del software.

!!! tip "Buena práctica"
    Descargar siempre el software desde la web oficial del fabricante o repositorios oficiales, y comprobar la suma de comprobación cuando esté disponible, especialmente en instalaciones de sistemas o software crítico.

## 2.7. Actualización y recuperación del sistema

**Actualización del sistema operativo:**

- **Windows Update**: distribuye actualizaciones de seguridad, correcciones y nuevas funciones.
- **Gestores de paquetes en Linux** (`apt upgrade`, `dnf upgrade`...): actualizan tanto el sistema base como las aplicaciones instaladas desde los repositorios.
- Es recomendable distinguir entre actualizaciones de seguridad (críticas, a aplicar cuanto antes) y actualizaciones de funcionalidad (pueden planificarse).

**Mecanismos de recuperación del sistema:**

- **Puntos de restauración (Windows)**: permiten volver la configuración del sistema a un estado anterior sin afectar a los archivos personales.
- **Partición/entorno de recuperación**: partición oculta con herramientas de diagnóstico y reparación, o entorno de recuperación desde un medio externo.
- **Imágenes de sistema (system image / clonado)**: copia completa del disco o partición del sistema, que permite restaurar el equipo por completo tras un fallo grave.
- **Modo seguro (Safe Mode) / modo de emergencia**: arranque del sistema con el mínimo de controladores y servicios, útil para diagnosticar problemas.

## 2.8. Documentación de instalaciones e incidencias

Documentar el trabajo técnico es una parte esencial de la profesión: facilita el mantenimiento futuro, permite a otros técnicos entender lo realizado y sirve como evidencia ante el cliente.

**Qué documentar en una instalación:**

- Fecha, equipo/s afectados y técnico responsable.
- Versión del sistema operativo y software instalado.
- Configuración aplicada (particionado, red, usuarios).
- Incidencias encontradas durante el proceso y solución aplicada.

**Gestión de incidencias:** en un entorno profesional, las incidencias suelen registrarse en un sistema de *tickets* (helpdesk) que recoge, como mínimo: descripción del problema, prioridad, técnico asignado, acciones realizadas y estado (abierta, en curso, resuelta).

!!! tip "Estructura de un informe técnico"
    Un buen informe de instalación o de incidencia debe ser breve, ordenado cronológicamente y reproducible por otra persona: **qué se hizo, por qué, cómo, y qué resultado se obtuvo**.

## 2.9. Herramientas ofimáticas y de trabajo colaborativo

Las **suites ofimáticas** agrupan aplicaciones para las tareas más comunes en un entorno de oficina:

| Tipo de aplicación | Función | Ejemplos |
| --- | --- | --- |
| Procesador de texto | Redacción y maquetación de documentos | Word, Writer (LibreOffice), Google Docs |
| Hoja de cálculo | Cálculos, tablas y gráficos | Excel, Calc, Google Sheets |
| Presentaciones | Diapositivas para exposiciones | PowerPoint, Impress, Google Slides |
| Gestor de bases de datos de escritorio | Bases de datos sencillas | Access, Base (LibreOffice) |

**Suites más habituales:** Microsoft 365 (propietaria, de suscripción), LibreOffice (libre y gratuita), Google Workspace (basada en la nube).

**Herramientas de trabajo colaborativo:**

- **Almacenamiento y edición compartida en la nube**: Google Drive, OneDrive, Nextcloud, que permiten que varias personas editen un mismo documento simultáneamente.
- **Control de versiones de documentos**: historial de cambios y posibilidad de recuperar versiones anteriores.

**Edición colaborativa en tiempo real frente a sincronización de archivos:** son dos modelos distintos, aunque a veces se confunden. Un editor colaborativo (Google Docs, Office Online) mantiene una conexión permanente con el servidor y **fusiona los cambios de cada persona a nivel de carácter**, casi al instante, mediante algoritmos de resolución de conflictos (fuera del alcance de este módulo). Un servicio de sincronización de carpetas (Dropbox, OneDrive/Google Drive de escritorio) sincroniza en cambio **el archivo completo** de vez en cuando: si dos personas editan el mismo archivo sin conexión a la vez, no hay fusión automática y se genera un archivo de "copia en conflicto" que hay que resolver a mano.
- **Comunicación y videoconferencia**: Google Meet, Microsoft Teams, Zoom.
- **Gestión de tareas y proyectos en equipo**: Trello, Asana, Microsoft Planner.

## 2.10. Utilidades de propósito general (antimalware, mantenimiento, recuperación de datos)

**Herramientas antimalware:** software que detecta y elimina programas maliciosos (virus, troyanos, ransomware, spyware). Suelen combinar análisis por firmas (comparación con una base de datos de amenazas conocidas) y análisis heurístico/de comportamiento (detección de patrones sospechosos). Ejemplos: Windows Defender, ClamAV (libre, muy usado en servidores Linux).

**Utilidades de mantenimiento del sistema:** liberación de espacio en disco (archivos temporales, caché), gestión de programas que se inician automáticamente, limpieza de entradas obsoletas de configuración.

**Recuperación de datos:** herramientas capaces de intentar recuperar archivos borrados accidentalmente o dañados por un fallo del sistema de archivos, aprovechando que al borrar un archivo normalmente solo se elimina su referencia en el sistema de archivos, no el contenido en sí (hasta que ese espacio se reutiliza). Ejemplos: Recuva, TestDisk/PhotoRec.

**Utilidades de compresión:** reducen el tamaño de los archivos y permiten agrupar varios en uno solo, facilitando su almacenamiento y transferencia. Formatos habituales: ZIP, RAR, 7z, TAR+GZIP (este último muy usado en Linux).

!!! warning "El antimalware no sustituye las buenas prácticas"
    Ninguna herramienta antimalware sustituye a unas buenas prácticas básicas: mantener el sistema actualizado, desconfiar de archivos adjuntos y enlaces desconocidos, y hacer copias de seguridad periódicas.

## Actividades

**Actividad 2.1 — Instalación documentada de un SO**
{: .actividad-titulo}

Instala una distribución Linux (por ejemplo, Ubuntu) en una máquina virtual, documentando cada fase del proceso (particionado elegido, configuración inicial, incidencias encontradas) a partir de esta **[plantilla de informe técnico de instalación](plantilla-informe-instalacion.md)**, siguiendo la estructura vista en el punto 2.8.

**Actividad 2.2 — Licenciamiento de sistemas operativos: comparativa aplicada**
{: .actividad-titulo}

Una empresa se plantea dos escenarios de renovación de equipos:

a) Comprar **5 equipos nuevos** para el departamento de administración, que necesitan compatibilidad total con Microsoft Office y una aplicación de gestión que solo existe para Windows.

b) Reutilizar **20 equipos antiguos** dados de baja de otro departamento, con hardware modesto, para montar un aula de formación.

Para cada escenario:

- Indica qué modalidad de licencia de Windows (OEM, retail o por volumen) o qué distribución Linux recomendarías, razonando el coste aproximado y las condiciones de cada modalidad (a qué equipo queda ligada la licencia, si se puede trasladar a otro, si permite redistribución o modificación).
- Explica la diferencia legal y práctica entre instalar una copia de evaluación, una versión OEM comprada sin equipo asociado y una distribución GNU/Linux, en términos de lo que la empresa puede y no puede hacer con cada una.
- Justifica, para el escenario (b), si compensaría migrar a una distribución Linux en lugar de licenciar Windows para esos 20 equipos, considerando el coste y la compatibilidad de software necesaria.

Presenta la comparativa en una tabla con, al menos: tipo de licencia, coste aproximado, posibilidad de redistribución/modificación y escenario recomendado.

**Actividad 2.3 — Arranque dual y reparación de GRUB**
{: .actividad-titulo}

En un entorno virtualizado, instala Windows y a continuación Linux en el mismo disco para configurar un arranque dual. Simula después un fallo del gestor de arranque (por ejemplo, reinstalando Windows) y documenta el procedimiento para reparar GRUB desde un *live CD*.

**Actividad 2.4 — Snapshots y virtualización**
{: .actividad-titulo}

Crea una máquina virtual, instala una aplicación y toma una instantánea (snapshot). Desinstala después la aplicación y restaura la instantánea, explicando en qué escenarios reales de un departamento de informática resulta útil esta técnica.
