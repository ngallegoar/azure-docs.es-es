---
title: Cómo funciona la conexión de un equipo de desarrollo a un clúster de AKS
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Describe los procesos que usa Azure Dev Spaces para conectar el equipo de desarrollo con el clúster de Azure Kubernetes Service
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores
ms.openlocfilehash: a74a5a623006ccd64441023c2c4bc9ad3dcb517e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241342"
---
# <a name="how-connecting-your-development-computer-to-your-aks-cluster-works"></a>Cómo funciona la conexión de un equipo de desarrollo a un clúster de AKS

Con Azure Dev Spaces, puede conectar su equipo de desarrollo al clúster de AKS, lo que le permite ejecutar y depurar el código en el equipo de desarrollo como si se estuviera ejecutando en el clúster. Azure Dev Spaces redirige el tráfico entre el clúster de AKS conectado. Para ello, ejecuta un pod en el clúster que actúa como agente remoto para redirigir el tráfico entre la máquina de desarrollo y el clúster. Esta redirección del tráfico permite que el código del equipo de desarrollo y los servicios que se ejecutan en el clúster de AKS se comuniquen como si estuvieran en el mismo clúster de AKS. Esta conexión también le permite ejecutar y depurar código con o sin un contenedor en el equipo de desarrollo. La conexión del equipo de desarrollo al clúster le ayuda a desarrollar rápidamente la aplicación y realizar pruebas de un extremo a otro.

## <a name="connecting-to-your-cluster"></a>Conectarse a los clúster

Puede conectarse al clúster de AKS existente mediante [Visual Studio Code][vs-code] con la extensión [Azure Dev Spaces][azds-vs-code] instalada y ejecutándose en MacOS o Windows 10. Al establecer una conexión, tiene la opción de redirigir todo el tráfico hacia y desde un pod nuevo o existente en el clúster al equipo de desarrollo.

> [!NOTE]
> Al usar Visual Studio Code para conectarse al clúster, la extensión de Azure Dev Spaces ofrece la opción de redirigir un servicio al equipo de desarrollo. Esta opción es una forma cómoda de identificar un POD para el redireccionamiento. Todo el redireccionamiento entre el clúster de AKS y el equipo de desarrollo es para un pod.

Para conectarse al clúster no es necesario tener Azure Dev Spaces habilitado en el clúster. En su lugar, cuando la extensión de Azure Dev Spaces establece una conexión con el clúster:

* Se reemplaza el contenedor del Pod en el clúster de AKS por un contenedor de agente remoto que redirige el tráfico al equipo de desarrollo. Al redirigir un nuevo Pod, Azure Dev Spaces crea un nuevo Pod en el clúster de AKS con el agente remoto.
* Ejecuta [kubectl port-forward][kubectl-port-forward] en el equipo de desarrollo para reenviar el tráfico desde el equipo de desarrollo al agente remoto que se ejecuta en el clúster.
* Recopila información de entorno del clúster mediante el agente remoto. Esta información de entorno incluye variables de entorno, servicios visibles, montajes de volúmenes y montajes de secretos.
* Configura el entorno en el terminal de Visual Studio Code de modo que el servicio del equipo de desarrollo pueda tener acceso a las mismas variables como si se estuviera ejecutando en el clúster.  
* Actualiza el archivo de hosts para asignar servicios en el clúster de AKS a direcciones IP locales en el equipo de desarrollo. Estas entradas de archivo de host permiten que el código que se ejecuta en el equipo de desarrollo realice solicitudes a otros servicios que se ejecutan en el clúster. Para actualizar el archivo de hosts, Azure Dev Spaces solicitará acceso de administrador en el equipo de desarrollo al conectarse al clúster.

Si tiene Azure Dev Spaces habilitado en el clúster, también tiene la opción de usar el [redireccionamiento de tráfico que ofrece Azure Dev Spaces][how-it-works-routing]. El redireccionamiento de tráfico que ofrece Azure Dev Spaces le permite conectarse a una copia del servicio que se ejecuta en un espacio de desarrollo secundario. El uso de un espacio de desarrollo secundario le ayuda a evitar que otras personas trabajen en el espacio de desarrollo principal, ya que solo redirige el tráfico destinado a la instancia del espacio secundario del servicio, sin modificar la instancia de espacio primario del servicio.

Una vez que se conecte al clúster, el tráfico se enrutará al equipo de desarrollo, independientemente de si el servicio se ejecuta en el equipo de desarrollo.

## <a name="running-code-on-your-development-computer"></a>Ejecutar código en el equipo de desarrollo

Después de establecer una conexión con el clúster de AKS, puede ejecutar cualquier código de forma nativa en el equipo, sin contenedores. Cualquier tráfico que reciba el agente remoto se redirige al puerto local especificado durante la conexión para que el código que se ejecuta de forma nativa pueda aceptar y procesar ese tráfico. Las variables de entorno, los volúmenes y los secretos del clúster se ponen a disposición del código que se ejecuta en el equipo de desarrollo. Además, debido a que las entradas de archivo de host y el reenvío de puerto que Azure Dev Spaces agregó al equipo del desarrollador, el código puede enviar tráfico a los servicios que se ejecutan en el clúster mediante los nombres de servicio del clúster, y ese tráfico se reenvía a los servicios que se ejecutan en el clúster.

Dado que el código se ejecuta en el equipo de desarrollo, tiene la flexibilidad de usar cualquier herramienta que use normalmente para el desarrollo con el fin de ejecutar el código y depurarlo. El tráfico se enruta entre el equipo de desarrollo y el clúster todo el tiempo que está conectado. Esta conexión persistente permite iniciar, detener y reiniciar el código tantas veces como sea necesario, sin tener que volver a establecer una conexión.

Además, Azure Dev Spaces proporciona una manera de replicar variables de entorno y archivos montados disponibles para pods en el clúster de AKS en el equipo de desarrollo mediante el archivo *azds-local.env*. También puede usar este archivo para crear nuevas variables de entorno y montajes de volúmenes.

## <a name="additional-configuration-with-azds-localenv"></a>Configuración adicional con azds-local. env

El archivo *azds-local.env* le permite replicar variables de entorno y archivos montados disponibles para los pods en el clúster de AKS. Puede especificar las siguientes acciones en un archivo *azds-local.env*:

* Descargue un volumen y establezca la ruta de acceso a ese volumen como una variable de entorno.
* Descargue un archivo individual o un conjunto de archivos de un volumen y móntelo en el equipo de desarrollo.
* Haga que un servicio esté disponible independientemente del clúster al que esté conectado.

Este es un ejemplo del archivo *azds-local.env*:

```
# This downloads the "whitelist" volume from the container,
# saves it to a temporary directory on your development computer,
# and sets the full path to an environment variable called WHITELIST_PATH.

WHITELIST_PATH=${volumes.whitelist}/whitelist

# This downloads a file from the container's 'default-token-<any>' mount directory 
# to /var/run/secrets/kubernetes.io/serviceaccount on your development computer.

KUBERNETES_IN_CLUSTER_CONFIG_OVERWRITE=${volumes.default-token-*|/var/run/secrets/kubernetes.io/serviceaccount}

# This makes the myapp1 service available to your development computer
# regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP1_SERVICE_HOST.

# If the myapp1 service is made available in this way, 
# you can also access it using "myapp1" and "myapp1.svc.cluster.local"
# in addition to the IP in the MYAPP1_SERVICE_HOST environment variable.

MYAPP1_SERVICE_HOST=${services.myapp1}

# This makes the service myapp2 in namespace mynamespace available to your 
# development computer regardless of the AKS cluster you are connected to and
# sets the local IP to an environment variable called MYAPP2_SERVICE_HOST.

MYAPP2_SERVICE_HOST=${services.mynamespace.myapp2}
```

Un archivo *azds-local.env* predeterminado no se crea automáticamente, por lo cual debe crear manualmente el archivo en la raíz del proyecto.

## <a name="diagnostics-and-logging"></a>Diagnósticos y registro

Cuando se conecta a un clúster de AKS, los registros de diagnóstico del clúster se registran en el [directorio temporal][azds-tmp-dir] del equipo de desarrollo. Con Visual Studio Code, también puede usar el comando *Mostrar información de diagnóstico* para imprimir las variables de entorno y las entradas DNS actuales del clúster de AKS.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a conectar el equipo de desarrollo local al clúster de AKS, consulte [Conexión del equipo de desarrollo a un clúster de AKS][connect].

[azds-tmp-dir]: troubleshooting.md#before-you-begin
[azds-vs-code]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[connect]: how-to/connect.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward
[vs-code]: https://code.visualstudio.com/download
