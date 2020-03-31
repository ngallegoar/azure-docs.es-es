---
title: Ejemplos del manifiesto de una aplicación contenedora en Azure Service Fabric
description: Obtenga información acerca de cómo configurar un manifiesto de servicio y de aplicación para una aplicación de Service Fabric de varios contenedores.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/08/2018
ms.author: pepogors
ms.openlocfilehash: 5efdbd17db20e69ace33b379ddbb99b2c4a20e69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229292"
---
# <a name="multi-container-application-and-service-manifest-examples"></a>Ejemplos de manifiesto de servicio y de aplicación con varios contenedores
A continuación se muestran ejemplos de manifiestos de servicio y de aplicación para una aplicación de Service Fabric de varios contenedores. El propósito de estos ejemplos es mostrar qué opciones están disponibles y cómo usarlas. Estos manifiestos de aplicación y de servicio se basan en los manifiestos del [ejemplo de contenedor de Windows Server 2016](https://github.com/Azure-Samples/service-fabric-containers/tree/master/Windows).

Se muestran las características siguientes:

|Manifest|Características|
|---|---|
|[Manifiesto de aplicación](#application-manifest)| [invalidar variables de entorno](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [configurar la asignación de puerto a host de contenedor](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery), [configurar la autenticación del registro de contenedor](service-fabric-get-started-containers.md#configure-container-repository-authentication), [gobernanza de recursos](service-fabric-resource-governance.md), [establecer el modo de aislamiento](service-fabric-get-started-containers.md#configure-isolation-mode), [especificar imágenes de contenedor de compilación específica del sistema operativo](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)| 
|[Manifiesto del servicio FrontEndService](#frontendservice-service-manifest)| [establecer variables de entorno](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [configurar un punto de conexión](service-fabric-get-started-containers.md#configure-communication), pasar comandos al contenedor, [importar un certificado en un contenedor](service-fabric-securing-containers.md)| 
|[Manifiesto del servicio BackEndService](#backendservice-service-manifest)|[establecer variables de entorno](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [configurar un punto de conexión](service-fabric-get-started-containers.md#configure-communication), [configurar un controlador de volumen](service-fabric-containers-volume-logging-drivers.md)| 

Consulte [Elementos del manifiesto de aplicación](#application-manifest-elements), [Elementos del manifiesto del servicio FrontEndService](#frontendservice-service-manifest-elements), y [Elementos del manifiesto del servicio BackEndService](#backendservice-service-manifest-elements) para más información sobre elementos específicos XML.

## <a name="application-manifest"></a>Manifiesto de aplicación

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Container.ApplicationType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="BackEndService_InstanceCount" DefaultValue="-1" />
    <Parameter Name="FrontEndService_InstanceCount" DefaultValue="-1" />
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="BlockIOWeight" DefaultValue="200" />
    <Parameter Name="MaximumIOBandwidth" DefaultValue="1024" />
    <Parameter Name="MemoryReservationInMB" DefaultValue="1024" />
    <Parameter Name="MemorySwapInMB" DefaultValue="4084"/>
    <Parameter Name="MaximumIOps" DefaultValue="20"/>
    <Parameter Name="MemoryFront" DefaultValue="4084" />
    <Parameter Name="MemoryBack" DefaultValue="2048" />
    <Parameter Name="CertThumbprint" DefaultValue=""/>
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="BackEndServicePkg" ServiceManifestVersion="1.0.0" />    
    
    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[MemoryFront]"/>

      <!-- Set resource governance at the code package level. -->
      <ResourceGovernancePolicy CodePackageRef="Code" CpuPercent="10" MemoryInMB="[MemoryFront]" BlockIOWeight="[BlockIOWeight]" MaximumIOBandwidth="[MaximumIOBandwidth]" MaximumIOps="[MaximumIOps]" MemoryReservationInMB="[MemoryReservationInMB]" MemorySwapInMB="[MemorySwapInMB]"/>
      
      <!-- Policies for activating container hosts. -->
      <ContainerHostPolicies CodePackageRef="Code" Isolation="process">
        
        <!-- Credentials for the repository hosting the container image.-->
        <RepositoryCredentials AccountName="sfsamples" Password="ENCRYPTED-PASSWORD" PasswordEncrypted="true"/>
        
        <!-- This binds the port the container is listening on (8905 in this sample) to an endpoint resource named "BackEndServiceTypeEndpoint", which is defined in the service manifest.  -->
        <PortBinding ContainerPort="8905" EndpointRef="BackEndServiceTypeEndpoint"/>
        
        <!-- Configure the Azure Files volume plugin.  Bind the source folder on the host VM or a remote share to the destination folder within the running container. -->
        <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
          <!-- Driver options to be passed to driver. The Azure Files volume plugin supports the following driver options:
            shareName (the Azure Files file share that provides the volume for the container), storageAccountName (the Azure storage account
            that contains the Azure Files file share), storageAccountKey (Access key for the Azure storage account that contains the Azure Files file share).
            These three driver options are required. -->
          <DriverOption Name="shareName" Value="" />
          <DriverOption Name="storageAccountName" Value="MY-STORAGE-ACCOUNT-NAME" />
          <DriverOption Name="storageAccountKey" Value="MY-STORAGE-ACCOUNT-KEY" />
        </Volume>
        
        <!-- Windows Server containers may not be compatible across different versions of the OS.  You can specify multiple OS images per container and tag 
        them with the build versions of the OS. Get the build version of the OS by running "winver" at a Windows command prompt. -->
        <ImageOverrides>
          <!-- If the underlying OS is build version 16299 (Windows Server version 1709), Service Fabric picks the container image tagged with Os="16299". -->
          <Image Name="sfsamples.azurecr.io/sfsamples/servicefabricbackendservice_1709" Os="16299" />
          
          <!-- An untagged container image is assumed to work across all versions of the OS and overrides the image specified in the service manifest. -->
          <Image Name="sfsamples.azurecr.io/sfsamples/servicefabricbackendservice_default" />          
        </ImageOverrides>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>

  <!-- Policies to be applied to the imported service manifest. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    
    <!-- This enables you to provide different values for environment variables when creating a FrontEndService
         Theses environment variables are declared in the FrontEndServiceType service manifest-->
    <EnvironmentOverrides CodePackageRef="Code">
      <EnvironmentVariable Name="BackendServiceName" Value="Container.Application/BackEndService"/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
      <EnvironmentVariable Name="IsContainer" Value="true"/>
    </EnvironmentOverrides>
    
    <!-- This policy maps the  port of the container (80) to the endpoint declared in the service, 
         FrontEndServiceTypeEndpoint which is exposed as port 80 on the host-->    
    <Policies>

      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[MemoryBack]"/>

      <!-- Policies for activating container hosts. -->
      <ContainerHostPolicies CodePackageRef="Code" Isolation="process">

        <!-- Credentials for the repository hosting the container image.-->
        <RepositoryCredentials AccountName="sfsamples" Password="ENCRYPTED-PASSWORD" PasswordEncrypted="true"/>

        <!-- Binds an endpoint resource (declared in the service manifest) to the exposed container port. -->
        <PortBinding ContainerPort="80" EndpointRef="FrontEndServiceTypeEndpoint"/>

        <!-- Import a certificate into the container.  The certificate must be installed in the LocalMachine store of all the cluster nodes.
          When the application starts, the runtime reads the certificate and generates a PFX file and password (on Windows) or a PEM file (on Linux).
          The PFX file and password are accessible in the container using the Certificates_ServicePackageName_CodePackageName_CertName_PFX and 
          Certificates_ServicePackageName_CodePackageName_CertName_Password environment variables. The PEM file is accessible in the container using the 
          Certificates_ServicePackageName_CodePackageName_CertName_PEM and Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey environment variables.-->
        <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[CertThumbprint]" />

        <!-- If the certificate is already in PFX or PEM form, you can create a data package inside your application and reference that certificate here. -->
        <CertificateRef Name="MyCert2" DataPackageRef="Data" DataPackageVersion="1.0.0" RelativePath="MyCert2.PFX" Password="ENCRYPTED-PASSWORD" IsPasswordEncrypted="true"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
        
    <Service Name="FrontEndService" >
      <StatelessService ServiceTypeName="FrontEndServiceType" InstanceCount="[FrontEndService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
        <Service Name="BackEndService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="BackEndServiceType" InstanceCount="[BackEndService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="frontendservice-service-manifest"></a>Manifiesto del servicio FrontEndService

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="FrontEndServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="FrontEndServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ContainerHost>
        <!--The repo and image on https://hub.docker.com or Azure Container Registry. -->
        <ImageName>sfsamples.azurecr.io/sfsamples/servicefabricfrontendservice:v1</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container or exe.  These variables are overridden in the application manifest. -->
    <EnvironmentVariables>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="IsContainer" Value=""/>
    </EnvironmentVariables>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />
  
  <!-- Data package is the contents of the Data directory under PackageRoot that contains an 
       independently-updateable and versioned static data that's consumed by the process at runtime. -->
  <DataPackage Name="Data" Version="1.0.0"/>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. For a guest executable is used to register with the NamingService at its REST endpoint
           with http scheme and port 80 -->
      <Endpoint Name="FrontEndServiceTypeEndpoint" UriScheme="http" Port="80"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="backendservice-service-manifest"></a>Manifiesto del servicio BackEndService

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="BackEndServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="BackEndServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ContainerHost>
        <!--The repo and image on https://hub.docker.com or Azure Container Registry. -->
        <ImageName>sfsamples.azurecr.io/sfsamples/servicefabricbackendservice:v1</ImageName>
        
        <!-- Pass comma delimited commands to your container. -->
        <Commands> dotnet, myproc.dll, 5 </Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container. These variables are overridden in the application manifest. -->
    <EnvironmentVariables>
      <EnvironmentVariable Name="IsContainer" Value="true"/>
    </EnvironmentVariables>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the host port on which to 
           listen. For a guest executable is used to register with the NamingService at its REST endpoint
           with http scheme. In this case since no port is specified, one is created and assigned dynamically
           to the service. This dynamically assigned host port is mapped to the container port (8905 in this sample),
            which was specified in the application manifest.-->
      <Endpoint Name="BackEndServiceTypeEndpoint" UriScheme="http" />
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

### <a name="policies-element"></a>Elemento Policies
Describe las directivas (enlace de punto de conexión, uso compartido de paquetes, ejecutar como y acceso de seguridad) que se van a aplicar en el manifiesto de servicio importado. Para más información, consulte [Elemento Policies](service-fabric-service-model-schema-elements.md#PoliciesElementServiceManifestImportPoliciesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="servicepackageresourcegovernancepolicy-element"></a>Elemento ServicePackageResourceGovernancePolicy
Define la directiva de gobernanza de recursos que se aplica en el nivel del paquete de servicio completo. Para más información, consulte [Elemento ServicePackageResourceGovernancePolicy](service-fabric-service-model-schema-elements.md#ServicePackageResourceGovernancePolicyElementServicePackageResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInServicePackageTypecomplexType)

### <a name="resourcegovernancepolicy-element"></a>Elemento ResourceGovernancePolicy
Especifica los límites de recursos de un paquete de código. Para más información, consulte [Elemento ResourceGovernancePolicy](service-fabric-service-model-schema-elements.md#ResourceGovernancePolicyElementResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelementDefinedInDigestedEndpointelement)

### <a name="containerhostpolicies-element"></a>Elemento ContainerHostPolicies
Especifica las directivas para activar los hosts de contenedor. Para más información, consulte [Elemento ContainerHostPolicies](service-fabric-service-model-schema-elements.md#ContainerHostPoliciesElementContainerHostPoliciesTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="repositorycredentials-element"></a>Elemento RepositoryCredentials
Las credenciales del repositorio de imágenes de contenedor del que extraer imágenes. Para más información, consulte [Elemento RepositoryCredentials](service-fabric-service-model-schema-elements.md#RepositoryCredentialsElementRepositoryCredentialsTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="portbinding-element"></a>Elemento PortBinding
Especifica el recurso de punto de conexión al que enlazar el puerto expuesto del contenedor. Para más información, consulte [Elemento PortBinding](service-fabric-service-model-schema-elements.md#PortBindingElementPortBindingTypeComplexTypeDefinedInServicePackageContainerPolicyTypecomplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="volume-element"></a>Elemento Volume
Especifica el volumen que se enlaza al contenedor. Para más información, consulte [Elemento Volume](service-fabric-service-model-schema-elements.md#VolumeElementContainerVolumeTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="driveroption-element"></a>Elemento DriverOption
Opciones de controlador que se pasan al controlador. Para más información, consulte [Elemento DriverOption](service-fabric-service-model-schema-elements.md#DriverOptionElementDriverOptionTypeComplexTypeDefinedInContainerLoggingDriverTypecomplexTypeDefinedInContainerVolumeTypecomplexType)

### <a name="imageoverrides-element"></a>Elemento ImageOverrides
Es posible que los contenedores Windows Server no sean compatibles con las distintas versiones del sistema operativo.  Puede especificar varias imágenes de sistema operativo por contenedor y etiquetarlas con las versiones de compilación del sistema operativo. Obtenga la versión de compilación del sistema operativo ejecutando "winver" en un símbolo del sistema de Windows. Si el sistema operativo subyacente tiene la versión de compilación 16299 (versión Windows Server 1709), Service Fabric seleccionará la imagen de contenedor etiquetada con Os="16299". Se supone que una imagen de contenedor no etiquetada funcionará con todas las versiones del sistema operativo y que reemplazará la imagen especificada en el manifiesto de servicio. Para más información, consulte [Elemento ImageOverrides](service-fabric-service-model-schema-elements.md#ImageOverridesElementImageOverridesTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="image-element"></a>Elemento de imagen
Imagen de contenedor correspondiente al número de versión de compilación del sistema operativo que se iniciará. Si el atributo del sistema operativo no está especificado, se supone que la imagen de contenedor funcionará en todas las versiones del sistema operativo y que reemplazará la imagen especificada en el manifiesto de servicio. Para más información, consulte [Elemento Image](service-fabric-service-model-schema-elements.md#ImageElementImageTypeComplexTypeDefinedInImageOverridesTypecomplexType)

### <a name="environmentoverrides-element"></a>Elemento EnvironmentOverrides
 Para más información, consulte [Elemento EnvironmentOverrides](service-fabric-service-model-schema-elements.md#EnvironmentOverridesElementEnvironmentOverridesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="environmentvariable-element"></a>Elemento EnvironmentVariable
Nombre de la variable de entorno. Para más información, consulte [Elemento EnvironmentVariable](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="certificateref-element"></a>Elemento CertificateRef
Especifica información sobre un certificado X509 que se va a exponer en el entorno del contenedor. El certificado debe instalarse en el almacén LocalMachine de todos los nodos de clúster.
Cuando la aplicación se inicia, el entorno de tiempo de ejecución lee el certificado y genera un archivo PFX y una contraseña (en Windows) o un archivo PEM (en Linux).
En el contenedor se puede acceder a la contraseña y al archivo PFX con las variables de entorno Certificates_ServicePackageName_CodePackageName_CertName_PFX y Certificates_ServicePackageName_CodePackageName_CertName_Password. En el contenedor se puede acceder al archivo PEM con las variables de entorno Certificates_ServicePackageName_CodePackageName_CertName_PEM y Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey. Para más información, consulte [Elemento CertificateRef](service-fabric-service-model-schema-elements.md#CertificateRefElementContainerCertificateTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="defaultservices-element"></a>Elemento DefaultServices
Declara instancias de servicio que se crean automáticamente cada vez que se crea una instancia de una aplicación en este tipo de aplicación. Para más información, consulte [Elemento DefaultServices](service-fabric-service-model-schema-elements.md#DefaultServicesElementDefaultServicesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInApplicationInstanceTypecomplexType)

### <a name="service-element"></a>Elemento Service
Declara que un servicio se cree automáticamente cuando se crea una instancia de la aplicación. Para más información, consulte [Elemento Service](service-fabric-service-model-schema-elements.md#ServiceElementanonymouscomplexTypeComplexTypeDefinedInDefaultServicesTypecomplexType)

### <a name="statelessservice-element"></a>Elemento StatelessService
Define un servicio sin estado. Para más información, consulte [Elemento StatelessService](service-fabric-service-model-schema-elements.md#StatelessServiceElementStatelessServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement)


## <a name="frontendservice-service-manifest-elements"></a>Elementos del manifiesto del servicio FrontEndService
### <a name="servicemanifest-element"></a>Elemento ServiceManifest
Describe mediante declaración el tipo de servicio y la versión. Enumera los paquetes de código, configuración y datos que se pueden actualizar de manera independiente que, juntos, constituyen un paquete de servicio para admitir uno o más tipos de servicio. También se especifican los recursos, la configuración de diagnóstico y los metadatos de servicio, como el tipo de servicio, las propiedades de mantenimiento y las métricas de equilibrio de carga. Para más información, consulte [Elemento ServiceManifest](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>Elemento ServiceTypes
Define los tipos de servicios que son compatibles con CodePackage en este manifiesto. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Los tipos de servicio se declaran en el nivel de manifiesto y no en el nivel de paquete de código. Para más información, consulte [Elemento ServiceTypes](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>Elemento StatelessServiceType
Describe un tipo de servicio sin estado. Para más información, consulte [Elemento StatelessServiceType](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>Elemento CodePackage
Describe un paquete de código que admite un tipo de servicio definido. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Se espera que los procesos resultantes registren los tipos de servicio admitidos en tiempo de ejecución. Cuando hay varios paquetes de código, se activan todos cada vez que el sistema busca cualquiera de los tipos de servicios declarados. Para más información, consulte [Elemento CodePackage](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>Elemento EntryPoint
El archivo ejecutable especificado por EntryPoint suele ser el host de servicios de ejecución prolongada. La presencia de un punto de entrada de configuración independiente evita tener que ejecutar el host de servicio con privilegios elevados durante largos períodos de tiempo. El archivo ejecutable especificado por EntryPoint se ejecuta después de que SetupEntryPoint se cierra correctamente. El proceso resultante se supervisa y reinicia (comenzando de nuevo con SetupEntryPoint) si alguna vez finaliza o se bloquea. Para más información, consulte [Elemento EntryPoint](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="containerhost-element"></a>Elemento ContainerHost
 Para más información, consulte [Elemento ContainerHost](service-fabric-service-model-schema-elements.md#ContainerHostElementContainerHostEntryPointTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="imagename-element"></a>Elemento ImageName
El repositorio y la imagen de [https://hub.docker.com](https://hub.docker.com) o Azure Container Registry. Para más información, consulte [Elemento ImageName](service-fabric-service-model-schema-elements.md#ImageNameElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="environmentvariables-element"></a>Elemento EnvironmentVariables
Pase variables de entorno al contenedor o exe.  Para más información, consulte [Elemento EnvironmentVariables](service-fabric-service-model-schema-elements.md#EnvironmentVariablesElementEnvironmentVariablesTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="environmentvariable-element"></a>Elemento EnvironmentVariable
Nombre de la variable de entorno. Para más información, consulte [Elemento EnvironmentVariable](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="configpackage-element"></a>Elemento ConfigPackage
Declara una carpeta, designada por el atributo Name, que contiene un archivo Settings.xml. Este archivo contiene secciones de configuración del par clave-valor definida por el usuario que el proceso puede volver a leer en tiempo de ejecución. Durante una actualización, si solo ha cambiado la versión de ConfigPackage, no se reiniciará el proceso en ejecución. En su lugar, una devolución de llamada notifica el proceso que los ajustes de configuración han cambiado, por lo que pueden volver a cargarse de forma dinámica. Para más información, consulte [Elemento ConfigPackage](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="datapackage-element"></a>Elemento DataPackage
Declara una carpeta, designada por el atributo Name, que contiene archivos de datos estáticos. Service Fabric reciclará todos los archivos EXE y DLLHOST especificados en los paquetes de host y soporte técnico cuando cualquiera de los paquetes de datos enumerados en el manifiesto de servicio se actualice. Para más información, consulte [Elemento DataPackage](service-fabric-service-model-schema-elements.md#DataPackageElementDataPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedDataPackageelement)

### <a name="resources-element"></a>Elemento Resources
Describe los recursos utilizados por este servicio, que se pueden declarar sin modificar el código compilado y cambiar cuando se implemente el servicio. El acceso a estos recursos se controla mediante las secciones Entidades de seguridad y Directivas del manifiesto de aplicación. Para más información, consulte [Elemento Resources](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Elemento Endpoints
Define los puntos de conexión del servicio. Para más información, consulte [Elemento Endpoints](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Elemento Endpoint
Para más información, consulte [Elemento Endpoint](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)


## <a name="backendservice-service-manifest-elements"></a>Elementos del manifiesto del servicio BackEndService
### <a name="servicemanifest-element"></a>Elemento ServiceManifest
Describe mediante declaración el tipo de servicio y la versión. Enumera los paquetes de código, configuración y datos que se pueden actualizar de manera independiente que, juntos, constituyen un paquete de servicio para admitir uno o más tipos de servicio. También se especifican los recursos, la configuración de diagnóstico y los metadatos de servicio, como el tipo de servicio, las propiedades de mantenimiento y las métricas de equilibrio de carga. Para más información, consulte [Elemento ServiceManifest](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType)

### <a name="servicetypes-element"></a>Elemento ServiceTypes
Define los tipos de servicios que son compatibles con CodePackage en este manifiesto. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Los tipos de servicio se declaran en el nivel de manifiesto y no en el nivel de paquete de código. Para más información, consulte [Elemento ServiceTypes](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="statelessservicetype-element"></a>Elemento StatelessServiceType
Describe un tipo de servicio sin estado. Para más información, consulte [Elemento StatelessServiceType](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>Elemento CodePackage
Describe un paquete de código que admite un tipo de servicio definido. Cuando se crea una instancia de un servicio en uno de estos tipos de servicio, todos los paquetes de código declarados en este manifiesto se activan mediante la ejecución de sus puntos de entrada. Se espera que los procesos resultantes registren los tipos de servicio admitidos en tiempo de ejecución. Cuando hay varios paquetes de código, se activan todos cada vez que el sistema busca cualquiera de los tipos de servicios declarados. Para más información, consulte [Elemento CodePackage](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement)

### <a name="entrypoint-element"></a>Elemento EntryPoint
El archivo ejecutable especificado por EntryPoint suele ser el host de servicios de ejecución prolongada. La presencia de un punto de entrada de configuración independiente evita tener que ejecutar el host de servicio con privilegios elevados durante largos períodos de tiempo. El archivo ejecutable especificado por EntryPoint se ejecuta después de que SetupEntryPoint se cierra correctamente. El proceso resultante se supervisa y reinicia (comenzando de nuevo con SetupEntryPoint) si alguna vez finaliza o se bloquea. Para más información, consulte [Elemento EntryPoint](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="containerhost-element"></a>Elemento ContainerHost
Para más información, consulte [Elemento ContainerHost](service-fabric-service-model-schema-elements.md#ContainerHostElementContainerHostEntryPointTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType)

### <a name="imagename-element"></a>Elemento ImageName
El repositorio y la imagen de [https://hub.docker.com](https://hub.docker.com) o Azure Container Registry. Para más información, consulte [Elemento ImageName](service-fabric-service-model-schema-elements.md#ImageNameElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="commands-element"></a>Elemento Commands
Pase una lista de comandos delimitados por comas al contenedor. Para más información, consulte [Elemento Commands](service-fabric-service-model-schema-elements.md#CommandsElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="environmentvariables-element"></a>Elemento EnvironmentVariables
Pase variables de entorno al contenedor o exe.  Para más información, consulte [Elemento EnvironmentVariables](service-fabric-service-model-schema-elements.md#EnvironmentVariablesElementEnvironmentVariablesTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="environmentvariable-element"></a>Elemento EnvironmentVariable
Nombre de la variable de entorno. Para más información, consulte [Elemento EnvironmentVariable](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="configpackage-element"></a>Elemento ConfigPackage
Declara una carpeta, designada por el atributo Name, que contiene un archivo Settings.xml. Este archivo contiene secciones de configuración del par clave-valor definida por el usuario que el proceso puede volver a leer en tiempo de ejecución. Durante una actualización, si solo ha cambiado la versión de ConfigPackage, no se reiniciará el proceso en ejecución. En su lugar, una devolución de llamada notifica el proceso que los ajustes de configuración han cambiado, por lo que pueden volver a cargarse de forma dinámica. Para más información, consulte [Elemento ConfigPackage](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="resources-element"></a>Elemento Resources
Describe los recursos utilizados por este servicio, que se pueden declarar sin modificar el código compilado y cambiar cuando se implemente el servicio. El acceso a estos recursos se controla mediante las secciones Entidades de seguridad y Directivas del manifiesto de aplicación. Para más información, consulte [Elemento Resources](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType)

### <a name="endpoints-element"></a>Elemento Endpoints
Define los puntos de conexión del servicio. Para más información, consulte [Elemento Endpoints](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Elemento Endpoint
 Para más información, consulte [Elemento Endpoint](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

