---
title: Recomendaciones de corrección de Azure Security Center | Microsoft Docs
description: En este artículo se explican las recomendaciones de corrección de Azure Security Center para proteger los recursos y cumplir con las directivas de seguridad.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 8be947cc-cc86-421d-87a6-b1e23077fd50
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 9beb617ed8626b1fda1c9db98d626ca70ee01755
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86042924"
---
# <a name="remediate-recommendations-in-azure-security-center"></a>Recomendaciones de corrección en Azure Security Center

Las recomendaciones ofrecen sugerencias sobre cómo proteger mejor los recursos. Para implementar una recomendación, siga los pasos de corrección proporcionados en la recomendación.

## <a name="remediation-steps"></a>Pasos de corrección <a name="remediation-steps"></a>

Después de revisar todas las recomendaciones, decida cuál se corrige primero. Se recomienda usar el [impacto de la puntuación segura](security-center-recommendations.md#monitor-recommendations) para ayudarle a priorizar qué hacer primero.

1. En la lista, haga clic en la recomendación.

1. Siga las instrucciones de la sección **Pasos para la corrección**. Cada recomendación tiene su propio conjunto de instrucciones. En la siguiente captura de pantalla se muestran los pasos de corrección para configurar aplicaciones, a fin de permitir solo el tráfico a través de HTTPS.

    ![Detalles de recomendación](./media/security-center-remediate-recommendations/security-center-remediate-recommendation.png)

1. Una vez completada, aparece una notificación que le informa si la corrección se ha realizado correctamente.

## <a name="quick-fix-remediation"></a>Solución por corrección rápida<a name="one-click"></a>

La corrección rápida le permite solucionar rápidamente una recomendación en varios recursos. Solo está disponible para recomendaciones específicas. La corrección rápida simplifica el proceso y permite mejorar rápidamente la puntuación de seguridad e incrementar la seguridad de su entorno.

Para implementar la solución por corrección rápida:

1. En la lista de recomendaciones que tienen la etiqueta **Corrección rápida**, haga clic en la recomendación.

    [![Seleccione Corrección rápida](media/security-center-remediate-recommendations/security-center-one-click-fix-select.png)](media/security-center-remediate-recommendations/security-center-one-click-fix-select.png#lightbox)

1. En la pestaña **Recursos con estado incorrecto**, seleccione los recursos en los que desea implementar la recomendación y haga clic en **Corregir**.

    > [!NOTE]
    > Algunos de los recursos enumerados podrían estar deshabilitados porque no tiene los permisos adecuados para modificarlos.

1. En el cuadro de confirmación, lea los detalles y las implicaciones de la corrección.

    ![Corrección rápida](./media/security-center-remediate-recommendations/security-center-one-click-fix-view.png)

    > [!NOTE]
    > Las implicaciones se muestran en el cuadro gris de la ventana **Corregir recursos** que se abre después de hacer clic en **Corregir**. Estas enumeran los cambios que se producen al continuar con la corrección rápida.

1. Inserte los parámetros pertinentes si es necesario y apruebe la corrección.

    > [!NOTE]
    > Una vez completada la corrección, pueden pasar varios minutos hasta que los recursos se vean en la pestaña **Recursos con estado correcto**. Para ver las acciones de la corrección, consulte el [registro de actividad](#activity-log).

1. Una vez completada, aparece una notificación que le informa si la corrección se ha realizado correctamente.

## <a name="quick-fix-remediation-logging-in-the-activity-log"></a>Registro de la corrección rápida en el registro de actividad <a name="activity-log"></a>

La operación de corrección usa una implementación de plantilla o una llamada API REST PATCH para aplicar la configuración en el recurso. Estas operaciones se registran en el [registro de actividad de Azure](../azure-resource-manager/management/view-activity-logs.md).


## <a name="recommendations-with-quick-fix-remediation"></a>Recomendaciones con corrección rápida

|Recomendación|Implicación|
|---|---|
|Se debe habilitar la auditoría en SQL Database|Esta acción habilitará la auditoría de SQL en estos servidores y sus bases de datos. <br>**Nota**: <ul><li>En cada región de la instancia de SQL Database seleccionada, se creará una cuenta de almacenamiento para guardar los registros de auditoría, que se compartirá con todos los servidores de esa región.</li><li>Para el correcto funcionamiento de la auditoría, no elimine el grupo de recursos o las cuentas de almacenamiento ni les cambie el nombre.</li></ul>|
|Se debe habilitar Advanced Data Security en Instancia administrada de SQL|Esta acción habilitará SQL Advanced Data Security (ADS) en las instancias administradas de SQL seleccionadas. <br>**Nota**: <ul><li>En todas las regiones y grupo de recursos de la instancia administrada de SQL seleccionada, se creará una cuenta de almacenamiento para guardar los resultados del examen y dicha cuenta la compartirán todas las instancias de esa región.</li><li> El precio de ADS es de 15 $ por instancia administrada de SQL.</li></ul>|
|Evaluación de vulnerabilidad debe estar habilitado en Instancia administrada de SQL|Esta acción habilitará Evaluación de vulnerabilidad de SQL en la instancia administrada de SQL seleccionada. <br>**Nota**:<ul><li>La evaluación de vulnerabilidad de SQL forma parte del paquete Advanced Data Security (ADS) de SQL. Si ADS no está habilitado, se habilitará automáticamente en la instancia administrada.</li><li>Para cada región y grupo de recursos de la instancia administrada de SQL seleccionada, se creará una cuenta de almacenamiento para almacenar los resultados del examen y dicha cuenta la compartirán todas las instancias de esa región.</li><li>El precio de ADS es de 15 $ por SQL Database.</li></ul>||
|Advanced Data Security debe estar habilitado en SQL Database|Esta acción habilitará Advanced Data Security (ADS) en estos servidores seleccionados y sus bases de datos. <br>**Nota**:<ul><li>Para cada región y grupo de recursos de la instancia de SQL Database seleccionada, se creará una cuenta de almacenamiento para almacenar los resultados del examen y dicha cuenta la compartirán todos los servidores de esa región.<</li><li>El precio de ADS es de 15 $ por SQL Database.</li></ul>||
|Evaluación de vulnerabilidad debe estar habilitado en SQL Database|Esta acción habilitará la evaluación de vulnerabilidad de SQL en estos servidores seleccionados y sus bases de datos. <br>**Nota**:<ul><li>La evaluación de vulnerabilidad de SQL forma parte del paquete Advanced Data Security (ADS) de SQL. Si ADS no se ha habilitado aún, se habilitará automáticamente en la instancia de SQL Database.</li><li>Para cada región y grupo de recursos de la instancia de SQL Database seleccionada, se creará una cuenta de almacenamiento para almacenar los resultados del examen y dicha cuenta la compartirán todas las instancias de esa región.</li><li>El precio de ADS es de 15 $ por SQL Database.</li></ul>||
|El cifrado de datos transparente en base de datos SQL debe estar habilitado|Esta acción habilita el cifrado de datos transparente de SQL Database en las bases de datos seleccionadas. <br>**Nota**: De forma predeterminada, se usarán las claves del cifrado de datos transparente administradas por el servicio.
|Se debe habilitar la transferencia segura a las cuentas de almacenamiento|Esta acción actualiza la seguridad de la cuenta de almacenamiento para permitir solo las solicitudes de conexiones seguras (HTTPS). <br>**Nota**:<ul><li>Se rechazarán todas las solicitudes que usen HTTP.</li><li>Cuando se use el servicio Azure Files, se producirá un error en la conexión sin cifrado, incluidos los escenarios que usan SMB 2.1 y SMB 3.0 sin cifrado, y algunas versiones del cliente SMB de Linux. Más información.</li></ul>|
|Acceso a la aplicación web solo a través de HTTPS|Esta acción redirigirá todo el tráfico de HTTP a HTTPS en los recursos seleccionados. <br>**Nota**:<ul><li>Se mostrará un punto de conexión HTTPS que no tiene un certificado SSL en el explorador con un error de privacidad. Por tanto, los usuarios que tienen un dominio personalizado deben comprobar que han configurado un certificado SSL.</li><li>Asegúrese de que los firewalls de aplicaciones web y de paquetes que protegen App Service permiten el reenvío de sesiones HTTPS.</li></ul>|
|Acceso a Function App solo a través de HTTPS|Esta acción redirigirá todo el tráfico de HTTP a HTTPS en los recursos seleccionados. <br>**Nota**:<ul><li>Se mostrará un punto de conexión HTTPS que no tiene un certificado SSL en el explorador con un error de privacidad. Por tanto, los usuarios que tienen un dominio personalizado deben comprobar que han configurado un certificado SSL.</li><li>Asegúrese de que los firewalls de aplicaciones web y de paquetes que protegen App Service permiten el reenvío de sesiones HTTPS.</li></ul>|
|Acceso a API App solo a través de HTTPS|Esta acción redirigirá todo el tráfico de HTTP a HTTPS en los recursos seleccionados. <br>**Nota**:<ul><li>Se mostrará un punto de conexión HTTPS que no tiene un certificado SSL en el explorador con un error de privacidad. Por tanto, los usuarios que tienen un dominio personalizado deben comprobar que han configurado un certificado SSL.</li><li>Asegúrese de que los firewalls de aplicaciones web y de paquetes que protegen App Service permiten el reenvío de sesiones HTTPS.</li></ul>|
|Recomendación de desactivación de la depuración remota para aplicaciones web|Esta acción deshabilita la depuración remota.|
|Recomendación de desactivación de la depuración remota para Function App|Esta acción deshabilita la depuración remota.|
|Se debe desactivar la depuración remota para aplicaciones de API|Esta acción deshabilita la depuración remota.|
|CORS no debe permitir que todos los recursos accedan a las aplicaciones web.|Esta acción impide que otros dominios accedan a la aplicación web. Para permitir dominios específicos, escríbalos en el campo Orígenes permitidos (separados por comas). <br>**Nota**: Si deja el campo vacío, se bloquearán todas las llamadas entre orígenes. Título del campo del parámetro: "Orígenes permitidos"|
|CORS no debe permitir que todos los recursos accedan a Function App|Esta acción impide que otros dominios accedan a la aplicación de función. Para permitir dominios específicos, escríbalos en el campo Orígenes permitidos (separados por comas). <br>**Nota**: Si deja el campo vacío, se bloquearán todas las llamadas entre orígenes. Título del campo del parámetro: "Orígenes permitidos"|
|CORS no debería permitir que todos los recursos accedan a la aplicación de API.|Esta acción impide que otros dominios accedan a la aplicación de API. Para permitir dominios específicos, escríbalos en el campo Orígenes permitidos (separados por comas). <br>**Nota**: Si deja el campo vacío, se bloquearán todas las llamadas entre orígenes. Título del campo del parámetro: "Orígenes permitidos"|
|Un agente de supervisión debe estar habilitado en las máquinas virtuales.|Esta acción instala un agente de supervisión en las máquinas virtuales seleccionadas. Seleccione un área de trabajo a la que informará el agente.<ul><li>Si la directiva de actualización está establecida en modo automático, se implementará en nuevas instancias existentes.</li><li>Si la directiva de actualización está establecida en modo manual y desea instalar el agente en las instancias existentes, active la casilla. [Más información](../virtual-machine-scale-sets/virtual-machine-scale-sets-faq.md#how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set)</li></ul>|
|Los registros de diagnóstico en Key Vault deben estar habilitados|Esta acción habilita los registros de diagnóstico en los almacenes de claves. Las métricas y los registros de diagnóstico se guardan en el área de trabajo seleccionada.|
|Los registros de diagnóstico en Service Bus deben estar habilitados.|Esta acción habilita los registros de diagnóstico en Service Bus. Las métricas y los registros de diagnóstico se guardan en el área de trabajo seleccionada.|

## <a name="next-steps"></a>Pasos siguientes

En este documento, se mostró cómo aplicar las recomendaciones de corrección de Security Center. Para más información sobre Security Center, consulte los temas:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
