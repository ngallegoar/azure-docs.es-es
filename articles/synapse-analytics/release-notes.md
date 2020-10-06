---
title: 'Notas de la versión: Azure Synapse Analytics (versión preliminar de las áreas de trabajo)'
description: Notas de la versión para Azure Synapse Analytics (versión preliminar de las áreas de trabajo)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: overview
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: f2050bf671af35df4faec8b0b1d53f4c10a9075e
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2020
ms.locfileid: "90031677"
---
# <a name="azure-synapse-analytics-workspaces-preview-release-notes"></a>Notas de la versión de Azure Synapse Analytics (versión preliminar de las áreas de trabajo)

En este artículo se describen las limitaciones y los problemas con Azure Synapse Analytics (áreas de trabajo). Para más información, consulte [¿Qué es Azure Synapse Analytics (versión preliminar para áreas de trabajo)?](overview-what-is.md)

[!INCLUDE [preview](includes/note-preview.md)]

## <a name="azure-cli"></a>Azure CLI

- Problema e impacto en el cliente: Las áreas de trabajo creadas por el SDK no pueden iniciar Synapse Studio.

- Solución alternativa: Complete los pasos siguientes: 
  1.    Cree el área de trabajo mediante la ejecución de `az synapse workspace create`.
  2.    Extraiga el identificador de identidad administrada mediante la ejecución de `$identity=$(az synapse workspace show --name {workspace name}  --resource-group {resource group name} --query "identity.principalId")`.
  3.    Agregue el área de trabajo como rol a la cuenta de almacenamiento mediante la ejecución de ` az role assignment create --role "Storage Blob Data Contributor" --assignee-object-id {identity } --scope {storage account resource id}`.
  4.    Agregue la regla de firewall mediante la ejecución de ` az synapse firewall-rule create --name allowAll --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255 `.

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es Azure Synapse?](overview-what-is.md)
* [Primeros pasos](get-started.md)
* [P+F](overview-faq.md)
