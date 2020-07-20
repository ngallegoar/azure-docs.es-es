---
title: 'Registro de Azure Peering Service: Azure Portal'
description: Aprenderá a registrar una conexión de Azure Peering Service mediante Azure Portal.
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/18/2020
ms.author: derekol
ms.openlocfilehash: 6646016a4529f46431ecb622c7546140a93a5d88
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871485"
---
# <a name="register-peering-service-by-using-the-azure-portal"></a>Registro de Peering Service mediante Azure Portal

Azure Peering Service es un servicio de red que mejora la conectividad de los clientes a los servicios en la nube de Microsoft, como Office 365, Dynamics 365, servicios de software como servicio (SaaS), Azure o cualquier otro servicio de Microsoft accesible a través de la red pública de Internet.

En este artículo, aprenderá a registrar una conexión de Peering Service mediante Azure Portal.

Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ahora.

> 

## <a name="prerequisites"></a>Requisitos previos

Debe tener lo siguiente:

### <a name="azure-account"></a>Cuenta de Azure

Debe tener una cuenta de Microsoft Azure válida y activa. Esta cuenta es necesaria para configurar la conexión de Peering Service. Peering Service es un recurso dentro de las suscripciones de Azure. 

### <a name="connectivity-provider"></a>Proveedor de conectividad

Puede trabajar con un proveedor de servicios Internet o con un asociado de intercambio de Internet para obtener Peering Service con el fin de conectar la red a la red de Microsoft.

Asegúrese de que los [proveedores de conectividad](location-partners.md) se asocien con Microsoft.



## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

En un explorador, vaya a Azure Portal e inicie sesión con su cuenta de Azure.

## <a name="register-a-peering-service-connection"></a>Registro de una conexión de Peering Service

1. Para registrar una conexión de Peering Service, seleccione **Crear un recurso** > **Peering Service**.

    ![Registro de Peering Service](./media/peering-service-portal/peering-servicecreate.png)
1. Escriba los detalles siguientes en la pestaña **Basics** (Aspectos básicos) de la página **Create a peering service connection** (Crear una conexión de Peering Service).

 
1. Seleccione la suscripción y el grupo de recursos asociado a la suscripción.

   ![Pestaña Aspectos básicos del registro de Peering Service](./media/peering-service-portal/peering-servicebasics.png)

1. Escriba un **Nombre** con el que registrar la instancia de Peering Service.
 
1. A continuación, seleccione el botón **Next:Configuration** (Siguiente:configuración) situado en la parte inferior de la página. Aparece la página **Configuration** (Configuración).

## <a name="configure-the-peering-service-connection"></a>Configuración de la conexión de Peering Service

1. En la página **Configuration** (Configuración), seleccione la ubicación en la que se debe habilitar Peering Service; para ello, seleccione la misma en la lista desplegable **Peering Service location** (Ubicación de Peering Service).

1. Seleccione el proveedor de servicios del que se debe obtener Peering Service; para ello, seleccione un nombre de proveedor en la lista desplegable **Peering Service provider** (Proveedor de Peering Service).
 
1. Seleccione **Create new prefix** (Crear nuevo prefijo) en la parte inferior de la sección **Prefixes** (Prefijos), aparecerán cuadros de texto. A continuación, escriba el nombre del recurso del prefijo y los prefijos asociados al proveedor de servicios.

1. Seleccione **Prefix Key** (Clave de prefijo) y agregue la clave de prefijo que le ha proporcionado su proveedor (ISP o IXP). Esta clave permite a Microsoft validar el prefijo y el proveedor que ha asignado el prefijo IP.
   > ![Pestaña Configuración del registro de Peering Service](./media/peering-service-portal/peering-serviceconfiguration.png)

1. Seleccione el botón **Review + create** (Revisar y crear) en la parte inferior izquierda de la página. Aparece la página **Review + create** (Revisar y crear) y Azure valida la configuración.
    

1. Cuando reciba el mensaje **Validation passed** (Validación superada), seleccione **Create** (Crear).

   > ![Pestaña Configuración del registro de Peering Service](./media/peering-service-portal/peering-service-prefix.png)


1. Después de registrar una conexión de Peering Service, se realiza una validación adicional de los prefijos incluidos. Puede revisar el estado de la validación en la sección **Prefixes** (Prefijos) del nombre del recurso. Si se produce un error en la validación, se muestra uno de los siguientes mensajes de error:

   - Prefijo de Peering Service no válido, el prefijo debe tener un formato válido. Solo se admiten prefijos IPv4.
   - No se recibió el prefijo del proveedor de Peering Service.
   - El anuncio de prefijo no tiene una comunidad de BGP válida. Póngase en contacto con el proveedor de Peering Service.
   - No se encontró la ruta de copia de seguridad. Póngase en contacto con el proveedor de Peering Service.
   - Prefijo recibido con una ruta de acceso de AS más larga. Póngase en contacto con el proveedor de Peering Service.
   - Prefijo recibido con AS privado en la ruta de acceso. Póngase en contacto con el proveedor de Peering Service.

### <a name="add-or-remove-a-prefix"></a>Adición o eliminación de un prefijo

Seleccione **Add prefixes** (Agregar prefijos) en la página **Prefixes** (Prefijos) para agregar prefijos.

Seleccione los puntos suspensivos (...) junto al prefijo de la lista y seleccione la opción **Delete** (Eliminar).

### <a name="delete-a-peering-service-connection"></a>Eliminación de una conexión de Peering Service

En la página **Todos los recursos**, seleccione la casilla de Peering Service y seleccione la opción **Eliminar** en la parte superior de la página.

> [!NOTE]
> No se puede modificar un prefijo existente.
>

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la conexión de Peering Service, consulte [Conexión de Peering Service](connection.md).
- Para más información sobre la telemetría de la conexión de Peering Service, consulte [Telemetría de la conexión de Peering Service](connection-telemetry.md).
- Para medir la telemetría, consulte [Medición de la telemetría de la conexión](measure-connection-telemetry.md).
- Para registrar la conexión mediante Azure PowerShell, consulte [Registro de una conexión de Peering Service: Azure PowerShell](powershell.md).
- Para registrar la conexión mediante la CLI de Azure, consulte [Registro de una conexión de Peering Service: CLI de Azure](cli.md).
