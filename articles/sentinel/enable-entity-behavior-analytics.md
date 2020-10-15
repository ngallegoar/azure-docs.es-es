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
ms.date: 09/15/2020
ms.author: yelevin
ms.openlocfilehash: c55ea0e7753faa6dc21b955d63a57d96e3849f70
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993730"
---
# <a name="enable-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Habilitación del análisis de comportamiento de usuarios y entidades (UEBA) en Azure Sentinel 



## <a name="prerequisites"></a>Requisitos previos

- El usuario debe tener asignados los roles de **administrador global** o **administrador de seguridad** en Azure AD para habilitar o deshabilitar UEBA, pero no para ejecutarlo.

## <a name="how-to-enable-user-and-entity-behavior-analytics"></a>Habilitación del análisis de comportamiento de usuarios y entidades

1. En el menú de navegación de Azure Sentinel, seleccione **Entity behavior (preview)** (Comportamiento de entidad [versión preliminar]).

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
