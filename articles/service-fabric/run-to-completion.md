---
title: Semántica de RunToCompletion en Service Fabric
description: Describe la semántica de RunToCompletion en Service Fabric.
author: shsha-msft
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: shsha
ms.openlocfilehash: 6f2f6aa4380fcf6909957118bf682275350ce68c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86261201"
---
# <a name="runtocompletion"></a>RunToCompletion

A partir de la versión 7.1, Service Fabric admite la semántica de **RunToCompletion** para [contenedores][containers-introduction-link] y aplicaciones [ejecutables de invitado][guest-executables-introduction-link]. Esta semántica habilita las aplicaciones y los servicios que completan una tarea y se cierran, a diferencia de las aplicaciones y los servicios que están siempre en ejecución.

Antes de continuar con este artículo, le recomendamos que se familiarice con el [modelo de aplicación de Service Fabric][application-model-link] y el [modelo de hospedaje de Service Fabric][hosting-model-link].

> [!NOTE]
> Actualmente, no se admiten la semántica de RunToCompletion en los servicios escritos con el modelo de programación [Reliable Services][reliable-services-link].
 
## <a name="runtocompletion-semantics-and-specification"></a>Semántica y especificación de RunToCompletion
La semántica de RunToCompletion se puede especificar como **ExecutionPolicy** al [importar ServiceManifest][application-and-service-manifests-link]. La directiva especificada es heredada por todos los CodePackages que componen ServiceManifest. El siguiente fragmento de código de ApplicationManifest.xml proporciona un ejemplo.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="RunToCompletionServicePackage" ServiceManifestVersion="1.0"/>
  <Policies>
    <ExecutionPolicy Type="RunToCompletion" Restart="OnFailure"/>
  </Policies>
</ServiceManifestImport>
```
**ExecutionPolicy** permite los dos atributos siguientes:
* **Tipo:** **RunToCompletion** es actualmente el único valor permitido para este atributo.
* **Restart:** Este atributo especifica la directiva de reinicio que se aplica a los CodePackages que componen ServicePackage, en caso de error. Se considera que un CodePackage que se cierra con un **código de salida distinto de cero** se es erróneo. Los valores permitidos para este atributo son **OnFailure** y **Never**, donde **OnFailure** es el valor predeterminado.

Con la directiva de reinicio establecida en **OnFailure**, si se produce un error en CodePackage **(código de salida distinto de cero)** , se reinicia, con interrupciones entre los errores repetidos. Con la directiva de reinicio establecida en **Never**, si se produce un error en CodePackage, el estado de implementación de DeployedServicePackage se marca como **Failed**, pero otros CodePackages pueden continuar con la ejecución. Si todos los CodePackages que componen ServicePackage se ejecutan hasta finalizar de manera correcta **(código de salida igual a 0)** , el estado de implementación de DeployedServicePackage se marca como **RanToCompletion**. 

## <a name="complete-example-using-runtocompletion-semantics"></a>Ejemplo completo del uso de la semántica de RunToCompletion

Veamos un ejemplo completo del uso de la semántica de RunToCompletion.

> [!IMPORTANT]
> En el siguiente ejemplo se supone que está familiarizado con la creación de [aplicaciones de contenedor de Windows con Service Fabric y Docker][containers-getting-started-link].
>
> En este ejemplo se hace referencia a mcr.microsoft.com/windows/nanoserver:1809. Los contenedores de Windows Server no son compatibles con todas las versiones del sistema operativo de un host. Para más información, consulte [Compatibilidad con versiones de contenedores de Windows](/virtualization/windowscontainers/deploy-containers/version-compatibility).

En el siguiente archivo ServiceManifest.xml se describe un ServicePackage que consta de dos CodePackages, que representan contenedores. *RunToCompletionCodePackage1* simplemente registra un mensaje en **stdout** y se cierra. *RunToCompletionCodePackage2* hace ping en la dirección de bucle invertido durante un tiempo y luego se cierra con un código de salida de **0**, **1** o **2**.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ServiceManifest Name="WindowsRunToCompletionServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>Windows RunToCompletion Service</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="WindowsRunToCompletionServiceType"  UseImplicitHost="true"/>
  </ServiceTypes>
  <CodePackage Name="RunToCompletionCodePackage1" Version="1.0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from RunToCompletionCodePackage1 &amp;&amp; exit 0</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>

  <CodePackage Name="RunToCompletionCodePackage2" Version="1.0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/v,/c,ping 127.0.0.1 &amp;&amp; set /a exitCode=%random% % 3 &amp;&amp; exit !exitCode!</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>
</ServiceManifest>
```

En el siguiente archivo ApplicationManifest.xml se describe una aplicación basada en el archivo ServiceManifest.xml descrito anteriormente. Especifica **RunToCompletion** **ExecutionPolicy** para *WindowsRunToCompletionServicePackage* con una directiva de reinicio de **OnFailure**. Tras la activación de *WindowsRunToCompletionServicePackage*, se iniciarán sus CodePackages constituyentes. *RunToCompletionCodePackage1* debe salir correctamente en la primera activación. Sin embargo, *RunToCompletionCodePackage2* puede generar un error **(código de salida distinto de cero)** , en cuyo caso, se reiniciará, ya que la directiva de reinicio es **OnFailure**.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WindowsRunToCompletionApplicationType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <Description>Windows RunToCompletion Application</Description>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="WindowsRunToCompletionServicePackage" ServiceManifestVersion="1.0"/>
    <Policies>
      <ExecutionPolicy Type="RunToCompletion" Restart="OnFailure"/>
    </Policies>
  </ServiceManifestImport>

  <DefaultServices>
    <Service Name="WindowsRunToCompletionService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="WindowsRunToCompletionServiceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="querying-deployment-status-of-a-deployedservicepackage"></a>Consulta del estado de implementación de un DeployedServicePackage
Se puede consultar el estado de implementación de un DeployedServicePackage desde PowerShell mediante [Get-ServiceFabricDeployedServicePackage][deployed-service-package-link] o desde C# mediante la API [GetDeployedServicePackageListAsync(cadena, URI, cadena)][deployed-service-package-fabricclient-link] de [FabricClient][fabric-client-link].

## <a name="considerations-when-using-runtocompletion-semantics"></a>Consideraciones al usar la semántica de RunToCompletion

Deben tenerse en servicio los siguientes puntos para la compatibilidad actual con RunToCompletion.
* Esta semántica solo se admite para [contenedores][containers-introduction-link] y aplicaciones [ejecutables de invitado][guest-executables-introduction-link].
* No se permiten los escenarios de actualización para aplicaciones con semántica de RunToCompletion. Los usuarios deben eliminar y volver a crear estas aplicaciones, de ser necesario.
* Los eventos de conmutación por error pueden hacer que CodePackages se vuelva a ejecutar después de que se complete correctamente, en el mismo nodo o en otros nodos del clúster. Algunos ejemplos de eventos de conmutación por error son los reinicios de nodo y las actualizaciones en tiempo de ejecución de Service Fabric en un nodo.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información relacionada, consulte los siguientes artículos.

* [Service Fabric y contenedores.][containers-introduction-link]
* [Service Fabric y archivos ejecutables de invitado.][guest-executables-introduction-link]

<!-- Links -->
[containers-introduction-link]: service-fabric-containers-overview.md
[containers-getting-started-link]: service-fabric-get-started-containers.md
[guest-executables-introduction-link]: service-fabric-guest-executables-introduction.md
[reliable-services-link]: service-fabric-reliable-services-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md
[application-and-service-manifests-link]: service-fabric-application-and-service-manifests.md
[setup-entry-point-link]: service-fabric-run-script-at-service-startup.md
[deployed-service-package-working-with-link]: service-fabric-hosting-model.md#work-with-a-deployed-service-package
[deployed-code-package-link]: /powershell/module/servicefabric/get-servicefabricdeployedcodepackage
[deployed-service-package-link]: /powershell/module/servicefabric/get-servicefabricdeployedservicepackage
[fabric-client-link]: /dotnet/api/system.fabric.fabricclient
[deployed-service-package-fabricclient-link]: /dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync
