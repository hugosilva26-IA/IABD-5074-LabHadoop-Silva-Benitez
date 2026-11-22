# Despliegue y auditoría de un clúster Apache Hadoop con Docker

Laboratorio práctico del Máster de IA y Big Data para desplegar un clúster Hadoop, comprobar el almacenamiento distribuido en HDFS, simular fallos y ejecutar trabajos MapReduce mediante YARN.

## 1. Arquitectura del clúster

El proyecto utiliza Apache Hadoop 3 mediante Docker Compose. El entorno está formado por 6 contenedores conectados a través de una red interna llamada `hadoop-net`.

| Contenedor | Función | Puerto |
|---|---|---|
| namenode | Gestiona HDFS | 9870 |
| datanode1 | Almacenamiento HDFS | Interno |
| datanode2 | Almacenamiento HDFS | Interno |
| datanode3 | Almacenamiento HDFS | Interno |
| resourcemanager | Gestiona los recursos de YARN | 8088 |
| nodemanager | Ejecuta las tareas de YARN | Interno |

## 2. Configuración principal

- Imagen utilizada: `apache/hadoop:3`.
- Sistema de archivos: HDFS.
- Tamaño de bloque: 64 MiB (67.108.864 bytes).
- Factor de replicación: 2.
- Memoria del NodeManager: 2048 MB.
- CPU del NodeManager: 2 vCores.
- Red Docker: `hadoop-net`.

## 3. Requisitos previos

- Docker Desktop instalado y funcionando.
- Docker Compose.
- Git.
- Al menos 6 GB de memoria RAM disponible.

## 4. Puesta en marcha

Desde la carpeta raíz del proyecto, entra en `docker`:

```bash
cd docker
```

Comprueba la configuración y levanta los contenedores:

```bash
docker compose config
docker compose up -d
```

Comprueba que los seis contenedores están funcionando:

```bash
docker compose ps
```

## 5. Interfaces web

Una vez iniciado el clúster, se puede acceder a las siguientes interfaces desde el navegador:

- **NameNode:** http://localhost:9870
- **YARN ResourceManager:** http://localhost:8088

## 6. Comprobaciones realizadas

Durante el laboratorio se han realizado las siguientes pruebas:

1. Despliegue del clúster Hadoop con Docker Compose.
2. Comprobación de los DataNodes registrados en HDFS.
3. Ingesta de un fichero de 150 MiB y auditoría de sus bloques mediante `hdfs fsck`.
4. Comprobación de la replicación de los bloques.
5. Simulación de la caída de `datanode2` y lectura del fichero desde HDFS.
6. Ejecución de WordCount mediante MapReduce sobre YARN.

## 7. Detener el entorno

Desde la carpeta `docker`, ejecuta:

```bash
docker compose down
```

Para eliminar también los volúmenes y los datos almacenados:

```bash
docker compose down -v
```

**Importante:** el segundo comando elimina los datos persistentes del clúster.

## 8. Estructura del repositorio

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
```

## 9. Autoría

Trabajo realizado por Hugo Silva Benítez para el laboratorio de Apache Hadoop del Máster de IA y Big Data.
