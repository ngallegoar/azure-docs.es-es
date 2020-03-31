---
title: 'Importación masiva para agregar miembros a un grupo: Azure Active Directory | Microsoft Docs'
description: Agregue miembros de grupo de forma masiva en el Centro de administración de Azure Active Directory.
services: active-directory
author: curtand
ms.author: curtand
manager: mtillman
ms.date: 09/11/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ff4234d9fd28e655e868108e37b09c5afe2767c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72517132"
---
# <a name="bulk-import-group-members-preview-in-azure-active-directory"></a>Importación masiva de los miembros del grupo (versión preliminar) en Azure Active Directory

Mediante el portal de Azure Active Directory (Azure AD), puede agregar una gran cantidad de miembros a un grupo mediante el uso de un archivo de valores separados por comas (CSV) para importar dichos miembros de forma masiva.

## <a name="to-bulk-import-group-members"></a>Para importar de forma masiva miembros de grupo

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de administrador de usuarios en la organización. Los propietarios de grupos también pueden importar de forma masiva miembros de los grupos que poseen.
1. En Azure AD, seleccione **Grupos** > **Todos los grupos**.
1. Abra el grupo al que va a agregar miembros y luego seleccione **Miembros**.
1. En la página **Miembros**, seleccione **Importar miembros**.
1. En la página **Importación masiva de los miembros del grupo (versión preliminar)** , seleccione **Descargar** para obtener la plantilla del archivo CSV con las propiedades de miembro de grupo requeridas.

    ![El comando Importar miembros está en la página de perfil del grupo](./media/groups-bulk-import-members/import-panel.png)

1. Abra el archivo CSV y agregue una línea para cada miembro del grupo que quiera importar al grupo (los valores necesarios son **Id. de objeto de miembro** o **Nombre principal del usuario)** . A continuación, guarde el archivo.

   ![El archivo CSV contiene los nombres y los identificadores de los miembros que se importarán](./media/groups-bulk-import-members/csv-file.png)

1. En la página **Importación masiva de los miembros del grupo (versión preliminar)** , en **Cargar archivo CSV**, vaya al archivo. Al seleccionar el archivo CSV, comienza su validación.
1. Cuando finalice la validación del contenido del archivo, aparecerá el mensaje **Archivo cargado correctamente** en la página de importación en bloque. Si hay errores, debe corregirlos para poder enviar el trabajo.
1. Cuando el archivo supere la validación, seleccione **Enviar** para iniciar la operación masiva de Azure que importa los miembros al grupo.
1. Cuando la operación de importación finalice, verá una notificación que indicará que la operación masiva se realizó correctamente.

## <a name="check-import-status"></a>Comprobación del estado de importación

Puede ver el estado de todas las solicitudes masivas pendientes en la página de **resultados de la operación masiva (versión preliminar)** .

   ![La página de resultados de la operación masiva muestra el estado de la solicitud masiva](./media/groups-bulk-import-members/bulk-center.png)

Para obtener más información sobre cada elemento de línea de la operación masiva, seleccione los valores de las columnas **Número de elementos correctos**, **Número de errores** o **Total de solicitudes**. Si se produjeron errores, se mostrarán sus motivos.

## <a name="bulk-import-service-limits"></a>Límites del servicio de importación en bloque

La ejecución de cada actividad masiva para importar una lista de miembros de grupo puede durar hasta una hora. Esto permite la importación de una lista de al menos 40 000 miembros.

## <a name="next-steps"></a>Pasos siguientes

- [Quitar miembros de un grupo de forma masiva](groups-bulk-remove-members.md)
- [Descarga de miembros de un grupo](groups-bulk-download-members.md)
- [Descargar una lista de todos los grupos](groups-bulk-download.md)
