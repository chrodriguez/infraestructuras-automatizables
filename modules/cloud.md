## Cloud Computing

---

## Cloud Computing

* Características de CC
* Clasificación de servicios de CC
* Clasificación de servicios emergentes de CC
* Ejemplos por tipo de CC
* Actividad práctica
	* Taller con Vagrant

---

## Características de CC

* Diferencia marcada entre el negocio y las herramientas necesarias para el negocio _(Distribuir contenido multimedia es el negocio)_
* Los clientes se enfocan en sus proyectos. Los proveedores de servicio se encargan del resto.
* Según el [NIST](https://www.nist.gov/) las principales características de CC son:
	* Auto gestión por demanda
	* Amplio acceso por medio de la red
	* Pool de recursos multitenant
	* Simple elasticidad

---

### Clasificación de servicios de CC

* **SaaS:** un servicio desarrollado, desplegado, corrido y mantenido por el proveedor, y que es accedido por web. El cliente utiliza el producto, sin interactuar con la infraestructura que provee el servicio.
* **PaaS:** un servicio que provee una plataforma de desarrollo y despliegue de aplicaciones web. El cliente no tiene control sobre la infraestructura, pero sí accede a los recursos que son administrados a través de APIs.
* **IaaS:** ofrecen utilidades y herramientas que permiten gestionar storage, vms, redes y otros recursos básicos. No se tiene acceso físico al HW.

---

### Clasificación de servicios de CC

* **On premises:** es un servicio de CC hosteado en máquinas que pertenecen a un mismo cliente. El cliente ofrece a su empresa servicios privados de _SaaS, PaaS, IaaS_

![CC clasificación](images/saas-vs-paas-vs-iaas-breakdown.jpg)

---

### Clasificación de servicios emergentes de CC

* **FaaS:** un servicio que provee una plataforma donde desarrollar, correr y desplegar funciones sin la complejidad de construir y mantener la infraestructura asociada con el servicio de una aplicación.
	* Este modelo se corresponde con una arquitectura _Serverless_ empleada en _microservicios_.
* **CaaS:** un servicio intermedio entre IaaS y PaaS. Se ofrece un servicio que independiza los motores de contenedores ([docker](https://www.docker.com/)/[rkt](https://coreos.com/rkt/)/[lxc](https://linuxcontainers.org/)), su orquestación y recursos.

---

### Ejemplos por tipo de CC

* **SaaS:** Gmail, Google Office, Dropbox.
* **PaaS:** [Heroku](https://www.heroku.com/), [Google AppEngine](https://developers.google.com/appengine/), [AWS Beanstalk](https://aws.amazon.com/es/elasticbeanstalk/).
* **IaaS:** [Digital Ocean Droplets](https://www.digitalocean.com/products/droplets/), [AWS EC2](https://aws.amazon.com/es/ec2/), [GCE](https://cloud.google.com/compute/), [Azure](https://azure.microsoft.com/es-es/services/virtual-machines/).
* **FaaS:** [AWS Lambda](https://aws.amazon.com/es/lambda/), [Google Functions](https://cloud.google.com/functions/), [Azure Functions](https://azure.microsoft.com/es-es/services/functions/), [Kubeless](https://kubeless.io/), [Open FaaS](https://www.openfaas.com/).
* **CaaS:** [AWS ECS](https://aws.amazon.com/es/ecs/), [AWS EKS](https://aws.amazon.com/es/eks/), [GKE](https://cloud.google.com/kubernetes-engine/), [AKS](https://azure.microsoft.com/es-es/services/kubernetes-service/).

---

## Actividad práctica

  <a href="https://www.vagrantup.com/">
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 223 60" class="logo" height="50">
  <path class="text" fill="#000000" d="M99.88 14.1h6.29l-9.56 32h-8.93l-9.56-32h6.29l7.73 26.66 7.74-26.66zm23.78 32h-4.8l-.43-1.59c-2.083 1.355-4.515 2.074-7 2.07-4.28 0-6.1-2.93-6.1-7 0-4.76 2.07-6.58 6.82-6.58h5.62v-2.42c0-2.59-.72-3.51-4.47-3.51-2.182.023-4.356.264-6.49.72l-.72-4.47c2.606-.723 5.296-1.096 8-1.11 7.35 0 9.51 2.59 9.51 8.46l.06 15.43zm-5.86-8.84h-4.32c-1.92 0-2.45.53-2.45 2.31s.53 2.35 2.35 2.35c1.55-.022 3.07-.435 4.42-1.2v-3.46zm17.05 2.54c-.74.36-1.26 1.057-1.39 1.87 0 .62.38.91 1.3 1 2.59.29 4 .43 6.77.72 3.8.43 5 2.31 5 5.67 0 5-1.83 7-10.57 7-3.04.003-6.067-.4-9-1.2l.72-4.37c2.576.654 5.222.99 7.88 1 4.66 0 5.57-.34 5.57-1.87s-.43-1.68-2.21-1.87c-2.69-.29-3.8-.43-6.77-.77-3.31-.38-4.61-1.49-4.61-4.47.102-1.657 1.02-3.156 2.45-4-2.16-1.3-3.17-3.46-3.17-6.29V30c.1-4.85 2.64-7.78 9.42-7.78 1.348-.01 2.692.15 4 .48h7.21v2.93c-.82.24-1.78.48-2.59.72.558 1.135.84 2.386.82 3.65v2.21c0 4.76-2.88 7.64-9.42 7.64-.47.01-.94-.006-1.41-.05zm1.34-12.88c-2.88 0-3.89 1.06-3.89 3.27V32c0 2.31 1.15 3.17 3.89 3.17s3.94-.91 3.94-3.17v-1.81c.01-2.19-1-3.26-3.93-3.26l-.01-.01zm25.9.68c-2.148.973-4.217 2.11-6.19 3.4v15.1H150V22.7h5l.38 2.59c1.906-1.29 3.974-2.32 6.15-3.07l.56 5.38zm19.6 18.5h-4.8l-.43-1.59c-2.083 1.355-4.515 2.074-7 2.07-4.28 0-6.1-2.93-6.1-7 0-4.76 2.07-6.58 6.82-6.58h5.62v-2.42c0-2.59-.72-3.51-4.47-3.51-2.182.023-4.356.264-6.49.72l-.72-4.47c2.606-.723 5.296-1.096 8-1.11 7.35 0 9.51 2.59 9.51 8.46l.06 15.43zm-5.86-8.84h-4.32c-1.92 0-2.45.53-2.45 2.31s.53 2.35 2.35 2.35c1.55-.022 3.07-.435 4.42-1.2v-3.46zm23.3 8.84V29.76c0-1.25-.53-1.87-1.87-1.87-2.147.252-4.22.932-6.1 2V46.1h-5.86V22.7h4.47l.58 2c2.92-1.46 6.11-2.295 9.37-2.45 3.89 0 5.28 2.74 5.28 6.92v17l-5.87-.07zm23.11-.44c-1.653.578-3.39.886-5.14.91-4.28 0-6.44-2-6.44-6.2v-13h-3.51V22.7h3.51v-5.81l5.86-.82v6.63h6l-.38 4.66h-5.62v12.25c-.076.576.124 1.154.54 1.56.414.408.995.597 1.57.51.994-.03 1.98-.19 2.93-.48l.68 4.46z"></path>
  <path class="front" fill="#1563FF" d="M58.03 10.12V4.63L44.84 12.3v4.64L34.29 39.7l-5.28 3.64v16.67l11.31-6.52 17.71-43.37zM29.01 31.47L21.1 13V7.78l-.05-.02-7.86 4.54v4.64L23.74 40.7l5.27-2.61v-6.62z"></path>
  <path class="shadow" fill="#104EB2" d="M50.12.01L36.94 7.73h-.01V13l-7.92 18.47v6.17l-5.27 3.06-10.55-23.76v-4.65l7.92-4.55L7.91.01 0 4.63v5.66l17.81 43.25 11.2 6.47V43.76l5.28-3.06-.07-.04 10.62-23.72v-4.65l13.19-7.66"></path>
  </svg>
  </a>

---

## Taller con Vagrant

Vagrant es una herramienta para construir y gestionar ambientes de VMs mediante un flujo de trabajo simple que se enfoca en la automatización, minimizando la configuración de ambientes complejos de desarrollo, compartirlos entre el equipo, incrementando así la productividad.

[Ir al taller](https://github.com/chrodriguez/infraestructuras-automatizables/tree/master/talleres/vagrant)
