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

Asegurar estar en directorio donde se encuentra el `Vagrantfile` y `ansible.cfg`
para correr los siguientes comandos.

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

Ping es un modulo de ansible que simplemente testea conectividad

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

> No especificar el módulo (el segundo ejemplo), implica usar el módulo
> **command** que no admite el uso de variables o interpolación, como es el caso
> de **shell**

### Uso de `ansible-console`

Correr el comando: `ansible-console all` y luego, en la consola REPL, ejecutar:

```
shell echo "Hola, la hora es $(date) en $(hostname)"
```

> Probar además un comando como `date`

### Uso de `ansible-doc`

Podemos verificar la documentación de los módulos siguientes con los siguientes
comandos:

```
ansible-doc ping
ansible-doc shell
```

### Uso de `ansible-inventory`

Permite visualizar el inventario de diferentes formas:

```
ansible-inventory --list
ansible-inventory --list -y
ansible-inventory --graph
```

### Ejecución concurrente de tareas

Es posible verificar la concurrencia de las tareas con la opción **-f**

```
ansible all -f1 -a 'echo hola'
ansible all -f10 -a 'echo hola'
```

### El usuario con el que se conecta con ssh

La configuración del inventario configura una variable global con el username
del usuario de conexión. Vamos a comentar esta configuración editando el archivo `ansible/hosts`:

```yaml
all:
  vars:
    ansible_ssh_host: 127.0.0.1
#    ansible_ssh_user: vagrant
...
```

Luego, probar el siguiente comando:

```
ansible all -a whoami 
```

Como ya no configura el usuario vagrant, las conexiones deberían fallar.
Ejecutamos entonces:

```
ansible all -a whoami -u vagrant
```

Los comandos se ejecutan con el usuario con el que se conecta, para convertirse
en otro usuario:

```
ansible all -a whoami -b
ansible all -a whoami --become --become-user nobody
```

### Transferencia de archivos

Es posible copiar archivos desde la máquina de control a todos los equipos
remotos:

```
ansible all -m copy -a 'src=/etc/hosts dest=/tmp/hosts'
```

Luego verificamos se han copiado:

```
ansible all -a "cat /tmp/hosts"
```

Podemos también crear un archivo con determinadas propiedades

```
ansible pares -m file -a "dest=/opt/prueba/uno/dos/tres/archivo mode=0760 owner=nobody group=users"
ansible pares -m file -a "dest=/opt/prueba/uno/dos/tres/archivo mode=0760 owner=nobody group=users state=directory"
```

> Notar que el último comando no funcionará porque requiere la opción **-b**

### Instalar un paquete

Existen módulos para diferentes distribuciones. Por ejemplo, para instalar con
apt, es posible utilizar el siguiente comando:

```
ansible -m apt package=mosh node-01 --become
```

Y luego para desinstalarlo:

```
ansible -m apt "package=mosh state=absent" node-01 --become
```

### Gestión de usuarios

Es posible gestionar usuarios y grupos. El siguiente comando crea un usuario en
los nodos pares:

```
ansible -m user -a 'name=juan group=nogroup' pares  --become
```

El siguiente comando elimina los usuarios creados y elimina su directorio HOME:

```
ansible -m user -a 'name=juan state=absent remove=true' all --become
```

### Descubrimiento del entorno

Ansible provee un módulo llamado setup que ofrece una basta cantidad de datos
propios del sistema donde corre. Estos valores son posibles de utilizar luego
como variables que son inferidas por el subsistema:

```
ansible -m setup node-01
ansible -m setup node-01 -a 'filter=*memtotal_mb'
ansible -m setup all --tree /tmp/salida
```

> El último comando, deja en la carpeta salida, la información colectada de cada
> nodo del inventario


### Uso de variables por host/grupos

Relativo al inventario, es posible crear directorios:

* `host_vars/`
* `group_vars/`

> Estos directorios se buscaran relativos cuando se usa `ansible-playbook` o,
> con el resto de los comandos en el inventario salvo que se especifique el
> argumento `--playbook-dir`

Dentro del directorio `group_vars/` se puede utilizar como nombre de los archivos, 
los grupos, por ejemplo _all/ungrouped/pares/impares_, y dentro del directorio 
`host_vars/` el  nombre de los archivos, los hosts.
Con estos archivos podemos crear variables de forma granular. Observar el
contenido de estos directorios, para así entender el siguiente ejemplo:

```
ansible -a 'echo Ejemplo de var {{ sample_var }}' all
```

