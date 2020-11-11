---
title: Uso de análisis de comportamiento de entidades para detectar amenazas avanzadas | Microsoft Docs
description: Habilitación del análisis del comportamiento de usuarios y entidades en Azure Sentinel y configuración de orígenes de datos
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2020
ms.author: yelevin
ms.openlocfilehash: b769703f7e7054571a3b65a021c0123d8ae07078
ms.sourcegitcommit: 8ad5761333b53e85c8c4dabee40eaf497430db70
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2020
ms.locfileid: "93148242"
---
# <a name="enable-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Habilitación del análisis de comportamiento de usuarios y entidades (UEBA) en Azure Sentinel 

> [!IMPORTANT]
>
> - La característica UEBA se encuentra ahora en **disponibilidad general** en las siguientes regiones y zonas geográficas de Azure Sentinel:
>    - Geografía de Estados Unidos
>    - Región de Oeste de Europa
>    - Geografía de Australia
>
> - En todas las demás regiones y zonas geográficas, esta característica permanece en **versión preliminar pública** y se proporciona sin un acuerdo de nivel de servicio. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

Para habilitar o deshabilitar esta característica (estos requisitos previos no son necesarios para usar la característica):

- El usuario debe ser miembro de la instancia de Azure Active Directory de su organización, no un usuario invitado.

- El usuario debe tener asignados los roles **Administrador global** o **Administrador de seguridad** en Azure AD.

- El usuario debe tener asignado al menos uno de los siguientes **roles de Azure** ([más información sobre RBAC de Azure](roles.md)):
    - **Colaborador de Azure Sentinel** en los niveles de área de trabajo o grupo de recursos.
    - **Colaborador de Log Analytics** en los niveles de grupo de recursos o suscripción.

- El área de trabajo no debe tener ningún bloqueo de recursos de Azure aplicado. [Más información sobre el bloqueo de recursos de Azure](../azure-resource-manager/management/lock-resources.md).

## <a name="how-to-enable-user-and-entity-behavior-analytics"></a>Habilitación del análisis de comportamiento de usuarios y entidades

1. En el menú de navegación de Azure Sentinel, seleccione **Entity behavior** (Comportamiento de entidad).

1. En el encabezado **Turn it on** (Activarlo), cambie el control de alternancia a **On** (Activar).

1. Haga clic en el botón **Select data sources** (Seleccionar orígenes de datos).

1. En el panel **selección de orígenes de datos** , active las casillas situadas junto a los orígenes de datos en los que quiere habilitar UEBA y, después, seleccione **Apply** (Aplicar).

    > [!NOTE]
    >
    > En la mitad inferior del panel de **selección de orígenes de datos** , verá una lista de orígenes de datos compatibles con UEBA que aún no ha habilitado. 
    >
    > Una vez que haya habilitado UEBA, tendrá la opción de conectar nuevos orígenes de datos para habilitarlos para UEBA directamente desde el panel del conector de datos si son compatibles con UEBA.

1. Seleccione **Go to entity search** (Ir a búsqueda de entidades). Esto le llevará al panel de búsqueda de entidades, que, desde ahora, será lo que vea cuando elija **Entity behavior** (Comportamiento de entidad) en el menú principal de Azure Sentinel.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a habilitar y configurar el análisis de comportamiento de usuarios y entidades (UEBA) en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
