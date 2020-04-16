---
title: Desarrollo de una aplicación web segura de Azure AD | Microsoft Docs
description: Esta sencilla aplicación de ejemplo implementa procedimientos recomendados de seguridad que mejoran la seguridad de la aplicación y la organización al desarrollar en Azure.
keywords: na
services: security
documentationcenter: na
author: TerryLanfear
manager: alclabo
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2019
ms.author: terrylan
ms.openlocfilehash: 599c4a31840b47294b43c4c4d1f0200b17f04540
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/07/2020
ms.locfileid: "80810535"
---
# <a name="develop-secure-app-for-an-azure-ad-app"></a>Desarrollo de una aplicación segura para una aplicación de Azure AD
## <a name="overview"></a>Información general

Este ejemplo es una instancia sencilla de Azure Active Directory con una aplicación web que contiene vínculos a recursos de seguridad para el desarrollo de aplicaciones en Azure. Esta aplicación implementa procedimientos recomendados de seguridad que pueden ayudar a mejorar seguridad de la aplicación y la organización al desarrollar aplicaciones en Azure.

Los scripts de implementación configuran la infraestructura. Después de ejecutar los scripts de implementación, deberá realizar cierta configuración manual en Azure Portal para vincular los componentes y los servicios. Este ejemplo está dirigido a desarrolladores con experiencia en Azure que trabajan en el sector minorista y quieren crear una instancia protegida de Azure Active Directory con la infraestructura segura de Azure. 


Al desarrollar e implementar esta aplicación, aprenderá a: 
- Crear una instancia de Azure Key Vault, y almacenar y recuperar sus secretos
- Implementar la aplicación web de Azure, que es un entorno dedicado aislado con acceso de firewall de front-end 
- Crear y configurar una instancia de Azure Application Gateway con un firewall que use el conjunto de 10 reglas principales de OWASP 
- Habilite el cifrado de datos en tránsito y en reposo mediante los servicios de Azure. 
- Configurar la directiva de Azure y Security Center para evaluar el cumplimiento 

Después de desarrollar e implementar esta aplicación, habrá configurado la aplicación web de ejemplo siguiente junto con las medidas de configuración y seguridad que se describen.

## <a name="architecture"></a>Architecture
La aplicación es una aplicación de n niveles típica con tres niveles. El front-end, el back-end y el nivel de base de datos con los componentes de supervisión y administración de secretos integrados se muestran aquí:

![Arquitectura de la aplicación](./media/secure-aad-app/architecture.png)

Esta solución usa los siguientes servicios de Azure. Los detalles de la arquitectura de implementación se encuentran en la sección Arquitectura de implementación. 

La arquitectura consta de los siguientes componentes:

- [Azure Application Gateway](../../application-gateway/index.yml). Proporciona la puerta de enlace y el firewall para la arquitectura de la aplicación.
- [Application Insights](../../azure-monitor/app/app-insights-overview.md). Proporciona un servicio de Application Performance Management (APM) extensible en varias plataformas.
- [Azure Key Vault](../../key-vault/index.yml) Almacena y cifra los secretos de la aplicación y administra la creación de directivas de acceso en torno a ellos.
- [Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md). Proporciona el servicio de administración de acceso e identidades basado en la nube, inicio de sesión y acceso a los recursos.
- [Azure DNS](../../dns/dns-overview.md). Proporciona el servicio para hospedar el dominio.
- [Azure Load Balancer](../../load-balancer/load-balancer-overview.md). Permite escalar las aplicaciones y crea alta disponibilidad para los servicios.
- [Aplicación web de Azure](../../app-service/overview.md).  Proporciona un servicio basado en HTTP para hospedar aplicaciones web.
- [Azure Security Center](../../security-center/index.yml). Proporciona protección contra amenazas avanzada en todas las cargas de trabajo híbridas de la nube.
- [Azure Policy](../../governance/policy/overview.md). Permite evaluar los recursos para comprobar que cumplen las directivas asignadas.

## <a name="threat-model"></a>Modelo de amenazas
El proceso de modelado de amenazas consiste en identificar posibles amenazas de seguridad para la empresa y la aplicación y, luego, asegurarse que exista un plan de mitigación adecuado.

En este ejemplo, se usó [Microsoft Threat Modeling Tool](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) para implementar el modelado de amenazas para la aplicación de ejemplo segura. Mediante la creación de diagramas de los componentes y los flujos de datos, puede identificar los problemas y las amenazas al principio del proceso de desarrollo. De este modo, se ahorra tiempo y dinero más adelante.

Este es el modelo de amenazas de la aplicación de ejemplo:

![Modelo de amenazas](./media/secure-aad-app/threat-model.png)

En la captura de pantalla siguiente se muestran algunas amenazas de ejemplo y posibles vulnerabilidades que genera Threat Modeling Tool. El modelo de amenazas ofrece información general sobre la superficie expuesta a ataques y solicita a los desarrolladores que piensen en cómo mitigar los problemas.

![Salida del modelo de amenazas](./media/secure-aad-app/threat-model-output.png)

### <a name="prerequisites"></a>Prerrequisitos
Para poner en marcha la aplicación, debe instalar estas herramientas:

- Un editor de código para modificar y ver el código de la aplicación. [Visual Studio Code](https://code.visualstudio.com/) es una opción de código abierto.
- La [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&viewFallbackFrom=azure-cli-latest,) en el equipo de desarrollo.
- [GIT](https://git-scm.com/) en el sistema. GIT se usa para clonar el código fuente localmente.
- [jq](https://stedolan.github.io/jq/), una herramienta de UNIX fácil de usar para consultar JSON.

Necesita una suscripción a Azure para implementar los recursos de la aplicación de ejemplo. Si no tiene una suscripción a Azure, puede [crear una cuenta gratuita](https://azure.microsoft.com/free/) para probar la aplicación de ejemplo.

Después de instalar estas herramientas, está listo para implementar la aplicación en Azure.

### <a name="implementation-guidance"></a>Guía de implementación
El script de implementación es un script que se puede dividir en cuatro fases. Cada fase implementa y configura un recurso de Azure que se encuentra en el [diagrama de arquitectura](#architecture).

Las cuatro fases son:

- Implementar Azure Key Vault
- Implementar Azure Web Apps
- Implementar Application Gateway con un firewall de aplicaciones web.
- Configurar una instancia de Azure AD con una aplicación implementada

Cada fase se basa en la anterior mediante la configuración de los recursos implementados previamente.

Para completar los pasos de implementación, asegúrese de que ha instalado las herramientas que se indican en [Requisitos previos](#prerequisites).

#### <a name="deploy-azure-key-vault"></a>Implementar Azure Key Vault
En esta sección, creará e implementará una instancia de Azure Key Vault que se usa para almacenar secretos y certificados.

Después de completar la implementación, tendrá una instancia de Azure Key Vault implementada en Azure.

Para implementar Azure Key Vault mediante PowerShell:
 
1. Declare las variables para Azure Key Vault.
2. Registre el proveedor de Azure Key Vault.
3. Cree el grupo de recursos para la instancia.
4. Cree la instancia de Azure Key Vault en el grupo de recursos creado en el paso 3.

#### <a name="the-below-azure-ad-user-will-have-admin-permissions-to-the-key-vault"></a>El usuario de Azure AD siguiente tendrá permisos de administrador en Key Vault:
    $keyVaultAdminUsers = @($user1,user2)

#### <a name="register-the-az-providers"></a>Registro de los proveedores de Az
    Register-AzResourceProvider -ProviderNamespace Microsoft.KeyVault

#### <a name="create-the-azure-key-vault-instance"></a>Creación de la instancia de Azure Key Vault
    New-AzKeyVault -Name $kvName 
                -ResourceGroupName $ResourceGroup 
                -Location 'East US'
                -EnabledForDiskEncryption

#### <a name="add-the-administrator-policies-to-the-key-vault"></a>Adición de las directivas de administrador a Key Vault
    foreach ($keyVaultAdminUser in $keyVaultAdminUsers) {
    $UserObjectId = (Get-AzADUser -SearchString $keyVaultAdminUser).Id
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName -ObjectId $UserObjectId 
    -PermissionsToKeys all -PermissionsToSecrets all -PermissionsToCertificates all
    }

#### <a name="to-create-an-access-policy-to-allow-a-user-to-get-and-list-cryptographic-keys-certificates-and-secrets-if-you-know-the-user-principal-name"></a>Para crear una directiva de acceso que permita a un usuario obtener y enumerar claves criptográficas, certificados y secretos si conoce el nombre principal de usuario:
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName 
                           -ResourceGroupName $resourceGroupName 
                           -UserPrincipalName 'user1@contoso.com 
                           -PermissionsToCertificates list, get 
                           -PermissionsToKeys list, get 
                           -PermissionsToSecrets list, get 

El procedimiento recomendado es usar identidades administradas para los recursos de Azure en las aplicaciones que usan Key Vault para acceder a los recursos. La seguridad aumenta cuando las claves de acceso a Key Vault no se almacenan en el código ni en la configuración.

En el contenedor se incluye un certificado raíz. Los pasos realizados para obtener el certificado son:

1. Descargar el archivo de certificado de la [entidad de certificación](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).
2. Descodificar el archivo del certificado:

   ```powershell
   openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
   ```
Este script crea una identidad asignada para la instancia de App Service que se puede usar con MSI para interactuar con Azure Key Vault sin codificar de forma rígida los secretos en el código ni en la configuración.

Vaya a la instancia de Azure Key Vault en el portal para autorizar la identidad asignada en la pestaña de la directiva de acceso. Seleccione **Agregar una nueva directiva de acceso**. En **Seleccionar entidad**, busque el nombre de la aplicación similar al nombre de la instancia de App Service creada.
Una entidad de servicio adjuntada a la aplicación debe estar visible. Selecciónela y guarde la página de directiva de acceso, tal y como se muestra en la siguiente captura de pantalla.

Dado que la aplicación solo necesita recuperar claves, seleccione el permiso **Get** en las opciones de secretos para permitir el acceso y, al mismo tiempo, reducir los privilegios concedidos.

![Directiva de acceso de Key Vault](./media/secure-aad-app/kv-access-policy.png)

*Crear una directiva de acceso a Key Vault*

Guarde la directiva de acceso y, a continuación, guarde el nuevo cambio en la pestaña **Directivas de acceso** para actualizar las directivas.

#### <a name="deploy-application-gateway-with-web-application-firewall-enabled"></a>Implementar Application Gateway con un firewall de aplicaciones web habilitado
En las aplicaciones web, no se recomienda exponer los servicios directamente al mundo exterior en Internet.
Las reglas de equilibrio de carga y firewall proporcionan más seguridad y control sobre el tráfico entrante y le ayudan a administrarlo.

Para implementar una instancia de Application Gateway:

1. Cree el grupo de recursos para hospedar Application Gateway.
2. Aprovisione una red virtual para conectarla a la puerta de enlace.
3. Cree una subred para la puerta de enlace en la red virtual.
4. Aprovisione una dirección IP pública.
5. Aprovisione Application Gateway.
6. Habilite el firewall de aplicaciones web en la puerta de enlace.

```
Connect-AzAccount
Select-AzSubscription -SubscriptionId '$SubscriptionId'
New-AzResourceGroup -Name appgw-rg -Location "East US"

#Create a virtual network and a subnet for the application gateway

#Assign an address range for the subnet to be used for the application gateway.

$gwSubnet = New-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

#Assign an address range to be used for the back-end address pool.

$nicSubnet = New-AzVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

#Create a virtual network with the subnets defined in the preceding steps.

$vnet = New-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

#Retrieve the virtual network resource and subnet resources to be used in the steps that follow.

$vnet = Get-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet


#Create a public IP address for the front-end configuration

$publicip = New-AzPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "East US" -AllocationMethod Dynamic

#Create an application gateway configuration object

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

#Create a front-end IP configuration

$fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

#Configure the back-end IP address pool with the IP addresses of the back-end web servers

$pool = New-AzApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 10.0.3.11

#Configure the front-end IP port for the public IP endpoint

$fp = New-AzApplicationGatewayFrontendPort -Name 'port01'  -Port 443

#Configure the certificate for the application gateway. This certificate is used to decrypt and reencrypt the traffic on the application gateway

$passwd = ConvertTo-SecureString  "P@ssword!1" -AsPlainText -Force 
$cert = New-AzApplicationGatewaySSLCertificate -Name cert01 -CertificateFile "C:\AAD\Securities\Certificates\sslcert.com.cer" -Password $passwd 


#Create the HTTP listener for the application gateway

$listener = New-AzApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert

#Upload the certificate to be used on the TLS/SSL-enabled back-end pool resources

#$authcert = New-AzApplicationGatewayAuthenticationCertificate -Name 'allowlistcert1' -CertificateFile C:\cert.cer

$trustedRootCert01 = New-AzApplicationGatewayTrustedRootCertificate -Name "test1" -CertificateFile "C:\AAD\Securities\Certificates\sslcert.com.cer"

#Configure the HTTP settings for the application gateway back end

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting01" -Port 443 -Protocol Https -CookieBasedAffinity Disabled -TrustedRootCertificate $trustedRootCert01 -HostName "test1"

#Create a load-balancer routing rule that configures the load balancer

$rule = New-AzApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

#Configure the instance size of the application gateway

$sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

#Configure the TLS/SSL policy to be used on the application gateway

$SSLPolicy = New-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -PolicyType Custom

$appgw = New-AzApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -TrustedRootCertificate $trustedRootCert01 -Verbose

```

#### <a name="deploy-azure-web-apps"></a>Implementación de Azure Web Apps
Azure App Service le permite compilar y hospedar aplicaciones web mediante lenguajes como Python, Ruby, C# y Java. Azure también admite contenedores personalizados, lo que permite que prácticamente todos los lenguajes de programación se ejecuten en la plataforma Azure App Service.

#### <a name="create-an-app-service-plan-in-free-tier"></a>Creación de un plan de App Service en el nivel Gratis
    New-AzAppServicePlan -Name $webappname -Location $location -ResourceGroupName $webappname -Tier Free

#### <a name="create-a-web-app"></a>Creación de una aplicación web
    New-AzWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName $webappname

    Write-Host "Configure a CNAME record that maps $fqdn to $webappname.azurewebsites.net"
    Read-Host "Press [Enter] key when ready ..."

#### <a name="before-continuing-go-to-your-azure-domain-name-system-configuration-ui-for-your-custom-domain-and-follow-the-instructions-at-httpsakamsappservicecustomdns-to-configure-a-cname-record-for-the-hostname-www-and-point-it-your-web-apps-default-domain-name"></a>Antes de continuar, vaya a la interfaz de usuario de configuración de Azure DNS correspondiente al dominio personalizado y siga las instrucciones que se indican en https://aka.ms/appservicecustomdns para configurar un registro CNAME para el nombre de host "www" y que apunte al nombre de dominio predeterminado de la aplicación web.

#### <a name="upgrade-app-service-plan-to-shared-tier-minimum-required-by-custom-domains"></a>Actualización del plan de App Service al nivel Compartido (mínimo requerido por los dominios personalizados)
    Set-AzAppServicePlan -Name $webappname -ResourceGroupName $webappname -Tier Shared

#### <a name="add-a-custom-domain-name-to-the-web-app"></a>Adición de un nombre de dominio personalizado a la aplicación web
    Set-AzWebApp -Name $webappname -ResourceGroupName $webappname `-HostNames @($fqdn,"$webappname.azurewebsites.net")

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones

### <a name="network"></a>Red
Una vez completada la implementación, tendrá una instancia de Application Gateway con el firewall de aplicaciones web habilitado.

La instancia de la puerta de enlace expone el puerto 443 para HTTPS. Esta configuración garantiza que la aplicación solo esté accesible en el puerto 443 a través de HTTPS.

El bloqueo de los puertos que no se usan y la limitación de la exposición de la superficie de ataque es una práctica de seguridad recomendada.

#### <a name="add-network-security-groups-to-the-app-service-instance"></a>Agregar grupos de seguridad de red a la instancia de App Service

Las instancias de App Service se pueden integrar con redes virtuales. Esta integración permite que se configuren con directivas de grupo de seguridad de red que administran el tráfico entrante y saliente de la aplicación.

1. Para habilitar esta característica, en la hoja de la instancia de Azure App Service, en **Configuración**, seleccione **Redes**. En el panel derecho, configure la opción **Integración de VNET**.

   ![Integración de una nueva red virtual](./media/secure-web-app/app-vnet-menu.png)

    *Integración de una nueva red virtual para App Service*
1. En la página siguiente, seleccione **Agregar VNET (versión preliminar)** .

1. En el menú siguiente, seleccione la red virtual creada en la implementación que empieza por `aad-vnet`. Puede crear un subconjunto nuevo o seleccionar uno existente.
   En este caso, cree una nueva subred. Defina el **Intervalo de direcciones** como **10.0.3.0/24** y asigne a la subred el nombre **app-subnet**.

   ![Configuración de la red virtual de App Service](./media/secure-web-app/app-vnet-config.png)

    *Configuración de la red virtual para App Service*

Ahora que ha habilitado la integración de la red virtual, puede agregar grupos de seguridad de red a nuestra aplicación.

1. En el cuadro de búsqueda, busque **grupos de seguridad de red**. Seleccione **Grupos de seguridad de red** en los resultados.

    ![Buscar grupos de seguridad de red](./media/secure-web-app/nsg-search-menu.png)

    *Buscar grupos de seguridad de red*

2. En el menú siguiente, seleccione **Agregar**. Escriba el **Nombre** del NSG y el **Grupo de recursos** en el que se debe ubicar. Este NSG se aplicará a la subred de la puerta de enlace de la aplicación.

    ![Crear un NSG](./media/secure-web-app/nsg-create-new.png)

    *Crear un NSG*

3. Una vez creado el NSG, selecciónelo. En su hoja, en **Configuración**, seleccione **Reglas de seguridad de entrada**. Configure estas opciones para permitir conexiones entrantes en la puerta de enlace de la aplicación a través del puerto 443.

   ![Configurar el NSG](./media/secure-web-app/nsg-gateway-config.png)

   *Configurar el NSG*

4. En las reglas de salida del NSG de puerta de enlace, agregue una regla que permita conexiones salientes a la instancia de App Service mediante la creación de una regla que tenga como destino la etiqueta de servicio `AppService`:

   ![Agregar reglas de salida para el NSG](./media/secure-web-app/nsg-outbound-allowappserviceout.png)

   *Agregar reglas de salida para el NSG*

    Agregue otra regla de salida para permitir que la puerta de enlace envíe reglas de salida a una red virtual.

   ![Agregar otra regla de salida](./media/secure-web-app/nsg-outbound-vnet.png)

    *Agregar otra regla de salida*

5. En la hoja de subredes del NSG, seleccione **Asociar**, seleccione la red virtual creada en la implementación y seleccione la subred de puerta de enlace denominada **gw-subnet**. El NSG se aplica a la subred.

6. Cree otro NSG como en el paso anterior; esta vez, para la instancia de App Service. Asígnele un nombre. Agregue la regla de entrada para el puerto 443 como lo hizo para el NSG de la puerta de enlace de la aplicación.

   Si tiene una instancia de App Service implementada en una instancia de App Service Environment, que no es el caso de esta aplicación, puede agregar reglas de entrada para permitir sondeos de Azure Service Health abriendo los puertos 454-455 en los grupos de seguridad de entrada de su NSG de App Service. Esta es la configuración:

   ![Agregar reglas para sondeos de Azure Service Health](./media/secure-web-app/nsg-create-healthprobes.png)

    *Agregar reglas para sondeos de Azure Service Health (solo App Service Environment)*

Para limitar la superficie expuesta a ataques, modifique la configuración de red de App Service para que solo la puerta de enlace de la aplicación tenga acceso a la aplicación.
Para aplicar esta configuración, vaya a la pestaña de red de App Service, seleccione la pestaña **Restricciones de IP** y cree una regla que permita que solo la dirección IP de Application Gateway acceda directamente al servicio. Puede recuperar la dirección IP de la puerta de enlace desde esta página de información general. En la pestaña **Dirección IP o CIDR**, escriba la dirección IP en este formato: `<GATEWAY_IP_ADDRESS>/32`.

![Permitir solo la puerta de enlace](./media/secure-web-app/app-allow-gw-only.png)

*Permitir que solo la dirección IP de la puerta de enlace tenga acceso a App Service*

### <a name="azure-domain-name-system"></a>Azure DNS 
Azure DNS es responsable de traducir (o resolver) el nombre de un sitio web o servicio en su dirección IP. Azure DNS (https://docs.microsoft.com/azure/dns/dns-overview) es un servicio de hospedaje para los dominios del Sistema de nombres de dominio que ofrece resolución de nombres mediante la infraestructura de Azure. Al hospedar dominios en Azure, los usuarios pueden administrar los registros del Sistema de nombres de dominio con las mismas credenciales, API, herramientas y facturación que los demás servicios de Azure. Azure DNS también admite dominios del Sistema de nombres de dominio privados.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption
Azure Disk Encryption aprovecha la característica BitLocker de Windows para proporcionar el cifrado de volumen de los discos de datos. La solución se integra con Azure Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

### <a name="identity-management"></a>Administración de identidades
Las siguientes tecnologías proporcionan funcionalidades para administrar el acceso a los datos del titular de tarjeta en el entorno de Azure:
- Azure Active Directory es el directorio multiinquilino basado en la nube y el servicio de administración de identidades de Microsoft. Todos los usuarios de esta solución se crean en Azure Active Directory, incluidos los usuarios que acceden a Azure WebApp.
- El control de acceso basado en rol de Azure permite a los administradores especificar permisos de acceso bien definidos para conceder solo el nivel de acceso que los usuarios necesitan para realizar su trabajo. En lugar de dar a cada usuario permisos ilimitados para los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a los datos del titular de tarjeta. El acceso a la suscripción está limitado al administrador de la suscripción.
- Azure Active Directory Privileged Identity Management permite a los clientes minimizar el número de usuarios con acceso a determinada información, como los datos del titular de tarjeta. Los administradores pueden usar Azure Active Directory Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también puede utilizarse para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- Azure Active Directory Identity Protection detecta posibles puntos vulnerables que afectan a las identidades de una organización, configura respuestas automatizadas para detectar acciones sospechosas relacionadas con esas identidades, investiga incidentes sospechosos y toma las medidas oportunas para resolverlos.
### <a name="secrets-management"></a>Administración de secretos
La solución usa Azure Key Vault para la administración de claves y secretos. Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Azure Key Vault ayudan a los clientes a proteger los datos y acceder a ellos:
   - Se configuran directivas de acceso avanzadas según las necesidades.
   - Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
   - Todas las claves y los secretos en Key Vault tienen fechas de expiración.
   - Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por un módulo de seguridad de hardware (HSM).
   - Con Key Vault, puede cifrar claves y secretos (por ejemplo, claves de autenticación, claves de cuenta de almacenamiento, claves de cifrado de datos, archivos .PFX y contraseñas) mediante claves que están protegidas por módulos de seguridad de hardware (HSM). 
   - Use el control de acceso basado en rol (RBAC) para asignar permisos a los usuarios, los grupos y las aplicaciones en un ámbito determinado.     
   - Use Key Vault para administrar los certificados TLS con renovación automática. 
   - Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
   - Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.
### <a name="azure-security-center"></a>Azure Security Center
Con Azure Security Center, los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a ellos. Además: 
   - Azure Security Center accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.
   - Azure Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Azure Security Center cuenta con un conjunto de alertas de seguridad predefinidas, que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Las reglas de alertas personalizadas de Azure Security Center permiten a los clientes definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.
   - Azure Security Center proporciona alertas de seguridad e incidentes clasificados por orden de prioridad, lo que facilita a los clientes la detección y solución de posibles problemas de seguridad. Se genera un informe de inteligencia de amenazas por cada amenaza detectada para ayudar a los equipos de respuesta a incidentes a investigar y corregir las amenazas.
### <a name="azure-application-gateway"></a>Azure Application Gateway 
   la arquitectura reduce el riesgo de aparición de puntos vulnerables en la seguridad, ya que usa Azure Application Gateway con el firewall de aplicaciones web configurado y el conjunto de reglas OWASP habilitado. Entre estas funcionalidades, cabe destacar:
   - TLS de un extremo a otro.
   - Deshabilitar TLS v1.0 y v1.1
   - Habilitar TLSv1.2
   - Firewall de aplicaciones web (modo de prevención).
   - Modo de prevención con el conjunto de reglas OWASP 3.0
   - Habilitar el registro de diagnóstico
   - Sondeos de estado personalizados
   - Azure Security Center y Azure Advisor proporcionan protección adicional y notificaciones. Asimismo, Azure Security Center también proporciona un sistema de reputación.
### <a name="logging-and-auditing"></a>Registro y auditoría
Los servicios de Azure proporcionan un registro completo de la actividad de usuario y del sistema, así como de mantenimiento del sistema:
   - Registros de actividad: los [registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
   - Registros de diagnóstico: los [registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) son todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault y registros de firewall y acceso a Application Gateway. Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. El usuario puede configurar la retención hasta 730 días para cumplir los requisitos de retención específicos de una organización.
### <a name="azure-monitor-logs"></a>Registros de Azure Monitor
   esos registros se consolidan en [registros de Azure Monitor](https://azure.microsoft.com/services/log-analytics/) para el procesamiento, el almacenamiento y la creación de informes de panel. Una vez recopilados, los datos se organizan en tablas independientes para cada tipo de datos dentro de las áreas de trabajo de Log Analytics, lo que permite que todos los datos se puedan analizar conjuntamente con independencia de su origen. Además, Azure Security Center se integra con los registros de Azure Monitor, lo que permite a los clientes usar consultas de Kusto para acceder a sus datos de eventos de seguridad y combinarlos con datos de otros servicios.

   Como parte de esta arquitectura se incluyen las siguientes [soluciones de supervisión](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) de Azure:

   - [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
   - [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health notifica el número de agentes implementados y su distribución geográfica, así como el número de agentes que no responden y el de agentes que envían datos operativos.
   - [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.
### <a name="azure-monitor"></a>Azure Monitor
   [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a realizar un seguimiento del rendimiento, mantener la seguridad e identificar tendencias y permite a las organizaciones auditar, crear alertas y archivar datos, incluido el seguimiento de las llamadas a API en sus recursos de Azure.
### <a name="application-insights"></a>Application Insights 
   [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) es un servicio de Application Performance Management extensible para desarrolladores web en varias plataformas. Application Insights detecta anomalías de rendimiento y los clientes pueden utilizarlo para supervisar la aplicación web activa. Incluye herramientas de análisis eficaces que ayudan a los clientes a diagnosticar problemas y comprender lo que hacen realmente los usuarios con la aplicación. Está diseñado para ayudar a los clientes a mejorar continuamente el rendimiento y la facilidad de uso.

### <a name="azure-key-vault"></a>Azure Key Vault
   Cree un almacén para la organización en el que almacenar las claves, y mantenga la responsabilidad de las tareas operativas como se indica a continuación:

   - Los datos almacenados en Key Vault incluyen:   
   - Clave de Application Insights
   - Clave de acceso de almacenamiento de los datos
   - Cadena de conexión
   - Nombre de la tabla de datos
   - Credenciales de usuario
   - Se configuran directivas de acceso avanzadas según las necesidades
   - Las directivas de acceso de Key Vault se definen con los permisos mínimos necesarios para las claves y los secretos.
   - Todas las claves y los secretos en Key Vault tienen fechas de expiración
   - Todas las claves de Key Vault están protegidas por el módulo de seguridad de hardware (HSM) [tipo de clave = clave RSA de 2048 bits protegida por el módulo de seguridad de hardware       
     (HSM)]
   - Se otorgan los permisos necesarios mínimos a todos los usuarios e identidades mediante el control de acceso basado en rol (RBAC)
   - Las aplicaciones no comparten un almacén de claves a menos que confíen entre sí y que necesiten tener acceso a los mismos secretos en tiempo de ejecución
   - Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
   - Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas

### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute
   Se debe configurar un túnel VPN o un circuito [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) seguros para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de aplicaciones web de PaaS. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

   Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión tiene lugar a través de Internet y permite a los clientes colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se usa el modo de túnel IPsec en esta opción como mecanismo de cifrado.

   Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. Azure ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Como las conexiones de ExpressRoute no se realizan a través de una conexión a Internet, ofrecen más confiabilidad, más velocidad, menor latencia y mayor seguridad que las conexiones a Internet normales. Además, dado que se trata de una conexión directa del proveedor de telecomunicaciones del cliente, los datos no viajan a través de Internet y, por lo tanto, no están expuestos a ella.

   Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

#### <a name="implement-azure-active-directory-oidc"></a>Implementación de OIDC de Azure Active Directory

1. Para clonar el repositorio de código fuente, use este comando de Git:

 ``` git
 git clone https://github.com/Azure-Samples/AAD-Security
   ```
## <a name="update-the-redirect-urls"></a>Actualización de las direcciones URL de redireccionamiento
1.  Vuelva a Azure Portal. En el panel de navegación izquierdo, seleccione el servicio Azure Active Directory y, después, elija Registros de aplicaciones.
2.  En la pantalla resultante, seleccione la aplicación WebApp-OpenIDConnect-DotNet-code-v2.
3.  En la pestaña Autenticación o en la sección URI de redirección, seleccione Web en el cuadro combinado y agregue los siguientes URI de redirección.
    https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net/signin-oidc O bien, en la sección Configuración avanzada, establezca la URL de cierre de sesión en https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net/signout-oidc.
4.  En la pestaña Personalización de marca, actualice la dirección URL de la página principal a la dirección del servicio de aplicación, por ejemplo https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net.
        Guarde la configuración.
5.  Si la aplicación llama a una API web, asegúrese de aplicar los cambios necesarios en el archivo appsettings.json del proyecto, de forma que llame a la dirección URL de la API publicada en lugar de a localhost.
Publicación del ejemplo
    1.  En la pestaña Información general de App Service, haga clic en el vínculo Obtener perfil de publicación para descargar el perfil de publicación y guárdelo. También se pueden usar otros mecanismos de implementación, como el control de código fuente.
    2.  Cambie a Visual Studio y vaya al proyecto WebApp-OpenIDConnect-DotNet-code-v2. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione Publicar. Haga clic en Importar perfil en la barra inferior e importe el perfil de publicación que descargó anteriormente.
    3.  Haga clic en Configurar y, en la pestaña Conexión, actualice la dirección URL de destino para que sea HTTPS en la dirección URL de la página principal, por ejemplo https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net. Haga clic en Siguiente.
    4.  En la pestaña Configuración, asegúrese de que la opción Habilitar la autenticación de organización NO está seleccionada. Haga clic en Guardar. Haga clic en Publicar en la pantalla principal.
    5.  Visual Studio publicará el proyecto y abrirá automáticamente un explorador con la dirección URL del proyecto. Si ve la página web predeterminada del proyecto, significa que la publicación se realizó correctamente.
#### <a name="implement-multi-factor-authentication-for-azure-active-directory"></a>Implementar Multi-Factor Authentication para Azure Active Directory
   Los administradores deben asegurarse de que las cuentas de suscripción del portal están protegidas. La suscripción es vulnerable a los ataques porque administra los recursos creados. Para proteger la suscripción, habilite Multi-Factor Authentication en la pestaña **Azure Active Directory** de la suscripción.

   Azure AD funciona según unas directivas que se aplican a un usuario o a grupos de usuarios que cumplen determinados criterios.
Azure crea una directiva predeterminada que especifica que los administradores necesitan la autenticación en dos fases para iniciar sesión en el portal.
Después de habilitar esta directiva, es posible que se le pida que cierre sesión y vuelva a iniciarla en Azure Portal.

Para habilitar MFA para los inicios de sesión de administrador:

   1. Vaya a la pestaña **Azure Active Directory** en Azure Portal.
   2. En la categoría de seguridad, seleccione el acceso condicional. Verá esta pantalla:

       ![Acceso condicional: directivas](./media/secure-aad-app/ad-mfa-conditional-add.png)

Si no puede crear una directiva:

   1. Vaya a la pestaña **MFA**.
   2. Seleccione el vínculo **Evaluación gratuita** de Azure AD Premium para suscribirse a la evaluación gratuita.

   ![Una evaluación gratuita de Azure AD Premium](./media/secure-aad-app/ad-trial-premium.png)

Vuelva a la pestaña de acceso condicional.

   1. Seleccione la pestaña de nueva directiva.
   2. Escriba el nombre de la directiva.
   3. Seleccione los usuarios o grupos para los que quiere habilitar MFA.
   4. En **Controles de acceso**, seleccione la pestaña **Conceder** y, a continuación, seleccione **Requerir autenticación multifactor** (y cualquier otra opción que quiera).

   ![Requerir MFA](./media/secure-aad-app/ad-mfa-conditional-add.png)

   Para habilitar la directiva, seleccione la casilla de la parte superior de la pantalla. También puede hacerlo desde la pestaña **Acceso condicional**. Si la directiva está habilitada, los usuarios necesitan MFA para iniciar sesión en el portal.

   Hay una directiva de línea de base que requiere MFA para todos los administradores de Azure. Puede habilitarla inmediatamente en el portal. Al habilitar esta directiva, puede que se invalide la sesión actual y que tenga que volver a iniciar sesión.

   Si la directiva de línea de base no está habilitada:
   1.   Seleccione **Requerir MFA para los administradores**.
   2.   Seleccione **Usar la directiva inmediatamente**.

   ![Seleccione Usar la directiva inmediatamente.](./media/secure-aad-app/ad-mfa-conditional-enable.png)

#### <a name="use-azure-sentinel-to-monitor-apps-and-resources"></a>Usar Azure Sentinel para supervisar aplicaciones y recursos

   A medida que crece una aplicación, resulta difícil agregar todas las señales y métricas de seguridad recibidas de los recursos y hacerlas útiles de forma orientada a la acción.

   Azure Sentinel está diseñado para recopilar datos, detectar los tipos de amenazas posibles y proporcionar visibilidad sobre los incidentes de seguridad.
Mientras espera la intervención manual, Azure Sentinel puede basarse en cuadernos de estrategias escritos previamente para iniciar las alertas y los procesos de administración de incidentes.

   La aplicación de ejemplo se compone de varios recursos que Azure Sentinel puede supervisar.
Para configurar Azure Sentinel, primero debe crear un área de trabajo de Log Analytics que almacene todos los datos recopilados de los distintos recursos.

Para crear este área de trabajo:

   1. En el cuadro de búsqueda de Azure Portal, busque **Log Analytics**. Seleccione **Áreas de trabajo de Log Analytics**.

   ![Buscar áreas de trabajo de Log Analytics](./media/secure-aad-app/sentinel-log-analytics.png)

   *Buscar áreas de trabajo de Log Analytics*

   2. En la página siguiente, seleccione **Agregar** y, a continuación, proporcione un nombre, un grupo de recursos y una ubicación para el área de trabajo.
   ![Creación de un área de trabajo de Log Analytics](./media/secure-aad-app/sentinel-log-analytics-create.png)

   *Creación de un área de trabajo de Log Analytics*

   3. Use el cuadro de búsqueda para buscar **Azure Sentinel**.

   ![Buscar Azure Sentinel](./media/secure-aad-app/sentinel-add.png)

   *Buscar Azure Sentinel*

   4. Seleccione **Agregar** y, a continuación, seleccione el área de trabajo de Log Analytics que creó antes.

   ![Agregar un área de trabajo de Log Analytics](./media/secure-aad-app/sentinel-workspace-add.png)

   *Agregar un área de trabajo de Log Analytics*

   5. En la página **Azure Sentinel: conectores de datos**, en **Configuración**, seleccione **Conectores de datos**. Verá una matriz de servicios de Azure que puede vincular a la instancia de almacenamiento de Log Analytics para su análisis en Azure Sentinel.

   ![Conectores de datos de Log Analytics](./media/secure-aad-app/sentinel-connectors.png)

      *Agregar un conector de datos a Azure Sentinel*

   Por ejemplo, para conectar Application Gateway, siga estos pasos:

   1. Abra la hoja de la instancia de Azure Application Gateway.
   2. En **Supervisión**, seleccione **Configuración de diagnóstico**.
   3. Seleccione **Agregar configuración de diagnóstico**.

   ![Agregar diagnósticos de Application Gateway](./media/secure-aad-app/sentinel-gateway-connector.png)
         
   *Agregar diagnósticos de Application Gateway*

   4. En la página **Configuración de diagnóstico** , seleccione el área de trabajo de Log Analytics que creó y, a continuación, seleccione todas las métricas que quiere recopilar y enviar a Azure Sentinel. Seleccione **Guardar**.

   ![Configuración del conector de Azure Sentinel](./media/secure-aad-app/sentinel-connector-settings.png)



## <a name="cost-considerations"></a>Consideraciones sobre el costo
   En caso de no tener ninguna cuenta de Azure, puede crear una gratuitamente. Vaya a la [página de la cuenta gratuita](https://azure.microsoft.com/free/) para comenzar, ver lo que puede hacer con una cuenta gratuita de Azure y averiguar qué productos son gratis durante 12 meses.

   Para implementar los recursos de la aplicación de ejemplo con las características de seguridad, debe pagar algunas características prémium. A medida que la aplicación se escala y los niveles gratis y las pruebas que ofrece Azure deben actualizarse para cumplir los requisitos de la aplicación, es posible que los costos aumenten. Puede usar la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) de Azure para calcular los costos.

## <a name="next-steps"></a>Pasos siguientes
   Los siguientes artículos pueden ayudarle a diseñar, desarrollar e implementar aplicaciones seguras.

- [Diseño](secure-design.md)
- [Desarrollo](secure-develop.md)
- [Implementación](secure-deploy.md)
