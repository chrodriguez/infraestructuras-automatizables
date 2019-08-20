## Ejemplo de gestión remota de múltiples nodos

Este directorio permite mostrar cómo ansible permite gestionar remotamente
múltiples nodos.

Para poder verificar esta funcionalidad, el presente directorio provee un
archivo `Vagrantfile` que inicia seis nodos. No usaremos el aprovisionamiento
usando ansible que comunmente realiza algunas tareas que aquí realizaremos
manualmente, porque la idea de esta guía es justamente experimentar con las
configuraciones necesarias y comandos de ansible.

### Iniciando el laboratorio

Corriendo el siguiente comando, iniciaremos seis nodos:

```
vagrant up
```

Al finalizar, podremos visualizar el archivo de configuración de ansible,
también presente en este directorio, realizando `cat ansible.cfg`

```ini
[defaults]

deprecation_warnings=False
host_key_checking=False
inventory      = ./ansible/hosts
```

Como podemos ver, el archivo únicamente indica el directorio de inventario y dos
opciones que simplifica al interacción esperada para este laboratorio.

El archivo `./ansible/hosts` puede analizarse para entender su sintaxis, la
misma es explicada en la [siguiente documentación de
ansible](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

Este archivo de inventario indica los nodos que vagrant inicia, y los agrupa en
dos grupos:

* pares
* impares

Este archivo de inventario es posible generarlo dinámicamente a partir de un
vagrant con más o menos nodos, usando el siguiente script, dentro del proyecto
vagrant, y con las máquinas virtuales corriendo:

```
bash utils/vagrant-ssh-config-to-ansible > ansible/hosts (Luego examinarlo)
```

### Cargar el virtualenv de ansible

Asumimos ya se instaló ansible de alguna de las formas explicadas en el apartado
anterior, y en caso de utilizar ansible con virtualenv se haya cargado el mismo.

```
ansible --version
```

Debería devolver en ese proyecto algo como los siguiente:

```
ansible 2.8.4
  config file = /.../talleres/ansible/01-remote-connection/ansible.cfg
  configured module search path = ....
  ansible python module location = ....
  executable location = ....
  python version =....

```

### Probando ansible con ping


```
ansible -mping all
ansible -m ping pares
ansible -m ping impares
```

### Ejecutar un comando remoto

Comparar la salida del siguiente comando:

```
ansible all -m shell -a 'echo "Hola mundo desde $( hostname )"'
```

con:

```
ansible all -a 'echo "Hola mundo desde $( hostname )"'
```

### Uso de `ansible-console`

Correr el comando: `ansible-console all` y luego, en la consola REPL, ejecutar:

```
shell echo "Hola, la hora es $(date) en $(hostname)"
```

### Uso de `ansible-doc`

Podemos verificar la documentación de los módulos siguientes con los siguientes
comandos:

* ansible-doc ping
* ansible-doc shell

### Uso de `ansible-inventory`

ansible-inventory --list | --graph

Mostrar como funca la concurrencia
ansible -f1 vs ansible -f10 -a echo hola

Mostrar que es posible (editando el hosts, comentando la variable que define que el usuario es vagrant)

ansible -a whoami -u test|vagrant

Mostrar el become

ansible -a whoami all
vs
ansible -a whoami all --become
vs 
ansible -a whoami all --become --become-user nobody

Copiar archivos desde la maq local a todas
ansible all -m copy -a 'src=/etc/hosts dest=/tmp/hosts'

verificamos con:
 ansible all -a "cat /tmp/hosts"

Crear un archivo con determinadas propiedades

ansible pares -m file -a "dest=/opt/prueba/uno/dos/tres/archivo mode=0760 owner=nobody group=users"

vs

ansible pares -m file -a "dest=/opt/prueba/uno/dos/tres/archivo mode=0760 owner=nobody group=users state=directory"

(necesidad de --become)

Instalar un paquete

ansible -m apt package=mosh node-01 --become

Desinsntalarlo

ansible -m apt "package=mosh state=absent" node-01 --become

Crear/eliminar usuarios

ansible -m user -a 'name=juan group=nogroup' pares  --become
ansible -m user -a 'name=juan state=absent remove=true' all --become

Descubrimiento del entorno para programar

ansible -m setup node-01

ansible -m setup node-01 -a 'filter=*memtotal_mb'
ansible -m setup all --tree /tmp/salida


Uso de variables por host/grupos

Relativo al inventario, es posible crear directorios:

host_vars/
group_vars/
 
>> estos dirs se buscaran relativos cuando se usa ansible-playbook o, con el resto de los comandos en el inventario salvo que se especifique el argumento --playbook-dir

Y dentro de ellos archivos con los nombres de los grupos (all/ungrouped/pares/impares) y dentro de los hosts, nombre de los hosts. con esos archivos podemos crear variables de forma bien granular

Mostrar el ejemplo y probar: 

ansible -a 'echo Ejemplo de var {{ sample_var }}' all

El merge de variables es:
* all
* padre
* child
* host


