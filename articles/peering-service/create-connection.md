---
title: 'Azure Peering Service: Crear '
description: En este tutorial aprenderá a registrar Azure Peering Service y un prefijo.
services: peering-service
author: derekolo
ms.service: peering-service
ms.topic: tutorial
ms.date: 05/2/2020
ms.author: derekol
Customer intent: With Azure Peering service enhancing the customer connectivity to Microsoft cloud services .
ms.openlocfilehash: f8f0ee71a8dd00498d868bb6dc11b3fa083df766
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96021102"
---
# <a name="tutorial-create-a-peering-service-connection"></a>Tutorial: Creación de una conexión de Peering Service

En este tutorial se muestra cómo crear un recurso de Peering Service y cómo configurar una conexión de Peering Service. 

1. Para registrar una conexión de Peering Service, seleccione **Crear un recurso** > **Peering Service**.

 
    ![Registro de Peering Service](./media/peering-service-portal/peering-servicecreate.png)

2. Escriba los detalles siguientes en la pestaña **Basics** (Aspectos básicos) de la página **Create a peering service connection** (Crear una conexión de Peering Service).
 
3. Seleccione la suscripción y el grupo de recursos asociado a la suscripción.

    ![Pestaña Aspectos básicos del registro de Peering Service](./media/peering-service-portal/peering-servicebasics.png)

4. Escriba un **Nombre** con el que registrar la instancia de Peering Service.

5. A continuación, seleccione el botón **Next:Configuration** (Siguiente:configuración) situado en la parte inferior de la página. Aparece la página **Configuration** (Configuración).
## <a name="configure-the-peering-service-connection"></a>Configuración de la conexión de Peering Service

1. En la página **Configuration** (Configuración), seleccione la ubicación en la que se debe habilitar Peering Service; para ello, seleccione la misma en la lista desplegable **Peering Service location** (Ubicación de Peering Service).

1. Seleccione el proveedor de servicios del que se debe obtener Peering Service; para ello, seleccione un nombre de proveedor en la lista desplegable **Peering Service provider** (Proveedor de Peering Service).
 
1. Seleccione **Create new prefix** (Crear nuevo prefijo) en la parte inferior de la sección **Prefixes** (Prefijos), aparecerán cuadros de texto. A continuación, escriba el nombre del recurso del prefijo y los prefijos asociados al proveedor de servicios.

1. Seleccione **Prefix Key** (Clave de prefijo) y agregue la clave de prefijo que le ha proporcionado su proveedor (ISP o IXP). Esta clave permite a Microsoft validar el prefijo y el proveedor que ha asignado el prefijo IP.

    ![Captura de pantalla que muestra la pestaña Configuration (Configuración) de la página Create a peering service connection (Crear una conexión de Peering Service) en la que puede escribir la clave de prefijo.](./media/peering-service-portal/peering-serviceconfiguration.png)

1. Seleccione el botón **Review + create** (Revisar y crear) en la parte inferior izquierda de la página. Aparece la página **Review + create** (Revisar y crear) y Azure valida la configuración.

 1. Cuando reciba el mensaje **Validation passed** (Validación superada), seleccione **Create** (Crear).

> ![Captura de pantalla que muestra la pestaña Review + create (Revisar y crear) de la página Create a peering service connection (Crear una conexión de Peering Service).](./media/peering-service-portal/peering-service-prefix.png)

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
## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la conexión de Peering Service, consulte [Conexión de Peering Service](connection.md).
- Para más información sobre la telemetría de la conexión de Peering Service, consulte [Telemetría de la conexión de Peering Service](connection-telemetry.md).
- Para medir la telemetría, consulte [Medición de la telemetría de la conexión](measure-connection-telemetry.md).
- Para registrar la conexión mediante Azure PowerShell, consulte [Registro de una conexión de Peering Service: Azure PowerShell](powershell.md).
- Para registrar la conexión mediante la CLI de Azure, consulte [Registro de una conexión de Peering Service: CLI de Azure](cli.md).