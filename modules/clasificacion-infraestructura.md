## Clasificación de la Infraestructura

---

## Tipo de Servidores

* Servidores físicos
	* Problemas
* Virtualización
	* Tipos de virtualización
	* Servidores virtuales
	* Hypervisores
	* Virtualización total
	* Paravirtualización
	* Virtualización a nivel del SO

---

## Servidores físicos

* Servidor dedicado
* No pueden compartirse por diferentes clientes
	* Toda la carga que realiza será para un único cliente
	* Puede utilizarse por varios usuarios simultáneamente, pero todos de un mismo cliente
* Controlables y monitorizables a través de [IPMI](https://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface)
	* Monitoreo de temperatura, voltage, ventilación, fuentes, chasis, etc
	* Acceso a log de eventos / Ciclo de apagado, encendido, reincio / Acceso al BIOS / Instalación remota

---

### Servidores físicos: problemas

* Subejecución de recursos. Se considera que menos del 20% de la capacidad del servidor puede utilizarse por un servicio estándar.
* Un único sistema operativo por servidor.
* Un mismo servidor con diferentes servicios podría aprovechar mejor los recursos.
	* Compromiso de seguridad.
	* Alta cohesión.
* Replicar uno o todos los servicios para realizar pruebas requiere otro servidor físico o virtual.

---

## Virtualización

* La virtualización es una abstracción de recursos computacionales.
	* No únicamente de servidores.
* Maximiza el uso del recursos físicos, minimizando la cantidad de ellos.
* Simplifica el aprovisionamiento.
* Proveen APIs que simplifican tareas de automatización habituales en un datacenter.

---

## Tipos de virtualización

* **Infraestructura:**
	* Red: VPN, VLAN, virtualización de routers, forewalls, segmentos, direccionamiento IP, etc.
	* Storage: LVM, RAID, NAS (NFS/CIFS), SAN (iSCSI)
* **Servidores:** uno o más máquinas virtuales en un mismo servidor físico.
	* Virtualización total
	* Paravirtualización
	* Virtualización del SO

---

## Tipos de virtualización

* **Aplicaciones:** aplica a conceptos muy diferentes entre sí
	* Thinclients: Citrix, Terminal Services
	* Wine
	* Lenguajes de alto nivel como por ejemplo JVM y CLR

---

## Servidores virtuales

* Abstracción del hardware físico y dispositivos
* Múltiples sistemas operativos y aplicaciones en un mismo servidor físico.
* Maximiza el uso del servidor físico, minimizando la cantidad de ellos.
* Simplifica el aprovisionamiento.
* Según la herramienta de virtualización:
	* Proveen APIs que simplifican tareas de automatización habituales en un datacenter.
	* Manejo de pool de recursos dinámico.

---

## Servidores virtuales

![virtualizacion](./images/virtualizacion.png)

<small>
Concepto de virtualización de un servidor
</small>

---

## Servidores virtuales

El protagonista en este tipo de virtualización es el **hypervisor**. 

Un hypervisor es un programa responsable de administrar múltiples SOs (o múltiples instancias del mismos SO) en un único sistema de cómputo.

Es responsable de gestionar el procesador, memoria y otros recursos del sistema
según la demanda que cada OS _guest_ requiera.

---

## Hypervisores

* **Bare Metal Hypervisors o de Tipo I:** se instalan directamente sobre un servidor físico. Es un SO pequeño que controla el HW, la asignación de recursos y monitoriza los guests. Los guests corren sobre una _**segunda**_ capa por sobre el HW.
* **Hosted Hypervisors o de Tipo II:** se instalan en un SO existente como una aplicación más. Los guests corren en una _**tercer**_ capa por sobre el HW.

---

## Servidores virtuales

![clasificacion de server virtuales](./images/server-virtualization-clasification.png)

<small>
Clasificaciones del tipo de virtualización de un servidor
</small>

---

## Virtualización total

* Simula el HW permitiendo al SO guest correr de forma aislada sin ninguna modificación.
* Existen dos tipos (el guest no requiere alterarse):
	* **Asistidos por HW (basados en Intel VT-x/AMD-V):** utiliza el HW directamente aprovechando la tecnología de virtualización del procesador
	* **Asistidos por SW (utiliza Binary translation):** penalizados en performance por trapear y virtualizar la ejecución de conjuntos de instrucciones no virtualizables.

---

### Productos de virtualización total

* **Asistidos por SW:** VMWare Workstation (32bits guests) / Microsoft Virtual PC / Oracle VirtualBox (32-bit guests)
* **Asistidos por HW:**
	* **Hypervisor de Tipo I:** VMWare ESXi/ESX / KVM / Microsoft Hyper-V / Citrix XenServer
	* **Hypervisor de Tipo II:** VMWare Workstation (64 bits guests only) / Oracle VirtualBox (64 bits guests only)

---

## Paravirtualización

* No necesita simular el HW para las máquinas virtuales: el Hypervisor se instala en un servidor físico, llamado _host_.
* Los guests virtuales _**tienen conocimiento**_ de encontrarse virtualizados a _**diferencia**_ de la virtualización total.
	* El SO guest requiere ser modificado para comunicarse con el _host_ y realizar llamadas al hypervisor como si fuesen system calls.
* Ejemplos: Citrix XenServer / IBM LPAR (usado por algunos mainframes y Linux zSeries), Oracle VM for Sparc (LDOM)

---

## Virtualización a nivel del SO

* También conocida como _**contenerización**_
* El SO instalado en el _host_ permite manejar múltiples espacios de usuario, procesos y recursos.
* Existe un pequeño o inexistente overhead por utilizar el kernel del SO anfitrión para su ejecución.
* _**Ejemplos:** Oracle Solaris Zones / Docker / Linux LXC_

