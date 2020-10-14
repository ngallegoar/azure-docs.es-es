---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/25/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: bed8475e5d6c7bf26003672b6cf9ce51a82384ad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91376891"
---
Para crear una cuenta de almacenamiento de uso general v2 en Azure Portal, siga estos pasos:

1. En el menú de Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Cuentas de almacenamiento**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Cuentas de almacenamiento**.
1. En la ventana **Cuentas de almacenamiento** que aparece, elija **Agregar**.
1. En la pestaña **Aspectos básicos**, seleccione la suscripción en la que se va a crear la cuenta de almacenamiento.
1. En el campo **Grupo de recursos**, seleccione el grupo de recursos deseado o cree uno nuevo.  Para más información sobre los grupos de recursos de Azure, consulte [Información general de Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).
1. Después, escriba un nombre para la cuenta de almacenamiento. El nombre que elija debe ser único en Azure. El nombre también debe tener una longitud de entre 3 y 24 caracteres, y solo puede contener números y letras minúsculas.
1. Seleccione una ubicación para la cuenta de almacenamiento o utilice la ubicación predeterminada.
1. Seleccione un nivel de rendimiento. El valor predeterminado es *Estándar*.
1. En el campo **Tipo de cuenta**, elija *Storage V2 (general-purpose v2)* [Storage V2 (uso general v2)].
1. Especifique cómo se replicará la cuenta de almacenamiento. La opción de replicación predeterminada es *Almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS)* . Para más información acerca de las opciones de replicación disponibles, consulte [Redundancia de Azure Storage](../articles/storage/common/storage-redundancy.md).
1. Especifique el nivel de acceso de los blobs en la cuenta de almacenamiento. El nivel predeterminado es *frecuente*. Para más información acerca de los niveles de acceso de los blobs, consulte [Niveles de acceso frecuente, esporádico y de archivo de los blobs](../articles/storage/blobs/storage-blob-storage-tiers.md).
1. Para utilizar Azure Data Lake Storage, elija la pestaña **Opciones avanzadas** y, después, establezca **Espacio de nombres jerárquico** en **Habilitado**. Para más información, consulte [Introducción a Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-introduction.md).
1. Seleccione **Revisar y crear** para revisar la configuración de la cuenta de almacenamiento y crear la cuenta.
1. Seleccione **Crear**.

En la imagen siguiente se muestra la configuración de la pestaña **Aspectos básicos** de una nueva cuenta de almacenamiento:

:::image type="content" source="media/storage-create-account-portal-include/account-create-portal.png" alt-text="Captura de pantalla en la que se muestra cómo crear una cuenta de almacenamiento en Azure Portal":::
