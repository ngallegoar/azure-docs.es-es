---
title: Implementación del agente de Azure File Sync
description: Implemente el agente de Azure File Sync. Un bloque de texto común, compartido entre los documentos de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 694becc49667204ef2071a140bb6330285089039
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78209279"
---
En esta sección, va a instalar el agente de Azure File Sync en Windows Server.
En la [guía de implementación](../articles/storage/files/storage-sync-files-deployment-guide.md) se muestra que tiene que desactivar la **seguridad mejorada de IE**. La seguridad mejorada de Internet Explorer es una medida de seguridad que no es aplicable con Azure File Sync y su desactivación le permite autenticarse en Azure sin ningún problema.

Abra PowerShell e instale los módulos necesarios con los siguientes comandos. Asegúrese de instalar el módulo completo y el proveedor de NuGet cuando se le solicite:

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

Si tiene problemas para conectarse a Internet desde el servidor, ahora es el momento de solucionarlos. Azure File Sync usa cualquier conexión de red disponible a Internet.
También se admite la exigencia de un servidor proxy para tener acceso a Internet. Ahora puede configurar un proxy en todo el equipo, o bien especificar un proxy que solo usará File Sync durante la instalación del agente.

Si esto quiere decir que necesita abrir los firewalls para este servidor, entonces podría ser un enfoque aceptable para usted. Al final de la instalación del servidor, después de completar el registro del servidor, habrá un informe de conectividad de red que mostrará las direcciones URL exactas de los puntos de conexión en Azure, con las que la sincronización de archivos debe comunicarse para la región que ha seleccionado. El informe también indica el motivo por el que se necesita la comunicación. Puede usar el informe para bloquear los firewalls de este servidor a direcciones URL específicas.

También puede seguir un enfoque más conservador, en el que no abra los firewalls, sino que limite el servidor para que se comunique con espacios de nombres DNS de nivel superior. Hay más documentación y detalles disponibles en el artículo [configuración de proxy y firewall de Azure File Sync](../articles/storage/files/storage-sync-files-firewall-and-proxy.md). Siga sus propios procedimientos recomendados de redes.

Al final del Asistente para la *instalación* del servidor, se muestra un Asistente para el *registro*.
Registre el servidor en el recurso de Azure del servicio de sincronización de almacenamiento anterior.

Estos pasos se describen con más detalle en la guía de implementación, incluidos los módulos de PowerShell anteriores que debe instalar primero: [Instalación de agente de Azure File Sync](../articles/storage/files/storage-sync-files-deployment-guide.md)

Debe usarse el agente más reciente y se puede descargar desde el Centro de descarga de Microsoft: [Agente de Azure File Sync](https://aka.ms/AFS/agent "Descarga del agente de Azure File Sync").

Después de una instalación y un registro de servidor correctos, puede comprobar que ha completado correctamente este paso: Desplácese al recurso del servicio de sincronización de almacenamiento en Azure Portal y, después, siga el menú de la izquierda hasta "Servidores registrados". Verá que el servidor aparece en este momento.
