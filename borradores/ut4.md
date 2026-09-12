# UT4. Configuración de sistemas GNU/Linux

[:material-arrow-left: Volver al índice de todas las unidades](index.md)

!!! tip "Duración"
    20 horas

!!! abstract "Resultados de aprendizaje que se trabajan"
    **RA3.** Gestiona la información del sistema identificando estructuras de almacenamiento y aplicando medidas para asegurar la integridad de los datos.

## 3.1. Sistemas de archivos y estructura de directorios

Un **sistema de archivos** define cómo se organiza, nombra y almacena la información en una unidad de almacenamiento (qué metadatos se guardan de cada archivo, cómo se gestiona el espacio libre, qué tamaño máximo admite, etc.).

**Sistemas de archivos más habituales:**

| Sistema de archivos | SO principal | Journaling | Tamaño máx. de archivo | Notas |
| --- | --- | --- | --- | --- |
| FAT32 | Multiplataforma | No | 4 GB | Muy compatible, usado en pendrives |
| exFAT | Multiplataforma | No | Sin límite práctico | Sustituto de FAT32 para unidades grandes |
| NTFS | Windows | Sí | Muy grande | Soporta permisos, cifrado, compresión |
| ext4 | Linux | Sí | Muy grande | Sistema de archivos por defecto en muchas distros |
| Btrfs / XFS | Linux | Sí | Muy grande | Snapshots, mayor rendimiento en servidores |
| APFS | macOS | Sí | Muy grande | Optimizado para SSD |

**Journaling:** técnica por la que el sistema de archivos registra los cambios antes de aplicarlos, permitiendo recuperar la coherencia del sistema tras un corte de energía o un cuelgue, sin necesidad de revisar todo el disco.

**Estructura de directorios:**

- **Windows**: organización basada en **unidades con letra** (`C:`, `D:`...), cada una con su propia raíz y árbol de carpetas.
- **GNU/Linux**: un único **árbol jerárquico** con raíz `/` (FHS - *Filesystem Hierarchy Standard*), donde las distintas unidades se **montan** como subdirectorios de ese árbol. Directorios más relevantes:

| Directorio | Contenido |
| --- | --- |
| `/bin`, `/usr/bin` | Programas y comandos básicos |
| `/etc` | Archivos de configuración del sistema |
| `/home` | Directorios personales de los usuarios |
| `/var` | Datos variables: logs, colas, cachés |
| `/mnt`, `/media` | Puntos de montaje de unidades adicionales |
| `/dev` | Archivos especiales que representan dispositivos |
| `/proc`, `/sys` | Información del kernel y procesos en ejecución |

**Montaje (mount):** proceso de asociar una unidad o partición a un punto concreto del árbol de directorios (en Linux, mediante el comando `mount` o el archivo `/etc/fstab` para montajes automáticos al arrancar).

## 3.2. Administración de discos: particiones y volúmenes

**Particionar** un disco consiste en dividirlo lógicamente en secciones independientes, cada una con su propio sistema de archivos.

**Esquemas de particionado:**

- **MBR**: hasta 4 particiones primarias, o 3 primarias + 1 extendida que puede contener varias particiones lógicas.
- **GPT**: hasta 128 particiones sin necesidad de particiones extendidas, y discos de mayor capacidad.

**Volúmenes:**

- **Disco básico (Windows)**: cada partición es un volumen independiente.
- **Disco dinámico (Windows)**: permite crear volúmenes que abarcan varios discos físicos (distribuidos, reflejados...).
- **LVM - Logical Volume Manager (Linux)**: capa de abstracción entre las particiones físicas y el sistema de archivos, formada por:
    - **PV (Physical Volume)**: partición o disco físico.
    - **VG (Volume Group)**: agrupación de uno o varios PV.
    - **LV (Logical Volume)**: "partición virtual" creada dentro de un VG, que se puede redimensionar con mucha más flexibilidad que una partición tradicional.

**Herramientas de gestión de discos:**

| SO | Herramienta gráfica | Herramienta por comandos |
| --- | --- | --- |
| Windows | Administración de discos | `diskpart` |
| Linux | GParted | `fdisk`, `parted`, `lvm` |

**RAID (Redundant Array of Independent Disks):** combina varios discos físicos en una única unidad lógica para mejorar el rendimiento, la tolerancia a fallos, o ambas cosas.

| Nivel | Nombre | Discos mínimos | Tolerancia a fallos | Capacidad útil | Idea |
| --- | --- | --- | --- | --- | --- |
| RAID 0 | Seccionado (*striping*) | 2 | Ninguna: un fallo pierde todos los datos | 100 % (suma de discos) | Reparte los datos en bloques entre los discos para maximizar la velocidad |
| RAID 1 | Reflejado (*mirroring*) | 2 | Tolera el fallo de un disco | 50 % (duplicada) | Duplica los mismos datos en dos discos |
| RAID 5 | Con paridad distribuida | 3 | Tolera el fallo de un disco | (n-1)/n del total | Reparte datos y paridad entre todos los discos: si uno falla, se reconstruye a partir de la paridad |

En Windows, estos niveles corresponden a los "discos dinámicos" ya mencionados (volumen seccionado = RAID 0, volumen reflejado = RAID 1); en Linux se gestionan por software con `mdadm`. Los servidores dedicados suelen usar además una controladora RAID hardware, más rápida y transparente para el sistema operativo.

!!! warning "RAID no es una copia de seguridad"
    El RAID protege frente al fallo de un disco, pero no frente a un borrado accidental, un ataque de ransomware o un error humano, que se replican igual en todos los discos del conjunto. Sigue siendo necesaria una copia de seguridad independiente (ver 3.3).

**Cuotas de disco:** permiten limitar el espacio que cada usuario puede ocupar en un volumen compartido, evitando que uno solo agote el espacio disponible para el resto.

- **Windows**: se habilitan por volumen NTFS (propiedades del disco → pestaña Cuota, o mediante directiva de grupo), definiendo un límite y, opcionalmente, un nivel de advertencia previo.
- **Linux**: el subsistema `quota` (comandos `edquota` para configurar y `repquota` para consultar) sobre sistemas de archivos que lo admitan (ext4 con las opciones `usrquota`/`grpquota`).

## 3.3. Copias de seguridad y restauración

Una **copia de seguridad (backup)** es una duplicación de la información que permite recuperarla ante un borrado accidental, un fallo de hardware, un ataque (p. ej. ransomware) o un desastre.

**Tipos de copia de seguridad:**

| Tipo | Qué copia | Ventaja | Inconveniente |
| --- | --- | --- | --- |
| Completa | Todos los datos seleccionados | Restauración simple (una sola copia) | Ocupa más espacio y tiempo |
| Incremental | Solo lo cambiado desde la última copia (de cualquier tipo) | Rápida y ligera | Restaurar requiere la completa + todas las incrementales, en orden |
| Diferencial | Solo lo cambiado desde la última copia **completa** | Restauración más simple que la incremental | Crece con el tiempo hasta la siguiente completa |

**Estrategia recomendada — regla 3-2-1:** mantener al menos **3** copias de los datos, en **2** soportes distintos, con **1** copia fuera de las instalaciones (offsite/nube).

**Herramientas habituales:** Copia de seguridad y restauración / Historial de archivos (Windows), Time Machine (macOS), `rsync` y `tar` (Linux), soluciones en la nube (OneDrive, Google Drive).

!!! warning "Una copia sin probar no es una copia fiable"
    Es tan importante hacer copias de seguridad como **verificar periódicamente que se pueden restaurar** correctamente. Una copia corrupta o incompleta solo se descubre, en el peor de los casos, cuando ya se necesita.

## Actividades

**Actividad 3.1 — Particionado y volúmenes**
{: .actividad-titulo}

En una máquina virtual Linux, añade un disco virtual adicional, particiónalo con `fdisk` o `parted`, formatea la partición con `ext4` y móntala de forma persistente en `/mnt/datos` (editando `/etc/fstab`). Repite el proceso creando un volumen lógico LVM (PV, VG, LV) sobre otro disco adicional y compara ambos enfoques.

**Actividad 3.2 — Estrategia de copias de seguridad**
{: .actividad-titulo}

Plantea una estrategia de copia de seguridad 3-2-1 para los datos de una pequeña empresa (documentos de facturación, código fuente de un proyecto y una base de datos). Indica qué tipo de copia (completa, incremental o diferencial) aplicarías cada día de la semana y justifica la elección en términos de espacio ocupado y tiempo de restauración.
