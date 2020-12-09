---
title: Creación de un área de trabajo con la protección contra la filtración de datos habilitada
description: En este artículo se explica cómo crear un área de trabajo con la protección contra la filtración de datos en Azure Synapse Analytics.
author: NanditaV
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 12/01/2020
ms.author: NanditaV
ms.reviewer: jrasnick
ms.openlocfilehash: 8f11f104e8d3d0a4f3a004ebe9f149cb4132d910
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96501332"
---
# <a name="create-a-workspace-with-data-exfiltration-protection-enabled"></a>Creación de un área de trabajo con la protección contra la filtración de datos habilitada
En este artículo se describe cómo crear un área de trabajo con la protección contra la filtración de datos habilitada y cómo administrar los inquilinos de Azure AD aprobados para esta área de trabajo.

>[!Note]
>Una vez creada el área de trabajo, no se puede cambiar su configuración de la red virtual administrada y de la protección contra la filtración de datos.

## <a name="prerequisites"></a>Requisitos previos
- Permisos para crear un recurso de área de trabajo en Azure.
- Permisos del área de trabajo de Synapse para crear puntos de conexión privados administrados.
- Suscripciones registradas para el proveedor de recursos de red. [Más información.](../../azure-resource-manager/management/resource-providers-and-types.md)

Siga los pasos que se describen en [Inicio rápido: Creación de un área de trabajo de Synapse](../quickstart-create-workspace.md) para empezar a crear el área de trabajo. Antes de crear el área de trabajo, use la siguiente información para agregar protección contra la filtración de datos al área de trabajo.

## <a name="add-data-exfiltration-protection-when-creating-your-workspace"></a>Adición de protección contra la filtración de datos al crear el área de trabajo
1. En la pestaña Redes, seleccione la casilla "Habilitar red virtual administrada".
1. Seleccione "Sí" en la opción "Allow outbound data traffic only to approved targets" (Permitir tráfico de datos de salida únicamente a destinos aprobados).
1. Elija los inquilinos de Azure AD aprobados para esta área de trabajo.
1. Revise la configuración y cree el área de trabajo.
:::image type="content" source="./media/how-to-create-a-workspace-with-data-exfiltration-protection/workspace-creation-data-exfiltration-protection.png" alt-text="Creación de un área de trabajo con la protección contra la filtración de datos":::

## <a name="manage-approved-azure-active-directory-tenants-for-the-workspace"></a>Administración de inquilinos de Azure Active Directory aprobados para el área de trabajo
1. En Azure Portal del área de trabajo, vaya a "Inquilinos de Azure AD aprobados". La lista de inquilinos de Azure AD aprobados para el área de trabajo se mostrará aquí. El inquilino del área de trabajo se incluye de forma predeterminada y no aparece en la lista.
1. Use "+Agregar" para incluir nuevos inquilinos en la lista aprobada.
1. Para quitar un inquilino de Azure AD de la lista aprobada, seleccione el inquilino y, después, seleccione "Eliminar" y "Guardar".
:::image type="content" source="./media/how-to-create-a-workspace-with-data-exfiltration-protection/workspace-manage-aad-tenant-list.png" alt-text="Creación de un área de trabajo con la protección contra la filtración de datos":::


## <a name="connecting-to-azure-resources-in-approved-azure-ad-tenants"></a>Conexión a recursos de Azure en inquilinos de Azure AD aprobados

Puede crear puntos de conexión privados administrados para conectarse a recursos de Azure que residan en inquilinos de Azure AD, que están aprobados para un área de trabajo. Siga los pasos indicados en la guía de [creación de puntos de conexión privados administrados](./how-to-create-managed-private-endpoints.md).

>[!IMPORTANT]
>Los recursos de inquilinos que no sean el inquilino del área de trabajo no deben tener implantadas reglas de firewall que impidan que los grupos de SQL se conecten a ellos. Los recursos de la red virtual administrada del área de trabajo, como los clústeres de Spark, pueden conectarse a través de vínculos privados administrados a recursos protegidos mediante firewall.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la [protección contra filtración de datos en las áreas de trabajo de Synapse](./workspace-data-exfiltration-protection.md)

Más información sobre [Red virtual del área de trabajo administrada](./synapse-workspace-managed-vnet.md)

Más información sobre los [puntos de conexión privados administrados](./synapse-workspace-managed-private-endpoints.md)

[Creación de puntos de conexión privados administrados a los orígenes de datos](./how-to-create-managed-private-endpoints.md)
