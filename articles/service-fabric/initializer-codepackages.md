---
title: Inicializador CodePackages en Service Fabric
description: Describe Inicializador CodePackages en Service Fabric.
author: shsha-msft
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: shsha
ms.openlocfilehash: 3be079b97c2660437344f88203fdda06cc6d6740
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86258978"
---
# <a name="initializer-codepackages"></a>Inicializador CodePackages

A partir de la versión 7.1, Service Fabric admite **Inicializador CodePackages** para [contenedores][containers-introduction-link] y aplicaciones [ejecutables de invitado][guest-executables-introduction-link]. Inicializador CodePackages brinda la oportunidad de realizar inicializaciones en el ámbito de ServicePackage antes de que otros CodePackages inicien la ejecución. Su relación con un ServicePackage es análoga a lo que es [SetupEntryPoint][setup-entry-point-link] para un CodePackage.

Antes de continuar con este artículo, le recomendamos que se familiarice con el [modelo de aplicación de Service Fabric][application-model-link] y el [modelo de hospedaje de Service Fabric][hosting-model-link].

> [!NOTE]
> Actualmente, no se admite Inicializador CodePackages en los servicios escritos con el modelo de programación [Reliable Services][reliable-services-link].
 
## <a name="semantics"></a>Semántica

Se espera que un Inicializador CodePackage se ejecute para la **finalización correcta (código de salida 0)** . Un Inicializador CodePackage con errores se reinicia hasta que se complete correctamente. Se permiten varios Inicializadores CodePackages y se ejecutan hasta la **finalización correcta**, **secuencialmente** **en un orden especificado** antes de que otros CodePackages de ServicePackage inicien la ejecución.

## <a name="specifying-initializer-codepackages"></a>Especificación de Inicializador CodePackages
Puede marcar un CodePackage como Inicializador si establece el atributo **Initializer** en **true** en ServiceManifest. Cuando hay varios Inicializadores CodePackages, se puede especificar su orden de ejecución mediante el atributo **ExecOrder**. **ExecOrder** debe ser un entero no negativo y solo es válido para Inicializadores CodePackages. En primer lugar, se ejecuta el Inicializador CodePackages con menor valor de **ExecOrder**. Si no se especifica **ExecOrder** para un Inicializador CodePackage, se supone un valor predeterminado de 0. No se especifica el orden de ejecución relativo de los Inicializadores CodePackages con el mismo valor de **ExecOrder**.

El siguiente fragmento de código de ServiceManifest describe tres CodePackages, dos de los cuales están marcados como inicializadores. Cuando se activa este ServicePackage, *InitCodePackage0* se ejecuta en primer lugar, ya que tiene el valor más bajo de **ExecOrder**. Tras la finalización correcta (código de salida 0) de *InitCodePackage0*, se ejecuta *InitCodePackage1*. Por último, tras la finalización correcta de *InitCodePackage1*, se ejecuta *WorkloadCodePackage*.

```xml
<CodePackage Name="InitCodePackage0" Version="1.0" Initializer="true" ExecOrder="0">
  ...
</CodePackage>

<CodePackage Name="InitCodePackage1" Version="1.0" Initializer="true" ExecOrder="1">
  ...
</CodePackage>

<CodePackage Name="WorkloadCodePackage" Version="1.0">
  ...
</CodePackage>
```
## <a name="complete-example-using-initializer-codepackages"></a>Ejemplo completo del uso del Inicializador CodePackages

Veamos un ejemplo completo con el Inicializador CodePackages.

> [!IMPORTANT]
> En el siguiente ejemplo se supone que está familiarizado con la creación de [aplicaciones de contenedor de Windows con Service Fabric y Docker][containers-getting-started-link].
>
> En este ejemplo se hace referencia a mcr.microsoft.com/windows/nanoserver:1809. Los contenedores de Windows Server no son compatibles con todas las versiones del sistema operativo de un host. Para más información, consulte [Compatibilidad con versiones de contenedores de Windows](/virtualization/windowscontainers/deploy-containers/version-compatibility).

El siguiente ServiceManifest.xml se basa en el fragmento de ServiceManifest descrito anteriormente. *InitCodePackage0*, *InitCodePackage1* y *WorkloadCodePackage* son CodePackages que representan contenedores. Tras la activación, *InitCodePackage0* se ejecuta primero. Registra un mensaje en un archivo y se cierra. A continuación, se ejecuta *InitCodePackage1*, que también registra un mensaje en un archivo y se cierra. Por último, *WorkloadCodePackage* comienza la ejecución. También registra un mensaje en un archivo, genera el contenido del archivo en **stdout** y, a continuación, hace ping para siempre.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ServiceManifest Name="WindowsInitCodePackageServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>Windows Init CodePackage Service</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="WindowsInitCodePackageServiceType"  UseImplicitHost="true"/>
  </ServiceTypes>
  <CodePackage Name="InitCodePackage0" Version="1.0" Initializer="true" ExecOrder="0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from InitCodePackage0. &gt; C:\WorkspaceOnContainer\log.txt</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>

  <CodePackage Name="InitCodePackage1" Version="1.0" Initializer="true" ExecOrder="1">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from InitCodePackage1. &gt;&gt; C:\WorkspaceOnContainer\log.txt</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>

  <CodePackage Name="WorkloadCodePackage" Version="1.0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from WorkloadCodePackage. &gt;&gt; C:\WorkspaceOnContainer\log.txt &amp;&amp; type C:\WorkspaceOnContainer\log.txt &amp;&amp; ping -t 127.0.0.1 &gt; nul</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>
</ServiceManifest>
```

En el siguiente archivo ApplicationManifest.xml se describe una aplicación basada en el archivo ServiceManifest.xml descrito anteriormente. Tenga en cuenta que especifica el mismo montaje de **Volumen** para todos los contenedores, es decir, **C:\WorkspaceOnHost** se monta en **C:\WorkspaceOnContainer** en los tres contenedores. El efecto neto es que todos los contenedores escriben en el mismo archivo de registro en el orden en que se activan.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WindowsInitCodePackageApplicationType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <Description>Windows Init CodePackage Application</Description>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="WindowsInitCodePackageServicePackage" ServiceManifestVersion="1.0"/>
    <Policies>
      <ContainerHostPolicies CodePackageRef="InitCodePackage0" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>

     <ContainerHostPolicies CodePackageRef="InitCodePackage1" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>

      <ContainerHostPolicies CodePackageRef="WorkloadCodePackage" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>

  <DefaultServices>
    <Service Name="WindowsInitCodePackageService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="WindowsInitCodePackageServiceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
Una vez que ServicePackage se ha activado correctamente, el contenido de **C:\WorkspaceOnHost\log.txt** debe ser el siguiente.

```console
C:\Users\test>type C:\WorkspaceOnHost\log.txt
Hi from InitCodePackage0.
Hi from InitCodePackage1.
Hi from WorkloadCodePackage.
```

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
[setup-entry-point-link]: service-fabric-run-script-at-service-startup.md
