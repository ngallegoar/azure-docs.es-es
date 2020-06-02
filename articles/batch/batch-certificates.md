---
title: 'Uso de certificados: Azure Batch'
description: Use certificados para habilitar la autenticación de aplicaciones
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 02/17/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 4da5fad63b148fa054eefb7f13424b46dc43bf29
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83764324"
---
# <a name="using-certificates-with-batch"></a>Uso de certificados con Batch

Actualmente, la principal razón para usar certificados con Batch es si tiene aplicaciones que se ejecutan en grupos que tienen que autenticarse con un punto de conexión. 

Si aún no tiene un certificado, puede crear un certificado autofirmado mediante la herramienta de línea de comandos `makecert`.

## <a name="upload-certificates-manually-through-the-azure-portal"></a>Carga manual de certificados mediante Azure Portal

1. Desde la cuenta de Batch a la que desea cargar un certificado, seleccione **Certificados** y, a continuación, seleccione **Agregar**. 

2. Cargue el certificado con una extensión .pfx o .cer. 

Una vez que se cargue, el certificado se agregará a la lista de certificados y usted podrá verificar la huella digital.

Ahora, cuando cree un grupo de Batch, podrá ir a Certificados dentro del grupo y asignar a ese grupo el certificado que cargó.

## <a name="next-steps"></a>Pasos siguientes

Batch tiene una API de certificado, [AZ batch certificate create](https://docs.microsoft.com/cli/azure/batch/certificate?view=azure-cli-latest#az-batch-certificate-create)

Para obtener información sobre el uso de Key Vault, consulte [Acceso seguro a Key Vault con Batch](credential-access-key-vault.md).
