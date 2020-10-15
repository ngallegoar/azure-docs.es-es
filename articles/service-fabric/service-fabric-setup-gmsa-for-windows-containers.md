---
title: Configuración de gMSA para servicios de contenedor de Azure Service Fabric
description: Aprenda ahora a configurar cuentas de servicio administradas de grupo (gMSA) para un contenedor que se ejecuta en Azure Service Fabric.
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: d34b4c6e11628b6a4843f8a9077ebf69c9e023fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86260878"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Configuración de gMSA para contenedores de Windows que se ejecutan en Service Fabric

Para configurar la gMSA (cuentas de servicio administradas de grupo), se coloca un archivo de especificación de credenciales (`credspec`) en todos los nodos del clúster. El archivo se puede copiar en todos los nodos mediante una extensión de VM.  El archivo `credspec` debe contener la información de cuenta de gMSA. Para obtener más información sobre el archivo `credspec`, consulte [Crear una especificación de credenciales](/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec). La especificación de credenciales y la etiqueta `Hostname` se especifican en el manifiesto de aplicación. La etiqueta `Hostname` debe coincidir con el nombre de cuenta de gMSA bajo la que se ejecuta el contenedor.  La etiqueta `Hostname` permite al contenedor autenticarse en otros servicios en el dominio mediante la autenticación Kerberos.  En el fragmento de código siguiente se muestra un ejemplo para especificar `Hostname` y `credspec` en el manifiesto de aplicación:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
A continuación, lea los siguientes artículos:

* [Implementación de un contenedor de Windows en Service fabric en Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementación de un contenedor de Docker en Service Fabric en Linux](service-fabric-get-started-containers-linux.md)
