---
title: Descripción de la recopilación proactiva de registros en un dispositivo de Azure Stack Edge Pro
description: Se describe cómo se realiza la recopilación proactiva de registros en un dispositivo de Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 11/03/2020
ms.author: alkohli
ms.openlocfilehash: f79de47ec0ffad11f650054b581dbbaae030edbf
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465532"
---
# <a name="proactive-log-collection-on-your-azure-stack-edge-device"></a>Recopilación proactiva de registros en un dispositivo de Azure Stack Edge

Puede habilitar la recopilación proactiva de registros en el dispositivo de Azure Stack Edge en función de los indicadores de estado del sistema para ayudar a solucionar de forma eficaz cualquier problema del dispositivo. En este artículo se describe qué es la recopilación proactiva de registros, cómo habilitarla y cómo se administran los datos cuando se habilita.
   
La información de este artículo se aplica a los dispositivos de Azure Stack Edge Pro con GPU, Azure Stack Edge Pro R y Azure Stack Edge Mini R.

## <a name="about-proactive-log-collection"></a>Acerca de la recopilación proactiva de registros

Los equipos de ingeniería y asistencia al cliente de Microsoft usan los registros del sistema del dispositivo de Azure Stack Edge para identificar y corregir de forma eficaz las incidencias que pueden aparecer durante la operación. La recopilación proactiva de registros es un método que alerta a Microsoft de que una incidencia o un evento (consulte la sección de indicadores de la recopilación proactiva de registros para los eventos de los que se realiza un seguimiento) se ha detectado en el dispositivo de Azure Stack Edge del cliente. Los registros de asistencia correspondientes a la incidencia se cargan automáticamente en una cuenta de Azure Storage administrada y controlada por Microsoft. Soporte técnico de Microsoft y los ingenieros de Microsoft revisan estos registros de asistencia para determinar el mejor curso de acción para resolver la incidencia con el cliente.    

> [!NOTE]
> Estos registros solo se usan con fines de depuración y proporcionan asistencia a los clientes en caso de que surjan incidencias. 


## <a name="enabling-proactive-log-collection"></a>Habilitación de la recopilación proactiva de registros

Puede habilitar la recopilación proactiva de registros al intentar activar el dispositivo a través de la interfaz de usuario local. 

1. En la interfaz de usuario web local del dispositivo, vaya a la página **Introducción**.
2. En el icono **Activación**, seleccione **Activar**. 

    ![Página "Detalles de la nube" de la interfaz de usuario web local 1](./media/azure-stack-edge-pro-r-deploy-activate/activate-1.png)
    
3. En el panel de **activación**:
    1. Escriba la **clave de activación** que obtuvo en [Obtener la clave de activación para Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-prep.md#get-the-activation-key).

    1. Puede habilitar la recopilación proactiva de registros para que Microsoft recopile los registros según el estado de mantenimiento del dispositivo. Los registros recopilados de este modo se cargan en una cuenta de Azure Storage.
    
    1. Seleccione **Aplicar**.

    ![Página "Detalles de la nube" de la interfaz de usuario web local 2](./media/azure-stack-edge-pro-r-deploy-activate/activate-2.png)



## <a name="proactive-log-collection-indicators"></a>Indicadores de la recopilación proactiva de registros

Una vez habilitada la recopilación proactiva de registros, los registros se cargan automáticamente cuando se detecta uno de los siguientes eventos en el dispositivo:  


|Alerta/Error/Condición  |Descripción  |
|---------|---------|
|AcsUnhealthyCondition     |Los servicios compatibles con Azure son incorrectos.         |
|IOTEdgeAgentNotRunningCondition      |El Agente de IoT Edge no está en ejecución.         |
|UpdateInstallFailedEvent | No se pudo instalar la actualización.        |

 
Microsoft seguirá agregando nuevos eventos a la lista anterior. No se necesita ningún consentimiento adicional para los nuevos eventos. Consulte esta página para obtener información sobre los indicadores de la recopilación proactiva de registros más actualizados.    
 

## <a name="other-log-collection-methods"></a>Otros métodos de recopilación de registros

Además de la recopilación proactiva de registros, que recopila registros específicos correspondientes a una incidencia específica detectada, hay otras recopilaciones de registros que pueden proporcionar información más profunda del estado y el comportamiento del sistema. Normalmente, estas otras recopilaciones de registros se pueden ejecutar durante una solicitud de soporte técnico o desencadenadas por Microsoft en función de los datos de telemetría que proporciona el dispositivo.  

## <a name="handling-data"></a>Control de datos

Si un cliente habilita la recopilación proactiva de registros, acepta la recopilación de registros de Microsoft del dispositivo de Azure Stack Edge, tal como se describe aquí. El cliente también acepta y da su consentimiento a la carga y retención de dichos registros en una cuenta de Azure Storage administrada y controlada por Microsoft.

Microsoft solo usa los datos para solucionar problemas de estado del sistema e incidencias. Los datos no se usan para marketing, publicidad ni ningún otro propósito comercial sin el consentimiento del cliente. 

Los datos que Microsoft recopila se controlan según nuestros procedimientos de privacidad estándar. Si un cliente decide revocar su consentimiento para la recopilación proactiva de registros, los datos recopilados con consentimiento antes de la revocación no se verán afectados.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de la [Recopilación de un paquete de soporte técnico](azure-stack-edge-gpu-troubleshoot.md#collect-support-package).