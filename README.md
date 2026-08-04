# 🚀 HomeLab: Infrastructure Observability & Centralized DNS Stack

Un entorno completo de infraestructura local desplegado mediante **Docker Compose** en un servidor **Debian**. El proyecto combina el monitoreo y la telemetría en tiempo real con **Prometheus** y **Grafana**, junto con el filtrado y la gestión centralizada de tráfico DNS mediante **AdGuard Home**.

---

## 🛠️ Arquitectura de Red y Dispositivos

```text
                             +-------------------------------------------------+
                             |                VM Debian (Server)              |
                             |  +--------------+  +------------+  +---------+  |
                             |  | AdGuard Home |  | Prometheus |  | Grafana |  |
                             |  +--------------+  +------------+  +---------+  |
                             +-----------------------^-------------------------+
                                                     |
             +--------------------+------------------+-------------------+--------------------+
             |                    |                                      |                    |
             v                    v                                      v                    v
 +-----------------------+ +-----------------------+          +-----------------------+ +-----------------------+
 | Notebook (Linux Mint) | | PC Principal (Win 11) |          | 2x Smartphones (Android)| | Smart TV              |
 | Node Exporter + DNS   | | Win Exporter + DNS    |          | DNS Filtering (Wi-Fi) | | DNS Filtering (Wi-Fi) |
 +-----------------------+ +-----------------------+          +-----------------------+ +-----------------------+
```


| Dispositivo / Cliente | Sistema Operativo | Rol en la Red | Método de Integración |
| :--- | :--- | :--- | :--- |
| **Servidor Central** | Debian (VM) | Host de Docker Compose (Grafana, Prometheus, AdGuard, Node Exporter) | Docker Bridge Network |
| **PC Principal** | Windows 11 | Telemetría de Hardware + Filtrado DNS | `windows_exporter` (Puerto 9182) |
| **Notebook** | Linux Mint | Telemetría de Hardware + Filtrado DNS | `node_exporter` (Puerto 9100) + `systemd-resolved` |
| **2x Smartphones** | Android | Bloqueo de publicidad en apps / navegación | Servidor DNS estático en Wi-Fi |
| **Smart TV** | Propietario | Bloqueo de rastreadores y telemetría | Servidor DNS estático en Wi-Fi |


⚙️ Componentes del Stack
AdGuard Home: Servidor DNS primario (53 UDP/TCP, Panel en 3005). Filtra anuncios, rastreadores y permite reescrituras DNS locales.

Prometheus: Base de datos de series temporales (9090) recolectando métricas de todos los nodos de la red.

Grafana: Panel unificado de visualización e infraestructura (3000).

Exporters (node_exporter & windows_exporter): Agentes ligeros para extraer métricas de CPU, RAM, red y disco en cada SO.


📁 Estructura del Repositorio

```text
.
├── docker-compose.yml       # Definición de servicios (AdGuard, Grafana, Prometheus, Node Exporter)
├── prometheus/
│   └── prometheus.yml       # Configuración de scrape jobs y targets
├── .gitignore               # Exclusión de configuraciones sensibles
└── README.md                # Documentación del proyecto
```

🚀 Despliegue Rápido
Requisitos
Docker Engine y Docker Compose V2.
Conexión a la red local.
