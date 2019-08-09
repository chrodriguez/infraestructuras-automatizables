## Virtualización total

* Simula el HW permitiendo al SO guest correr de forma aislada sin ninguna modificación.
* Existen dos tipos (el guest no requiere alterarse):
	* **Asistidos por HW (basados en Intel VT-x/AMD-V):** utiliza el HW directamente aprovechando la tecnología de virtualización del procesador
	* **Asistidos por SW (utiliza Binary translation):** penalizados en performance por trapear y virtualizar la ejecución de conjuntos de instrucciones no virtualizables.


