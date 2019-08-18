### III - Configuración

* La configuración de una aplicación es lo único que puede variar entre despliegues de una misma versión en diferentes ambientes:
	* Bases de datos, cachés u otros backing services.
	* Credenciales para servicios externos tales como Amazon S3, APIs, etc
	* Valores propios del despliegue.
* Guardar constantes de configuración en el código **viola 12 factor**.
	* _La configuración varía entre despliegues, no el código_.
* Se propone utilizar **variables de entorno**.
