## Ejemplo de playbook

En esta guía mostraremos como utilizar `ansible-playbook` integrado con
**vagrant** a través de la integración mediante el _aprovisionamiento de ansible_.

Para el ejemplo, se incluyen dos máquinas dentro del `Vagrantfile` basadas en
dos distribuciones esencialmente diferentes:

* Ubuntu
* CentOS

Decimos que son diferentes, y las elegimos exclusivamente para mostrar una forma
de escribir un playbook que sea agnóstico a la distribución.
Ubuntu, en su distribución llama al paquete que instala el web server apache,
como **apache2**, mientras que CentOS lo llama **httpd**.

### El playbook

Pude analizarse el playbook en este mismo directorio, considerando del mismo los
siguientes aspectos:

* **hosts:** indica el conjunto de equipos del inventario en los que aplica este
  playbook
* **tasks:** lista de tareas que el playbook debe ejecutar de forma idempotente,
  en el orden en que se especifica. Notar que algunas se ejecutan sólo si se
  cumplen condiciones explicitadas con `when:`. Además, las tareas pueden
  notificar _handlers_ usando `notify:`.
* **handlers:** lista de tareas que se ejecutan cuando alguna tarea se ejecuta.
  Los handlers además de poderse notificar por tareas, pueden suscribirse a
  tareas usando `listen:`

### Aprovisionando con vagrant

Una vez iniciado el ambiente con `vagrant up` ansible correrá. En caso de querer
correr nuevamente todo, es posible realizarlo de las dos siguientes formas:

```
vagrant provision
```

o usando directamente  `ansible-playbook` de la siguiente forma:

```
ANSIBLE_HOST_KEY_CHECKING=false ansible-playbook \
  -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory playbook.yml -u vagrant
```

