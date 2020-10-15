---
title: Ejemplos de manifiesto de la aplicación de Reliable Services
description: Obtenga información acerca de cómo configurar un manifiesto de servicio y de aplicación para una aplicación de Service Fabric de Reliable Services.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: pepogors
ms.openlocfilehash: f40e54f5260f827f0b18c833d23d1f57b5ebc3a3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84701105"
---
# <a name="reliable-services-application-and-service-manifest-examples"></a>Ejemplos de manifiesto de servicio y de aplicación de servicios confiables
Los siguientes son ejemplos de los manifiestos de aplicación y de servicio para una aplicación de Service Fabric con un front-end web de ASP.NET Core y un back-end con estado. El propósito de estos ejemplos es mostrar qué opciones están disponibles y cómo usarlas. Estos manifiestos de aplicación y de servicio se basan en los manifiestos de la [Guía de inicio rápido de Service Fabric .NET](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/).

Se muestran las características siguientes:

|Manifest|Características|
|---|---|
|[Manifiesto de aplicación](#application-manifest)| [gobernanza de recursos](service-fabric-resource-governance.md), [ejecutar un servicio como una cuenta de administrador local](service-fabric-application-runas-security.md), [aplicar una directiva predeterminada para todos los paquetes de código de servicio](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages), [crear entidades de seguridad de usuario y grupo](service-fabric-application-runas-security.md), compartir un paquete de datos entre instancias de servicio, [invalidar los extremos de servicio](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)| 
|Manifiesto del servicio FrontEndService| [Ejecutar un script al inicio del servicio](service-fabric-run-script-at-service-startup.md), [definir un punto de conexión HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest) | 
|Manifiesto del servicio BackEndService| [Declarar un paquete de configuración](service-fabric-application-and-service-manifests.md), [declarar un paquete de datos](service-fabric-application-and-service-manifests.md), [configurar un punto de conexión](service-fabric-service-manifest-resources.md)| 

Consulte [Elementos del manifiesto de aplicación](#application-manifest-elements), [Elementos del manifiesto del servicio VotingWeb](#votingweb-service-manifest-elements), y [Elementos del manifiesto del servicio VotingData](#votingdata-service-manifest-elements) para más información sobre elementos específicos XML.

## <a name="application-manifest"></a>Manifiesto de aplicación

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="Memory" DefaultValue="4084" />
    <Parameter Name="BlockIOWeight" DefaultValue="200" />
    <Parameter Name="MaximumIOBandwidth" DefaultValue="1024" />
    <Parameter Name="MemoryReservationInMB" DefaultValue="1024" />
    <Parameter Name="MemorySwapInMB" DefaultValue="4084"/>
    <Parameter Name="Port" DefaultValue="8081" />
    <Parameter Name="Protocol" DefaultValue="tcp" />
    <Parameter Name="Type" DefaultValue="internal" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <!-- Override endpoints declared in the service manifest. -->
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="DataEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
      </Endpoints>
    </ResourceOverrides>

    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      
      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[Memory]"/>

      <!-- Set resource governance at the code package level. -->
      <ResourceGovernancePolicy CodePackageRef="Code" CpuPercent="10" MemoryInMB="[Memory]" BlockIOWeight="[BlockIOWeight]" 
                                MaximumIOBandwidth="[MaximumIOBandwidth]" MaximumIOps="[MaximumIOps]" MemoryReservationInMB="[MemoryReservationInMB]" 
                                MemorySwapInMB="[MemorySwapInMB]"/>

      <!-- Share the data package across multiple instances of the VotingData service-->
      <PackageSharingPolicy PackageRef="Data"/>

      <!-- Give read rights on the "DataEndpoint" endpoint to the Customer2 account.-->
      <SecurityAccessPolicy GrantRights="Read" PrincipalRef="Customer2" ResourceRef="DataEndpoint" ResourceType="Endpoint"/>         
    </Policies>
  </ServiceManifestImport>
  
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    
    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      <!-- Run the setup entry point (defined in the imported service manifest) as the SetupAdminUser account 
      (declared in the following Principals section). -->
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      
    </Policies>

  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <!-- Define users and groups required to run the services and access resources.  Principals are used in the Policies section(s). -->
  <Principals>
    <!-- Declare a set of groups as security principals, which can be referenced in policies. Groups are useful if there are multiple users 
    for different service entry points and they need to have certain common privileges that are available at the group level. -->
    <Groups>
      <!-- Create a group that has administrator privileges. -->
      <Group Name="LocalAdminGroup">
        <Membership>
          <SystemGroup Name="Administrators" />
        </Membership>
      </Group>
    </Groups>
    <Users>
      <!-- Declare a user and add the user to the Administrators system group. The SetupAdminUser account is used to run the 
      setup entry point of the VotingWebPkg code package (described in the preceding Policies section).-->
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
      <!-- Create a user. Local user accounts are created on the machines where the application is deployed. By default, these accounts 
      do not have the same names as those specified here. Instead, they are dynamically generated and have random passwords. -->
      <User Name="Customer1" >
        <MemberOf>
          <!-- Add the user to the local administrators group.-->
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <!-- Create a user as a local user with the specified account name and password. Local user accounts are created on the machines 
      where the application is deployed. -->
      <User Name="Customer2" AccountType="LocalUser" AccountName="Customer1" Password="MyPassword">
        <MemberOf>
          <!-- Add the user to the local administrators group.-->
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <!-- Create a user as NetworkService. -->
      <User Name="MyDefaultAccount" AccountType="NetworkService" />      
    </Users>
  </Principals>
  <!-- Policies applied at the application level. -->
  <Policies>
    <!-- Specify a default user account for all code packages that don’t have a specific RunAsPolicy defined in 
    the ServiceManifestImport section(s). -->
    <DefaultRunAsPolicy UserRef="MyDefaultAccount" />
    
  </Policies>
</ApplicationManifest>

```

## <a name="votingweb-service-manifest"></a>Manifiesto del servicio VotingWeb

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <!-- A privileged entry point that by default runs with the same credentials as Service Fabric (typically the NetworkService account) before 
    any other entry point. Use the setup entry point to set system environment variables, give the account running the service (NETWORK SERVICE, by default) 
    access to a certificate's private key, or perform other setup tasks. In the application manifest, you can change the security permissions to run the startup script 
    under a local system account or an administrator account.  -->
    <SetupEntryPoint>
      <ExeHost>
        <!-- The setup script to run. -->
        <Program>Setup.bat</Program>
        <!-- Pass arguments to the script when it runs.-->
        <Arguments>MyValue</Arguments>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
        <!-- Warning! Do not use console redirection in a production application, only use it for local development and debugging. Redirects console output from the startup
        script to an output file in the application folder called "log" on the cluster node where the application is deployed and run. Also set the number of output files
        to retain and the maximum file size (in KB). -->
        <ConsoleRedirection FileRetentionCount="10" FileMaxSizeInKb="20480"/>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- Configure a HTTPS endpoint on port 443. This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>

```

## <a name="votingdata-service-manifest"></a>Manifiesto del servicio VotingData

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingDataPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="VotingDataType"  HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingData.exe</Program>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Declares a folder, named by the Name attribute, under PackageRoot that contains a Settings.xml file. This file contains sections of user-defined, 
  key-value pair settings that the process can read back at run time. During an upgrade, if only the ConfigPackage version has changed, 
  then the running process is not restarted. Instead, a callback notifies the process that configuration settings have changed so they can be reloaded dynamically. -->
  <ConfigPackage Name="Config" Version="1.0.0" />
  
  <!-- Declares a folder, named by the Name attribute, under PackageRoot which contains static data files to be consumed by the process at run time. -->
  <DataPackage Name="Data" Version="1.0.0"/>

  <Resources>
    <Endpoints>
      <!-- Define an internal (used for intra-application communication) TCP endpoint. Since no port is specified, one is created and assigned dynamically
           to the service.-->
      <Endpoint Name="DataEndpoint" Protocol="tcp" Type="Internal" />
    </Endpoints>
  </Resources>

</ServiceManifest>

```

## <a name="application-manifest-elements"></a>Elementos del manifiesto de aplicación
### <a name="applicationmanifest-element"></a>Elemento ApplicationManifest
Describe mediante declaración el tipo de aplicación y la versión. Para crear un tipo de aplicación, se hace referencia a uno o varios manifiestos de servicio de los servicios constituyentes. Los valores de configuración de los servicios constituyentes pueden invalidarse mediante la configuración de aplicación parametrizada. También se pueden declarar servicios predeterminados, plantillas de servicio, entidades de seguridad, directivas, configuraciones de diagnóstico y certificados en el nivel de aplicación. Para más información, consulte [Elemento ApplicationManifest](service-fabric-service-model-schema-elements.md#ApplicationManifestElementApplicationManifestTypeComplexType)

### <a name="parameters-element"></a>Elemento Parameters
Declara los parámetros que se usan en este manifiesto de aplicación. El valor de estos parámetros puede proporcionarse cuando se crea una instancia de la aplicación y se puede usar para invalidar los valores de configuración del servicio o de la aplicación. Para más información, consulte [Elemento Parameters](service-fabric-service-model-schema-elements.md#ParametersElementanonymouscomplexTypeComplexTypeDefinedInApplicationManifestTypecomplexType)

### <a name="parameter-element"></a>Elemento Parameter
Un parámetro de aplicación que se usará en este manifiesto. El valor del parámetro puede cambiarse durante la creación de instancias de la aplicación, o, si no se proporciona ningún valor, se usa el valor predeterminado. Para más información, consulte [Elemento Parameter](service-fabric-service-model-schema-elements.md#ParameterElementanonymouscomplexTypeComplexTypeDefinedInParameterselement)

### <a name="servicemanifestimport-element"></a>Elemento ServiceManifestImport
Importa un manifiesto de servicio creado por el desarrollador del servicio. Para cada servicio constituyente de la aplicación se debe importar un manifiesto de servicio. Se invalida la configuración y se pueden declarar directivas para el manifiesto de servicio. Para más información, consulte [Elemento ServiceManifestImport](service-fabric-service-model-schema-elements.md#ServiceManifestImportElementanonymouscomplexTypeComplexTypeDefinedInApplicationManifestTypecomplexType)

### <a name="servicemanifestref-element"></a>Elemento ServiceManifestRef
Importa el manifiesto de servicio por referencia. Actualmente el archivo de manifiesto de servicio (ServiceManifest.xml) debe estar presente en el paquete de compilación. Para más información, consulte [Elemento ServiceManifestRef](service-fabric-service-model-schema-elements.md#ServiceManifestRefElementServiceManifestRefTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="resourceoverrides-element"></a>Elemento ResourceOverrides
Especifica las invalidaciones de recursos para los puntos de conexión declarados en recursos de manifiesto de servicio. Para más información, consulte [Elemento ResourceOverrides](service-fabric-service-model-schema-elements.md#ResourceOverridesElementResourceOverridesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="endpoints-element"></a>Elemento Endpoints
Los puntos de conexión que invalidar. Para más información, consulte [Elemento Endpoints](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourceOverridesTypecomplexType)

### <a name="endpoint-element"></a>Elemento Endpoint
El punto de conexión, declarado en el manifiesto de servicio, que invalidar. Para más información, consulte [Elemento Endpoint](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

### <a name="policies-element"></a>Elemento Policies
Describe las directivas (enlace de punto de conexión, uso compartido de paquetes, ejecutar como y acceso de seguridad) que se van a aplicar en el manifiesto de servicio importado. Para más información, consulte [Elemento Policies](service-fabric-service-model-schema-elements.md#PoliciesElementServiceManifestImportPoliciesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="servicepackageresourcegovernancepolicy-element"></a>Elemento ServicePackageResourceGovernancePolicy
Define la directiva de gobernanza de recursos que se aplica en el nivel del paquete de servicio completo. Para más información, consulte [Elemento ServicePackageResourceGovernancePolicy](service-fabric-service-model-schema-elements.md#ServicePackageResourceGovernancePolicyElementServicePackageResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInServicePackageTypecomplexType)

### <a name="resourcegovernancepolicy-element"></a>Elemento ResourceGovernancePolicy
Especifica los límites de recursos de un paquete de código. Para más información, consulte [Elemento ResourceGovernancePolicy](service-fabric-service-model-schema-elements.md#ResourceGovernancePolicyElementResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelementDefinedInDigestedEndpointelement)

### <a name="packagesharingpolicy-element"></a>Elemento PackageSharingPolicy
Indica si se debe compartir un paquete de código, configuración o datos entre las instancias de servicio del mismo tipo de servicio. Para más información, consulte [Elemento PackageSharingPolicy](service-fabric-service-model-schema-elements.md#PackageSharingPolicyElementPackageSharingPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexType)

### <a name="securityaccesspolicy-element"></a>Elemento SecurityAccessPolicy
Concede permisos de acceso a una entidad de seguridad en un recurso (por ejemplo, un punto de conexión) definido en un manifiesto de servicio. Por lo general, resulta muy útil controlar y restringir el acceso de los servicios a diferentes recursos con el fin de reducir los riesgos de seguridad. Esto es especialmente importante cuando la aplicación se crea a partir de una colección de servicios de un catálogo de soluciones, desarrollados por distintas personas. Para más información, consulte [Elemento SecurityAccessPolicy](service-fabric-service-model-schema-elements.md#SecurityAccessPolicyElementSecurityAccessPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInSecurityAccessPolicieselementDefinedInDigestedEndpointelement)

### <a name="runaspolicy-element"></a>Elemento RunAsPolicy
Especifica el usuario local o la cuenta del sistema local que ejecutará un paquete de código de servicio. Se admiten cuentas de dominio en implementaciones de Windows Server donde esté disponible Azure Active Directory. De forma predeterminada, las aplicaciones se ejecutan con la cuenta con la que se ejecuta el proceso Fabric.exe. Las aplicaciones también pueden ejecutarse con otras cuentas, lo que se debe declarar en la sección Entidades de seguridad. Si se aplica una directiva RunAs a un servicio y el manifiesto de servicio declara que hay recursos de puntos de conexión con el protocolo HTTP, es preciso especificar también una directiva SecurityAccessPolicy para asegurarse de que los puertos asignados a dichos puntos de conexión aparezcan correctamente en la lista de control de acceso de la cuenta de usuario RunAs en la que se ejecuta el servicio. Para un punto de conexión HTTPS, también debe definir un elemento EndpointBindingPolicy para indicar el nombre del certificado que se devuelve al cliente. Para más información, consulte [Elemento RunAsPolicy](service-fabric-service-model-schema-elements.md#RunAsPolicyElementRunAsPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="defaultservices-element"></a>Elemento DefaultServices
Declara instancias de servicio que se crean automáticamente cada vez que se crea una instancia de una aplicación en este tipo de aplicación. Para más información, consulte [Elemento DefaultServices](service-fabric-service-model-schema-elements.md#DefaultServicesElementDefaultServicesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInApplicationInstanceTypecomplexType)

### <a name="service-element"></a>Elemento Service
Declara que un servicio se cree automáticamente cuando se crea una instancia de la aplicación. Para más información, consulte [Elemento Service](service-fabric-service-model-schema-elements.md#ServiceElementanonymouscomplexTypeComplexTypeDefinedInDefaultServicesTypecomplexType)

### <a name="statefulservice-element"></a>Elemento StatefulService
Define un servicio con estado. Para más información, consulte [Elemento StatefulService](service-fabric-service-model-schema-elements.md#StatefulServiceElementStatefulServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)

### <a name="statelessservice-element"></a>Elemento StatelessService
Define un servicio sin estado. Para más información, consulte [Elemento StatelessService](service-fabric-service-model-schema-elements.md#StatelessServiceElementStatelessServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)

### <a name="principals-element"></a>Elemento Principals
Describe las entidades de seguridad (usuarios, grupos) necesarias para que esta aplicación ejecute servicios y proteja recursos. Se hace referencia a las entidades de seguridad en las secciones de directivas. Para más información, consulte [Elemento Principals](service-fabric-service-model-schema-elements.md#PrincipalsElementSecurityPrincipalsTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInEnvironmentTypecomplexType)

### <a name="groups-element"></a>Elemento Groups
Declara un conjunto de grupos como entidades de seguridad, a las que se puede hacer referencia en las directivas. Los grupos son útiles si hay varios usuarios para distintos puntos de entrada de servicio y es preciso que tengan ciertos privilegios comunes disponibles en el nivel de grupo. Para más información, consulte [Elemento Groups](service-fabric-service-model-schema-elements.md#GroupsElementanonymouscomplexTypeComplexTypeDefinedInSecurityPrincipalsTypecomplexType)

### <a name="group-element"></a>Elemento Group
Declara un grupo como una entidad de seguridad, a la que se puede hacer referencia en las directivas. Para más información, consulte [Elemento Group](service-fabric-service-model-schema-elements.md#GroupElementanonymouscomplexTypeComplexTypeDefinedInGroupselement)

### <a name="membership-element"></a>Elemento Membership
 Para más información, consulte [Elemento Membership](service-fabric-service-model-schema-elements.md#MembershipElementanonymouscomplexTypeComplexTypeDefinedInGroupelement)

### <a name="systemgroup-element"></a>Elemento SystemGroup
 Para más información, consulte [Elemento SystemGroup](service-fabric-service-model-schema-elements.md#SystemGroupElementanonymouscomplexTypeComplexTypeDefinedInMembershipelement)

### <a name="users-element"></a>Elemento Users
Declara un conjunto de usuarios como entidades de seguridad, a las que se puede hacer referencia en las directivas. Para más información, consulte [Elemento Users](service-fabric-service-model-schema-elements.md#UsersElementanonymouscomplexTypeComplexTypeDefinedInSecurityPrincipalsTypecomplexType)

### <a name="user-element"></a>Elemento User
Declara un usuario como una entidad de seguridad, a la que se puede hacer referencia en las directivas. Para más información, consulte [Elemento User](service-fabric-service-model-schema-elements.md#UserElementanonymouscomplexTypeComplexTypeDefinedInUserselement)

### <a name="memberof-element"></a>Elemento MemberOf
Los usuarios se pueden agregar a cualquier grupo de pertenencia existente, por lo que puede heredar todas las propiedades y la configuración de seguridad de ese grupo de pertenencia. El grupo de pertenencia se puede usar para proteger los recursos externos a los que deben acceder distintos servicios o el mismo servicio (en otra máquina). Para más información, consulte [Elemento MemberOf](service-fabric-service-model-schema-elements.md#MemberOfElementanonymouscomplexTypeComplexTypeDefinedInUserelement)

### <a name="systemgroup-element"></a>Elemento SystemGroup
El grupo del sistema al que agregar el usuario.  El grupo del sistema debe definirse en la sección Grupos. Para más información, consulte [Elemento SystemGroup](service-fabric-service-model-schema-elements.md#SystemGroupElementanonymouscomplexTypeComplexTypeDefinedInMemberOfelement)

### <a name="group-element"></a>Elemento Group
El grupo al que se agrega el usuario.  El grupo debe estar definido en la sección Grupos. Para más información, consulte [Elemento Group](service-fabric-service-model-schema-elements.md#GroupElementanonymouscomplexTypeComplexTypeDefinedInMemberOfelement)

### <a name="policies-element"></a>Elemento Policies
Describe las directivas (recopilación de registros, ejecución predeterminada, mantenimiento y acceso de seguridad) que se van a aplicar en el nivel de aplicación. Para más información, consulte [Elemento Policies](service-fabric-service-model-schema-elements.md#PoliciesElementApplicationPoliciesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInEnvironmentTypecomplexType)

### <a name="defaultrunaspolicy-element"></a>Elemento DefaultRunAsPolicy
Especifique una cuenta de usuario predeterminada para todos los paquetes de código de servicio que no tengan ningún elemento RunAsPolicy específico definido en la sección ServiceManifestImport. Para más información, consulte [Elemento DefaultRunAsPolicy](service-fabric-service-model-schema-elements.md#DefaultRunAsPolicyElementanonymouscomplexTypeComplexTypeDefinedInApplicationPoliciesTypecomplexType)




## <a name="votingweb-service-manifest-elements"></a>Elementos del manifiesto del servicio VotingWeb
### <a name="servicemanifest-element"></a>Elemento ServiceManifest
Describe mediante declaración el tipo de servicio y la versión. Enumera los paquetes de código, configuración y datos que se pueden actualizar de manera independiente que, juntos, constituyen un paquete de servicio para admitir uno o más tipos de servicio. También se especifican los recursos, la configuración de diagnóstico y los metadatos de servicio, como el tipo de servicio, las propiedades de mantenimiento y las métricas de equilibrio de carga. Para más información, consulte [Elemento ServiceManifest](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>Elemento ServiceTypes
Define los tipos de servicios que son compatibles con CodePackage en este manifiesto. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Los tipos de servicio se declaran en el nivel de manifiesto y no en el nivel de paquete de código. Para más información, consulte [Elemento ServiceTypes](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>Elemento StatelessServiceType
Describe un tipo de servicio sin estado. Para más información, consulte [Elemento StatelessServiceType](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>Elemento CodePackage
Describe un paquete de código que admite un tipo de servicio definido. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Se espera que los procesos resultantes registren los tipos de servicio admitidos en tiempo de ejecución. Cuando hay varios paquetes de código, se activan todos cada vez que el sistema busca cualquiera de los tipos de servicios declarados. Para más información, consulte [Elemento CodePackage](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="setupentrypoint-element"></a>Elemento SetupEntryPoint
Es un punto de entrada con privilegios que se ejecuta de forma predeterminada con las mismas credenciales que Service Fabric (normalmente, la cuenta NETWORKSERVICE) antes que cualquier otro punto de entrada. El archivo ejecutable especificado por EntryPoint suele ser el host de servicios de ejecución prolongada. La presencia de un punto de entrada de configuración independiente evita tener que ejecutar el host de servicio con privilegios elevados durante largos períodos de tiempo. Para más información, consulte [Elemento SetupEntryPoint](service-fabric-service-model-schema-elements.md#SetupEntryPointElementanonymouscomplexTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>Elemento ExeHost
 Para más información, consulte [Elemento ExeHost](service-fabric-service-model-schema-elements.md#ExeHostElementExeHostEntryPointTypeComplexTypeDefinedInSetupEntryPointelement)

### <a name="program-element"></a>Elemento Program
El nombre del archivo ejecutable.  Por ejemplo, "MySetup.bat" o "MyServiceHost.exe". Para más información, consulte [Elemento Program](service-fabric-service-model-schema-elements.md#ProgramElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="arguments-element"></a>Elemento Arguments
 Para más información, consulte [Elemento Arguments](service-fabric-service-model-schema-elements.md#ArgumentsElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="workingfolder-element"></a>Elemento WorkingFolder
El directorio de trabajo para el proceso en el paquete de código en el nodo de clúster donde se implementa la aplicación. Puede especificar tres valores: Work (el valor predeterminado), CodePackage o CodeBase. CodeBase especifica que el directorio de trabajo está establecido en el directorio en el que se define el archivo EXE en el paquete de código. CodePackage establece que el directorio de trabajo sea la raíz del paquete de código, independientemente de dónde se defina el archivo EXE en el directorio del paquete de código. Work establece que el directorio de trabajo sea una carpeta única creada en el nodo.  Esta carpeta es la misma para la instancia de aplicación completa. De forma predeterminada, el directorio de trabajo de todos los procesos en la aplicación se establece en la carpeta de trabajo de aplicación. Aquí es donde los procesos pueden escribir los datos. No se recomienda escribir datos en el paquete de código ni en el código base ya que esas carpetas podrían compartirse entre diferentes instancias de aplicación y es posible que se eliminen. Para más información, consulte [Elemento WorkingFolder](service-fabric-service-model-schema-elements.md#WorkingFolderElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="consoleredirection-element"></a>Elemento ConsoleRedirection

> [!WARNING]
> No use la redirección de consola en una aplicación de producción, solo se usa para la depuración y el desarrollo locales. Redirige la salida de la consola del script de inicio a un archivo de salida en la carpeta de aplicación denominada "log", en el nodo del clúster donde se implementa y ejecuta la aplicación. Para más información, consulte [Elemento ConsoleRedirection](service-fabric-service-model-schema-elements.md#ConsoleRedirectionElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="entrypoint-element"></a>Elemento EntryPoint
El archivo ejecutable especificado por EntryPoint suele ser el host de servicios de ejecución prolongada. La presencia de un punto de entrada de configuración independiente evita tener que ejecutar el host de servicio con privilegios elevados durante largos períodos de tiempo. El archivo ejecutable especificado por EntryPoint se ejecuta después de que SetupEntryPoint se cierra correctamente. El proceso resultante se supervisa y reinicia (comenzando de nuevo con SetupEntryPoint) si alguna vez finaliza o se bloquea. Para más información, consulte [Elemento EntryPoint](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>Elemento ExeHost
 Para más información, consulte [Elemento ExeHost](service-fabric-service-model-schema-elements.md#ExeHostElementanonymouscomplexTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="configpackage-element"></a>Elemento ConfigPackage
Declara una carpeta, denominada por el atributo Nombre, debajo de PackageRoot que contiene un archivo Settings.xml. Este archivo contiene secciones de configuración del par clave-valor definida por el usuario que el proceso puede volver a leer en tiempo de ejecución. Durante una actualización, si solo ha cambiado la versión de ConfigPackage, no se reiniciará el proceso en ejecución. En su lugar, una devolución de llamada notifica el proceso que los ajustes de configuración han cambiado, por lo que pueden volver a cargarse de forma dinámica. Para más información, consulte [Elemento ConfigPackage](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="resources-element"></a>Elemento Resources
Describe los recursos utilizados por este servicio, que se pueden declarar sin modificar el código compilado y cambiar cuando se implemente el servicio. El acceso a estos recursos se controla mediante las secciones Entidades de seguridad y Directivas del manifiesto de aplicación. Para más información, consulte [Elemento Resources](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Elemento Endpoints
Define los puntos de conexión del servicio. Para más información, consulte [Elemento Endpoints](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Elemento Endpoint
El punto de conexión, declarado en el manifiesto de servicio, que invalidar. Para más información, consulte [Elemento Endpoint](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)



## <a name="votingdata-service-manifest-elements"></a>Elementos del manifiesto del servicio VotingData
### <a name="servicemanifest-element"></a>Elemento ServiceManifest
Describe mediante declaración el tipo de servicio y la versión. Enumera los paquetes de código, configuración y datos que se pueden actualizar de manera independiente que, juntos, constituyen un paquete de servicio para admitir uno o más tipos de servicio. También se especifican los recursos, la configuración de diagnóstico y los metadatos de servicio, como el tipo de servicio, las propiedades de mantenimiento y las métricas de equilibrio de carga. Para más información, consulte [Elemento ServiceManifest](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>Elemento ServiceTypes
Define los tipos de servicios que son compatibles con CodePackage en este manifiesto. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Los tipos de servicio se declaran en el nivel de manifiesto y no en el nivel de paquete de código. Para más información, consulte [Elemento ServiceTypes](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statefulservicetype-element"></a>Elemento StatefulServiceType
Describe un tipo de servicio con estado. Para más información, consulte [Elemento StatefulServiceType](service-fabric-service-model-schema-elements.md#StatefulServiceTypeElementStatefulServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>Elemento CodePackage
Describe un paquete de código que admite un tipo de servicio definido. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Se espera que los procesos resultantes registren los tipos de servicio admitidos en tiempo de ejecución. Cuando hay varios paquetes de código, se activan todos cada vez que el sistema busca cualquiera de los tipos de servicios declarados. Para más información, consulte [Elemento CodePackage](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>Elemento EntryPoint
El archivo ejecutable especificado por EntryPoint suele ser el host de servicios de ejecución prolongada. La presencia de un punto de entrada de configuración independiente evita tener que ejecutar el host de servicio con privilegios elevados durante largos períodos de tiempo. El archivo ejecutable especificado por EntryPoint se ejecuta después de que SetupEntryPoint se cierra correctamente. El proceso resultante se supervisa y reinicia (comenzando de nuevo con SetupEntryPoint) si alguna vez finaliza o se bloquea. Para más información, consulte [Elemento EntryPoint](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>Elemento ExeHost
 Para más información, consulte [Elemento ExeHost](service-fabric-service-model-schema-elements.md#ExeHostElementanonymouscomplexTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="program-element"></a>Elemento Program
El nombre del archivo ejecutable.  Por ejemplo, "MySetup.bat" o "MyServiceHost.exe". Para más información, consulte [Elemento Program](service-fabric-service-model-schema-elements.md#ProgramElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="workingfolder-element"></a>Elemento WorkingFolder
El directorio de trabajo para el proceso en el paquete de código en el nodo de clúster donde se implementa la aplicación. Puede especificar tres valores: Work (el valor predeterminado), CodePackage o CodeBase. CodeBase especifica que el directorio de trabajo está establecido en el directorio en el que se define el archivo EXE en el paquete de código. CodePackage establece que el directorio de trabajo sea la raíz del paquete de código, independientemente de dónde se defina el archivo EXE en el directorio del paquete de código. Work establece que el directorio de trabajo sea una carpeta única creada en el nodo.  Esta carpeta es la misma para la instancia de aplicación completa. De forma predeterminada, el directorio de trabajo de todos los procesos en la aplicación se establece en la carpeta de trabajo de aplicación. Aquí es donde los procesos pueden escribir los datos. No se recomienda escribir datos en el paquete de código ni en el código base ya que esas carpetas podrían compartirse entre diferentes instancias de aplicación y es posible que se eliminen. Para más información, consulte [Elemento WorkingFolder](service-fabric-service-model-schema-elements.md#WorkingFolderElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="configpackage-element"></a>Elemento ConfigPackage
Declara una carpeta, denominada por el atributo Nombre, debajo de PackageRoot que contiene un archivo Settings.xml. Este archivo contiene secciones de configuración del par clave-valor definida por el usuario que el proceso puede volver a leer en tiempo de ejecución. Durante una actualización, si solo ha cambiado la versión de ConfigPackage, no se reiniciará el proceso en ejecución. En su lugar, una devolución de llamada notifica el proceso que los ajustes de configuración han cambiado, por lo que pueden volver a cargarse de forma dinámica. Para más información, consulte [Elemento ConfigPackage](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="datapackage-element"></a>Elemento DataPackage
Declara una carpeta, denominada por el atributo Nombre, debajo de PackageRoot que contiene archivos de datos estáticos que el proceso en tiempo de ejecución va a consumir. Service Fabric reciclará todos los archivos EXE y DLLHOST especificados en los paquetes de host y soporte técnico cuando cualquiera de los paquetes de datos enumerados en el manifiesto de servicio se actualice. Para más información, consulte [Elemento DataPackage](service-fabric-service-model-schema-elements.md#DataPackageElementDataPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedDataPackageelement)

### <a name="resources-element"></a>Elemento Resources
Describe los recursos utilizados por este servicio, que se pueden declarar sin modificar el código compilado y cambiar cuando se implemente el servicio. El acceso a estos recursos se controla mediante las secciones Entidades de seguridad y Directivas del manifiesto de aplicación. Para más información, consulte [Elemento Resources](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Elemento Endpoints
Define los puntos de conexión del servicio. Para más información, consulte [Elemento Endpoints](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Elemento Endpoint
El punto de conexión, declarado en el manifiesto de servicio, que invalidar. Para más información, consulte [Elemento Endpoint](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

