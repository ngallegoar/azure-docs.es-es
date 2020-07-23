---
title: Errores de la consola serie de Azure | Microsoft Docs
description: Errores comunes en la consola serie de Azure
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/20/2019
ms.author: alsin
ms.openlocfilehash: cad12a55332a6c7898f9709776c58d7dba8dd81a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86526442"
---
# <a name="common-errors-within-the-azure-serial-console"></a>Errores comunes en la consola serie de Azure
Existe un conjunto de errores conocidos en la consola serie de Azure. A continuación encontrará una lista de dichos errores y los pasos de mitigación.

## <a name="common-errors"></a>Errores comunes

Error                             |   Mitigación
:---------------------------------|:--------------------------------------------|
"La consola serie de Azure requiere que se habiliten los diagnósticos de arranque. Haga clic aquí para configurar los diagnósticos de arranque de la máquina virtual". | Asegúrese de que la máquina virtual o el conjunto de escalado de máquinas virtuales tiene el [diagnóstico de arranque](boot-diagnostics.md) habilitado. Si usa la consola serie en una instancia de conjunto de escalado de máquinas virtuales, asegúrese de que la instancia tenga el modelo más reciente.
"La consola serie de Azure requiere que haya una máquina virtual en ejecución. Use el botón Inicio anterior para iniciar la máquina virtual".  | La instancia de la máquina virtual o del conjunto de escalado de máquinas virtuales debe encontrarse en un estado iniciado para tener acceso a la consola serie (la VM no debe estar detenida ni desasignada). Compruebe que la instancia de la máquina virtual o del conjunto de escalado de máquinas virtuales está en ejecución e inténtelo de nuevo.
"La consola serie de Azure no está habilitada para esta suscripción, póngase en contacto con el administrador de la suscripción para habilitarla". | La consola serie de Azure se puede deshabilitar en el nivel de suscripción. Si es administrador de suscripciones, puede [habilitar y deshabilitar la consola serie de Azure](./serial-console-enable-disable.md). Si no es administrador de suscripciones, debe ponerse en contacto con el administrador de suscripciones para conocer los pasos siguientes.
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnósticos de arranque de la VM. | Asegúrese de que los diagnósticos de arranque no tengan ningún firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnósticos de arranque accesible para que la consola serie funcione. Por su diseño, la consola serie no puede operar con firewalls de la cuenta de almacenamiento habilitados en la cuenta de almacenamiento del diagnóstico de arranque.
No tiene los permisos necesarios para usar esta máquina virtual como consola serie. Asegúrese de tener los permisos del rol de Colaborador de la máquina virtual como mínimo.| El acceso a la consola serie requiere tener acceso de nivel de colaborador o superior en la máquina virtual o en el conjunto de escalado de máquinas virtuales. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
No se encontró la cuenta de almacenamiento usada para los diagnósticos de arranque en esta máquina virtual. Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual, que esta cuenta de almacenamiento no se ha eliminado y que tiene acceso a esta cuenta de almacenamiento. | Asegúrese de que no ha eliminado la cuenta de almacenamiento de diagnósticos de arranque para la máquina virtual o el conjunto de escalado de máquinas virtuales.
Se encontró un error al conectar la consola serie a la máquina virtual: "Solicitud incorrecta" (400) | Esto puede ocurrir si el identificador URI del diagnóstico de arranque es incorrecto. Por ejemplo, se usó "http://" en lugar de "https://". El identificador URI del diagnóstico de arranque se puede corregir con este comando: `az vm boot-diagnostics enable --name vmName --resource-group rgName --storage https://<storageAccountUri>.blob.core.windows.net/`
No tiene los permisos necesarios para escribir en la cuenta de almacenamiento de diagnósticos de arranque de esta máquina virtual. Asegúrese de tener al menos permisos de colaborador de máquina virtual. | El acceso a la consola serie requiere el acceso de nivel de colaborador en la cuenta de almacenamiento de diagnósticos de arranque. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
No se puede determinar el grupo de recursos para la cuenta de almacenamiento de diagnósticos de arranque *&lt;STORAGEACCOUNTNAME&gt;* . Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual y acceda a esta cuenta de almacenamiento. | El acceso a la consola serie requiere el acceso de nivel de colaborador en la cuenta de almacenamiento de diagnósticos de arranque. Para obtener más información, consulte la [página de información general](serial-console-overview.md).
El aprovisionamiento de esta máquina virtual aún no se ha realizado correctamente. Asegúrese de que la máquina virtual está totalmente implementada y vuelva a intentar la conexión de la consola serie. | Es posible que la máquina virtual o el conjunto de escalado de máquinas virtuales todavía se estén aprovisionando. Espere un poco y vuelva a intentarlo.
Socket web está cerrado o no se pudo abrir. | Es posible que tenga que agregar acceso de firewall a `*.console.azure.com`. Un enfoque más detallado, pero más largo, consiste en permitir el acceso de firewall a los [intervalos IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653), que cambian con bastante frecuencia.
La consola serie no funciona con una cuenta de almacenamiento que use Azure Data Lake Storage Gen2 con espacios de nombres jerárquicos. | Se trata de un problema conocido con los espacios de nombres jerárquicos. Para mitigar el problema, asegúrese de que la cuenta de almacenamiento con diagnóstico de arranque de la máquina virtual no se crea mediante Azure Data Lake Storage Gen2. Esta opción solo puede establecerse durante la creación de la cuenta de almacenamiento. Es posible que deba crear cuenta de almacenamiento con diagnóstico de arranque independiente sin Azure Data Lake Storage Gen2 habilitado para mitigar este problema.
Se encontró un error al conectar la consola serie a la máquina virtual: "Prohibido" (SubscriptionNotEnabled): nombre de suscripción sin definir, el identificador \<subscription id> está en estado no habilitado sin definir. | Esta incidencia puede producirse si se ha deshabilitado la suscripción que ha creado un usuario para la cuenta de almacenamiento de Cloud Shell. Para mitigarla, inicie Cloud Shell y [realice los pasos necesarios](../../cloud-shell/persisting-shell-storage.md#unmount-clouddrive-1) a fin de volver a aprovisionar una cuenta de almacenamiento de respaldo para Cloud Shell en la suscripción actual.

## <a name="next-steps"></a>Pasos siguientes
* Más información sobre la [consola serie de Azure para máquinas virtuales Linux](./serial-console-linux.md)
* Más información sobre la [consola serie de Azure para máquinas virtuales Windows](./serial-console-windows.md)
