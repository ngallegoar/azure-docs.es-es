---
title: Implementar un socio de seguridad de confianza de Azure Firewall Manager
description: Obtenga información sobre cómo implementar una seguridad de confianza de Azure Firewall Manager con el Azure Portal.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: victorh
ms.openlocfilehash: bcea9a8674e4b1979698b7d28eb4192172b0dc11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73931318"
---
# <a name="deploy-a-trusted-security-partner-preview"></a>Implementación de un asociado de seguridad de confianza (versión preliminar)

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

*Los socios de seguridad de confianza*en Azure Firewall Manager le permiten usar sus conocidas y mejores ofertas de seguridad como servicio (SECaaaS) de terceros para proteger el acceso a Internet de los usuarios.

Para más información sobre los escenarios compatibles y las directrices de mejores prácticas, consulte [¿Qué son los asociado de seguridad de confianza (versión preliminar)?](trusted-security-partners.md).

Para esta versión preliminar los asociados de seguridad admitidos son **ZScaler** e **iboss**. Las regiones admitidas son WestCentralUS, NorthCentralUS, Westus, WestUS2 y EastUS.

## <a name="prerequisites"></a>Prerrequisitos

> [!IMPORTANT]
> La versión preliminar de Azure Firewall Manager se debe habilitar explícitamente mediante el comando `Register-AzProviderFeature` de PowerShell.

Desde el símbolo del sistema de PowerShell, ejecute los siguientes comandos:

```azure-powershell
connect-azaccount
Register-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network
```
El registro de la característica tarda en completarse un máximo de 30 minutos. Ejecute el siguiente comando para comprobar el estado del registro:

`Get-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network`

## <a name="deploy-a-third-party-security-provider-in-a-new-hub"></a>Implementación de un proveedor de seguridad de terceros en un nuevo centro de conectividad

1. Inicie sesión en Azure Portal en https://portal.azure.com.
2. En **Búsqueda**, escriba **Firewall Manager** y selecciónelo en **Servicios**.
3. Vaya a **Introducción**. Seleccione **Crear un centro virtual protegido**. 
4. Escriba la suscripción y el grupo de recursos, seleccione una región admitida y agregue la información de la virtual WAN y del centro de conectividad. 
5. **Implementar puerta de enlace VPN** está habilitado de forma predeterminada. Se necesita una VPN Gateway para implementar un socio de seguridad de confianza en el centro de conectividad. 
6. Seleccione **Siguiente: Azure Firewall**
   > [!NOTE]
   > Los socios de seguridad de confianza se conectan al centro de conectividad con túneles de VPN Gateway. Si elimina VPN Gateway, se perderán las conexiones con los socios de seguridad de confianza.
7. Si desea implementar Azure Firewall para filtrar el tráfico privado junto con el proveedor de servicios externo para filtrar el tráfico de Internet, seleccione una directiva para Azure Firewall. Vea los [escenarios compatibles](trusted-security-partners.md#key-scenarios).
8. Si solo desea implementar un proveedor de seguridad de terceros en el centro de conectividad, seleccione **Azure Firewall: Habilitada/deshabilitada** para establecerla como **deshabilitada**. 
9. Seleccione **Siguiente: Socios de seguridad de confianza**.
10. Seleccione **Socio de seguridad de confianza** para establecerlo como **habilitado**. Seleccione un socio. 
11. Seleccione **Next** (Siguiente). 
12. Revise el contenido y, luego seleccione **Crear**.

La implementación de la puerta de enlace VPN puede durar más de 30 minutos.

Para comprobar que se ha creado el centro de conectividad, vaya a Azure Firewall Manager-> Centros de conectividad protegidos. Seleccione la página del centro de conectividad->Información general para mostrar el nombre del socio y el estado como **Conexión de seguridad pendiente**.

Una vez que se crea el centro de conectividad y se configure el socio de seguridad, continúe con la conexión al proveedor de seguridad al centro de conectividad.

## <a name="deploy-a-third-party-security-provider-in-an-existing-hub"></a>Implementación de un proveedor de seguridad de terceros en un nuevo centro de conectividad

También puede seleccionar un centro de conectividad existente en una Virtual WAN y convertirlo en un *centro virtual protegido*.

1. En **Introducción**, seleccione **Convertir centros de conectividad existentes**.
2. Seleccione una suscripción y un centro de conectividad existente. Siga los pasos restantes para implementar un proveedor de terceros en un nuevo centro de conectividad.

Recuerde que se debe implementar una puerta de enlace de VPN para convertir un centro de conectividad existente en un centro protegido con proveedores externos.

## <a name="configure-third-party-security-providers-to-connect-to-a-secured-hub"></a>Configuración de proveedores de seguridad de terceros para conectarse a un centro de conectividad protegido

Para configurar túneles en VPN Gateway del centro de conectividad virtual, los proveedores de terceros necesitan derechos de acceso al centro. Para ello, asocie una entidad de servicio a la suscripción o grupo de recursos, y conceda derechos de acceso. A continuación, debe proporcionar estas credenciales a terceros mediante el portal.

1. Crear una entidad de servicio de Azure Active Directory (AD): Puede omitir la dirección URL de redireccionamiento. 

   [Cómo: Uso del portal para crear una aplicación de Azure AD y una entidad de servicio con acceso a los recursos](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)
2. Agregue los derechos de acceso y el ámbito de la entidad de servicio.
   [Cómo: Uso del portal para crear una aplicación de Azure AD y una entidad de servicio con acceso a los recursos](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)

   > [!NOTE]
   > Puede limitar el acceso solo a su grupo de recursos para un control más granular.
3. Siga las instrucciones de [ZScaler: configuración de la integración con Microsoft Azure Virtual WAN](https://help.zscaler.com/zia/configuring-microsoft-azure-virtual-wan-integration) para:

   - Inicie sesión en el portal de asociados y agregue sus credenciales para conceder al asociado de confianza acceso a su centro protegido.
   - Sincronice los centros virtuales en el portal de asociados y configure el túnel de acceso al concentrador virtual. Puede hacerlo una vez que se hayan validado las credenciales de autenticación de Azure AD.
   
4. Puede ver el estado de creación del túnel en el portal de la Azure Virtual WAN en Azure. Una vez que los túneles se muestren **conectados** tanto en Azure como en el portal asociado, continúe con los siguientes pasos para configurar las rutas en la selección de las sucursales y VNets que deberán enviar el tráfico de Internet al asociado.

## <a name="configure-route-settings"></a>Configurar los parámetros de la ruta

1. Examine Azure Firewall Manager -> Centros de conectividad seguros. 
2. Seleccione un centro de conectividad. El estado del centro de conectividad debe mostrar ahora **Provisto**en lugar de **Conexión de seguridad pendiente**.

   Asegúrese de que el proveedor de terceros pueda conectarse al centro de conectividad. El estado de los túneles de la puerta de enlace VPN deben ser **Conectados**. Este estado refleja mejor el estado de la conexión entre el centro de conectividad y el socio de terceros, en comparación con el estado anterior.
3. Seleccione el centro de conectividad, y vaya a **Configuración de ruta**.

   Cuando se implementa un proveedor externo en el centro de conectividad, este se convierte en un *centro virtual protegido*. Esto garantiza que el proveedor de terceros anuncia una ruta 0.0.0.0.0/0 (predeterminada) hacia el centro de conectividad. Sin embargo, las conexiones de VNet y los sitios conectados al centro de conectividad no reciben esta ruta a menos que elija qué conexiones deben recibir esta ruta predeterminada.
4. En **Tráfico de Internet**, seleccione **VNet-a-Internet** o **Rama-a-Internet** o ambos para configurar las rutas de envío a través de terceros.

   Esto solo indica qué tipo de tráfico debe ser enrutado al centro de conectividad, pero sin afectar las rutas de VNets o sucursales. Estas rutas no se propagan a todos los VNets/ramas conectados al centro de conectividad de forma predeterminada.
5. Debe seleccionar **conexiones seguras** y seleccionar las conexiones en las que se deben establecer estas rutas. Esto indica qué VNets/sucursales pueden empezar a enviar tráfico de Internet al proveedor de terceros.
6. Desde **Configuración de ruta**, seleccione **Conexiones seguras** en Tráfico de Internet y, luego seleccione la red VNet o las sucursales (*sitios* en la Virtual WAN) que desea proteger. Seleccione **Tráfico seguro de Internet**.
   ![Tráfico seguro de Internet](media/deploy-trusted-security-partner/secure-internet-traffic.png)
7. Regrese a la página del centro de conectividad. El estado de **Socio de seguridad de confianza** del centro de conectividad ahora debe estar **Seguro**.

## <a name="branch-or-vnet-internet-traffic-via-third-party-service"></a>Tráfico de Internet de sucursales o VNet a través de un servicio de terceros

A continuación, puede comprobar si las máquinas virtuales VNet o la sucursal pueden tener acceso a Internet y validar que el tráfico fluye hacia el servicio de terceros.

Una vez finalizados los pasos de configuración de la ruta, las máquinas virtuales VNet y las sucursales reciben una ruta de servicio de 0/0 a terceros. No puede RDP o SSH en estas máquinas virtuales. Para iniciar sesión, puede implementar el servicio [Azure Bastion](../bastion/bastion-overview.md) en una red VNet emparejada.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Protección de una red en la nube con la versión preliminar de Azure Firewall Manager en Azure Portal](secure-cloud-network.md)




