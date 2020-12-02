---
title: Solución de problemas de seguridad y control de acceso
description: Aprenda a solucionar problemas de seguridad y control de acceso en Azure Data Factory.
services: data-factory
author: lrtoyou1223
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/19/2020
ms.author: lle
ms.reviewer: craigg
ms.openlocfilehash: 21a1523016502bd7b0a8682461f1fc16acda2ebb
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "96008739"
---
# <a name="troubleshoot-azure-data-factory-security-and-access-control-issues"></a>Solución de problemas de seguridad y control de acceso de Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se exploran los métodos comunes de solución de problemas de seguridad y control de acceso en Azure Data Factory.

## <a name="common-errors-and-messages"></a>Errores habituales y mensajes

### <a name="invalid-or-empty-authentication-key-issue-after-disabling-public-network-access"></a>Problema de clave de autenticación vacía o no válida después de deshabilitar el acceso a la red pública

#### <a name="symptoms"></a>Síntomas

El entorno de ejecución de integración autohospedado produce el error "La clave de autenticación no es válida o está vacía" después de deshabilitar el acceso a la red pública en Data Factory.

#### <a name="cause"></a>Causa

Lo más probable es que el problema se deba a una incidencia en la resolución de DNS, ya que la deshabilitación de la conectividad pública y el establecimiento de un punto de conexión privado no ayudan a volver a conectarse.

#### <a name="resolution"></a>Resolución

1. Hizo PsPing al FQDN de ADF y encontró que el búfer se dirigía a un punto de conexión público de ADF, incluso después de deshabilitarlo.

1. Cambie el archivo de host en la máquina virtual para asignar la dirección IP privada al FQDN y vuelva a ejecutar PsPing. El búfer podrá dirigirse entonces a la dirección IP privada correcta de ADF.

1. Vuelva a registrar el entorno de ejecución de integración autohospedado y verá que está en funcionamiento.
 

### <a name="unable-to-register-ir-authentication-key-on-self-hosted-vms-due-to-private-link"></a>No se puede registrar la clave de autenticación del entorno de ejecución de integración en máquinas virtuales autohospedadas debido a un vínculo privado

#### <a name="symptoms"></a>Síntomas

No se puede registrar la clave de autenticación del entorno de ejecución de integración en una máquina virtual autohospedada debido a un vínculo privado habilitado.

La información de error se muestra de la manera siguiente:

`
Failed to get service token from ADF service with key *************** and time cost is: 0.1250079 seconds, the error code is: InvalidGatewayKey, activityId is: XXXXXXX and detailed error message is Client IP address is not valid private ip Cause Data factory couldn’t access the public network thereby not able to reach out to the cloud to make the successful connection.
`

#### <a name="cause"></a>Causa

El problema puede estar en la máquina virtual en la que intenta instalar el SHIR. El acceso a la red pública debe estar habilitado para conectarse a la nube.

#### <a name="resolution"></a>Resolución

 **Solución 1:** Para resolver el problema, puede seguir los pasos siguientes:

1. Vaya al siguiente vínculo: 
    
    https://docs.microsoft.com/rest/api/datafactory/Factories/Update

1. Haga clic en la opción **Probar** y rellene todos los detalles necesarios. Pegue también a continuación la propiedad en el campo **Cuerpo**:

    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ``` 

1. Haga clic en **Ejecutar** al final de la página para ejecutar la función. Asegúrese de recibir el código de respuesta 200. La propiedad que se ha pegado también se mostrará en la definición de JSON.

1. Después, puede intentar agregar de nuevo la clave de autenticación del entorno de ejecución de integración a dicho entorno.


**Solución 2:** Puede consultar el artículo siguiente para ver la solución:

https://docs.microsoft.com/azure/data-factory/data-factory-private-link

Pruebe a habilitar el acceso a la red pública con la interfaz de usuario.

![Habilitación del acceso a la red pública](media/self-hosted-integration-runtime-troubleshoot-guide/enable-public-network-access.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más ayuda para solucionar problemas, pruebe los siguientes recursos:

*  [Private Link para Data Factory](data-factory-private-link.md)
*  [Blog de Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Solicitud de características de Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vídeos de Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Página de preguntas y respuestas de Microsoft](/answers/topics/azure-data-factory.html)
*  [Foro de Stack Overflow para Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Información de Twitter sobre Data Factory](https://twitter.com/hashtag/DataFactory)