---
title: Configuración de un clúster Linux de Azure Service Fabric en Windows
description: En este artículo se explica cómo configurar los clústeres Linux de Service Fabric que se ejecutan en máquinas de desarrollo de Windows. Este enfoque es especialmente útil para el desarrollo multiplataforma.
ms.topic: conceptual
ms.date: 10/16/2020
ms.openlocfilehash: 7b25a84e76773baea9f17430df1b7ba13aa661aa
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93087084"
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Configuración de un clúster Linux de Service Fabric en la máquina del desarrollador de Windows

En este documento se describe cómo configurar un clúster local de Service Fabric para Linux en una máquina de desarrollo Windows. La configuración de un clúster Linux local es útil para probar rápidamente las aplicaciones que se destinan a clústeres Linux, pero se desarrollan en un equipo Windows.

## <a name="prerequisites"></a>Requisitos previos
Los clústeres de Service Fabric basados en Linux no se ejecutan en Windows, pero para habilitar la creación de prototipos multiplataforma, se proporciona un contenedor de Docker con clústeres de máquina única Linux de Service Fabric, que se puede implementar mediante Docker para Windows.

Antes de comenzar, necesita:

* Al menos 4 GB de RAM
* La versión más reciente de [Docker para Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)
* Docker debe ejecutarse en modo de contenedores de Linux

>[!TIP]
> Para instalar Docker en una máquina Windows, siga los pasos que se indican en la [documentación de Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions). Después de instalar, [compruebe la instalación](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine).
>

## <a name="create-a-local-container-and-setup-service-fabric"></a>Creación de un contenedor local e instalación de Service Fabric
Para configurar un contenedor de Docker local y hacer que un clúster de Service Fabric se ejecute en él, siga estos pasos:


1. Actualice la configuración del demonio de Docker en el host con lo siguiente y reinicie dicho demonio: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    La forma recomendada de actualizarla es ir a: 

    * Icono de Docker > Configuración > Motor de Docker.
    * Agregar los nuevos campos enumerados anteriormente.
    * Aplicar y reiniciar: reinicie el demonio de Docker para que los cambios surtan efecto.

2. Inicie el clúster mediante PowerShell.<br/>
    <b>Ubuntu 18.04 LTS:</b>
    ```powershell
    docker run --name sftestcluster -d -v /var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mcr.microsoft.com/service-fabric/onebox:u18
    ```

    <b>Ubuntu 16.04 LTS:</b>
    ```powershell
    docker run --name sftestcluster -d -v /var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mcr.microsoft.com/service-fabric/onebox:u16
    ```

    >[!TIP]
    > De forma predeterminada, se extraerá la imagen con la versión más reciente de Service Fabric. Para ver revisiones concretas, consulte la página [Service Fabric OneBox](https://hub.docker.com/_/microsoft-service-fabric-onebox) en Docker Hub.



3. Opcional: cree una imagen de Service Fabric extendida.

    En un directorio nuevo, cree un archivo denominado `Dockerfile` para crear la imagen personalizada:

    >[!NOTE]
    >Dicha imagen se puede adaptar con un Dockerfile para agregar programas o dependencias al contenedor.
    >Por ejemplo, si se agrega `RUN apt-get install nodejs -y`, será posible usar aplicaciones `nodejs` como ejecutables de invitado.
    ```Dockerfile
    FROM mcr.microsoft.com/service-fabric/onebox:u18
    RUN apt-get install nodejs -y
    EXPOSE 19080 19000 80 443
    WORKDIR /home/ClusterDeployer
    CMD ["./ClusterDeployer.sh"]
    ```
    
    >[!TIP]
    > De forma predeterminada, se extraerá la imagen con la versión más reciente de Service Fabric. Para revisiones concretas, visite la página [Docker Hub](https://hub.docker.com/r/microsoft/service-fabric-onebox/).

    Para crear una imagen reutilizable a partir de `Dockerfile`, abra una ventana de terminal y ejecute `cd` para cambiar al directorio donde se encuentra su `Dockerfile` y, después, ejecute:

    ```powershell 
    docker build -t mysfcluster .
    ```
    
    >[!NOTE]
    >Esta operación tardará un tiempo, pero solo se necesita una vez.

    Ya se puede iniciar rápidamente una copia local de Service Fabric, siempre que se necesite. Para ello, hay que ejecutar:

    ```powershell 
    docker run --name sftestcluster -d -v /var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mysfcluster
    ```

    >[!TIP]
    >Al especificar un nombre para la instancia del contenedor, puede tratarlo de forma más legible. 
    >
    >Si la aplicación está escuchando en determinados puertos, estos deben especificarse mediante etiquetas `-p` adicionales. Por ejemplo, si la aplicación está escuchando en el puerto 8080, agregue la siguiente etiqueta `-p`:
    >
    >`docker run -itd -p 19000:19000 -p 19080:19080 -p 8080:8080 --name sfonebox mcr.microsoft.com/service-fabric/onebox:u18`
    >


4. El clúster tardará un poco de tiempo en iniciarse; puede ver los registros con el comando siguiente o pasar al panel para ver el mantenimiento de los clústeres `http://localhost:19080`:

    ```powershell 
    docker logs sftestcluster
    ```

5. Una vez que el clúster se implementa correctamente, como se ve en el paso 4, puede ir a ``http://localhost:19080`` desde su máquina Windows para buscar el panel de Service Fabric Explorer. En este momento, puede conectarse a este clúster mediante las herramientas de la máquina del desarrollador de Windows e implementar aplicaciones destinadas a los clústeres de Service Fabric para Linux. 

    > [!NOTE]
    > El complemento de Eclipse no se admite actualmente en Windows. 

6. Cuando haya acabado, detenga el contenedor y límpielo con este comando:

    ```powershell 
    docker rm -f sftestcluster
    ```

### <a name="known-limitations"></a>Limitaciones conocidas 
 
 A continuación se muestran limitaciones conocidas del clúster local que se ejecuta en un contenedor para dispositivos Mac: 
 
 * El servicio DNS no se ejecuta y actualmente no se admite dentro del contenedor. [Problema n.° 132](https://github.com/Microsoft/service-fabric/issues/132)
 * La ejecución de aplicaciones basadas en contenedores requiere que se ejecute SF en un host de Linux. Actualmente no se admiten aplicaciones de contenedor anidadas.

## <a name="next-steps"></a>Pasos siguientes
* [Creación e implementación de la primera aplicación de Java para Service Fabric en Linux con Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* Introducción a [Eclipse](./service-fabric-get-started-eclipse.md)
* Consulte otros [ejemplos de Java](https://github.com/Azure-Samples/service-fabric-java-getting-started).
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
