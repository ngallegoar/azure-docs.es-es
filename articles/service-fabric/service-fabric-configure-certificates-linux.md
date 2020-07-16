---
title: Configuración de certificados para aplicaciones en Linux
description: Configuración de certificados para una aplicación con el sistema de tiempo de ejecución de Service Fabric en un clúster Linux
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: pepogors
ms.openlocfilehash: a97c8b8315fe3be405aed9c6570004afb8fafd1d
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86258675"
---
# <a name="certificates-and-security-on-linux-clusters"></a>Certificados y seguridad en clústeres Linux

En este artículo se proporciona información acerca de cómo configurar los certificados X.509 en clústeres Linux.

## <a name="location-and-format-of-x509-certificates-on-linux-nodes"></a>Ubicación y formato de los certificados X.509 en nodos Linux

Por lo general, Service Fabric espera que los certificados X.509 se encuentren en el directorio */var/lib/sfcerts* en los nodos de clúster Linux. Esto es así para los certificados de clúster, los certificados de cliente, etc. En algunos casos, puede especificar una ubicación distinta de la carpeta *var/lib/sfcerts* para los certificados. Por ejemplo, en el caso de servicios de Reliable Services creados con el SDK de Service Fabric para Java, puede especificar una ubicación diferente con el paquete de configuración (Settings.xml) para algunos certificados específicos de la aplicación. Para más información, consulte [Certificados a los que se hace referencia en el paquete de configuración (Settings.xml)](#certificates-referenced-in-the-configuration-package-settingsxml).

En el caso de los clústeres Linux, Service Fabric espera que los certificados estén como un archivo .pem que contiene el certificado y la clave privada o como un archivo .crt que contiene el certificado y un archivo .key que contiene la clave privada. Todos los archivos deben estar en formato PEM. 

Si instala el certificado desde Azure Key Vault mediante una [plantilla de Resource Manager](./service-fabric-cluster-creation-create-template.md) o comandos de [PowerShell](/powershell/module/az.servicefabric/?view=azps-2.6.0), el certificado se instala en el formato correcto en el directorio */var/lib/sfcerts* en cada nodo. Si instala un certificado con otro método, debe asegurarse de que el certificado está instalado correctamente en los nodos del clúster.

## <a name="certificates-referenced-in-the-application-manifest"></a>Certificados a los que se hace referencia en el manifiesto de aplicación

Los certificados especificados en el manifiesto de aplicación, por ejemplo, mediante los elementos [**SecretsCertificate**](./service-fabric-service-model-schema-elements.md#secretscertificate-element) o [**EndpointCertificate**](./service-fabric-service-model-schema-elements.md#endpointcertificate-element), debe estar presente en el directorio */var/lib/sfcerts*. Los elementos que se usan para especificar los certificados en el manifiesto de aplicación no tienen un atributo de ruta de acceso, por lo que los certificados deben estar presentes en el directorio predeterminado. Estos elementos tienen el atributo opcional **X509StoreName**. El valor predeterminado es "My", que apunta al directorio */var/lib/sfcerts* en nodos Linux. Cualquier otro valor no está definida en un clúster Linux. Se recomienda omitir el atributo **X509StoreName** para las aplicaciones que se ejecutan en clústeres Linux. 

## <a name="certificates-referenced-in-the-configuration-package-settingsxml"></a>Certificados a los que se hace referencia en el paquete de configuración (Settings.xml)

Para algunos servicios, puede configurar los certificados X.509 en [ConfigPackage](./service-fabric-application-and-service-manifests.md) (de forma predeterminada, Settings.xml). Este es el caso, por ejemplo, cuando se declaran certificados que se usan para proteger los canales RPC para los servicios de Reliable Services creados con los SDK de Service Fabric para .NET Core o Java. Hay dos maneras de hacer referencia a los certificados en el paquete de configuración. La compatibilidad es diferente en los SDK para .NET Core y Java.

### <a name="using-x509-securitycredentialstype"></a>Uso de SecurityCredentialsType X509

Con los SDK para .NET o Java, puede especificar **X509** como valor de **SecurityCredentialsType**. Esto corresponde al tipo `X509Credentials` ([.NET](/previous-versions/azure/reference/mt124925(v=azure.100))/[Java](/java/api/system.fabric.x509credentials)) de `SecurityCredentials` ([.NET](/previous-versions/azure/reference/mt124894(v=azure.100))/[Java](/java/api/system.fabric.securitycredentials)).

La referencia a **X509** busca el certificado en un almacén de certificados. El siguiente código XML muestra los parámetros utilizados para especificar la ubicación del certificado:

```xml
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
```

Para un servicio que se ejecuta en Linux, **LocalMachine**/**My** apunta a la ubicación predeterminada de los certificados, el directorio */var/lib/sfcerts*. En el caso de Linux, cualquier otra combinación de **CertificateStoreLocation** y **CertificateStoreName** no está definida. 

Especifique siempre **LocalMachine** en el parámetro **CertificateStoreLocation**. No es necesario especificar el parámetro **CertificateStoreName** porque el valor predeterminado es "My". Cuando se hace referencia a **X509**, los archivos de certificado deben encontrarse en el directorio */var/lib/sfcerts*, en el nodo de clúster.  

El siguiente código XML muestra una sección **TransportSettings** basada en este estilo:

```xml
<Section Name="HelloWorldStatefulTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
    <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
</Section>
```

### <a name="using-x509_2-securitycredentialstype"></a>Uso de SecurityCredentialsType X509_2

Con el SDK para Java, puede especificar **X509_2** como valor de **SecurityCredentialsType**. Esto corresponde al tipo `X509Credentials2` ([Java](/java/api/system.fabric.x509credentials2)) de `SecurityCredentials` ([Java](/java/api/system.fabric.securitycredentials)). 

Cuando se hace referencia a **X509_2**, especifique un parámetro de ruta de acceso para poder encontrar el certificado en un directorio distinto de */var/lib/sfcerts*.  El siguiente código XML muestra los parámetros utilizados para especificar la ubicación del certificado: 

```xml
     <Parameter Name="SecurityCredentialsType" Value="X509_2" />
     <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
```

El siguiente código XML muestra una sección **TransportSettings** basada en este estilo.

```xml
<!--Section name should always end with "TransportSettings".-->
<!--Here we are using a prefix "HelloWorldStateless".-->
<Section Name="HelloWorldStatelessTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509_2" />
    <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
    <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
</Section>
```

> [!NOTE]
> El certificado se especifica como un archivo .crt en el código XML anterior. Esto implica que también hay un archivo .key que contiene la clave privada en la misma ubicación.

## <a name="configure-a-reliable-services-app-to-run-on-linux-clusters"></a>Configuración de una aplicación de Reliable Services para ejecutarse en clústeres Linux

El SDK de Service Fabric permite comunicarse con las API en tiempo de ejecución de Service Fabric y aprovechar así la plataforma. Al ejecutar aplicaciones que usan esta funcionalidad en clústeres Linux seguros, debe configurar la aplicación con un certificado que pueda usar para validar con el sistema de tiempo de ejecución de Service Fabric. Las aplicaciones que contienen servicios de Reliable Services de Service Fabric escritos con los SDK para .NET Core o Java requieren esta configuración. 

Para configurar una aplicación, agregue un elemento [**SecretsCertificate**](./service-fabric-service-model-schema-elements.md#secretscertificate-element) en la etiqueta **Certificates**, que se encuentra en la etiqueta **ApplicationManifest** en el archivo *ApplicationManifest.xml*. El siguiente código XML muestra un certificado que se hace referencia por su huella digital: 

```xml
   <Certificates>
       <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
   </Certificates>   
```

Puede hacer referencia el certificado de clúster o a un certificado que instale en cada nodo del clúster. En Linux, los archivos de certificado deben estar presentes en el directorio */var/lib/sfcerts*. Para más información, consulte [Ubicación y formato de los certificados X.509 en nodos Linux](#location-and-format-of-x509-certificates-on-linux-nodes).
