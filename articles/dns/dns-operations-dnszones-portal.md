---
title: Administración de zonas DNS en DNS de Azure - Azure Portal | Microsoft Docs
description: Puede administrar zonas DNS con Azure Portal. Este artículo describe cómo actualizar, eliminar y crear zonas DNS en Azure DNS
services: dns
documentationcenter: na
author: rohinkoul
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: rohink
ms.openlocfilehash: 002f210048c18c6dd99dfb5981bacce8666ee563
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76936783"
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Administración de zonas DNS en Azure Portal

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [CLI de Azure clásica](dns-operations-dnszones-cli-nodejs.md)
> * [CLI de Azure](dns-operations-dnszones-cli.md)

En este artículo se muestra cómo administrar sus zonas DNS mediante Azure Portal. También se pueden administrar las zonas DNS mediante la [CLI de Azure](dns-operations-dnszones-cli.md) multiplataforma o Azure [PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>Creación de una zona DNS

1. Inicio de sesión en Azure Portal
2. En el menú de la izquierda, vaya a **Crear un recurso > Redes >Zona DNS** para abrir la hoja **Crear zona DNS**.

    ![Zona DNS](./media/dns-operations-dnszones-portal/openzone650.png)

4. En la hoja **Crear zona DNS**, escriba los valores siguientes y haga clic en **Crear**:


   | **Configuración** | **Valor** | **Detalles** |
   |---|---|---|
   |**Nombre**|contoso.com|El nombre de la zona DNS|
   |**Suscripción**|[Su suscripción]|Seleccione la suscripción en la que se creará la zona DNS.|
   |**Grupos de recursos**|**Crear nuevo:** contosoDNSRG|Cree un grupo de recursos. El nombre del grupo de recursos debe ser único dentro de la suscripción seleccionada. Para más información sobre los grupos de recursos, lea el artículo [Información general de Azure Resource Manager](../azure-resource-manager/management/overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Ubicación**|Oeste de EE. UU.||

> [!NOTE]
> El grupo de recursos se refiere a la ubicación del grupo de recursos y no tiene efecto alguno sobre la zona DNS. La ubicación de la zona DNS siempre es "global" y no se muestra.

## <a name="list-dns-zones"></a>Enumeración de zonas DNS

En Azure Portal, vaya a **Más servicios** > **Redes** > **Zonas DNS**. Cada zona DNS tiene su propio recurso e información como el número de conjuntos de registros y los servidores de nombres se pueden ver visibles desde esta vista. La columna **SERVIDORES DE NOMBRES** no está en la vista predeterminada. Para agregarla, haga clic en **Columnas**, seleccione **Servidores de nombres** y haga clic en **Listo**.

![enumeración de zonas DNS](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Eliminar una zona DNS

Navegue a una zona DNS en el portal. En la hoja **Zona DNS**, haga clic en **Eliminar zona**. Luego se le pide que confirme que desea eliminar la zona DNS. Al eliminar una zona DNS, también se eliminan todos los registros contenidos en la zona.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a trabajar con la zona y registros DNS en [Introducción a DNS de Azure con Azure Portal](dns-getstarted-portal.md).