### X - Aproximación entre dev y ops

* Mantener todos los ambientes lo más similares que sea posible.
* Evitar las diferencias de ambientes considerando:
	* **Diferencias de tiempo:** evitar que el desarrollo de un feature no se suba al repositorio de fuentes en períodos largos de tiempo.
	* **Diferencias de personal:**el despliegue lo hacen operadores y lo desarrollan programadores. Unificar roles.
	* **Diferencias de herramientas:** producción usa Postgres y desarrollo SQLite.
