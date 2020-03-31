---
title: Almacenamiento del paquete de AppSource en Azure Storage y generación de una dirección URL con la clave SAS
description: Detalla los pasos necesarios para cargar y proteger un paquete de AppSource.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: dsindona
ms.openlocfilehash: 57bc370fd160b8b3d6d7941ea28cd460c99f3d96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285374"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>Almacenamiento del paquete de AppSource en Azure Storage y generación de una dirección URL con la clave SAS
=============================================================================

Para mantener los archivos protegidos, todos los asociados deben almacenar su archivo de paquete de AppSource en una cuenta de almacenamiento de blobs de Azure y usar una clave SAS para compartirlo. Recuperaremos el archivo de paquete en su ubicación de almacenamiento de Azure para la certificación y para usarlo para las evaluaciones de AppSource.

Para cargar el paquete en el almacenamiento de blobs, siga estos pasos:

1. Vaya a <https://azure.microsoft.com> y cree una evaluación gratuita o una cuenta de pago.

2. Inicie sesión en [Azure Portal](https://portal.azure.com/).

3. Para crear una nueva cuenta de almacenamiento, haga clic en **+ Nuevo** y vaya a la cuenta **Datos y almacenamiento**.

   ![Hoja Datos y almacenamiento de Microsoft Azure Portal](media/CRMScreenShot7.png)

4. Escriba un **nombre** y un nombre para el **grupo de recursos** y haga clic en el botón **Crear**.

   ![Creación de una cuenta de almacenamiento en Microsoft Azure Portal](media/CRMScreenShot8.png)

5. Desplácese al grupo de recursos recién creado y cree un nuevo contenedor de blobs.

   ![Carga del paquete como blob mediante Microsoft Azure Portal](media/CRMScreenShot9.png)

6. Si aún no lo ha hecho, descargue e instale el [Explorador de Microsoft Azure Storage](https://storageexplorer.com/).

7. Abra el Explorador de Storage y use el icono para conectarse a su cuenta de Azure Storage.

8. Desplácese hasta el contenedor de blobs que creó y haga clic en **Cargar** para agregar el archivo ZIP del paquete.

   ![Carga del paquete mediante el Explorador de Microsoft Azure Storage](media/CRMScreenShot10.png)

9. Haga clic con el botón derecho en el archivo y seleccione **Obtener firma de acceso compartido**.

   ![Obtención de la Firma de acceso compartido de un archivo de Azure](media/CRMScreenShot11.png)

10. Modifique el valor de **Tiempo de expiración** para activar SAS durante un mes y, a continuación, haga clic en **Crear**.

    ![Modificación de la fecha de expiración de SAS de un archivo de Azure](media/CRMScreenShot12.png)

11. Copie el campo de la URL y guárdelo para usarlo más adelante. Deberá escribir esta dirección URL cuando cree la oferta asociada. 

    ![Copia de la dirección URL de SAS de un archivo de Azure](media/CRMScreenShot13.png)

