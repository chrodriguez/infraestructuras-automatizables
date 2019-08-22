# Taller Vagrant

Este taller nos guiará en la aplicación del flujo de trabajo con [Vagrant](https://www.vagrantup.com/) de [HashiCorp](https://www.hashicorp.com/).

## Temas

* [Instalación de vagrant](#instalación-de-vagrant)
   * [Verificando la instalación](#verificando-la-instalación)
* [Gestión de un proyecto Vagrant](#gestión-de-un-proyecto-vagrant)
* [Boxes](#boxes)
   * [Instalación de nuevos boxes](#instalación-de-nuevos-boxes)
   * [Listando los boxes instalados](#listando-los-boxes-instalados)
   * [Usando un box](#usando-un-box)
   * [Buscar boxes](#buscar-boxes)
* [Iniciando y Conectando](#iniciando-y-conectando)
* [Carpetas sincronizadas](#carpetas-sincronizadas)
   * [Otras carpetas sincronizadas](#otras-carpetas-sincronizadas)
* [Aprovisionamiento](#aprovisionamiento)
   * [¿Cómo instalar un servidor web que exponga el contenido del proyecto?](#cómo-instalar-un-servidor-web-que-exponga-el-contenido-del-proyecto)
   * [Aprovisionando](#aprovisionando)
* [Configuración de la Red](#configuración-de-la-red)
   * [Redirección de puertos](#redirección-de-puertos)


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
concepto de **carpteas sincronizadas**.

Por defecto, Vagrant comparte la carpeta del proyecto completa dentro de la VM
bajo el directorio `/vagrant`. 

Considerar que cuando se conecta por ssh a la VM usando `vagrant ssh`, se
conecta con el usuario `vagrant`, por lo que al conectarse, el usuario está
parado en el directorio `/home/vagrant` que es **diferente** al directorio
**sincronizado** `/vagrant`.

La salida desde la VM al comando `ls /vagrant` mínimamente debería mostrar el
`Vagrantfile`. Puede verificar que al crear un archivo en ese directorio aparece
en la máquina anfitrión:

* Conectarse a la VM: `vagrant ssh`
* Crear un archivo: `touch /vagrant/foo`
* Desconectar de la VM: `Ctrl+D`
* Verificar que en el directorio del proyecto aparece `foo`: `ls`

### Otras carpetas sincronizadas

Para sincronizar otras carpetas, Vagrant ofrece la opción `config.vm.synced_folder ORIGEN, DESTINO [, disabled: true]`. Por ejemplo:

```ruby
Vagrant.configure("2") do |config|
  # other config here
  config.vm.synced_folder "src/", "/srv/website", disabled: true
end
```

El ejemplo anterior comparte la carpeta del proyecto `src/` dentro de la VM en
`/srv/website`. El origen puede ser relativo o absoluto: si es relativo, es
relativo al proyecto Vagrant. La carpeta destino será creada recursivamente si
no existe y montará el directorio en nombre del usuario y grupo con el que hace
ssh. Las carpetas padres del árbol serán propias del usuario root.

## Aprovisionamiento

Ya disponemos de una máquina virtual que monta en un directorio de nuestra
máquina anfitrión los archivos que podemos editar desde nuestro editor
preferido. La idea ahora es poder servir nuestra producción con un servidor web
desde nuestra VM.

Podríamos simplemente instalar un servidor web en nuestra VM y listo, pero otro
integrante de nuestro equipo va a tener que repetir estos pasos _- que para
ejemplos reales, seguramente no es tan simple como instalar un servidor web-_.
Por ello, Vagrant ofrece soporte de _automatización del aprovisionamiento_, y de
esta forma, automáticamente instalar aplicaciones cuando se inicia con `vagrant
up` o fuerza el aprovisionamiento.

Antes de proceder, podemos verificar que nuestra VM no tiene un servidor web
instalado. Para ello accedemos a la VM usando `vagrant ssh` y verificamos:

```
curl localhost
```

El comando anterior debería dar error por no poder conectar con localhost:

```
curl: (7) Failed to connect to localhost port 80: Connection refuse
```

### ¿Cómo instalar un servidor web que exponga el contenido del proyecto?

Debemos instalar apache, y poner como **DocumentRoot** el directorio `/vagrant`.
Podemos hacer esto con el siguiente script:

```bash
#!/usr/bin/env bash

apt update 
apt install -y apache2
if ! [ -L /var/www/html ]; then
  rm -rf /var/www/html
  ln -fs /vagrant /var/www/html
fi
```

Creamos entonces un archivo llamado `bootstrap.sh` con el contenido anterior y
luego configuramos en `Vagrantfile` las siguientes directivas:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```

La directiva `config.vm.provision` indica que se utilizará el script
`bootstrap.sh` del directorio `/vagrant` para aprovisionar el ambiente.

### Aprovisionando

El aprovisionamiento se da en las siguientes situaciones:

* Corriendo `vagrant provision` en una VM encendida
* Corriendo `vagrant up` en una VM nunca creada
* Corriendo `vagrant reload --provision` en una VM encendida

Una vez aprovisionada la VM, podemos verificar si ahora podemos correr dentro de
la VM:

```
curl localhost
```

## Configuración de la Red

La VM nos permite editar archivos que son expuestos por un web server. Sin
embargo, no es posible acceder al web server más que desde la consola. En esta
sección veremos cómo es posible exponer servicios de la VM usando
configuraciones de red que ofrece Vagrant.

### Redirección de puertos

Una de las opciones de configuración que ofrece Vagrant es la redirección de
puertos. Esto permite mapear puertos de la máquina anfitrión con puertos de la
VM.
Veamos cómo exponer el puerto 80 de la VM en el puerto 9070:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provision :shell, path: "bootstrap.sh"
  config.vm.network :forwarded_port, guest: 80, host: 9070
end
```

Con `vagrant reload` los cambios serán tomados, y podremos entonces acceder con
un navegador a la URL: http://localhost:9070.

Existen otros mecanismos de configuración de redes que pueden utilizarse
siguiendo la [documentación de
redes](https://www.vagrantup.com/docs/networking/).

## Ambientes multi máquinas

En vagrant es posible crear un `Vagrantfile` que defina y configure diferentes
máquinas en el mismo archivo.

### Ejemplo de dos vms declarativas

El siguiente ejemplo instancia 2 vms:

```ruby
Vagrant.configure("2") do |config|

  config.vm.define "vm-01" do |machine|
    machine.vm.box = "ubuntu/bionic64"
  end

  config.vm.define "vm-02" do |machine|
    machine.vm.box = "ubuntu/bionic64"
  end
end
```

Para conectar con alguna de las vms, debe utilizarse el nombre, por ejemplo:

```
vagrant ssh vm-01
```

### Ejemplo de N vms usando código

El siguiente ejemplo arranca la cantidad de máquinas indicada por la variable de
ambiente `VM_COUNT`. Si la misma no se setea, inicia 5 máquinas, todas con 256MB
de memoria y una CPU:

```ruby
Vagrant.configure("2") do |config|
  1.upto(Integer(ENV['VM_COUNT'] || 5)) do |number|
    vmname = "node-#{"%02d" % number}"
    config.vm.define vmname do |instance|
      instance.vm.box = "ubuntu/bionic64"
      instance.vm.hostname = vmname
    end
  end
  config.vm.provider "virtualbox" do |v|
    v.memory = 256
    v.cpus = 1
  end
end
```

Podemos probar la anterior arquitectura de la siguiente forma:

```
VM_COUNT=2 vagrant up
```

> Iniciará 2 vms

```
vagrant up
```

> Iniciará 5 vms

### Ejemplo con diferentes boxes

El siguiente ejemplo instancia dos vms, una con centos/7 y otra con
ubuntu/bionic. Además indica el hostname de cada vm

```ruby
Vagrant.configure("2") do |config|

  base_port = 8080
  { 'centos/7' => 'centos-apache', 'ubuntu/bionic64' => 'ubuntu-apache' }.each do |box, hostname|
    config.vm.define hostname do |machine|
      machine.vm.box = box
      machine.vm.hostname = hostname
      machine.vm.network "forwarded_port", guest: 80, host: base_port
      base_port+=1
    end
  end

end
```
