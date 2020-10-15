---
title: 'Movimiento de datos de Blob Storage con el Explorador de Azure Storage: Team Data Science Process'
description: Aprenda a usar el Explorador de Azure Storage para cargar y descargar datos de Azure Blob Storage.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: bfc63c6f5aca92fb7fda9e3ecf63ce4c332b12ef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "76720918"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Mover datos hacia y desde Azure Blob Storage con el Explorador de Azure Storage
El Explorador de Azure Storage es una herramienta gratuita de Microsoft que permite trabajar con los datos de Azure Storage en Windows, macOS y Linux. En este tema se describe cómo usarlo para cargar y descargar datos del Almacenamiento de blobs de Azure. La herramienta se puede descargar desde el [Explorador de Microsoft Azure Storage](https://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Si está usando la máquina virtual que se configuró con los scripts ofrecidos por [Máquinas virtuales de ciencia de datos en Azure](virtual-machines.md),el Explorador de Azure Storage ya está instalado en la máquina virtual.
> 
> [!NOTE]
> Para ver una introducción completa a Azure Blob Storage, vea los [Aspectos básicos sobre los blobs de Azure](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) y [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Prerequisites
En este documento se supone que tiene una suscripción de Azure y una cuenta de almacenamiento y la clave de almacenamiento correspondiente para dicha cuenta. Antes de cargar o descargar datos, debe conocer su nombre de cuenta de Azure Storage y la clave de cuenta. 

* Para configurar una suscripción a Azure, consulte [Prueba gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).
* Para obtener instrucciones sobre cómo crear una cuenta de almacenamiento y cómo obtener información sobre la cuenta y la clave, consulte [Acerca de las cuentas de Azure Storage](../../storage/common/storage-create-storage-account.md). Anote la clave de acceso de la cuenta de almacenamiento, ya que la necesita para conectarse a la cuenta con la herramienta Explorador de Azure Storage.
* La herramienta Explorador de Azure Storage puede descargarse desde [aquí](https://storageexplorer.com/). Acepte los valores predeterminados durante la instalación.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Usar el Explorador de Azure Storage
En los pasos siguientes se describe cómo cargar y descargar datos mediante el Explorador de Azure Storage. 

1. Inicie Explorador de Microsoft Azure Storage.
2. Para que aparezca el Asistente **Iniciar sesión en una cuenta...** , seleccione el icono de **Configuración de la cuenta de Azure**, luego **Agregar una cuenta** y escriba las credenciales. 
![Agregar una cuenta de Azure Storage](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. Para que aparezca el Asistente para **conectar a Azure Storage**, seleccione el icono de **Conectar a Azure Storage**. ![Haga clic en "Conectar a Azure Storage"](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Escriba la clave de acceso de la cuenta de Azure Storage en el Asistente para **conectar a Azure Storage** y luego haga clic en **Siguiente**. ![Escribir la clave de acceso de la cuenta de Azure Storage](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Escriba el nombre de la cuenta de almacenamiento en el cuadro **Nombre de cuenta** y, después, seleccione **Siguiente**. ![Adjuntar almacenamiento externo](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Ahora debería aparecer la cuenta de almacenamiento agregada. Para crear un contenedor de blobs en una cuenta de almacenamiento, haga clic con el botón derecho en el nodo **Contenedores de blob** de esa cuenta, seleccione **Crear contenedor de blobs** y escriba un nombre.
7. Para cargar datos en un contenedor, seleccione el contenedor de destino y haga clic en el botón **Cargar**.
![Cuentas de almacenamiento](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Haga clic en **...** a la derecha del cuadro **Archivos**, seleccione uno o varios archivos para cargarlos desde el sistema de archivos y haga clic en **Cargar** para empezar a cargarlos.![Cargar archivos](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Para descargar datos, seleccione el blob en el contenedor correspondiente para descarga y haga clic en **Descargar**. ![Descargar archivos](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

