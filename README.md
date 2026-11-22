# Despliegue y Auditoría de Clúster Multi-Nodo Apache Hadoop con Docker

Laboratorio práctico de despliegue de infraestructura como código (IaC), auditoría física de bloques en HDFS, simulación de tolerancia a fallos y ejecución distribuida MapReduce sobre YARN.

---

## Ficha Resumen de la Arquitectura

El entorno despliega un clúster distribuido de **Apache Hadoop 3** (`apache/hadoop:3`) compuesto por **6 contenedores** interconectados mediante una red puente aislada (`hadoop-net`).

| Servicio / Contenedor | Rol en el Clúster | Imagen Docker | Puertos Expuestos |
| :--- | :--- | :--- | :--- |
| **`namenode`** | Maestro HDFS (gestión de metadatos y espacio de nombres) | `apache/hadoop:3` | `9870:9870` (Web UI) |
| **`datanode1`** | Nodo trabajador de almacenamiento HDFS 1 | `apache/hadoop:3` | Interno (`9864` / `9866`) |
| **`datanode2`** | Nodo trabajador de almacenamiento HDFS 2 | `apache/hadoop:3` | Interno (`9864` / `9866`) |
| **`datanode3`** | Nodo trabajador de almacenamiento HDFS 3 | `apache/hadoop:3` | Interno (`9864` / `9866`) |
| **`resourcemanager`** | Maestro YARN (planificación y arbitraje de recursos) | `apache/hadoop:3` | `8088:8088` (Web UI) |
| **`nodemanager`** | Nodo trabajador YARN (ejecución de contenedores de cómputo) | `apache/hadoop:3` | Interno (`8042`) |

### Parámetros Principales de Configuración (`docker/config`)
* **Sistema de ficheros por defecto (`fs.defaultFS`):** `hdfs://namenode:8020`
* **Tamaño de bloque HDFS (`dfs.blocksize`):** `64 MiB` (`67108864 bytes`)
* **Factor de replicación (`dfs.replication`):** `2`
* **Recursos asignados al `NodeManager`:** `2048 MB` de memoria RAM y `2 vCores`

---

## Estructura del Repositorio

```text
IABD-5074-LabHadoop-Silva-Benitez/
├── .gitignore
├── README.md
├── docker/
│   ├── config
│   └── docker-compose.yml
├── evidencias/
│   ├── 01_namenode_ui_9870.png
│   ├── 02_hdfs_fsck_bloques.png
│   ├── 03_simulacion_fallo_datanode2.png
│   └── 04_yarn_wordcount_8088.png
└── informe/
    └── Silva_Benitez_InformeLabHadoop.pdf