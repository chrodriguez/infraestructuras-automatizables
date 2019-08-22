## Escalabilidad

---

## Escalabilidad

* ¿Qué significa?
* Tipos de escalamiento
* ¿Cualquier aplicación escala?
* Conceptos relacionados con escalabilidad

---

## ¿Qué significa?

El escalamiento nos permite ajustar la capacidad de un recurso cambiando su
escala.

Se escala para crecer o decrecer, según sea la necesidad del recurso en
cuestión.

**Pareciera que la única necesidad es la de crecer.**

---

## Tipos de escalamiento

* **Escalamiento vertical:** consiste en aumentar los recursos. Paradójicamente, no escala en grandes magnitudes.
* [Escalamiento en ejes de un cubo:](https://microservices.io/articles/scalecube.html)
	* **X-scaling:** replica una aplicación detrás de un load balancer.
	* **Y-scaling:** es una aplicación que implementa microservicios.
	* **Z-scaling:** particionamiento de datos, sharding. Muy usado en bases de datos o ejemplo de dovecot / imap proxy

---

### ¿Cualquier aplicación escala?

* No
* En lenguajes como Java debe tenerse particular cuidado con patrones como Singleton.
* Hay que considerar los recursos utilizados por la aplicación:
	* Sesiones
	* Logs
	* Bases de datos
	* Assets / Uploads

---

## Conceptos relacionados

* Instancias descartables
* Healthchecks y auto-healing
* Caching

