## VI - Procesos

* Ejecutar la aplicación como uno o más procesos sin estado.
* Los procesos que componen una aplicación 12 factor **no tienen estado y no comparten nada**.
	* Cualquier información que deba persistirse debe utilizar algún backing service con estado (por ejemplo bases de datos, NFS, S3).
