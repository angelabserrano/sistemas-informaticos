# UT5. Administración de sistemas GNU/Linux

[:material-arrow-left: Volver al índice de todas las unidades](index.md)

!!! tip "Duración"
    30 horas

!!! abstract "Resultados de aprendizaje que se trabajan"
    **RA4.** Gestiona sistemas operativos utilizando comandos y herramientas gráficas evaluando las necesidades del sistema.

## 4.1. Tareas automáticas y planificación

Los sistemas operativos permiten **automatizar tareas repetitivas** (copias de seguridad, limpieza de archivos temporales, actualizaciones) para que se ejecuten sin intervención manual, en un horario determinado.

**Windows — Programador de tareas (Task Scheduler):** permite definir un desencadenador (hora, evento, inicio de sesión) y una acción (ejecutar un programa o script) mediante interfaz gráfica (`taskschd.msc`) o el comando `schtasks`.

**Linux — cron:** el demonio `cron` ejecuta tareas según lo definido en un **crontab**, con el formato:

```text
# minuto  hora  día-mes  mes  día-semana   comando
   0       3      *       *       *        /usr/local/bin/backup.sh
```

Cada usuario puede tener su propio crontab (`crontab -e`), y existen directorios como `/etc/cron.daily` para tareas del sistema. En distribuciones modernas, los **timers de systemd** ofrecen una alternativa más flexible e integrada con el registro de logs del sistema.

## 4.2. Usuarios, grupos y directivas de seguridad

**Cuentas de usuario:** identifican a cada persona (o servicio) que accede al sistema, permitiendo aplicar permisos y auditar su actividad de forma individual.

- **Tipos de cuenta**: administrador/root (control total del sistema) y usuario estándar (permisos limitados, recomendado para el uso diario).
- **Grupos**: agrupan usuarios para asignarles permisos de forma conjunta, en lugar de uno a uno.

**Directivas de seguridad habituales:**

- **Política de contraseñas**: longitud mínima, complejidad, caducidad, historial (para evitar reutilizar contraseñas anteriores).
- **Bloqueo de cuenta**: número de intentos fallidos antes de bloquear temporalmente el acceso.
- **Directivas de grupo (GPO, Windows)**: permiten aplicar de forma centralizada una configuración (seguridad, restricciones, software) a un conjunto de equipos o usuarios de un dominio.
- **UAC (User Account Control, Windows)**: eleva puntualmente los privilegios de una aplicación o acción concreta (instalar software, modificar el registro, crear un servicio...), pidiendo confirmación explícita en lugar de trabajar todo el rato con privilegios de administrador. Su equivalente conceptual en Linux es `sudo`: cada comando se ejecuta con privilegios elevados solo cuando se invoca explícitamente, en vez de iniciar sesión permanentemente como root.
- **PAM - Pluggable Authentication Modules (Linux)**: sistema modular que gestiona cómo se autentican los usuarios y qué políticas de contraseña se aplican.

!!! tip "Principio de mínimo privilegio"
    Cada usuario (o servicio) debe disponer únicamente de los permisos estrictamente necesarios para realizar su trabajo, nunca más. Reduce el impacto de un error o de una cuenta comprometida.

## 4.3. Permisos locales y listas de control de acceso

**Permisos en Windows (NTFS):** se aplican sobre archivos y carpetas e incluyen niveles como *lectura*, *escritura*, *modificar*, *lectura y ejecución* y *control total*. Por defecto, los permisos se **heredan** de la carpeta superior, aunque pueden personalizarse para un archivo o subcarpeta concretos.

**Permisos en Linux:** cada archivo tiene un propietario (*owner*), un grupo propietario y unos permisos para tres categorías: propietario (`u`), grupo (`g`) y resto de usuarios (`o`), sobre tres operaciones: **lectura (r)**, **escritura (w)** y **ejecución (x)**.

```text
-rwxr-xr--  1 ana  profesores  4096 ene 10 10:00 practica.sh
 │└┬┘└┬┘└┬┘
 │ │  │   └─ otros: r-- (solo lectura)
 │ │  └───── grupo: r-x (lectura y ejecución)
 │ └──────── propietario: rwx (lectura, escritura y ejecución)
 └────────── tipo de archivo ( - archivo normal, d directorio)
```

- `chmod`: cambia los permisos (`chmod 754 archivo` o `chmod u+x archivo`).
- `chown`: cambia el propietario y/o grupo (`chown ana:profesores archivo`).

**Listas de control de acceso (ACL):** permiten definir permisos más finos que el modelo básico usuario/grupo/otros, asignando permisos específicos a usuarios o grupos concretos adicionales.

- **Windows**: pestaña de seguridad avanzada de un archivo/carpeta.
- **Linux**: comandos `setfacl` (asignar) y `getfacl` (consultar).

**Cifrado como capa adicional de protección:** los permisos y las ACL solo se aplican mientras el sistema operativo está en marcha y los hace cumplir; alguien con acceso físico al disco (extrayéndolo o arrancando desde otro medio) puede saltárselos por completo. El cifrado protege también en ese escenario:

- **EFS (Encrypting File System, Windows)**: cifra archivos o carpetas individuales en un volumen NTFS, de forma transparente para el usuario que los cifró (se activa con una casilla en las propiedades avanzadas del archivo).
- **BitLocker (Windows)**: cifra el volumen completo, incluido el sistema operativo, protegiendo los datos si el disco se pierde, se roba o se extrae del equipo. Disponible en las ediciones Pro/Enterprise/Education de Windows.
- **LUKS (Linux Unified Key Setup)**: estándar de cifrado de particiones o discos completos en Linux, habitual como opción al particionar durante la instalación (ver UT3).

!!! note "Idea clave"
    Los permisos y las ACL controlan **quién puede acceder** mientras el sistema está en marcha; el cifrado protege los datos **aunque alguien se salte el sistema operativo** por completo, accediendo al disco directamente. Son capas complementarias, no alternativas.

## 4.4. Servicios y procesos

**Proceso:** instancia de un programa en ejecución, con su propio espacio de memoria y estado (en ejecución, en espera, detenido...).

**Servicio (Windows) / demonio o daemon (Linux):** proceso que se ejecuta en segundo plano, habitualmente sin interacción directa del usuario, prestando una función del sistema (servidor web, gestión de impresión, sincronización horaria...).

**Gestión de procesos:**

| SO | Herramienta gráfica | Comandos |
| --- | --- | --- |
| Windows | Administrador de tareas | `tasklist`, `taskkill` |
| Linux | — | `ps`, `top` / `htop`, `kill`, `killall` |

**Gestión de servicios:**

| SO | Herramienta gráfica | Comandos |
| --- | --- | --- |
| Windows | `services.msc` | `sc`, PowerShell `Get-Service` / `Start-Service` / `Stop-Service` |
| Linux (systemd) | — | `systemctl start/stop/restart/status/enable/disable <servicio>` |

## 4.5. Comandos de administración (SO libres y propietarios)

Aunque las tareas se puedan realizar mediante entorno gráfico, el uso de la **línea de comandos** es imprescindible en administración de sistemas: es más rápida, permite automatizar tareas mediante scripts, y es a menudo la única vía de acceso en servidores remotos.

| Tarea | Windows (CMD / PowerShell) | Linux |
| --- | --- | --- |
| Listar archivos | `dir` | `ls -l` |
| Cambiar de directorio | `cd` | `cd` |
| Copiar archivos | `copy` | `cp` |
| Mover/renombrar | `move` / `ren` | `mv` |
| Eliminar archivos | `del` | `rm` |
| Crear directorio | `mkdir` | `mkdir` |
| Ver contenido de un archivo | `type` | `cat` |
| Configuración de red | `ipconfig` | `ip a` / `ifconfig` |
| Comprobar conectividad | `ping` | `ping` |
| Procesos en ejecución | `tasklist` | `ps aux` |
| Información del sistema | `systeminfo` | `uname -a` |
| Buscar en archivos | `findstr` | `grep` |

**PowerShell** merece mención aparte: a diferencia del CMD tradicional, sus comandos (*cmdlets*, con formato `Verbo-Nombre`, p. ej. `Get-Process`) trabajan con objetos estructurados en lugar de texto plano, lo que facilita mucho la automatización avanzada en entornos Windows.

## 4.6. Monitorización, registros y herramientas de mantenimiento

**Registros de sistema (logs):** almacenan de forma cronológica los eventos relevantes del sistema (errores, inicios de sesión, arranque de servicios), fundamentales para diagnosticar problemas y para auditoría de seguridad.

- **Windows**: **Visor de eventos** (`eventvwr.msc`), organizado en registros de Aplicación, Seguridad y Sistema, entre otros.
- **Linux**: tradicionalmente en `/var/log` (`syslog`, `auth.log`...); en distribuciones con systemd, el comando `journalctl` centraliza la consulta de logs.

**Monitorización del rendimiento:**

| SO | Herramientas |
| --- | --- |
| Windows | Administrador de tareas, Monitor de rendimiento, Monitor de recursos |
| Linux | `top`, `htop`, `vmstat`, `iostat`, `free -h` (memoria), `df -h` (disco) |

**Herramientas de mantenimiento del sistema:** liberador de espacio en disco, desfragmentación (relevante en HDD, innecesaria y contraproducente en SSD, donde en su lugar se usa la operación **TRIM**), comprobación de errores del sistema de archivos (`chkdsk` en Windows, `fsck` en Linux).

!!! note "Idea clave"
    Desfragmentar un SSD no mejora su rendimiento (no tiene partes mecánicas que se beneficien de datos contiguos) y reduce innecesariamente su vida útil por escrituras adicionales. Los sistemas operativos actuales lo detectan y aplican TRIM en su lugar.

## Actividades

**Actividad 4.1 — Copia de seguridad automatizada**
{: .actividad-titulo}

Configura una tarea programada (crontab en Linux o Programador de tareas en Windows) que copie una carpeta de datos a otra ubicación cada noche. Indica si la copia configurada es completa, incremental o diferencial y justifica la elección.

**Actividad 4.2 — Permisos en Linux**
{: .actividad-titulo}

En una máquina virtual Linux, crea un script `practica.sh` y aplica con `chmod` los permisos `rwxr-xr--`. Comprueba con `ls -l` el resultado, cámbialos a notación octal equivalente y explica qué usuario podría ejecutarlo.

**Actividad 4.3 — Usuarios, grupos y herencia de permisos**
{: .actividad-titulo}

Crea dos usuarios y un grupo. Asigna una carpeta compartida con permisos distintos para el grupo y para "otros", y comprueba, iniciando sesión con cada usuario, que el resultado es el esperado.

**Actividad 4.4 — Diagnóstico mediante logs**
{: .actividad-titulo}

A partir de un fragmento de log proporcionado por el profesorado (Visor de eventos o `journalctl`), identifica qué servicio falló, a qué hora y plantea una hipótesis razonada sobre la causa.

**Actividad 4.5 — Windows vs. Linux por comandos**
{: .actividad-titulo}

Completa una tabla con 10 tareas de administración (listar procesos, comprobar espacio en disco, reiniciar un servicio...) indicando el comando equivalente en Windows y en Linux, y pruébalos en una máquina virtual de cada sistema.
