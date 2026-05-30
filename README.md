# Reconocimiento de Red y Detección de Servicios con Nmap

## Descripción del Proyecto
Este repositorio contiene la documentación detallada de un laboratorio práctico de reconocimiento perimetral y auditoría de infraestructura de red utilizando **Nmap** en un entorno controlado con **Kali Linux**. El objetivo principal fue mapear los hosts activos dentro del segmento local, descubrir puertos abiertos, enumerar versiones de servicios en ejecución e identificar vulnerabilidades potenciales o configuraciones inseguras en los dispositivos activos.

---

## Metodología y Comandos Ejecutados

El proceso de auditoría técnica se estructuró en dos fases principales mediante la interfaz de línea de comandos (CLI):

### Fase 1: Descubrimiento de Hosts Activos (Ping Sweep)
Para identificar qué dispositivos se encontraban encendidos en el segmento de red sin realizar un escaneo invasivo de puertos, se ejecutó un barrido ICMP utilizando el comando:

```bash
nmap -sn 192.168.100.14/24
````

![Barrido de Red con Nmap](https://github.com/jmonge12/Network-Reconnaissance-Nmap/blob/8c3490a2b6ca61a2e3af189faa8b799651ba9104/barrido%20de%20red%20con%20nmap.png)


Hallazgos: El comando detectó 3 hosts activos en la red:

192.168.100.1 (Puerta de enlace / Router principal Huawei Technologies)

192.168.100.3 (Dispositivo final Micro-Star Intl)

192.168.100.14 (Host local / Máquina virtual de Kali Linux

---

### Fase 2: Escaneo Exhaustivo y Detección de Versiones (Stealth Scan)
Se seleccionó como objetivo la dirección IP del router principal (`192.168.100.1`) para realizar un análisis profundo utilizando el motor de scripts de Nmap (NSE) y la detección de sistemas operativos:

```bash
sudo nmap -sS -sV -sC -O -v 192.168.100.1
````

![Resultados del Escaneo de Puertos](https://github.com/jmonge12/Network-Reconnaissance-Nmap/blob/89fb60d23b30f1c0f50415c2ba8a18694a925d1a/resultados%20del%20escaneo%20de%20puertos.png)

El escaneo logró extraer mucha información de la interfaz web del router de forma pública:

- Detectó que el fabricante real es Huawei Technologies.

- Extrajo los datos internos del certificado SSL (válido hasta 2027).

- Permitió a Nmap adivinar con un 99% de precisión que el sistema operativo interno es Linux.

Estos comando sirven para:
````
-sS: Escaneo TCP SYN sigiloso.

-sV: Detección activa de versiones en los servicios identificados.

-sC: Ejecución de scripts predeterminados para enumerar fallas de seguridad y certificados.

-O: Intento de identificación del Sistema Operativo.
````
---

### Información de Infraestructura Recolectada:
Dirección MAC: 78:57:73:54:02:D0 (Huawei Technologies)

Predicción del Sistema Operativo: Núcleo Linux (versiones estimadas entre Linux 2.6.32 - 3.13 o superior con un 99% de precisión).

Distancia de Red: 1 salto de red (Hop).

---

### Recomendaciones de Mitigación (Blue Team / Defensa)
Desactivar UPnP: Se recomienda deshabilitar el servicio UPnP en el puerto 49152 si no es estrictamente necesario, disminuyendo la superficie de ataque frente a inyecciones de red o apertura automática de puertos maliciosos.

Ocultamiento de Banners: Restringir la cantidad de metadatos expuestos en el puerto de la interfaz web (80) para evitar que atacantes externos automaticen búsquedas de exploits orientados a hardware específico de Huawei o Linksys.

