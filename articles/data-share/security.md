---
title: Información general de seguridad para Azure Data Share
description: Información general de seguridad para Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 06/05/2020
ms.openlocfilehash: 10f31b74b461941b15f13e45f90b5fbc408c90fe
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108420"
---
# <a name="security-overview-for-azure-data-share"></a>Información general de seguridad para Azure Data Share

En este artículo se proporciona una información general de la seguridad del servicio de Azure Data Share.

## <a name="security-overview"></a>Introducción a la seguridad

Azure Data Share aprovecha la seguridad subyacente que ofrece Azure para proteger los datos tanto en reposo como en tránsito. Los datos se cifran en reposo, si el almacén de datos subyacente admite esta opción. También se cifran en tránsito. Los metadatos de un recurso compartido de datos también se cifran tanto en reposo como en tránsito. 

Se pueden establecer controles de acceso en el nivel de recursos de Azure Data Share para asegurarse de que solo acceden los que tienen autorización. 

Azure Data Share aprovecha la identidad administrada (anteriormente conocida como MSI) para tener acceso a los almacenes de datos que sirven para el uso compartido de datos. No hay intercambio de credenciales entre un proveedor de datos y un consumidor de datos. Para obtener más información sobre las identidades administradas, consulte [Identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities). Para obtener más información sobre los roles y los permisos necesarios para compartir datos, consulte [Roles y requisitos](concepts-roles-permissions.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).




