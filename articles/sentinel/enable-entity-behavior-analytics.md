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
ms.openlocfilehash: 140228a65be166bc172e81267c4449b49621e02c
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94425786"
---
# <a name="enable-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Habilitación del análisis de comportamiento de usuarios y entidades (UEBA) en Azure Sentinel 

> [!IMPORTANT]
>
> - Las características de UEBA y de las páginas de entidad ahora están en **disponibilidad general** en las siguientes zonas geográficas y regiones de Azure Sentinel:
>    - Geografía de Estados Unidos
>    - Región de Oeste de Europa
>    - Geografía de Australia
>
> - En todas las demás regiones y regiones geográficas, estas características permanecen en **VERSIÓN PRELIMINAR**. Consulte [Términos de uso complementarios para las Versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para conocer los términos legales adicionales que se aplican a las características de Azure que se encuentran en la versión beta, en versión preliminar o que todavía no se han publicado para que estén disponibles con carácter general.

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

1. En el panel **selección de orígenes de datos**, active las casillas situadas junto a los orígenes de datos en los que quiere habilitar UEBA y, después, seleccione **Apply** (Aplicar).

    > [!NOTE]
    >
    > En la mitad inferior del panel de **selección de orígenes de datos**, verá una lista de orígenes de datos compatibles con UEBA que aún no ha habilitado. 
    >
    > Una vez que haya habilitado UEBA, tendrá la opción de conectar nuevos orígenes de datos para habilitarlos para UEBA directamente desde el panel del conector de datos si son compatibles con UEBA.

1. Seleccione **Go to entity search** (Ir a búsqueda de entidades). Esto le llevará al panel de búsqueda de entidades, que, desde ahora, será lo que vea cuando elija **Entity behavior** (Comportamiento de entidad) en el menú principal de Azure Sentinel.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a habilitar y configurar el análisis de comportamiento de usuarios y entidades (UEBA) en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
