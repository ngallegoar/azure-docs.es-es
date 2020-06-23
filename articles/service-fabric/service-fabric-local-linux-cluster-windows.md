---
title: Configuración de un clúster Linux de Azure Service Fabric en Windows
description: En este artículo se explica cómo configurar los clústeres Linux de Service Fabric que se ejecutan en máquinas de desarrollo de Windows. Esto es especialmente útil para el desarrollo multiplataforma.
author: suhuruli
ms.topic: conceptual
ms.date: 11/20/2017
ms.author: suhuruli
ms.openlocfilehash: 1798d57f9fc4004fd2c7d1f1901fcddf850626c2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82193333"
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Configuración de un clúster Linux de Service Fabric en la máquina del desarrollador de Windows

Este documento describe cómo configurar una instancia local de Service Fabric para Linux en máquinas de desarrollo Windows. La configuración de un clúster Linux local es útil para probar rápidamente las aplicaciones que se destinan a clústeres Linux, pero se desarrollan en un equipo Windows.

## <a name="prerequisites"></a>Requisitos previos
Los clústeres de Service Fabric basados en Linux no se ejecutan de forma nativa en Windows. Para ejecutar un clúster local de Service Fabric, se proporciona una imagen preconfigurada de contenedor de Docker. Antes de comenzar, necesita:

* Al menos 4 GB de RAM
* Versión más reciente de [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)
* Docker debe ejecutarse en modo de Linux

>[!TIP]
> * Puede seguir los pasos mencionados en la [documentación](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) oficial de Docker para instalar Docker en Windows. 
> * Una vez que haya terminado la instalación, compruebe si se instaló correctamente siguiendo los pasos mencionados [aquí](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine)


## <a name="create-a-local-container-and-setup-service-fabric"></a>Creación de un contenedor local e instalación de Service Fabric
Para configurar un contenedor local de Docker y hacer que un clúster de Service Fabric se ejecute en él, realice los pasos siguientes en PowerShell:


1. Actualice la configuración del demonio de Docker en el host con lo siguiente y reinicie dicho demonio: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    La manera aconsejable de actualizar es ir al icono de Docker > Configuración > Demonio > Avanzada y realizar allí la actualización. Luego reinicie el demonio de Docker para que los cambios surtan efecto. 

2. En un directorio nuevo, cree un archivo denominado `Dockerfile` para generar una imagen de Service Fabric:

    ```Dockerfile
    FROM mcr.microsoft.com/service-fabric/onebox:latest
    WORKDIR /home/ClusterDeployer
    RUN ./setup.sh
    #Generate the local
    RUN locale-gen en_US.UTF-8
    #Set environment variables
    ENV LANG=en_US.UTF-8
    ENV LANGUAGE=en_US:en
    ENV LC_ALL=en_US.UTF-8
    EXPOSE 19080 19000 80 443
    #Start SSH before running the cluster
    CMD /etc/init.d/ssh start && ./run.sh
    ```

    >[!NOTE]
    >Dicho archivo se puede adaptar para agregar otros programas o dependencias al contenedor.
    >Por ejemplo, si se agrega `RUN apt-get install nodejs -y`, será posible usar aplicaciones `nodejs` como ejecutables de invitado.
    
    >[!TIP]
    > De forma predeterminada, se extraerá la imagen con la versión más reciente de Service Fabric. Para ver información acerca de revisiones concretas, visite la página de [Docker Hub](https://hub.docker.com/r/microsoft/service-fabric-onebox/)

3. Para crear una imagen reutilizable a partir de `Dockerfile`, abra una ventana de terminal y ejecute `cd` para cambiar al directorio donde se encuentra su `Dockerfile` y, después, ejecute:

    ```powershell 
    docker build -t mysfcluster .
    ```
    
    >[!NOTE]
    >Esta operación tardará un tiempo, pero solo se necesita una vez.

4. Ya se puede iniciar rápidamente una copia local de Service Fabric, siempre que se necesite. Para ello, hay que ejecutar:

    ```powershell 
    docker run --name sftestcluster -d -v //var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mysfcluster
    ```

    >[!TIP]
    >Al especificar un nombre para la instancia del contenedor, puede tratarlo de forma más legible. 
    >
    >Si la aplicación está escuchando en determinados puertos, estos deben especificarse mediante etiquetas `-p` adicionales. Por ejemplo, si la aplicación está escuchando en el puerto 8080, agregue la siguiente etiqueta `-p`:
    >
    >`docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox mcr.microsoft.com/service-fabric/onebox:latest`
    >

5. El clúster tardará un poco de tiempo en iniciarse; puede ver los registros con el comando siguiente o pasar al panel para ver el mantenimiento de los clústeres `http://localhost:19080`:

    ```powershell 
    docker logs sftestcluster
    ```

6. Tras completar correctamente el paso 5, puede ir a ``http://localhost:19080`` desde su equipo Windows y podría ver Service Fabric Explorer. En este momento, puede conectarse a este clúster mediante cualquier herramienta desde la máquina del desarrollador de Windows e implementar aplicaciones destinadas a los clústeres de Service Fabric para Linux. 

    > [!NOTE]
    > El complemento de Eclipse no se admite actualmente en Windows. 

7. Cuando haya acabado, puede detener y limpiar el contenedor con este comando:

    ```powershell 
    docker rm -f sftestcluster
    ```

### <a name="known-limitations"></a>Limitaciones conocidas 
 
 A continuación se muestran limitaciones conocidas del clúster local que se ejecuta en un contenedor para dispositivos Mac: 
 
 * El servicio DNS no se ejecuta y no es compatible [Problema 132](https://github.com/Microsoft/service-fabric/issues/132)

## <a name="next-steps"></a>Pasos siguientes
* Introducción a [Eclipse](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-eclipse)
* Consulte otros [ejemplos de Java](https://github.com/Azure-Samples/service-fabric-java-getting-started).


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
