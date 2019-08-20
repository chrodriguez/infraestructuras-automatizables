Instalar:

* python3 y python3-venv
* Usando venv, correr:
python3 -m venv ansible-env
* Cargamos el ambiente de ansible:
source ansible-env/bin/activate (se sale con deactivate)
* Instalamos una version vieja de ansible
pip install "ansible~=2.7.0"
* Como upgradear a la ultima ultima version estable
pip3 install ansible
* Iniciamos N vms con vagrant
cd 01-remote-connection && vagrant up

Editar ansible.cfg e ansible/hosts

El ansible/hosts se puede autogenerar con

bash utils/vagrant-ssh-config-to-ansible > ansible/hosts (Luego examinarlo)

ansible -mping all
ansible -m ping pares / impares

Luego ejecutar el comando remoto:

 ansible all -m shell -a 'echo "Hola mundo desde $( hostname )"'

y comparar con

  ansible all -a 'echo "Hola mundo desde $( hostname )"'


ansible-console all

shell echo "Hola, la hora es $(date) en $(hostname)"


ansible-doc ping
ansible-doc shell
ansible-inventory --list | --graph

Mostrar como funca la concurrencia
ansible -f1 vs ansible -f10 -a echo hola

Mostrar que es posible (editando el hosts, comentando la variable que define que el usuario es vagrant)

ansible -a whoami -u test|vagrant

Mostrar el become


Analizar ansible-vault y ansible-pull
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

Playbooks

Antes de  empezar con esto, explicar el concepto de idempotencia, sobre todo para mostrarlo con el modulo shell, y la importancia del creates

Ejemplo de como instalar apache 02-p[laybook
Ansible lint: pip3 install ansible-lint

Mostrar que el lint da error por usar latest
