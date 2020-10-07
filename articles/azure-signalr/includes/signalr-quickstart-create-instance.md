---
title: archivo de inclusión
description: archivo de inclusión
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: f738daab7ddcf0403f546e7c9ffeaeccb66bc6b7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91355657"
---
## <a name="create-an-azure-signalr-service-instance"></a>Creación de una instancia del servicio Azure SignalR

La aplicación se conectará a una instancia del servicio SignalR en Azure.

1. Seleccione el botón Nuevo de la esquina superior izquierda en Azure Portal. En la pantalla Nuevo, escriba *SignalR Service* (Servicio SignalR) en el cuadro de búsqueda y presione Entrar.

    ![Captura de pantalla que muestra la búsqueda de SignalR Service en Azure Portal.](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. Seleccione **SignalR Service** (Servicio SignalR) en los resultados de la búsqueda y, a continuación, seleccione **Crear**.

1. Escriba la siguiente configuración.

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nombre del recurso** | Nombre único globalmente | Nombre que identifica la nueva instancia del servicio SignalR. Los caracteres válidos son `a-z`, `0-9` y `-`.  | 
    | **Suscripción** | Su suscripción | Suscripción en que se creará esta nueva instancia del servicio SignalR. | 
    | **[Grupo de recursos](../../azure-resource-manager/management/overview.md)** |  myResourceGroup | Nombre del nuevo grupo de recursos en el que se va a crear la instancia del servicio SignalR. | 
    | **Ubicación** | Oeste de EE. UU. | Seleccione una [región](https://azure.microsoft.com/regions/) cerca de usted. |
    | **Plan de tarifa** | Gratuito | Pruebe el servicio Azure SignalR de forma gratuita. |
    | **Recuento de unidades** |  No aplicable | El recuento de unidades especifica cuántas conexiones puede aceptar la instancia del servicio SignalR. Solo es configurable en el nivel Estándar. |
    | **Modo de servicio** |  Sin servidor | Para su uso con Azure Functions o la API REST. |

    ![Captura de pantalla que muestra la pestaña Aspectos básicos de SignaIR con valores.](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. Seleccione **Crear** para empezar a implementar la instancia del servicio SignalR.

1. Una vez implementada la instancia, ábrala en el portal y busque su página de configuración. Cambie la configuración del modo de servicio *Sin servidor* solo si usa Azure SignalR Service a través del enlace de Azure Functions o la API de REST. Déjelo en *Clásico* o *Predeterminado* en caso contrario.