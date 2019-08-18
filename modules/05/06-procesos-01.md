## VI - Procesos

* Los compresores de assets, se recomiendan que se apliquen en la **fase de construcción** en vez del momento de ejecución.
	* Esto responde algunas cuestiones acerca de cómo armar un `Dockerfile`.
* El uso de sticky sessions asumen que alguna aplicación guarda datos que no son sin estado, por ejemplo en memoria. **Esto viola 12 factor app**.
	* Tratar de usar caches como Memcached o Redis.
