---
title: Implementar el agente de Azure File Sync
description: Implemente el agente de Azure File Sync. Un bloque de texto común, compartido entre los documentos de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 66388f14949b4f398de18c9162ca453e7fb3cbe1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82143584"
---
En esta sección se instala el agente de Azure File Sync en una instancia de Windows Server.

En la [guía de implementación](../articles/storage/files/storage-sync-files-deployment-guide.md) se indica que es preciso desactivar la **configuración de seguridad mejorada de Internet Explorer**. Esta medida de seguridad no se puede aplicar con Azure File Sync. Si la desactiva, puede autenticarse en Azure sin problemas.

Abra PowerShell e instale los módulos necesarios mediante los siguientes comandos. Asegúrese de instalar el módulo completo y el proveedor de NuGet cuando se le solicite.

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

Si tiene problemas para conectarse a Internet desde el servidor, ahora es el momento de solucionarlos. Azure File Sync usa cualquier conexión de red disponible a Internet. También se admite la exigencia de un servidor proxy para tener acceso a Internet. Ya puede configurar un proxy en toda la máquina, o bien especificar un proxy que solo va a usar Azure File Sync durante la instalación del agente.

Si para configurar un proxy, es preciso que abra los firewalls para este servidor, el enfoque podría ser aceptable. Al final de la instalación del servidor, después de haber completado el registro del servidor, un informe de conectividad de red le mostrará las direcciones URL exactas de los puntos de conexión en Azure, con las que Azure File Sync necesita comunicarse en la región que ha seleccionado. El informe también indica el motivo por el que se necesita la comunicación. Puede usar el informe para bloquear los firewalls de este servidor en direcciones URL específicas.

También puede seguir un enfoque más conservador y no abrir totalmente los firewalls, sino limitar el servidor para que se comunique con espacios de nombres DNS de nivel superior. Para más información, consulte [Configuración del proxy y el firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md). Siga sus propios procedimientos recomendados de redes.

Al final del Asistente para la *instalación* del servidor, se abrirá un Asistente para el *registro* del servidor. Registre el servidor en el recurso de Azure del servicio de sincronización de almacenamiento anterior.

Estos pasos se describen con más detalle en la guía de implementación, incluidos los módulos de PowerShell que debe instalar primero: [Instalación de agente de Azure File Sync](../articles/storage/files/storage-sync-files-deployment-guide.md).

Use el agente más reciente. Puede descargarlo del Centro de descarga de Microsoft: [Agente de Azure File Sync](https://aka.ms/AFS/agent "Descarga del agente de Azure File Sync").

Después de una instalación y un registro del servidor correctos, puede comprobar que ha completado correctamente este paso. Vaya al recurso del servicio de sincronización de almacenamiento en Azure Portal y, después, siga el menú de la izquierda hasta **Servidores registrados**. Verá que el servidor aparece en esa lista.
