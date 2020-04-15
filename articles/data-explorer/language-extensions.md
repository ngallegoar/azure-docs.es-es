---
title: Administración de extensiones de lenguaje en el clúster de Azure Data Explorer
description: Use la extensión de lenguaje para integrar otros idiomas en las consultas de KQL de Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: orhasban
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 88c3047afebf3d31b50145c6df47776fdc1ddcd4
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522676"
---
# <a name="manage-language-extensions-in-your-azure-data-explorer-cluster-preview"></a>Administración de extensiones de lenguaje en el clúster de Azure Data Explorer (versión preliminar)

La característica de extensiones de lenguaje permite usar complementos de extensión de lenguaje para integrar otros idiomas en las consultas KQL de Azure Explorador de datos. Cuando se ejecuta una función definida por el usuario (UDF) mediante un script pertinente, el script obtiene datos tabulares como entrada y se espera que genere una salida tabular. El entorno de ejecución del complemento se hospeda en un [espacio aislado](/azure/kusto/concepts/sandboxes), un entorno aislado y seguro, que se ejecuta en los nodos del clúster. En este artículo, administrará el complemento de extensiones de lenguaje en el clúster de Azure Data Explorer con Azure Portal.

> [!NOTE]
> Las extensiones de lenguaje de Azure Data Explorer que se admiten actualmente son Python y R.

## <a name="prerequisites"></a>Prerrequisitos

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.
* Cree [un clúster y una base de datos de Azure Data Explorer](create-cluster-database-portal.md).

## <a name="enable-language-extensions-on-your-cluster"></a>Habilitación de las extensiones de lenguaje en el clúster

> [!WARNING]
> Revise las [limitaciones](#limitations) antes de habilitar una extensión de lenguaje.

Para habilitar las extensiones de lenguaje en el clúster, haga lo siguiente:

1. En Azure Portal, vaya al clúster de Azure Data Explorer. 
1. En **Configuración**, seleccione **Configuraciones**. 
1. En el panel **Configuraciones**, seleccione **Activado** para habilitar una extensión de lenguaje.
1. Seleccione **Guardar**.
 
    ![Extensión de lenguaje activada](media/language-extensions/configurations-enable-extension.png)

> [!NOTE]
> La habilitación del proceso de extensión de lenguaje puede tardar unos minutos. Durante ese tiempo, se suspenderá el clúster.
 
## <a name="run-language-extension-integrated-queries"></a>Ejecución de consultas integradas con la extensión de lenguaje

* Aprenda a [ejecutar consultas KQL integradas con Python](/azure/kusto/query/pythonplugin).
* Aprenda a [ejecutar consultas KQL integradas con R](/azure/kusto/query/rplugin). 

## <a name="disable-language-extensions-on-your-cluster"></a>Deshabilitación de las extensiones de lenguaje en el clúster

> [!NOTE]
> La deshabilitación de las extensiones de lenguaje puede tardar unos minutos.

Para deshabilitar las extensiones de lenguaje en el clúster, haga lo siguiente:

1. En Azure Portal, vaya al clúster de Azure Data Explorer. 
1. En **Configuración**, seleccione **Configuraciones**. 
1. En el panel **Configuraciones**, seleccione **Desactivado** para deshabilitar una extensión de lenguaje.
1. Seleccione **Guardar**.

    ![Extensión de lenguaje desactivada](media/language-extensions/configurations-disable-extension.png)

## <a name="limitations"></a>Limitaciones

* La característica de extensiones de lenguaje no admite el [cifrado de disco](manage-cluster-security.md). 
* El espacio aislado en tiempo de ejecución de las extensiones de lenguaje asigna espacio en disco, aunque no se ejecute ninguna consulta en el ámbito del lenguaje pertinente.

