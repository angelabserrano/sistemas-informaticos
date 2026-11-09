# Guía: configuración de máquinas virtuales en VirtualBox

[:material-arrow-left: Volver al índice de todas las unidades](index.md)

Varias actividades del módulo (especialmente en la UT2) piden configurar y conectar varias **máquinas virtuales (VM)** entre sí: para calcular y comprobar direccionamiento IP, para verificar que dos subredes no se ven entre sí... Para estas primeras prácticas partimos de una máquina virtual **ya preparada**, que el profesorado proporciona como archivo **OVA**, en lugar de instalar un sistema operativo desde cero (eso lo veremos más adelante en el curso).

Esta guía explica cómo importar esa OVA en **VirtualBox** (versión 7.2, la instalada en los talleres), ajustar sus parámetros y, sobre todo, cómo configurar correctamente su **red**, que es la parte que más confusión suele generar la primera vez.

!!! tip "¿Nunca has usado VirtualBox?"
    No hace falta memorizar nada de esta página: tenla abierta al lado mientras importas tu primera máquina virtual y sigue los pasos en orden.

## 1. Importar la máquina virtual (archivo OVA)

1. Pide al profesorado el archivo **.ova** correspondiente a la actividad y guárdalo en tu equipo.
2. Abre VirtualBox y ve a **Archivo > Importar servicio virtualizado** (*File > Import Appliance*).
3. Selecciona el archivo `.ova` que te han proporcionado y pulsa **Siguiente**.
4. VirtualBox mostrará un resumen de la configuración de la máquina (nombre, RAM, CPU, disco...). Puedes cambiar aquí mismo el **nombre** de la VM (por ejemplo, `UT2-PC1`) para identificarla fácilmente si vas a importar varias.
5. Pulsa **Importar** y espera a que termine el proceso.

## 2. Ajustar los parámetros de la máquina importada

Con la VM ya importada (todavía apagada), selecciónala en la lista y entra en **Configuración**:

| Sección | Qué ajustar |
| --- | --- |
| Sistema > Procesador | Número de núcleos virtuales asignados (1-2 suele bastar para las prácticas) |
| Sistema > Placa base | Memoria RAM asignada; no superes la mitad de la RAM física del equipo del taller, para no afectar a otras VMs que tengas abiertas a la vez |
| Pantalla > Memoria de vídeo | El valor por defecto suele ser suficiente |

!!! note "¿Y si necesito más adelante instalar un sistema operativo desde cero?"
    Esa parte se verá más adelante en el curso, con el asistente **Nueva** de VirtualBox. Por ahora, basta con importar la OVA que indique el profesorado.

## 3. Configuración de red

Esta es la parte **clave** para las actividades de la UT2. Se configura en **Configuración > Red**, pestaña **Adaptador 1**, desplegable **Conectado a:**.

| Modo | ¿Salida a Internet? | ¿Las VMs se ven entre sí? | ¿Visible desde la red física del aula? |
| --- | --- | --- | --- |
| **NAT** | Sí | No (cada VM está aislada tras su propio NAT) | No |
| **Adaptador puente** (*Bridged*) | Sí | Sí, como si fueran equipos más de la red del aula | Sí |
| **Red interna** (*Internal Network*) | No | Sí, entre las VMs que compartan el mismo nombre de red interna | No |
| **Red solo anfitrión** (*Host-only*) | No | Sí, entre ellas y con el equipo físico (host) | No |
| **Red NAT** (*NAT Network*) | Sí | Sí, entre las VMs de la misma red NAT | No |

**Cómo conectar varias VMs entre sí con Red interna:** en cada una de las VMs que quieras que se vean, entra en Configuración > Red > Adaptador 1 > Conectado a: **Red interna**, y escribe **el mismo nombre** en el campo "Nombre" (por ejemplo, `ut2-lab`). Solo las VMs que compartan exactamente ese nombre forman parte de la misma red interna.

### Qué adaptador usar en cada actividad

| Actividad | Adaptador recomendado | Motivo |
| --- | --- | --- |
| 2.3 — Direccionamiento IP | Red interna | Solo necesitas que la VM tenga una IP configurada y puedas hacer `ping` a otro equipo de la misma red; no hace falta salida a Internet. |
| 2.4 — Subnetting | Red interna | Igual que la anterior; además te permite comprobar que dos VMs en subredes distintas **no** se alcanzan entre sí, sin riesgo de interferir con la red real. |

Si en algún momento necesitas que la VM también tenga salida a Internet (por ejemplo, para actualizar paquetes del sistema), puedes añadir un **segundo adaptador de red** (pestaña **Adaptador 2**) en modo **NAT**, y dejar el Adaptador 1 en Red interna para las pruebas de red del módulo.

## 4. Instantáneas (snapshots)

Antes de hacer un cambio que pueda romper la configuración de red o del sistema (por ejemplo, antes de cambiar la configuración de red), toma una **instantánea**: botón derecho sobre la VM > **Instantánea** > **Tomar**. Si algo falla, puedes **restaurar** esa instantánea y volver al estado anterior sin tener que importar la OVA de nuevo.

## 5. Resumen rápido

1. Archivo > Importar servicio virtualizado → selecciona la OVA → ajusta el nombre si vas a importar varias → Importar.
2. Revisa en Configuración la RAM y CPU asignadas.
3. Configuración > Red > Adaptador 1 > **Red interna** (mismo nombre en todas las VMs del ejercicio).
4. Toma una instantánea antes de cambios importantes.
5. Arranca la VM: el sistema operativo ya está instalado, listo para trabajar.

## 6. Configurar la red en Ubuntu: DHCP o IP manual {#red-ubuntu}

Hasta ahora hemos decidido en VirtualBox **a qué red se conecta** la tarjeta de la VM. Falta decidir, **dentro de Ubuntu**, qué dirección IP usa esa tarjeta: una que le asigne un servidor **DHCP** (automática) o una que escribimos nosotros a mano (**estática**).

### 6.1. Identificar la tarjeta de red

Abre una terminal (<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>T</kbd>) y ejecuta:

```bash
ip -br a
```

Verás algo parecido a esto:

```text
lo        UNKNOWN   127.0.0.1/8 ::1/128
enp0s3    UP        192.168.10.10/24 fe80::a00:27ff:fe4e:66a1/64
```

- `lo` es la interfaz *loopback* (el propio equipo); ignórala.
- `enp0s3` es la tarjeta correspondiente al **Adaptador 1** de VirtualBox. Si has activado un Adaptador 2, aparecerá como `enp0s8`.

!!! note "En esta guía usamos `enp0s3`"
    Es el nombre habitual en VirtualBox, pero compruébalo con `ip -br a`: si tu tarjeta se llama distinto, sustitúyelo en todos los comandos.

### 6.2. Consultar la configuración actual

| Qué quieres saber | Comando |
| --- | --- |
| IP y máscara | `ip a show enp0s3` |
| Puerta de enlace | `ip route` (línea que empieza por `default via ...`) |
| Servidor DNS | `resolvectl status enp0s3` |
| Todo junto | `nmcli device show enp0s3` |
| Datos de la concesión DHCP (servidor, duración...) | `nmcli -f DHCP4 device show enp0s3` |

!!! tip "¿Cómo sé si la IP la ha dado un DHCP?"
    En la salida de `ip a show enp0s3`, una dirección obtenida por DHCP aparece con la palabra **`dynamic`** y un tiempo `valid_lft` que va disminuyendo. Una IP estática no tiene `dynamic` y su `valid_lft` es `forever`.

    Si ves una dirección `169.254.x.x` o ninguna dirección IPv4, el equipo **no ha encontrado ningún servidor DHCP**. En una **Red interna** esto es lo normal, porque en ella no hay ningún servidor DHCP: tendrás que asignar la IP de forma **manual**, como se explica a continuación.

### 6.3. Cambiar entre DHCP e IP estática

En Ubuntu Desktop la red la gestiona **NetworkManager**, y puedes cambiarla de dos formas equivalentes: desde el entorno gráfico (la más sencilla si es tu primera vez) o desde la terminal con `nmcli`. Elige la pestaña que prefieras.

=== "Entorno gráfico"

    1. Abre **Configuración** (*Settings*) > **Red** (*Network*).
    2. En el apartado **Cableada** (*Wired*), pulsa el icono del **engranaje** :material-cog: de la conexión.
    3. Ve a la pestaña **IPv4**.
    4. Elige el **Método IPv4**:
        - **Automático (DHCP)**: no hay que rellenar nada más.
        - **Manual**: rellena los campos:

            | Campo | Ejemplo (Actividad 2.3, red `192.168.10.0/24`) |
            | --- | --- |
            | Dirección | `192.168.10.10` (en la otra VM, por ejemplo `192.168.10.11`) |
            | Máscara de red | `255.255.255.0` (o `24`) |
            | Puerta de enlace | Déjala **vacía** (ver nota al final de este apartado) |
            | DNS | Déjalo vacío; no hace falta para hacer `ping` a una IP |

    5. Pulsa **Aplicar**.
    6. **Importante:** los cambios no se aplican hasta reconectar. Desactiva el interruptor de la conexión cableada y vuelve a activarlo.
    7. Comprueba el resultado en una terminal con `ip a show enp0s3` e `ip route`.

=== "Terminal con nmcli"

    Hace lo mismo que el entorno gráfico, pero desde la terminal.

    1. Averigua el **nombre de la conexión** (columna `NAME`), que suele ser `Conexión cableada 1`, `Wired connection 1` o `netplan-enp0s3`:

        ```bash
        nmcli connection show
        ```

    2. **Pasar a IP estática** (sustituye el nombre de la conexión por el tuyo, entre comillas):

        ```bash
        sudo nmcli connection modify "Conexión cableada 1" ipv4.method manual ipv4.addresses 192.168.10.10/24
        ```

    3. **Volver a DHCP** (automática), borrando los valores fijos:

        ```bash
        sudo nmcli connection modify "Conexión cableada 1" ipv4.method auto ipv4.addresses "" ipv4.gateway "" ipv4.dns ""
        ```

    4. Tras cualquiera de los dos cambios, **reactiva la conexión** para que se aplique:

        ```bash
        sudo nmcli connection up "Conexión cableada 1"
        ```

!!! note "¿Y la puerta de enlace en una Red interna?"
    La puerta de enlace solo hace falta para llegar a **otras redes** (por ejemplo, Internet) a través de un router. En la Red interna `ut2-lab` no hay ningún router y las VMs están en la misma red, así que se comunican directamente: puedes dejarla vacía. Además, si el equipo tiene un Adaptador 2 en modo NAT para salir a Internet, rellenarla en `enp0s3` "pisaría" la ruta por defecto del adaptador NAT y te quedarías sin Internet.

## 7. Problemas frecuentes {#problemas-red}

| Síntoma | Causa probable | Solución |
| --- | --- | --- |
| La VM tiene una IP `169.254.x.x` o ninguna IPv4 | En la Red interna no hay servidor DHCP | Es lo esperado: asigna la IP de forma manual (ver [apartado 6.3](#red-ubuntu)). |
| He cambiado la configuración en el entorno gráfico pero la IP sigue igual | La conexión no se ha reactivado | Desactiva y vuelve a activar la conexión cableada, o ejecuta `sudo nmcli connection up "<nombre de la conexión>"`. |
| `ping` a la otra VM no responde (*Destination Host Unreachable*) | Las VMs no están en la misma Red interna | En VirtualBox, comprueba que ambas tienen el Adaptador 1 en **Red interna** con **exactamente el mismo nombre** (`ut2-lab`; distingue mayúsculas y minúsculas). |
| `ping` a la otra VM no responde y la Red interna es correcta | IPs en redes distintas, o error al teclear la máscara | Comprueba con `ip a` en las dos VMs que las IPs pertenecen a la misma red (mismo prefijo y misma máscara, p. ej. `/24`) y que no son iguales. |
| Las dos VMs tienen la misma IP | Se ha copiado la misma dirección en ambas | Cada equipo necesita una IP **distinta** dentro de la red (p. ej. `.10` y `.11`). |
| Dos VMs importadas de la misma OVA tienen la misma MAC | Al importar se conservaron las direcciones MAC | Con la VM apagada: Configuración > Red > Adaptador 1 > Avanzadas > botón de **regenerar la dirección MAC**. |

## Apéndice: crear una máquina virtual desde cero

*Esto no hace falta para las primeras actividades, en las que se parte de una OVA. Lo veremos más adelante en el curso, cuando toque instalar un sistema operativo desde cero.*

1. Abre VirtualBox y pulsa **Nueva**.
2. En la primera pantalla del asistente, rellena:
    - **VM Name**: un nombre descriptivo (por ejemplo, `UT2-PC1`).
    - **VM Folder**: la carpeta donde se guardará (el valor por defecto suele servir).
    - **ISO Image**: selecciona la imagen ISO del sistema operativo que vas a instalar, si ya la tienes descargada. Si la dejas en "no seleccionado", podrás instalar el sistema manualmente más adelante.
    - **OS** y **OS Version**: el sistema operativo y la versión concreta (VirtualBox suele detectarlos solo a partir del nombre de la ISO).

    ![Asistente "New Virtual Machine" de VirtualBox 7.2, con los campos VM Name, VM Folder, ISO Image, OS y OS Version](img/virtualbox_nueva_vm.png){width=500}

    !!! warning "No marques \"Proceed with Unattended Installation\""
        Esa opción instala el sistema operativo automáticamente, sin que veas ni controles los pasos de la instalación. Para las prácticas del módulo **debe quedar desmarcada**: la instalación se hace manualmente, paso a paso.

3. En las pantallas siguientes del asistente, asigna la **memoria RAM** y crea un **disco duro virtual** nuevo (tipo **VDI**, reservado **dinámicamente**, 20-25 GB suele ser suficiente).
4. Monta la imagen **ISO** de instalación en Configuración > Almacenamiento, y arranca la VM para instalar el sistema operativo paso a paso.
