---
title: Procedimientos recomendados de seguridad de Azure Service Fabric
description: Procedimientos recomendados y consideraciones de diseño para mantener protegidos los clústeres y las aplicaciones de Azure Service Fabric.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: dcdc338bdcdb2c04f6b8894ccb358bc773b95c07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79229456"
---
# <a name="azure-service-fabric-security"></a>Seguridad de Azure Service Fabric 

Para más información acerca de los [procedimientos recomendados de seguridad de Azure](https://docs.microsoft.com/azure/security/), revise [Procedimientos recomendados de seguridad de Azure Service Fabric](https://docs.microsoft.com/azure/security/fundamentals/service-fabric-best-practices).

## <a name="key-vault"></a>Key Vault

[Azure Key Vault](https://docs.microsoft.com/azure/key-vault/) es el servicio de administración de secretos recomendado para aplicaciones y clústeres de Azure Service Fabric.
> [!NOTE]
> Si se implementan certificados o secretos de una instancia de Key Vault en un conjunto de escalado de máquinas virtuales como un secreto de conjunto de escalado de máquinas virtuales, la instancia de Key Vault y el conjunto de escalado de máquinas virtuales deben compartir ubicación.

## <a name="create-certificate-authority-issued-service-fabric-certificate"></a>Creación de un certificado de Service Fabric emitido por una entidad de certificación

Un certificado de Azure Key Vault se puede crear o importar en una instancia de Key Vault. Cuando se crea un certificado de Key Vault, la clave privada se crea en la instancia de Key Vault y no se expone al propietario del certificado. Estas son las formas de crear un certificado en Key Vault:

- Cree un certificado autofirmado para crear un par de claves pública y privada para asociarlo a un certificado. El certificado se firmará con su propia clave. 
- Cree manualmente un nuevo certificado para crear un par de claves pública y privada y generar una solicitud de firma de certificado X.509. La solicitud de firma puede estar firmada por la autoridad de registro o la entidad de certificación. Para completar el certificado KV en Key Vault, el certificado X.509 se puede combinar con el par de claves pendiente. Aunque este método requiere más pasos, proporciona mayor seguridad, ya que la clave privada se crea en Key Vault y se restringe. Esto se explica en el diagrama siguiente. 

Revise [Métodos de creación de certificados de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/create-certificate) para obtener más detalles.

## <a name="deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets"></a>Implementación de certificados de Key Vault en conjuntos de escalado de máquinas virtuales de clúster de Service Fabric

Para implementar certificados desde un almacén de claves colocalizado en un conjunto de escalado de máquinas virtuales, use el conjunto de escalado de máquinas virtuales [osProfile](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile). A continuación, se indican las propiedades de la plantilla de Resource Manager:

```json
"secrets": [
   {
       "sourceVault": {
           "id": "[parameters('sourceVaultValue')]"
       },
       "vaultCertificates": [
          {
              "certificateStore": "[parameters('certificateStoreValue')]",
              "certificateUrl": "[parameters('certificateUrlValue')]"
          }
       ]
   }
]
```

> [!NOTE]
> El almacén debe estar habilitado para la implementación de plantillas de Resource Manager.

## <a name="apply-an-access-control-list-acl-to-your-certificate-for-your-service-fabric-cluster"></a>Aplicación de una lista de control de acceso (ACL) a su certificado para el clúster de Service Fabric

El publicador de [extensiones de conjuntos de escalado de máquinas virtuales](https://docs.microsoft.com/cli/azure/vmss/extension?view=azure-cli-latest) Microsoft.Azure.ServiceFabric se usa para configurar la seguridad de los nodos.
Para aplicar una ACL a los certificados para los procesos de clúster de Service Fabric, use las siguientes propiedades de plantilla de Resource Manager:

```json
"certificate": {
   "commonNames": [
       "[parameters('certificateCommonName')]"
   ],
   "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

## <a name="secure-a-service-fabric-cluster-certificate-by-common-name"></a>Protección de un certificado de clúster de Service Fabric por nombre común

Para proteger su clúster de Service Fabric mediante el certificado `Common Name`, use la propiedad de plantilla de Resource Manager [certificateCommonNames](https://docs.microsoft.com/rest/api/servicefabric/sfrp-model-clusterproperties#certificatecommonnames) de la siguiente manera:

```json
"certificateCommonNames": {
    "commonNames": [
        {
            "certificateCommonName": "[parameters('certificateCommonName')]",
            "certificateIssuerThumbprint": "[parameters('certificateIssuerThumbprint')]"
        }
    ],
    "x509StoreName": "[parameters('certificateStoreValue')]"
}
```

> [!NOTE]
> El clúster de Service Fabric usará el primer certificado válido que se encuentre en el almacén de certificados del host. En Windows, este será el certificado con la fecha de expiración más tardía que coincida con su nombre común y la huella digital del emisor.

Los dominios de Azure, como *\<SU SUBDOMINIO\>.cloudapp.azure.com o \<SU SUBDOMINIO\>.trafficmanager.net, pertenecen a Microsoft. Las entidades de certificación no emitirán certificados para dominios a usuarios no autorizados. La mayoría de los usuarios deberá adquirir un dominio a un registrador o ser un administrador de dominio autorizado para que una entidad de certificación le emita un certificado con ese nombre común.

Para más información sobre cómo configurar el servicio DNS para resolver el dominio en una dirección IP de Microsoft, consulte cómo configurar [Azure DNS para hospedar su dominio](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns).

> [!NOTE]
> Después de la delegación de los servidores de nombres de dominio a los servidores de nombre de la zona de Azure DNS, agregue los dos registros siguientes a la zona DNS:
> - Un registro "A" para el dominio APEX que NO es `Alias record set` en todas las direcciones IP en las que se resolverá su dominio personalizado.
> - Un registro "C" para subdominios de Microsoft aprovisionados que no son `Alias record set`. Por ejemplo, podría utilizar el nombre DNS de Traffic Manager o Load Balancer.

Para actualizar el portal para mostrar un nombre DNS personalizado para su clúster de Service Fabric `"managementEndpoint"`, actualice las siguientes propiedades de plantilla de Resource Manager del clúster de Service Fabric:

```json
 "managementEndpoint": "[concat('https://<YOUR CUSTOM DOMAIN>:',parameters('nt0fabricHttpGatewayPort'))]",
```

## <a name="encrypting-service-fabric-package-secret-values"></a>Cifrado de los valores de secretos del paquete de Service Fabric

Entre los valores habituales que se cifran en paquetes de Service Fabric se incluyen credenciales de Azure Container Registry (ACR), variables de entorno, opciones de configuración y claves de cuenta de almacenamiento de complemento de volumen de Azure.

Para [configurar un certificado de cifrado y cifrar secretos en clústeres de Windows](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-windows):

Genere un certificado autofirmado para cifrar su secreto:

```powershell
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
```

Siga las instrucciones de [Implementación de certificados de Key Vault en conjuntos de escalado de máquinas virtuales de clúster de Service Fabric](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) para implementar dichos certificados en los conjuntos de escalado de máquinas virtuales de su clúster de Service Fabric.

Cifre el secreto con el siguiente comando de PowerShell y, a continuación, actualice el manifiesto de la aplicación de Service Fabric con el valor cifrado:

``` powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Para [configurar un certificado de cifrado y cifrar secretos en clústeres de Linux](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management-linux):

Genere un certificado autofirmado para cifrar sus secretos:

```bash
user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
user@linux:~$ cat TestCert.prv >> TestCert.pem
```

Siga las instrucciones de [Implementación de certificados de Key Vault en conjuntos de escalado de máquinas virtuales de clúster de Service Fabric](#deploy-key-vault-certificates-to-service-fabric-cluster-virtual-machine-scale-sets) para implementar dichos certificados en los conjuntos de escalado de máquinas virtuales de su clúster de Service Fabric.

Cifre el secreto mediante los comandos siguientes y, a continuación, actualice el manifiesto de la aplicación de Service Fabric con el valor cifrado:

```bash
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```

Después de cifrar los valores protegidos, [especifique los secretos cifrados en la aplicación de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#specify-encrypted-secrets-in-an-application) y [descifre los secretos cifrados desde el código de servicio](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management#decrypt-encrypted-secrets-from-service-code).

## <a name="include-certificate-in-service-fabric-applications"></a>Inclusión de certificados en aplicaciones de Service Fabric

Para proporcionar a la aplicación acceso a los secretos, incluya el certificado agregando un elemento **SecretsCertificate** al manifiesto de aplicación.

```xml
<ApplicationManifest … >
  ...
  <Certificates>
    <SecretsCertificate Name="MyCert" X509FindType="FindByThumbprint" X509FindValue="[YourCertThumbrint]"/>
  </Certificates>
</ApplicationManifest>
```
## <a name="authenticate-service-fabric-applications-to-azure-resources-using-managed-service-identity-msi"></a>Autentique las aplicaciones de Service Fabric en recursos de Azure mediante Managed Service Identity (MSI)

Para más información sobre las identidades administradas para recursos de Azure, consulte [¿Qué es Managed Identities for Azure Resources?](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work).
Los clústeres de Azure Service Fabric se hospedan en Virtual Machine Scale Sets, que admite [Managed Service Identity](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-managed-identities-for-azure-resources).
Para obtener una lista de servicios en los que se puede usar MSI para autenticarse, consulte [Servicios de Azure que admiten la autenticación de Azure AD](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-msi#azure-services-that-support-azure-ad-authentication).


Para habilitar la identidad administrada asignada por el sistema durante la creación de un conjunto de escalado de máquinas virtuales o un conjunto de escalado de máquinas virtuales existente, declare la siguiente propiedad `"Microsoft.Compute/virtualMachinesScaleSets"`:

```json
"identity": { 
    "type": "SystemAssigned"
}
```
Para más información, consulte [¿Qué es Managed Identities for Azure Resources?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-template-windows-vmss#system-assigned-managed-identity).

Si ha creado una [identidad administrada asignada por el usuario](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm#create-a-user-assigned-managed-identity), declare el recurso siguiente en la plantilla para asignarla al conjunto de escalado de máquinas virtuales. Reemplace `\<USERASSIGNEDIDENTITYNAME\>` por el nombre de la identidad administrada asignada por el usuario que ha creado:

```json
"identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
        "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
    }
}
```

Antes de que su aplicación de Service Fabric pueda utilizar una identidad administrada, se deben conceder permisos a los recursos de Azure que necesita para autenticarse.
Los siguientes comandos conceden acceso a un recurso de Azure:

```bash
principalid=$(az resource show --id /subscriptions/<YOUR SUBSCRIPTON>/resourceGroups/<YOUR RG>/providers/Microsoft.Compute/virtualMachineScaleSets/<YOUR SCALE SET> --api-version 2018-06-01 | python -c "import sys, json; print(json.load(sys.stdin)['identity']['principalId'])")

az role assignment create --assignee $principalid --role 'Contributor' --scope "/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/<PROVIDER NAME>/<RESOURCE TYPE>/<RESOURCE NAME>"
```

En el código de su aplicación de Service Fabric, [obtenga un token de acceso](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token#get-a-token-using-http) para Azure Resource Manager con un REST similar al siguiente:

```bash
access_token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true | python -c "import sys, json; print json.load(sys.stdin)['access_token']")

```

A continuación, la aplicación de Service Fabric puede usar el token de acceso para autenticarse en recursos de Azure que admiten Active Directory.
El ejemplo siguiente muestra cómo hacerlo para el recurso Cosmos DB:

```bash
cosmos_db_password=$(curl 'https://management.azure.com/subscriptions/<YOUR SUBSCRIPTION>/resourceGroups/<YOUR RG>/providers/Microsoft.DocumentDB/databaseAccounts/<YOUR ACCOUNT>/listKeys?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer $access_token" | python -c "import sys, json; print(json.load(sys.stdin)['primaryMasterKey'])")
```
## <a name="windows-security-baselines"></a>Líneas base de seguridad de Windows
[Es recomendable que implemente una configuración estándar del sector que sea ampliamente conocida y esté probada, como las líneas base de seguridad de Microsoft, en lugar de crear una línea base usted mismo](https://docs.microsoft.com/windows/security/threat-protection/windows-security-baselines). Una opción para aprovisionarlas en Virtual Machine Scale Sets consiste en usar el controlador de extensión Desired State Configuration (DSC) de Azure para configurar las máquinas virtuales a medida que se conecten, de modo que ejecuten el software de producción.

## <a name="azure-firewall"></a>Azure Firewall
[Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de Azure Virtual Network. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad en la nube sin restricciones.](https://docs.microsoft.com/azure/firewall/overview) Esto permite limitar el tráfico de salida HTTP/S a una lista especificada de nombres de dominio completos (FQDN) con caracteres comodín. Esta característica no requiere terminación de SSL. Se recomienda que aproveche las [etiquetas FQDN de Azure Firewall](https://docs.microsoft.com/azure/firewall/fqdn-tags) para Windows Update y que habilite el tráfico de red a los puntos de conexión de Microsoft Windows Update para que pueda fluir a través del firewall. En [Implementación de Azure Firewall mediante una plantilla](https://docs.microsoft.com/azure/firewall/deploy-template) encontrará un ejemplo de definición de la plantilla de recursos Microsoft.Network/azureFirewalls. En las reglas de firewall habituales de las aplicaciones de Service Fabric se permite lo siguiente para la red virtual de clústeres:

- *download.microsoft.com
- *servicefabric.azure.com
- *.core.windows.net

Estas reglas de firewall complementan los grupos de seguridad de red de salida permitidos, que incluirían ServiceFabric y Storage, como destinos aceptados desde la red virtual.

## <a name="tls-12"></a>TLS 1.2
[TSG](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides/blob/master/Security/TLS%20Configuration.md)

## <a name="windows-defender"></a>Windows Defender 

De forma predeterminada, el antivirus Windows Defender está instalado en Windows Server 2016. Para más información, consulte [Antivirus Windows Defender en Windows Server 2016](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016). La interfaz de usuario está instalada de forma predeterminada en algunas SKU, pero no es necesaria. Para reducir cualquier impacto en el rendimiento y sobrecarga en el consumo de recursos provocados por Windows Defender, siempre que sus directivas de seguridad permitan excluir procesos y rutas de acceso para software de código abierto, declare las siguientes propiedades de plantilla de Resource Manager de extensión de conjuntos de escalado de máquinas virtuales para excluir su clúster de Service Fabric de los análisis:


```json
 {
    "name": "[concat('VMIaaSAntimalware','_vmNodeType0Name')]",
    "properties": {
        "publisher": "Microsoft.Azure.Security",
        "type": "IaaSAntimalware",
        "typeHandlerVersion": "1.5",
        "settings": {
            "AntimalwareEnabled": "true",
            "Exclusions": {
                "Paths": "[concat(parameters('svcFabData'), ';', parameters('svcFabLogs'), ';', parameters('svcFabRuntime'))]",
                "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
            },
            "RealtimeProtectionEnabled": "true",
            "ScheduledScanSettings": {
                "isEnabled": "true",
                "scanType": "Quick",
                "day": "7",
                "time": "120"
            }
        },
        "protectedSettings": null
    }
}
```

> [!NOTE]
> Consulte su documentación antimalware para obtener reglas de configuración si no utiliza Windows Defender. Windows Defender no es compatible con Linux.

## <a name="platform-isolation"></a>Aislamiento de la plataforma
De forma predeterminada, las aplicaciones de Service Fabric reciben acceso al propio tiempo de ejecución de Service Fabric, que se manifiesta de formas diferentes: [variables de entorno](service-fabric-environment-variables-reference.md) que apuntan a rutas de acceso de archivo en el host correspondiente a los archivos de la aplicación y de Fabric, un punto de conexión de comunicación entre procesos que acepta solicitudes específicas de la aplicación y el cliente de certificados que Fabric espera que la aplicación use para autenticarse. En caso de que el propio servicio hospede código que no es de confianza, es aconsejable deshabilitar este acceso al tiempo de ejecución de SF, a menos que se necesite explícitamente. Para eliminar el acceso al tiempo de ejecución, use la declaración siguiente en la sección Directivas del manifiesto de aplicación: 

```xml
<ServiceManifestImport>
    <Policies>
        <ServiceFabricRuntimeAccessPolicy RemoveServiceFabricRuntimeAccess="true"/>
    </Policies>
</ServiceManifestImport>

```

## <a name="next-steps"></a>Pasos siguientes

* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster de Service Fabric para Windows Server](service-fabric-cluster-creation-for-windows-server.md).
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Creación de un clúster de Linux](service-fabric-cluster-creation-via-portal.md).
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md).

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
