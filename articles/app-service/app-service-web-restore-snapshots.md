---
title: Restauración de una aplicación a partir de una instantánea
description: Obtenga información sobre cómo restaurar la aplicación desde una instantánea. Realice la recuperación de una pérdida de datos inesperada en el nivel Premium con las copias automáticas de las propiedades reemplazadas.
ms.assetid: 4164f9b5-f735-41c6-a2bb-71f15cdda417
ms.topic: article
ms.date: 04/04/2018
ms.reviewer: nicking
ms.custom: seodec18
ms.openlocfilehash: b17a49535541c8f75f65cdbe9986a895427f3877
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255148"
---
# <a name="restore-an-app-in-azure-from-a-snapshot"></a>Restauración de una aplicación en Azure desde una instantánea
Este artículo muestra cómo restaurar una aplicación en [Azure App Service](../app-service/overview.md) desde una instantánea. Puede restaurar la aplicación a un estado anterior basado en una de las instantáneas de la aplicación. No es necesario habilitar la copia de seguridad de instantáneas, la plataforma guarda automáticamente una instantánea de todas las aplicaciones para fines de recuperación de datos.

Las instantáneas son copias de instantáneas incrementales y ofrecen varias ventajas sobre las [copias de seguridad](manage-backup.md) normales:
- No hay errores de copia de archivos debido a bloqueos de archivo.
- Ninguna limitación de tamaño de almacenamiento.
- No se requiere ninguna configuración.

La restauración desde instantáneas está disponible para las aplicaciones que se ejecutan en el nivel **Premium** o superior. Para obtener más información sobre la escalación de su aplicación, vea [Escalación de una aplicación web en el Servicio de aplicaciones de Azure](manage-scale-up.md).

## <a name="limitations"></a>Limitaciones

- Esta característica actualmente está en versión preliminar.
- Solo puede restaurar en la misma aplicación o en una ranura que pertenezca a esa aplicación.
- App Service detiene la aplicación de destino o la ranura de destino mientras se realiza la restauración.
- App Service mantiene instantáneas correspondientes a tres meses con fines de recuperación de datos de la plataforma.
- Solo se pueden restaurar instantáneas de los últimos 30 días.
- App Services en una instancia de App Service Environment no admite instantáneas.
 

## <a name="restore-an-app-from-a-snapshot"></a>Restauración de una aplicación desde una instantánea

1. En la página **Configuración** de la aplicación en [Azure Portal](https://portal.azure.com), haga clic en **Copias de seguridad** para mostrar la página **Copias de seguridad**. A continuación, haga clic en **Restaurar** en la sección **Instantánea (Versión preliminar)** .
   
    ![](./media/app-service-web-restore-snapshots/1.png)

2. En la página **Restaurar**, seleccione la instantánea para restaurar.
   
    ![](./media/app-service-web-restore-snapshots/2.png)
   
3. Especifique el destino de la restauración de la aplicación en **Destino de restauración**.
   
    ![](./media/app-service-web-restore-snapshots/3.png)
   
   > [!WARNING]
   > Si elige **Sobrescribir**, se borrarán y sobrescribirán todos los datos existentes en el sistema de archivos actual de la aplicación. Antes de hacer clic en **Aceptar**, asegúrese de que es lo que desea hacer.
   > 
   > 
      
   > [!Note]
   > Debido a limitaciones técnicas actuales, solo puede restaurar a aplicaciones de la misma unidad de escala. Esta limitación se eliminará en una próxima versión.
   > 
   > 
   
    Puede seleccionar **Aplicación existente** para restaurar a una ranura. Antes de usar esta opción, debe haber creado una ranura en la aplicación.

4. Puede elegir restaurar la configuración del sitio.
   
    ![](./media/app-service-web-restore-snapshots/4.png)

5. Haga clic en **OK**.
