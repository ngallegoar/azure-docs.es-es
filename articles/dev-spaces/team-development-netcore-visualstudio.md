---
title: Desarrollo en equipo mediante .NET Core y Visual Studio
services: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
author: DrEsteban
ms.author: stevenry
ms.date: 12/09/2018
ms.topic: tutorial
description: En este tutorial se muestra cómo usar Azure Dev Spaces y Visual Studio para el desarrollo en equipo en una aplicación .NET Core en Azure Kubernetes Service
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s '
ms.openlocfilehash: 5d917dc71ef02b5197ed8d20ec31c538a1af4c14
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87072973"
---
# <a name="team-development-using-net-core-and-visual-studio-with-azure-dev-spaces"></a>Desarrollo en equipo mediante .NET Core y Visual Studio con Azure Dev Spaces

En este tutorial, aprenderá cómo un equipo de desarrolladores puede colaborar de forma simultánea en el mismo clúster de Kubernetes gracias a Dev Spaces.

## <a name="learn-about-team-development"></a>Aprenda sobre el desarrollo en equipo.

Hasta ahora, ha ejecutado el código de la aplicación como si fuera el único desarrollador que trabaja en la aplicación. En esta sección, aprenderá cómo Azure Dev Spaces simplifica el desarrollo en equipo:
* Habilite un equipo de desarrolladores para trabajar en el mismo entorno, trabajando en un espacio de desarrollo compartido o en espacios de desarrollo distintos, según sea necesario.
* Permitirá que cada desarrollador efectúe una iteración en su código de forma aislada y sin peligro de interrumpir a otros desarrolladores.
* Probará el código de un extremo a otro, antes de confirmarlo, sin tener que crear bocetos o dependencias simuladas.

### <a name="challenges-with-developing-microservices"></a>Dificultades con el desarrollo de microservicios
Por el momento, la aplicación de ejemplo no es compleja. Pero en el trabajo de desarrollo del mundo real, las dificultades pronto surgen a medida que se agregan más servicios y crece el equipo de desarrollo.

* La máquina de desarrollo puede no tener recursos suficientes para ejecutar a la vez cada uno de los servicios que necesite.
* Algunos servicios pueden tener que estar accesibles públicamente. Por ejemplo, un servicio puede necesitar tener un punto de conexión que responda a un webhook.
* Si desea ejecutar un subconjunto de servicios, tiene que conocer la jerarquía de dependencias completa entre todos los servicios. Determinar esta jerarquía puede ser difícil, especialmente a medida que el número de servicios vaya aumentando.
* Algunos desarrolladores lo que hacen es simular o realizar bocetos de muchas de sus dependencias de servicios. Este enfoque puede ayudar, pero administrar dichas simulaciones pronto puede afectar al costo del desarrollo. Además, puede ocurrir que el entorno de desarrollo acabe no pareciéndose al de producción, lo que puede llevar a que se produzcan errores imperceptibles.
* De todo ello se desprende que es difícil realizar cualquier prueba de integración. Las pruebas de integración solo pueden suceder de forma realista tras una confirmación, lo que significa que verá problemas más adelante en el ciclo de desarrollo.

    ![Imagen que muestra la complejidad de las pruebas de integración al ilustrar las relaciones entre un servicio de aplicaciones y sus dependencias.](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>Trabajo en un espacio de desarrollo compartido
Con Azure Dev Spaces se puede configurar un espacio de desarrollo *compartido* en Azure. Cada desarrollador puede centrarse en su parte de la aplicación y desarrollar de forma iterativa *código de confirmación previa* en un espacio de desarrollo que ya contiene los restantes servicios y recursos en la nube de los que dependen sus escenarios. Las dependencias siempre están actualizadas y los desarrolladores trabajan de una manera que refleja el entorno de producción.

### <a name="work-in-your-own-space"></a>Trabajar en su propio espacio de trabajo
A medida que desarrolla código para su servicio, y antes de estar preparado para insertarlo en el repositorio, el código estará normalmente en buenas condiciones. Aún estará dándole forma, probándolo y experimentando con soluciones de manera iterativa. Azure Dev Spaces proporciona el concepto de un **espacio**, que le permite trabajar de forma aislada sin miedo a que le interrumpan los miembros de su equipo.

## <a name="use-dev-spaces-for-team-development"></a>Uso de Dev Spaces para el desarrollo en equipo
Vamos a explicar estas ideas con un ejemplo concreto utilizando nuestra aplicación de ejemplo *webfrontend* -> *mywebapi*. Imaginemos un escenario donde un desarrollador, Scott, tiene que realizar un cambio en el servicio *mywebapi*, y *solo* en ese servicio. *Webfrontend* no tiene que cambiar como parte de la actualización de Scott.

_Sin_ usar Dev Spaces, Scott tendría varias maneras de desarrollar y probar su actualización, pero ninguna de ellas es ideal:
* Ejecutar TODOS los componentes de forma local, lo que requiere una máquina de desarrollo más potente con Docker instalado y, potencialmente, MiniKube.
* Ejecutar TODOS los componentes en un espacio de nombres aislado en el clúster de Kubernetes. Puesto que *webfrontend* no va a cambiar, usar un espacio de nombres aislado es un desperdicio de los recursos de clúster.
* Ejecutar SOLO *mywebapi* y realizar llamadas REST manuales para probar. Este tipo de prueba no comprueba el flujo completo.
* Agregar código centrado en el desarrollo a *webfrontend* que permita al desarrollador enviar solicitudes a una instancia diferente de *mywebapi*. La incorporación de este código complica el servicio *webfrontend*.

### <a name="set-up-your-baseline"></a>Establecimiento de la base de referencia
En primer lugar, tenemos que implementar una base de referencia de nuestros servicios. Esta implementación representará el "último punto correcto conocido" para poder comparar fácilmente el comportamiento del código local frente a la versión insertada en el repositorio. A continuación, crearemos un espacio secundario asentado en esta base de referencia, para que podamos probar los cambios de *mywebapi* dentro del contexto de la aplicación más grande.

1. Clone [la aplicación de ejemplo de Dev Spaces](https://github.com/Azure/dev-spaces): `git clone https://github.com/Azure/dev-spaces && cd dev-spaces`
1. Extraiga del repositorio la rama remota *azds_updates*: `git checkout -b azds_updates origin/azds_updates`
1. Cierre las sesiones de depuración o de F5 en ambos servicios, pero mantenga abierto el proyecto en sus ventanas de Visual Studio.
1. Cambie a la ventana de Visual Studio con el proyecto _mywebapi_.
1. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y seleccione **Propiedades**.
1. Seleccione la pestaña **Depurar** a la izquierda para mostrar la configuración de Azure Dev Spaces.
1. Seleccione **Cambiar** para crear el espacio que se usarán al presionar F5 o Ctrl+F5 para su servicio.
1. En el cuadro desplegable Espacio, seleccione **\<Create New Space…\>** .
1. Asegúrese de que el espacio primario se establece en **\<none\>** , y escriba el nombre de espacio **dev**. Haga clic en Aceptar.
1. Presione Ctrl + F5 para ejecutar _mywebapi_ sin el depurador asociado.
1. Cambie a la ventana de Visual Studio con el proyecto _webfrontend_ y presione Ctrl+F5 para ejecutarlo también.

> [!Note]
> En ocasiones es necesario actualizar el explorador después de que la página web se muestra inicialmente a continuación de una operación Ctrl+F5.

> [!TIP]
> Los pasos anteriores configuran manualmente una base de referencia, pero se recomienda a los equipos el uso de CI/CD para mantener al día automáticamente la base de referencia con el código confirmado.
>
> Visite nuestra [guía para configurar CI/CD con Azure DevOps](how-to/setup-cicd.md) con el fin de crear un flujo de trabajo similar al del siguiente diagrama.
>
> ![Diagrama de CI/CD de ejemplo](media/common/ci-cd-complex.png)

Cualquiera que abra la dirección URL pública y vaya a la aplicación web, invocará la ruta de acceso al código que ha escrito, que recorrerá ambos servicios mediante el espacio predeterminado _dev_. Ahora, suponga que quiere seguir desarrollando *mywebapi* ¿cómo podría hacerlo sin interrumpir a otros desarrolladores que usan el espacio de desarrollo? Para ello, configurará su propio espacio.

### <a name="create-a-new-dev-space"></a>Creación de un nuevo espacio de desarrollo
En Visual Studio, puede crear espacios adicionales que se usarán al presionar F5 o Ctrl+F5 para su servicio. Puede llamar a un espacio como quiera y puede ser flexible sobre lo que significa (p. ej. _sprint4_ o _demo_).

Siga estos pasos para crear un nuevo espacio:
1. Cambie a la ventana de Visual Studio con el proyecto *mywebapi*.
2. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y seleccione **Propiedades**.
3. Seleccione la pestaña **Depurar** a la izquierda para mostrar la configuración de Azure Dev Spaces.
4. Desde aquí, puede cambiar o crear el clúster o el espacio que se usará al presionar F5 o Ctrl+F5. *Asegúrese de que el espacio de Azure Dev Spaces que creó anteriormente está seleccionado*.
5. En el cuadro desplegable Espacio, seleccione **\<Create New Space…\>** .

    ![Captura de pantalla que muestra la selección de "Crear un espacio" en el cuadro desplegable Espacio en la página de propiedades del proyecto de depuración del Explorador de soluciones de Visual Studio.](media/get-started-netcore-visualstudio/Settings.png)

6. En el cuadro de diálogo **Agregar espacio**, configure el espacio principal como **dev** y escriba el nombre del nuevo espacio. Puede usar su nombre (por ejemplo, "scott") para el nuevo espacio de forma que sus compañeros puedan identificar fácilmente el espacio en el que está trabajando. Haga clic en **OK**.

    ![Captura de pantalla que muestra el cuadro de diálogo Agregar espacio para añadir un nuevo espacio de desarrollo para el desarrollo en equipo.](media/get-started-netcore-visualstudio/AddSpace.png)

7. Ahora verá el clúster de AKS y el nuevo espacio seleccionados en la página de propiedades del proyecto.

    ![Captura de pantalla que muestra el clúster de AKS "MyAKS" y el espacio "scott" seleccionado en la página de propiedades del proyecto de depuración del Explorador de soluciones de Visual Studio.](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>Actualización del código de *mywebapi*

1. En el proyecto *mywebapi*, realice un cambio de código al método `string Get(int id)` en el archivo `Controllers/ValuesController.cs` de la manera siguiente:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Establezca un punto de interrupción en este bloque de código actualizado (quizás ya tenga uno establecido desde antes).
3. Presione F5 para iniciar el servicio _mywebapise_, que se iniciará en su clúster mediante el espacio seleccionado. El espacio seleccionado es _scott_.

Este es un diagrama que le ayudará a comprender cómo funcionan los distintos espacios. La ruta de acceso morada muestra una solicitud a través del espacio _dev_, que es la ruta de acceso predeterminada que se usa si no hay ningún espacio antepuesto a la dirección URL. La ruta de acceso rosa muestra una solicitud a través del espacio _dev/scott_.

![Diagrama que muestra las diferencias en los nombres de ruta de acceso y el enrutamiento de solicitudes mediante el espacio "dev/scott" recién creado y el espacio "dev" predeterminado.](media/common/Space-Routing.png)

Esta funcionalidad integrada de Azure Dev Spaces le permite probar el código de un extremo a otro de un entorno compartido sin necesidad de que cada desarrollador vuelva a crear la pila completa de servicios en su espacio. Este enrutamiento requiere el reenvío de encabezados de propagación en el código de aplicación, como se ilustra en el paso anterior de esta guía.

### <a name="test-code-running-in-the-_devscott_-space"></a>Prueba del código que se ejecuta en el espacio _dev/scott_
Para probar la nueva versión de *mywebapi* junto con *webfrontend*, abra el explorador en la dirección URL del punto de acceso público para *webfrontend*, por ejemplo `http://dev.webfrontend.123456abcdef.eus.azds.io` y vaya a la página Acerca de. Verá el mensaje original "Hello from webfrontend and Hello from mywebapi".

Ahora, agregue la parte "scott.s." a la dirección URL para que muestre algo parecido a http\://scott.s.dev.webfrontend.123456abcdef.eus.azds.io y actualice el explorador. Se llegará al punto de interrupción establecido en el proyecto *mywebapi*. Presione F5 para continuar; en el explorador debería ver el nuevo mensaje "Hello from webfrontend and mywebapi now says something new". El motivo es que la ruta de acceso al código actualizado en *mywebapi* se ejecuta en el espacio _dev/scott_.

Una vez que tenga un espacio _dev_ que contenga siempre los cambios más recientes, y suponiendo que la aplicación está diseñada para aprovechar las ventajas del enrutamiento basado en espacios de DevSpace, como se describe en esta sección del tutorial, debería resulta fácil ver cómo Dev Spaces puede ayudar en gran medida a probar las nuevas características en el contexto de la aplicación más grande. En lugar de tener que implementar _todos_ los servicios en su espacio privado, puede crear un espacio privado que se derive de _dev_y solo "subir" los servicios en los que realmente esté trabajando. La infraestructura de enrutamiento de Dev Spaces controlará el resto mediante el uso de todos los servicios de su espacio privado que pueda encontrar, mientras se usará por defecto la versión más reciente que se ejecuta en el espacio _dev_. Y aún mejor _varios_ desarrolladores pueden desarrollar activamente servicios diferentes al mismo tiempo en su propio espacio sin interrumpirse entre sí.

### <a name="well-done"></a>¡Listo!
¡Ha completado la guía de introducción! Ha aprendido a:

> [!div class="checklist"]
> * Configurar Azure Dev Spaces con un clúster de Kubernetes administrado en Azure.
> * Desarrollar código de forma iterativa en contenedores.
> * Desarrollar de forma independiente dos servicios distintos y usar la detección de servicios DNS de Kubernetes para realizar una llamada a otro servicio.
> * Desarrollar y probar de forma productiva el código en un entorno de equipo.
> * Establecer una base de referencia de la funcionalidad con el uso de Dev Spaces para probar fácilmente los cambios aislados dentro del contexto de una aplicación de microservicio más grande

Ahora que ha explorado Azure Dev Spaces, [comparta su espacio de desarrollo con un miembro del equipo](how-to/share-dev-spaces.md) y ayúdele a ver lo fácil que es colaborar con otros.

## <a name="clean-up"></a>Limpieza
Para eliminar completamente una instancia de Azure Dev Spaces en un clúster, incluidos todos los espacios de desarrollo y los servicios en ejecución dentro de él, utilice el comando `az aks remove-dev-spaces`. Tenga en cuenta que esta acción es irreversible. Puede agregar compatibilidad para Azure Dev Spaces de nuevo en el clúster, pero será como si comienza de nuevo. No se restaurarán los servicios y espacios anteriores.

En el ejemplo siguiente se enumeran los controladores de Azure Dev Spaces en la suscripción activa y, a continuación, se elimina el controlador de Azure Dev Spaces que está asociado con el clúster de AKS "myaks" en el grupo de recursos "myaks-rg".

```cmd
azds controller list
```

```azurecli
az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```
