# Plantilla: informe técnico de instalación

Modelo de informe para completar la **Actividad 2.1**, siguiendo la estructura vista en el punto **2.8. Documentación de instalaciones e incidencias**. Se muestra ya relleno con un caso de ejemplo (instalación de Ubuntu en una máquina virtual): copia esta estructura y sustituye los datos por los de tu propia instalación.

!!! abstract "Datos generales"
    | Campo | Detalle |
    | --- | --- |
    | Fecha | 14/07/2026 |
    | Equipo / máquina virtual | VM "Ubuntu-Practica01" (VirtualBox, 2 vCPU, 4 GB RAM, 25 GB disco) |
    | Técnico responsable | Nombre y apellidos del alumno |
    | Sistema operativo instalado | Ubuntu 22.04.4 LTS (64 bits) |

!!! note "Configuración aplicada"
    - **Particionado:** disco único de 25 GB, esquema guiado con LVM; `/` (20 GB, ext4), `swap` (2 GB), `/boot` (1 GB).
    - **Red:** adaptador en modo NAT, hostname `ubuntu-practica01`, IP asignada por DHCP.
    - **Usuarios:** cuenta de administración `alumno` con privilegios sudo; sin usuario root habilitado.
    - **Otras opciones:** idioma español (España), zona horaria Europe/Madrid, actualizaciones e instalación de terceros activadas durante el asistente.

!!! danger "Incidencias encontradas"
    | Descripción del problema | Prioridad | Acciones realizadas | Estado |
    | --- | --- | --- | --- |
    | El instalador no detectaba el disco virtual | Alta | Se revisó el controlador SATA asignado a la VM y se reinició el asistente | Resuelta |
    | Sin conexión a red tras el primer arranque | Media | Se comprobó el adaptador de red de la VM y se reinició el servicio `NetworkManager` | Resuelta |

    Si no surge ninguna incidencia, indícalo explícitamente ("Sin incidencias") en lugar de omitir el apartado.

!!! success "Resumen del proceso"
    Se instaló Ubuntu 22.04 LTS en una máquina virtual nueva para disponer de un entorno de pruebas Linux. Se arrancó desde la ISO oficial, se eligió instalación guiada con LVM para simplificar el particionado, y se configuró un único usuario con permisos de administración. Tras resolver dos incidencias menores (disco no detectado y red no operativa), la instalación finalizó correctamente y el sistema arranca y actualiza sin errores.

!!! tip "Cómo usarla"
    Copia esta plantilla en tu documento de entrega y ve sustituyendo los datos de ejemplo por los reales a medida que avanza tu instalación, no al final: es más fiel a lo realmente sucedido y evita olvidar incidencias.
