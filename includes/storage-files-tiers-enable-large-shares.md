---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 327e86642041a607ada7c1173bb053b12a41832c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011775"
---
De forma predeterminada, los recursos compartidos de archivos estándar solo pueden abarcar hasta 5 TiB, aunque el límite de recursos compartidos se puede aumentar hasta 100 TiB. Para ello, la característica de *recursos compartidos de archivos grandes* debe estar habilitada en el nivel de cuenta de almacenamiento. Las cuentas de almacenamiento premium (cuentas de almacenamiento *FileStorage*) no tienen la marca de la característica de recursos compartidos de archivos grandes, ya que todos los recursos compartidos de archivos premium ya están habilitados para aprovisionar hasta la capacidad completa de 100 TiB.

Solo puede habilitar recursos compartidos de archivos grandes en cuentas de almacenamiento estándar con redundancia local o con redundancia de zona. Una vez habilitada la marca de la característica de recursos compartidos de archivos grandes, no puede cambiar el nivel de redundancia a almacenamiento con redundancia geográfica o con redundancia de zona geográfica.

Para habilitar recursos compartidos de archivos grandes en una cuenta de almacenamiento existente, vaya a la vista **Configuración** en la tabla de contenido de la cuenta de almacenamiento y cambie el conmutador de recursos compartido de archivos a habilitado:

![Captura de pantalla de la habilitación del conmutador de recursos compartidos de archivos grandes en Azure Portal](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

También puede habilitar recursos compartidos de archivos de 100 TiB mediante el cmdlet [`Set-AzStorageAccount`](/powershell/module/az.storage/set-azstorageaccount) de PowerShell y el comando [`az storage account update`](/cli/azure/storage/account#az-storage-account-update) de la CLI de Azure. Para obtener instrucciones detalladas sobre cómo habilitar los recursos compartidos de archivos de gran tamaño, consulte [Habilitar y crear recursos compartidos de archivos de gran tamaño](../articles/storage/files/storage-files-how-to-create-large-file-share.md).

Para más información sobre cómo crear recursos compartidos de archivos en nuevas cuentas de almacenamiento, consulte [Creación de un recurso compartido de archivos de Azure](../articles/storage/files/storage-how-to-create-file-share.md).