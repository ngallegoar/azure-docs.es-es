---
title: Desarrollo en equipo mediante Node.js y Visual Studio Code
services: azure-dev-spaces
ms.date: 07/09/2018
ms.topic: tutorial
description: En este tutorial se muestra cómo usar Azure Dev Spaces y Visual Studio Code para el desarrollo en equipo en una aplicación Node.js en Azure Kubernetes Service
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.custom: devx-track-javascript
ms.openlocfilehash: 851ba1aa6fa6a90fb4f659f1173818946a3e82d3
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87420369"
---
# <a name="team-development-using-nodejs-and-visual-studio-code-with-azure-dev-spaces"></a>Desarrollo en equipo mediante Node.js y Visual Studio Code con Azure Dev Spaces

En este tutorial, aprenderá cómo un equipo de desarrolladores puede colaborar de forma simultánea en el mismo clúster de Kubernetes gracias a Dev Spaces.

## <a name="learn-about-team-development"></a>Aprenda sobre el desarrollo en equipo.
Hasta ahora, ha ejecutado el código de la aplicación como si fuera el único desarrollador que trabaja en la aplicación. En esta sección, aprenderá cómo Azure Dev Spaces simplifica el desarrollo en equipo:
* Habilite un equipo de desarrolladores para trabajar en el mismo entorno, trabajando en un espacio de desarrollo compartido o en espacios de desarrollo distintos, según sea necesario.
* Permitirá que cada desarrollador efectúe una iteración en su código de forma aislada y sin peligro de interrumpir a otros desarrolladores.
* Probará el código de un extremo a otro, antes de confirmarlo, sin tener que crear bocetos o dependencias simuladas.

### <a name="challenges-with-developing-microservices"></a>Dificultades con el desarrollo de microservicios
Por el momento, la aplicación de ejemplo no es muy compleja. Pero en el trabajo de desarrollo del mundo real, las dificultades pronto surgen a medida que se agregan más servicios y crece el equipo de desarrollo. Puede ser poco realista ejecutarlo todo localmente para el desarrollo.

* La máquina de desarrollo puede no tener recursos suficientes para ejecutar a la vez cada uno de los servicios que necesite.
* Algunos servicios pueden tener que estar accesibles públicamente. Por ejemplo, un servicio puede necesitar tener un punto de conexión que responda a un webhook.
* Si desea ejecutar un subconjunto de servicios, tiene que conocer la jerarquía de dependencias completa entre todos los servicios. Determinar esto puede ser difícil, especialmente a medida que el número de servicios vaya aumentando.
* Algunos desarrolladores lo que hacen es simular o realizar bocetos de muchas de sus dependencias de servicios. Este enfoque puede ayudar, pero administrar dichas simulaciones pronto puede afectar al costo del desarrollo. Además, puede ocurrir que el entorno de desarrollo acabe no pareciéndose en nada al de producción, lo que facilita que aparezcan errores imperceptibles.
* De todo ello se desprende que es difícil realizar cualquier prueba de integración. Las pruebas de integración solo pueden suceder de forma realista tras una confirmación, lo que significa que verá problemas más adelante en el ciclo de desarrollo.

    ![Imagen que muestra la complejidad de las pruebas de integración al ilustrar las relaciones entre un servicio de aplicaciones y sus dependencias.](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Trabajo en un espacio de desarrollo compartido
Con Azure Dev Spaces se puede configurar un espacio de desarrollo *compartido* en Azure. Cada desarrollador puede centrarse en su parte de la aplicación y desarrollar de forma iterativa *código de confirmación previa* en un espacio de desarrollo que ya contiene los restantes servicios y recursos en la nube de los que dependen sus escenarios. Las dependencias siempre están actualizadas y los desarrolladores trabajan de una manera que refleja el entorno de producción.

### <a name="work-in-your-own-space"></a>Trabajar en su propio espacio de trabajo
A medida que desarrolla código para su servicio, y antes de estar preparado para insertarlo en el repositorio, el código estará normalmente en buenas condiciones. Aún estará dándole forma, probándolo y experimentando con soluciones de manera iterativa. Azure Dev Spaces proporciona el concepto de un **espacio**, que le permite trabajar de forma aislada sin miedo a que le interrumpan los miembros de su equipo.

## <a name="use-dev-spaces-for-team-development"></a>Uso de Dev Spaces para el desarrollo en equipo
Vamos a explicar estas ideas con un ejemplo concreto utilizando nuestra aplicación de ejemplo *webfrontend* -> *mywebapi*. Imaginemos un escenario donde un desarrollador, Scott, tiene que realizar un cambio en el servicio *mywebapi*, y *solo* en ese servicio. *Webfrontend* no tiene que cambiar como parte de la actualización de Scott.

_Sin_ usar Dev Spaces, Scott tendría varias maneras de desarrollar y probar su actualización, pero ninguna de ellas es ideal:
* Ejecutar TODOS los componentes localmente. Esto requiere una máquina de desarrollo más potente con Docker instalado y, potencialmente, MiniKube.
* Ejecutar TODOS los componentes en un espacio de nombres aislado en el clúster de Kubernetes. Puesto que *webfrontend* no va a cambiar, esto es un desperdicio de los recursos de clúster.
* Ejecutar SOLO *mywebapi* y realizar llamadas REST manuales para probar. Este método no prueba el flujo completo.
* Agregar código centrado en el desarrollo a *webfrontend* que permita al desarrollador enviar solicitudes a una instancia diferente de *mywebapi*. Esto complica el servicio *webfrontend*.

### <a name="set-up-your-baseline"></a>Establecimiento de la base de referencia
En primer lugar, tenemos que implementar una base de referencia de nuestros servicios. Esta implementación representará el "último punto correcto conocido" para poder comparar fácilmente el comportamiento del código local frente a la versión insertada en el repositorio. A continuación, crearemos un espacio secundario asentado en esta base de referencia, para que podamos probar los cambios de *mywebapi* dentro del contexto de la aplicación más grande.

1. Clone [la aplicación de ejemplo de Dev Spaces](https://github.com/Azure/dev-spaces): `git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. Extraiga del repositorio la rama remota *azds_updates*: `git checkout -b azds_updates origin/azds_updates`
1. Seleccione el espacio _dev_: `azds space select --name dev`. Cuando se le pida que seleccione un espacio de desarrollo principal, elija _\<none\>_ .
1. Vaya hasta el directorio _mywebapi_ y ejecute: `azds up -d`
1. Vaya al directorio _webfrontend_ y ejecute: `azds up -d`
1. Ejecute `azds list-uris` para ver el punto de conexión público para _webfrontend_

> [!TIP]
> Los pasos anteriores configuran manualmente una base de referencia, pero se recomienda a los equipos el uso de CI/CD para mantener al día automáticamente la base de referencia con el código confirmado.
>
> Visite nuestra [guía para configurar CI/CD con Azure DevOps](how-to/setup-cicd.md) con el fin de crear un flujo de trabajo similar al del siguiente diagrama.
>
> ![Diagrama de CI/CD de ejemplo](media/common/ci-cd-complex.png)

En este momento la base de referencia debe estar ejecutándose. Ejecute el comando`azds list-up --all` y verá una salida similar a la siguiente:

```
$ azds list-up --all

Name                          DevSpace  Type     Updated  Status
----------------------------  --------  -------  -------  -------
mywebapi                      dev       Service  3m ago   Running
mywebapi-56c8f45d9-zs4mw      dev       Pod      3m ago   Running
webfrontend                   dev       Service  1m ago   Running
webfrontend-6b6ddbb98f-fgvnc  dev       Pod      1m ago   Running
```

La columna DevSpace muestra que ambos servicios se están ejecutando en un espacio denominado _dev_. Cualquiera que abra la dirección URL pública y vaya a la aplicación web, invocará la ruta de acceso al código insertado en el repositorio y que se ejecuta mediante ambos servicios. Ahora suponga que quiere seguir desarrollando _mywebapi_. ¿Cómo puede realizar cambios de código y probarlos y sin interrumpir a otros desarrolladores que usan el entorno de desarrollo? Para ello, configurará su propio espacio.

### <a name="create-a-dev-space"></a>Creación de un espacio de desarrollo
Para ejecutar su propia versión de _mywebapi_ en un espacio que no sea _dev_, puede crear su propio espacio mediante el comando siguiente:

```cmd
azds space select --name scott
```

Cuando se le solicite, seleccione _dev_ como el **espacio de desarrollo principal**. Esto significa que nuestro nuevo espacio _dev/scott_, derivará del espacio _dev_. En breve veremos cómo nos ayudará esto con las pruebas.

Manteniendo nuestro caso hipotético de introducción, hemos usado el nombre _scott_ en el nuevo espacio para que los compañeros puedan identificar quién está trabajando en él. Pero le puede dar cualquier nombre que desee, con flexibilidad sobre lo que significa, como _sprint4_ o _demo_. En cualquier caso, _dev_ actúa como la base de referencia para todos los programadores que trabajan en una parte de esta aplicación:

![Diagrama que muestra un espacio de desarrollo sencillo.](media/common/ci-cd-space-setup.png)

Ejecute el comando `azds space list` para ver una lista de todos los espacios del entorno de desarrollo. La columna _Seleccionado_ indica el espacio de que ha seleccionado actualmente (true/false). En su caso, el espacio denominado _dev/scott_ se seleccionó automáticamente al crearse. Puede seleccionar otro espacio en cualquier momento con el comando `azds space select`.

Vamos a verlo en acción.

### <a name="make-a-code-change"></a>Cambio de código
Vaya a la ventana de VS Code para `mywebapi` y realice una edición de código para el controlador GET `/` predeterminado en `server.js`, por ejemplo:

```javascript
app.get('/', function (req, res) {
    res.send('mywebapi now says something new');
});
```

### <a name="run-the-service"></a>Ejecución del servicio

Presione la tecla F5 (o escriba `azds up` en la ventana de terminal) para ejecutar el servicio. El servicio se ejecutará automáticamente en el espacio recién seleccionado _dev/scott_. Confirme que el servicio se ejecuta en su propio espacio mediante la ejecución de `azds list-up`:

```cmd
$ azds list-up

Name                      DevSpace  Type     Updated  Status
mywebapi                  scott     Service  3m ago   Running
webfrontend               dev       Service  26m ago  Running
```

Observe que hay una instancia de *mywebapi* en ejecución ahora en el espacio _dev/scott_. La versión que se ejecuta en _dev_ sigue ejecutándose, pero no aparece en la lista.

Enumere las direcciones URL del espacio actual ejecutando `azds list-uris`.

```cmd
$ azds list-uris

Uri                                                                        Status
-------------------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                                Tunneled
http://scott.s.dev.webfrontend.6364744826e042319629.ce.azds.io/  Available
```

Observe que la dirección URL de punto de acceso público de *webfrontend* incluye el prefijo *scott.s*. Esta dirección URL es única para el espacio _dev/scott_. Este prefijo de dirección URL indica el controlador de entrada para enrutar las solicitudes a la versión _dev/scott_ de un servicio. Cuando Dev Spaces administra una solicitud con esta dirección URL, el controlador de entrada intenta primero enrutar la solicitud al servicio *webfrontend* del espacio _dev/scott_. Si se produce un error, la solicitud se enrutará al servicio *webfrontend* del espacio _dev_ como reserva. Observe también que hay una dirección URL de localhost para acceder al servicio a través de localhost mediante la funcionalidad de *reenvío de puerto* de Kubernetes. Para más información acerca de las direcciones URL y el enrutamiento en Azure Dev Spaces, consulte [How Azure Dev Spaces works and is configured](how-dev-spaces-works.md) (Funcionamiento y configuración de Azure Dev Spaces).

![Enrutamiento de espacio](media/common/Space-Routing.png)

Esta característica integrada de Azure Dev Spaces le permite probar el código en un espacio compartido sin que sea necesario que cada desarrollador vuelva a crear la pila completa de servicios en su espacio. Este enrutamiento requiere que el código de la aplicación reenvíe encabezados de propagación, como se ilustra en el paso anterior de esta guía.

### <a name="test-code-in-a-space"></a>Prueba de código en un espacio
Para probar la nueva versión de *mywebapi* junto con *webfrontend*, abra el explorador en la dirección URL del punto de acceso público de *webfrontend* y vaya a la página Acerca de. Debería ver el nuevo mensaje.

Ahora, quite la parte "scott.s." de la dirección URL y actualice el explorador. Debería ver el comportamiento anterior (con la versión de *mywebapi* que se ejecuta en _dev_).

Una vez que tenga un espacio _dev_ que contenga siempre los cambios más recientes, y suponiendo que la aplicación está diseñada para aprovechar las ventajas del enrutamiento basado en espacios de DevSpace, como se describe en esta sección del tutorial, debería resulta fácil ver cómo Dev Spaces puede ayudar en gran medida a probar las nuevas características en el contexto de la aplicación más grande. En lugar de tener que implementar _todos_ los servicios en su espacio privado, puede crear un espacio privado que se derive de _dev_y solo "subir" los servicios en los que realmente esté trabajando. La infraestructura de enrutamiento de Dev Spaces controlará el resto mediante el uso de todos los servicios de su espacio privado que pueda encontrar, mientras se usará por defecto la versión más reciente que se ejecuta en el espacio _dev_. Y aún mejor _varios_ desarrolladores pueden desarrollar activamente servicios diferentes al mismo tiempo en su propio espacio sin interrumpirse entre sí.

### <a name="well-done"></a>¡Listo!
¡Ha completado la guía de introducción! Ha aprendido a:

> [!div class="checklist"]
> * Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
> * Desarrollar código de forma iterativa en contenedores.
> * Desarrollar de forma independiente dos servicios distintos y usar la detección de servicios DNS de Kubernetes para realizar una llamada a otro servicio.
> * Desarrollar y probar de forma productiva el código en un entorno de equipo.
> * Establecer una base de referencia de la funcionalidad con el uso de Dev Spaces para probar fácilmente los cambios aislados dentro del contexto de una aplicación de microservicio más grande

Ahora que ha explorado Azure Dev Spaces, [comparta su espacio de desarrollo con un miembro del equipo](how-to/share-dev-spaces.md) y comience a colaborar.

## <a name="clean-up"></a>Limpieza
Para eliminar completamente una instancia de Azure Dev Spaces en un clúster, incluidos todos los espacios de desarrollo y los servicios en ejecución dentro de él, utilice el comando `az aks remove-dev-spaces`. Tenga en cuenta que esta acción es irreversible. Puede agregar compatibilidad para Azure Dev Spaces de nuevo en el clúster, pero será como si comienza de nuevo. No se restaurarán los servicios y espacios anteriores.

En el ejemplo siguiente se enumeran los controladores de Azure Dev Spaces en la suscripción activa y, a continuación, se elimina el controlador de Azure Dev Spaces que está asociado con el clúster de AKS "myaks" en el grupo de recursos "myaks-rg".

```cmd
azds controller list
```

```azurecli
az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
