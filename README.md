# Chococrispy 🍫🥣

**Chococrispy** es una utilidad ligera para sistemas Linux diseñada para mitigar el **bufferbloat** y reducir las latencias altas (lag spikes) mediante la limitación y modelado inteligente del tráfico de red utilizando el algoritmo **CAKE** y la redirección de interfaces con `ifb`.

Al limitar ligeramente tu conexión al 90% (o el valor que configures) de tu ancho de banda nominal, evitas que los búferes del router se saturen durante descargas o subidas pesadas, manteniendo una conexión fluida y con baja latencia incluso bajo alta saturación.

---

## Características

* **Mitigación de Bufferbloat:** Aplica disciplinantes de cola avanzados (`cake`) para priorizar paquetes interactivos.
* **Control Simétrico:** Gestiona tanto el tráfico de subida (`upload`) como el de bajada (`download`) usando interfaces virtuales (`ifb`).
* **Fácil Configuración:** Parámetros clave centralizados en un único archivo de configuración.
* **Empaquetado Nativo:** Se distribuye como un paquete `.deb` estándar para una instalación limpia en distribuciones basadas en Debian/Ubuntu.

---

##  Requisitos

* Un sistema operativo Linux con soporte para `iproute2` (`tc`, `ip`).
* Una interfaz de red compatible (por defecto configurada para `eth0`).
* Permisos de superusuario (`root`).

---

## Estructura del Proyecto

```text
.
├── .gitignore
├── bin/
│   └── build                # Script para compilar el paquete .deb
├── src/
│   ├── DEBIAN/
│   │   └── control          # Metadatos del paquete Debian
│   ├── etc/
│   │   └── chococrispy/
│   │       └── cake.conf    # Configuración principal (interfaz, velocidad, latencia)
│   └── usr/
│       └── local/
│           └── sbin/
│               └── chococrispy  # Script principal de control de tráfico
└── usr/
    └── bin/                 # Directorio de salida para el archivo .deb compilado

```

---

## Instalación

### 1. Construir el paquete `.deb`

Dirígete a la raíz del repositorio y ejecuta el script de compilación:

```bash
./bin/build

```

Esto generará el archivo `chococrispy.deb` dentro de la carpeta `usr/bin/`.

### 2. Instalar el paquete

Puedes instalar el paquete generado usando `dpkg`:

```bash
sudo dpkg -i usr/bin/chococrispy.deb

```

Si surge algún problema de dependencias faltantes, corrígelo con:

```bash
sudo apt-get install -f

```

---

## Configuración

Antes de ejecutar el script, asegúrate de ajustar los parámetros de tu conexión en el archivo de configuración ubicado en `/etc/chococrispy/cake.conf`:

```bash
sudo nano /etc/chococrispy/cake.conf

```

Parámetros disponibles:

* `iface`: Tu interfaz de red principal (ej. `eth0`, `enp3s0`, etc.).
* `latency`: La latencia base estimada de tu conexión (ej. `50ms`).
* `download`: Límite de ancho de banda de bajada recomendado (sugerido: ~90% de tu velocidad real contratada, ej. `2500kbit`).
* `upload`: Límite de ancho de banda de subida recomendado (ej. `2500kbit`).

---

## Uso

Una vez instalado y configurado, puedes ejecutar el script directamente desde la terminal con privilegios de administrador:

```bash
sudo chococrispy

```

---


## Licencia

Este proyecto es software libre y está licenciado bajo la **GNU General Public License v3.0 (GPLv3)**. Consulta el archivo [LICENSE](LICENSE) para obtener más detalles.``
