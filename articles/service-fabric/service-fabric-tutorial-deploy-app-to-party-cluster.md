---
title: Implementación de una aplicación de Service Fabric en un clúster de Azure
description: Aprenda a implementar una aplicación existente en un clúster de Azure Service Fabric recién creado desde Visual Studio.
author: athinanthny
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 4cd21669b30b8ec83d6a0538c90d4cec5cafe32a
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86244997"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Tutorial: Implementación de una aplicación de Service Fabric en un clúster en Azure

Este tutorial es la segunda parte de una serie. En él se muestra cómo implementar una aplicación de Azure Service Fabric en un clúster nuevo en Azure.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Crear un clúster.
> * Implementar una aplicación en un clúster remoto con Visual Studio.

En esta serie de tutoriales, se aprende a:
> [!div class="checklist"]
> * [Crear una aplicación .NET de Service Fabric](service-fabric-tutorial-create-dotnet-app.md).
> * Implementar la aplicación en un clúster remoto.
> * [Agregar un punto de conexión HTTPS a un servicio de front-end de ASP.NET Core](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md).
> * [Configuración de CI/CD con Azure Pipelines](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Configurar la supervisión y el diagnóstico de la aplicación](service-fabric-tutorial-monitoring-aspnet.md).

## <a name="prerequisites"></a>Prerequisites

Antes de empezar este tutorial:

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Instale Visual Studio 2019](https://www.visualstudio.com/) y las cargas de trabajo de **desarrollo de Azure** y de **desarrollo web y de ASP.NET**.
* [Instale el SDK de Service Fabric](service-fabric-get-started.md).

> [!NOTE]
> Es posible que una cuenta gratuita no cumpla los requisitos para crear una máquina virtual. Esto impedirá la finalización del tutorial. Además, una cuenta no profesional o no educativa puede tener problemas de permisos en la creación del certificado en el almacén de claves asociado con el clúster. Si experimenta un error relacionado con la creación de certificados, use el portal para crear el clúster en su lugar. 

## <a name="download-the-voting-sample-application"></a>Descarga de la aplicación de ejemplo de votación

Si no compiló la aplicación de ejemplo de votación en la [primera parte de esta serie de tutoriales](service-fabric-tutorial-create-dotnet-app.md), puede descargarla. En una ventana Comandos, ejecute el siguiente código para clonar el repositorio de la aplicación de ejemplo en la máquina local.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart 
```

Abra la aplicación en Visual Studio, que se ejecuta como administrador, y compile la aplicación.

## <a name="create-a-cluster"></a>Crear un clúster

Ahora que la aplicación está lista, cree un clúster de Service Fabric e implemente la aplicación en él. Un [clúster de Service Fabric](./service-fabric-deploy-anywhere.md) es un conjunto de máquinas físicas o virtuales conectadas a la red, en las que se implementan y administran los microservicios.

En este tutorial, cree un clúster de prueba de tres nodos en el entorno de desarrollo integrado de Visual Studio y, después, publique la aplicación en dicho clúster. Para obtener información acerca de cómo crear clústeres de producción, consulte el [tutorial para crear y administrar clústeres](service-fabric-tutorial-create-vnet-and-windows-cluster.md). También puede implementar la aplicación en un clúster existente que creó anteriormente a través de [Azure Portal](https://portal.azure.com), mediante scripts de [PowerShell](./scripts/service-fabric-powershell-create-secure-cluster-cert.md) o de la [CLI de Azure](./scripts/cli-create-cluster.md), o desde un [plantilla de Azure Resource Manager](service-fabric-tutorial-create-vnet-and-windows-cluster.md).

> [!NOTE]
> La aplicación de votación y muchas otras aplicaciones usan el proxy inverso de Service Fabric para comunicarse entre servicios. Los clústeres creados desde Visual Studio tienen el proxy inverso habilitado de forma predeterminada. Si va a realizar la implementación en un clúster existente, debe [habilitar el proxy inverso en el clúster](service-fabric-reverseproxy-setup.md) para que la aplicación de votación funcione.


### <a name="find-the-votingweb-service-endpoint"></a>Buscar el punto de conexión de servicio VotingWeb

El servicio web de front-end de la aplicación de votación está escuchando un puerto específico (el 8080 si ha seguido en los pasos descritos en [la primera parte de esta serie de tutoriales](service-fabric-tutorial-create-dotnet-app.md)). Cuando la aplicación se implementa en un clúster de Azure, el clúster y la aplicación se ejecutan detrás de un equilibrador de carga de Azure. El puerto de la aplicación se debe abrir en Azure Load Balancer mediante una regla. Dicha regla envía el tráfico entrante a través del equilibrador de carga al servicio web. El puerto se encuentra en el archivo **VotingWeb/PackageRoot/ServiceManifest.xml** del elemento **Endpoint**. 

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Tome nota del punto de conexión de servicio, ya que se necesita en un paso posterior.  Si va a realizar la implementación en un clúster existente, abra este puerto mediante la creación de una regla y un sondeo de equilibrio de carga en Azure Load Balancer mediante un [script de PowerShell](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) o a través del equilibrador de carga de este clúster de [Azure Portal](https://portal.azure.com).

### <a name="create-a-test-cluster-in-azure"></a>Creación de un clúster de prueba en Azure
En el Explorador de soluciones, haga clic con el botón derecho en **Voting** y seleccione **Publicar**.

En **Punto de conexión**, seleccione **Crear nuevo clúster**.  Si va a realizar la implementación en un clúster existente, seleccione el punto de conexión del clúster en la lista.  Se abre el cuadro de diálogo Crear clúster de Service Fabric.

En la pestaña **Clúster**, escriba el **nombre del clúster** (por ejemplo, "mytestcluster"), seleccione su suscripción, seleccione una región para el clúster (como Centro-sur de EE. UU.), escriba el número de nodos del clúster (se recomiendan tres nodos para un clúster de prueba) y escriba un grupo de recursos (como "mytestclustergroup"). Haga clic en **Next**.

![Crear un clúster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

En la pestaña **Certificado**, escriba la contraseña y la ruta de acceso de salida del certificado del clúster. Los certificados autofirmados se crean como los archivos PFX y se guardan en la ruta de acceso de salida especificada.  El certificado se usa tanto para la seguridad de nodo a nodo como para la de cliente a nodo.  No use un certificado autofirmado para clústeres de producción.  Este certificado lo usa Visual Studio para autenticarse con el clúster e implementar aplicaciones. Seleccione **Importar certificado** para instalar el archivo PFX en el almacén CurrentUser\My certificate del equipo.  Haga clic en **Next**.

![Crear un clúster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/certificate.png)

En la pestaña **Detalle de máquina virtual**, escriba el **nombre de usuario** y la **contraseña** de la cuenta de administrador del clúster.  Seleccione la **imagen de máquina virtual** de los nodos del clúster y el **tamaño de máquina virtual** de cada uno de los nodos del clúster.  Haga clic en la pestaña **Opciones avanzadas**.

![Crear un clúster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/vm-detail.png)

En **Puertos**, escriba el punto de conexión de servicio VotingWeb del paso anterior (por ejemplo, 8080).  Cuando se crea el clúster, se abren estos puertos en Azure Load Balancer para reenviar el tráfico al clúster.  Haga clic en **Crear** para crear el clúster, lo que tarda varios minutos.

![Crear un clúster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/advanced.png)

## <a name="publish-the-application-to-the-cluster"></a>Publicación de la aplicación en el clúster

Cuando el clúster nuevo esté listo, puede implementar la aplicación Voting directamente desde Visual Studio.

En el Explorador de soluciones, haga clic con el botón derecho en **Voting** y seleccione **Publicar**. Aparece el cuadro de diálogo **Publicar**.

En **Punto de conexión**, seleccione el punto de conexión del clúster que creó en el paso anterior.  Por ejemplo, "mytestcluster.southcentral.cloudapp.azure.com:19000". Si selecciona **Parámetros de conexión avanzada** , se debería rellenar automáticamente la información del certificado.  
![Publicar una aplicación de Service Fabric](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

Seleccione **Publicar**.

Una vez que aplicación esté implementada, abra un explorador y escriba la dirección del clúster, seguida de **:8080**. O especifique otro puerto si hay uno configurado. Un ejemplo es `http://mytestcluster.southcentral.cloudapp.azure.com:8080`. Se ve la aplicación en ejecución en el clúster de Azure. En la página web de votación, pruebe a agregar y eliminar opciones de votación y a votar por una o varias de estas opciones.

![Ejemplo de votación de Service Fabric](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Pasos siguientes
En esta parte del tutorial, ha aprendido a:

> [!div class="checklist"]
> * Crear un clúster.
> * Implementar una aplicación en un clúster remoto con Visual Studio.

Avance hasta el siguiente tutorial:
> [!div class="nextstepaction"]
> [Habilitamiento de HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
