# Taller Vagrant

Este taller nos guiará en la aplicación del flujo de trabajo con [Vagrant](https://www.vagrantup.com/) de [HashiCorp](https://www.hashicorp.com/).

## Instalación de vagrant

En caso de ya disponer instalado vagrant, avanzar a la siguiente sección.

Para instalar Vagrant, es necesario [descargar el paquete que corresponda para su
sistema](https://www.vagrantup.com/downloads.html). Una vez instalado, se provee
del comando `vagrant`. Si luego de instalar el paquete, no dispone del comando
en su terminal, pruebe de reiniciar su sesión en el sistema.

### Verificando la instalación

Luego de instalar Vagrant, verificar la instalación abriendo una terminal y
corriendo el comando `vagrant`. la salida esperada será:

```
$ vagrant 
Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.
...
```

> Evitar instalar Vagrant desde los paquetes de su SO debido que generalmente
> son versiones antiguas del producto.

## Gestión de un proyecto Vagrant

El primer paso de configurar un proyecto Vagrant, es crear un archivo `Vagrantfile`. Este archivo tiene dos objetivos:

* Marcar el directorio raíz del proyecto: _muchas configuraciones de vagrant son
  relativas a este directorio._
* Describir el tipo de máquina(s) y recursos se necesitan por el proyecto, así
  como qué aplicaciones instalar y de qué forma se accederá.

Para ello, Vagrant provee subcomandos para inicializar el directorio:
`vagrant init`. Sigamos el siguiente ejemplo:

```
mkdir vagrant_getting_started
cd vagrant_getting_started
vagrant init ubuntu/bionic64
```

Este comando creará el archivo `Vagrantfile` en el directorio. Observando el
contenido se ven muchos comentarios y ejemplos. La primer impresión es bastante
intimidante, pero en breve lo modificaremos.

El archivo `Vagrantfile` puede ponerse bajo control de versiones, permitiendo
así que otros miembros del equipo compartan las definiciones creadas para
iniciar un entorno de trabajo determinado.

## Boxes

Antes de trabajar con Vagrant, uno acostumbraba a trabajar con VMs que se
instalan desde cero. Este proceso lento y tedioso. Vagrant utiliza imágenes base
para rápidamente clonar una nueva VM. Las imágenes base se llaman _**boxes**_
dentro de Vagrant. Especificar el box a utilizar en un `Vagrantfile` sería el
paso inmediato a inicializar el proyecto.

### Instalación de nuevos boxes

Al correr el comando de incialización del proyecto, pudo observarse que, si el
box aún no se encontraba descargado, se descarga desde internet la imagen base.
Cada vez que se utiliza un box diferente, la imagen base se almacena localmente
para acelerar futuros reusos de la misma.

Es posible agregar más boxes a nuestra biblioteca local de Vagrant con el
comando:

```
vagrant box add ubuntu/xenial64
```

### Listando los boxes instalados

Para ver los boxes instalados, podemos utilizar el comando:

```
vagrant box list
```

La salida del comando mostrará el driver (virtualbox por defecto), el nombre del
box y la versión. La versión de una misma imagen irá cambiando en el tiempo.

> La versión pude especificarse en el comando `vagrant box add --box-version` de
> la misma forma que en el `Vagrantfile`

### Usando un box

Una vez que un box ha sido agregado, y se ha descargado, entonces podemos
referenciarlo editando el `Vagrantfile`.

Tomamos el ejemplo que hemos creado oportunamente, y editamos el `Vagrantfile`
con el siguiente contenido:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
end
```

Podemos incluso indicar versión del box, y URL de donde descargarlo:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_version = "20190807.0.0"
  config.vm.box_url = "https://app.vagrantup.com/ubuntu/boxes/bionic64"
end
```

### Buscar boxes

Para buscar boxes, el repositorio más confiable es el [Catálogo de boxes de
Vagrant](https://app.vagrantup.com/boxes/search). 

## Iniciando y Conectando

Es momento de iniciar nuestro primer ambiente Vagrant. Para ello es necesario
correr desde la terminal:

```
vagrant up
```

En unos minutos, el comando finalizará y tendremos una VM corriendo Ubuntu. No
se visualizará nada porque Vagrant correrá la VM sin interfaz de usuario. Para
conectar con la VM, es posible conectarse por **ssh** mediante el comando: 

```
vagrant ssh
```

El comando anterior nos conectará mediante ssh con la VM que está corriendo. En
esta sesión podemos realizar el comando que se nos ocurra, incluso correr algo
como `sudo rm -fr /`, pero **cuidado** porque Vagrant comparte con la máquina
anfitriona el directorio raíz del proyecto dentro de la VM en `/vagrant`. Sin
embargo, podemos probar correr el comando:

```
sudo rm -fr /bin
ls
```

Veremos que no será posible ejecutar el comando `ls` porque eliminamos todo el
directorio `/bin`. Entonces simplemente nos desconectamos de la VM a la cuál
dejamos inconsistente y podemos reiniciar una nueva instancia funcional con el
comando:

```
vagrant destroy -f && vagrant up
```

Con el comando anterior eliminamos la instancia creada, y creamos una nueva
inmediatamente.

## Carpetas sincronizadas

Dado que nos es de gran utilidad disponer de todo un ambiente virtualizado que
cumpla con las expectativas necesarias, para por ejemplo desarrollar con
herramientas diversas, utilizar un editor por consola, no es de agrado para
muchos desarrolladores. Para dar solución a este problema, Vagrant provee el
conepto de **carpteas sincronizadas**.

Por defecto, Vagrant comparte la carpeta del proyecto completa dentro de la VM
bajo el directorio `/vagrant`. 

Considerar que cuando se conecta por ssh a la VM usando `vagrant ssh`, se
conecta con el usuario `vagrant`, por lo que al conectarse, el usuario está
parado en el directorio `/home/vagrant` que es **diferente** al directorio
**sincronizado** `/vagrant`.

La salida desde la VM al comado `ls /vagrant` mínimamente debería mostrar el
`Vagrantfile`. Puede verificar que al crear un archivo en ese directorio aparece
en la máquina anfitrión:

* Conectarse a la VM: `vagrant ssh`
* Crear un archivo: `touch /vagrant/foo`
* Desconectar de la VM: `Ctrl+D`
* Verificar que en el directorio del proyecto aparece `foo`: `ls`

