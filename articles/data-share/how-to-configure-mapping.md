---
title: Configuración de una asignación de conjunto de datos en Azure Data Share
description: Aprenda a configurar una asignación de conjunto de datos para un recurso compartido recibido en Azure Data Share.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 1ff432c2073536448c8194ebe537c8bf8cf00663
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76964250"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share"></a>Configuración de una asignación de conjunto de datos para un recurso compartido recibido en Azure Data Share

En este artículo se explica cómo configurar una asignación de conjunto de datos para un recurso compartido recibido en Azure Data Share. Deberá hacer esto si aceptó una invitación de un recurso compartido de datos, pero seleccionó "Accept and configure later" (Aceptar y configurar más adelante) o si los datos se comparten en contexto. Es posible que quiera configurar una asignación de conjunto de datos si necesita cambiar el destino de los datos que se comparten con usted o si quiere recibir datos en una instancia de SQL Server. 

## <a name="navigate-to-a-received-data-share"></a>Vaya a un recurso compartido de datos recibido.

En el servicio Azure Data Share, vaya al recurso compartido recibido y seleccione la pestaña **Detalles**. 

![Asignación de conjuntos de datos](./media/dataset-mapping.png "Asignación de conjuntos de datos") 

Active la casilla situada junto al conjunto de datos al que desearía asignar un destino. Seleccione **Unmap** (Desasignar) para desasignar la asignación existente. Seleccione **+ Map to target** (+ Asignar a destino) para elegir un nuevo almacén de destino. 

![Asignación a destino](./media/dataset-map-target.png "Asignar a destino") 

## <a name="select-a-new-target-store"></a>Selección de un nuevo almacén de destino

Seleccione un tipo de datos de destino en el que desee colocar los datos. En el uso compartido basado en instantáneas, todos los datos que ya existan en cualquier cuenta de almacenamiento ya asignada no se moverán automáticamente al almacén de destino nuevo. Para el uso compartido en contexto, seleccione un almacén de datos en la ubicación especificada. La ubicación es el centro de datos de Azure donde se encuentra el almacén de datos de origen del proveedor de datos.

![Cuenta de almacenamiento de destino](./media/dataset-map-target-sql.png "Almacenamiento de destino") 

## <a name="select-a-file-format-sql-sources-only"></a>Selección de un formato de archivo (solo orígenes de SQL)

Si los datos de origen proceden de un origen basado en SQL, puede elegir el formato en el que se reciben. 

![Elegir formato](./media/sql-file-formats.png "Formatos de archivo SQL")

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).



