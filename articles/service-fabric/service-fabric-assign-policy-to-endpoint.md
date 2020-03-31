---
title: Asignación de directivas de acceso a puntos de conexión de servicio
description: Obtenga información acerca de cómo asignar directivas de acceso de seguridad a los puntos de conexión HTTP o HTTPS del servicio de Service Fabric.
ms.topic: conceptual
ms.date: 03/21/2018
ms.openlocfilehash: c7d30e85848f045b5724bb8bdc6e5c810102c044
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614662"
---
# <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Asignación de una directiva de acceso de seguridad a los puntos de conexión HTTP y HTTPS
Si aplica una directiva de RunAs y el manifiesto de servicio declara los recursos de punto de conexión HTTP, debe especificar **SecurityAccessPolicy**.  El elemento **SecurityAccessPolicy** garantiza que los puertos asignados a estos puntos de conexión se restrinjan correctamente a la cuenta de usuario que el servicio ejecuta. En caso contrario, **http.sys** no tendrá acceso al servicio y aparecerán errores en las llamadas del cliente. En el ejemplo siguiente se aplica la cuenta Customer1 a un punto de conexión denominado **EndpointName**, que le concede derechos de acceso total.

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

En el caso de un punto de conexión HTTPS, también es preciso indicar el nombre del certificado que se va a devolver al cliente. Hace referencia al certificado mediante **EndpointBindingPolicy**.  El certificado se define en la sección **Certificados** del manifiesto de aplicación.

```xml
<Policies>
  <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
  <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies>
```

> [!WARNING] 
> Si usa HTTPS, no utilice el mismo puerto y certificado para distintas instancias de servicio (independientes de la aplicación) implementadas en el mismo nodo. Al actualizar dos servicios diferentes que usan el mismo puerto en diferentes instancias de aplicación, se producirá un error de actualización. Para más información, consulte [Actualización de varias aplicaciones con puntos de conexión HTTPS](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints).
> 

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Para ver los próximos pasos, lea los siguientes artículos:
* [Entender el modelo de aplicación](service-fabric-application-model.md)
* [Especificación de los recursos en un manifiesto de servicio](service-fabric-service-manifest-resources.md)
* [Implementar una aplicación](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
