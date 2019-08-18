### V - Construir, distribuir, ejecutar

* Separar siempre la etapa de construcción de ejecución.
* Los fuentes se transforman en un despliegue al completarse las etapas de:
	* **Construcción:** compilación del código en binarios ejecutables o intermedios.
		* _No aplica a lenguajes interpretados. **Sí para docker**_.
	* **Distribución:** preparado de un paquete listo para ser desplegado.
	* **Ejecución:** etapa que se instancia luego del despliegue en un ambiente.
