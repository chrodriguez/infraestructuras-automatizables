## Paravirtualización

* No necesita simular el HW para las máquinas virtuales: el Hypervisor se instala en un servidor físico, llamado _host_.
* Los guests virtuales _**tienen conocimiento**_ de encontrarse virtualizados a _**diferencia**_ de la virtualización total.
	* El SO guest requiere ser modificado para comunicarse con el _host_ y realizar llamadas al hypervisor como si fuesen system calls.
* Ejemplos: Citrix XenServer / IBM LPAR (usado por algunos mainframes y Linux zSeries), Oracle VM for Sparc (LDOM)
