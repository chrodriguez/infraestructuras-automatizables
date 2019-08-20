# Taller sobre Ansible

Ansible es una de las herramientas más populares de automatización de la
infraestructura, por su simplicidad en cuanto a requerimientos para su
funcionamiento y curva de aprendizaje.

La gran ventaja de ansible por sobre otras herramientas es que no requiere
agentes en los equipos que gestiona, porque su requerimiento central es openssh,
que es componente fundamental de un servidor.

Toda la gestión se realiza desde un nodo de control cuyo requerimiento es el de
disponer de python (versión 2 o 3, preferiblemente 3).

## Instalación

Ansible puede instalarse con paquetes de la distribución o usando el manejador
de paquetes de python [pip](https://pypi.org/project/pip/). De todas las formas
disponibles para [instalar ansible que explica la
documentación](https://docs.ansible.com/ansible/latest/installation_guide/index.html),
recomendamos el uso de pip y virtualenv de python, dado que de esta forma es
simple manejar versiones específicas de ansible por proyecto. 
La razón que justifica nuestra elección, es que los playbooks que uno escribe
para un cliente, pueden quedar obsoletos en nuevas versiones. Para mantener
funcionales en el tiempo diferentes versiones de playbooks para diferentes
clientes, recomendamos que se utilice este esquema que permite manejar por
proyecto o cliente diferentes versiones de ansible.

A continuación, se muestra la forma de instalar ansible en Ubuntu 18.04:

### Instalar dependencias

Las únicas dependencias para poder trabajar con ansible son:

* Python 3
* virutalenv

Instalamos las dependencias con:

```
apt install -y python3 y python3-venv
```

Con estos paquetes, creamos un **virtualenv** usando el sigiuente comando:

```
python3 -m venv ansible-env
```

> El comando anterior, simplemente crea un directorio llamado `ansible-env/` con
> una estructura específica de virtualenv que permite encapsular todas las
> dependencias y librerías de ese ambiente virtual de python.

Para utilizar este ambiente virtual, se debe correr el siguiente comando desde nuestra consola:

```
source ansible-env/bin/activate 
```

> Al correr ese comando, veremos que el prompt mostrará por delante de todo, el
> nombre del ambiente virtual, en este caso **(ansible-env)**. Para salir del ambiente virtual 
> se utiliza el comando `deactivate`.

### Instalamos ansible en virtualenv

Vamos a verificar como instalar una versión anterior de ansible, y luego
actualizamos:

```
pip3 install "ansible~=2.7.0"
```

> El comando anterior instala ansible en la versión más actualizada de la rama 2.7.

Verificamos la version de ansible:

```
ansible --version
```

Ahora actualizamos a la ultima versión estable:

```
pip3 install ansible
```

Verificamos nuevamente la versión:

```
ansible --version
```

## Usando ansible

Ansible nos permitirá realizar múltiples tareas, desde manejar y orquestar
múltiples equipos a partir de un inventario, hasta configurar equipos usando
playbooks.

Para interactuar con ansible, el proyecto provee comandos para diferentes
tareas. Los comandos más utilizados son:

* `ansible`:  herramienta para realizar tareas remotas.
* `ansible-console`: consola [REPL](https://es.wikipedia.org/wiki/REPL) para
  correr tareas de ansible.
* `ansible-doc`: ver documentación de módulos instalados.
* `ansible-inventory`: permite visualizar el inventario configurado.
* `ansible-playbook`: corre playbooks de ansible ejecutando tareas determinadas
  en un grupo de equipos.

En los siguientes talleres, se podrá experimentar con estos comandos que
permiten visualizar las principales características de ansible:

* [Ejemplo de gestión remota de múltiples nodos](./01-remote-connection)
* Ejemplo de playbook


## TODO
Analizar ansible-vault 

Playbooks

Antes de  empezar con esto, explicar el concepto de idempotencia, sobre todo para mostrarlo con el modulo shell, y la importancia del creates

Ejemplo de como instalar apache 02-p[laybook
Ansible lint: pip3 install ansible-lint

Mostrar que el lint da error por usar latest
