---
title: Sondeos de Azure Service Fabric
description: Aprenda a modelar sondeos de ejecución en Azure Service Fabric mediante archivos de manifiesto de servicio y aplicación.
ms.topic: conceptual
ms.date: 3/12/2020
ms.openlocfilehash: 38f3888a29bf505b723d40bc7cd08fb0c7e29eff
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81427444"
---
# <a name="liveness-probe"></a>Sondeo de ejecución
A partir de la versión 7.1, Service Fabric admite el mecanismo de sondeo de ejecución para aplicaciones en [contenedores][containers-introduction-link]. El sondeo de ejecución ayuda a anunciar la vivacidad de aplicaciones en contenedores y, cuando no responden a tiempo, se producirá un reinicio.
En este artículo se proporciona información general sobre cómo definir un sondeo de ejecución a través de archivos de manifiesto.

Antes de continuar con este artículo, le recomendamos que se familiarice con el [modelo de aplicación de Service Fabric][application-model-link] y el [modelo de hospedaje de Service Fabric][hosting-model-link].

> [!NOTE]
> El sondeo de ejecución solo se admite para contenedores en el modo de red NAT.

## <a name="semantics"></a>Semántica
Solo puede especificar un sondeo de ejecución por contenedor y puede controlar su comportamiento con los campos siguientes:

* `initialDelaySeconds`: Retraso inicial en segundos para comenzar la ejecución del sondeo una vez que se ha iniciado el contenedor. El valor admitido es int. El valor predeterminado es 0. El mínimo es de 0.

* `timeoutSeconds`: Período en segundos tras el cual se considera que el sondeo es erróneo si no se ha completado correctamente. El valor admitido es int. El valor predeterminado es 1. El valor mínimo es 1.

* `periodSeconds`: Período en segundos para especificar la frecuencia de sondeo. El valor admitido es int. El valor predeterminado es 10. El valor mínimo es 1.

* `failureThreshold`: Una vez que se alcanza FailureThreshold, el contenedor se reiniciará. El valor admitido es int. El valor predeterminado es 3. El valor mínimo es 1.

* `successThreshold`: En caso de error, para que el sondeo se considere correcto, debe ejecutarse correctamente para SuccessThreshold. El valor admitido es int. El valor predeterminado es 1. El valor mínimo es 1.

Habrá como máximo un sondeo por contenedor en un momento dado. Si el sondeo no se completa en **timeoutSeconds**, seguimos esperando y contando hasta **failureThreshold**. 

Además, ServiceFabric generará los siguientes [informes de mantenimiento][health-introduction-link] de sondeo en DeployedServicePackage:

* `Ok`: Si el sondeo se completa correctamente para **successThreshold**, se notifica el estado como correcto.

* `Error`: Si failureCount del sondeo == **failureThreshold** antes de reiniciar el contenedor, se notifica un error.

* `Warning`: 
    1. Si se produce un error en el sondeo y failureCount < **failureThreshold**, se notifica una advertencia. Este informe de mantenimiento se conserva hasta que failureCount alcanza **failureThreshold** o **successThreshold**.
    2. Si se completa correctamente después de un error, todavía se informa de la advertencia, pero con la actualización de la finalización correcta consecutiva.

## <a name="specifying-liveness-probe"></a>Especificación del sondeo de ejecución

Puede especificar el sondeo en ApplicationManifest.xml en ServiceManifestImport:

El sondeo puede ser cualquiera de los siguientes:

1. HTTP
2. TCP
3. Exec 

## <a name="http-probe"></a>Sondeo HTTP

En el caso de sondeo HTTP, Service Fabric enviará una solicitud HTTP al puerto y la ruta de acceso especificados. Un código de retorno mayor o igual que 200 y menor que 400 indica que se ha completado correctamente.

Este es un ejemplo de cómo especificar un sondeo HttpGet:

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <HttpGet Path="/" Port="8081" Scheme="http">
              <HttpHeader Name="Foo" Value="Val"/>
              <HttpHeader Name="Bar" Value="val1"/>
            </HttpGet>
          </Probe>
        </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

El sondeo HttpGet tiene propiedades adicionales que puede establecer:

* `path`: Ruta de acceso en la solicitud HTTP.

* `port`: Puerto de acceso para los sondeos. El rango es de 1 a 65535. Mandatory.

* `scheme`: Esquema que se va a usar para conectarse al paquete de código. Si se establece en HTTPS, se omite la verificación de certificados. El valor predeterminado es HTTP.

* `httpHeader`: Encabezados que se van a establecer en la solicitud. Puede especificar varios de estos.

* `host`: Dirección IP del host al que se va a conectar.

## <a name="tcp-probe"></a>Sondeo TCP

En el caso del sondeo TCP, Service Fabric intentará abrir un socket en el contenedor con el puerto especificado. Si puede establecer una conexión, el sondeo se considera correcto. Este es un ejemplo de cómo especificar el sondeo que usa el socket TCP:

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <TcpSocket Port="8081"/>
          </Probe>
        </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

## <a name="exec-probe"></a>Sondeo exec

Este sondeo emitirá un comando exec en el contenedor y esperará a que se complete el comando.

> [!NOTE]
> El comando exec toma una cadena separada por comas. El siguiente comando en el ejemplo funcionará para un contenedor de Linux.
> Si está probando un contenedor de Windows, use <Command>cmd</Command>.

```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <CodePackagePolicy CodePackageRef="Code">
        <Probes>
          <Probe Type="Liveness" FailureThreshold="5" SuccessThreshold="2" InitialDelaySeconds="10" PeriodSeconds="30" TimeoutSeconds="20">
            <Exec>
              <Command>ping,-c,2,localhost</Command>
            </Exec>
          </Probe>        
       </Probes>
      </CodePackagePolicy>
    </Policies>
  </ServiceManifestImport>
```

## <a name="next-steps"></a>Pasos siguientes
Para obtener información relacionada, consulte los siguientes artículos.
* [Service Fabric y contenedores.][containers-introduction-link]

<!-- Links -->
[containers-introduction-link]: service-fabric-containers-overview.md
[health-introduction-link]: service-fabric-health-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md

