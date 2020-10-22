---
title: Información general de seguridad para Azure Data Share
description: Información general de seguridad para Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 06/05/2020
ms.openlocfilehash: 5d815c27ecc7825f0bc1e6772654b094a799b63d
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92216562"
---
# <a name="security-overview-for-azure-data-share"></a>Información general de seguridad para Azure Data Share

En este artículo se proporciona una información general de la seguridad del servicio de Azure Data Share.

## <a name="security-overview"></a>Introducción a la seguridad

Azure Data Share aprovecha la seguridad subyacente que ofrece Azure para proteger los datos tanto en reposo como en tránsito. Los datos se cifran en reposo, si el almacén de datos subyacente admite esta opción. También se cifran en tránsito. Los metadatos de un recurso compartido de datos también se cifran tanto en reposo como en tránsito. 

Se pueden establecer controles de acceso en el nivel de recursos de Azure Data Share para asegurarse de que solo acceden los que tienen autorización. 

Azure Data Share aprovecha la identidad administrada (anteriormente conocida como MSI) para tener acceso a los almacenes de datos que sirven para el uso compartido de datos. No hay intercambio de credenciales entre un proveedor de datos y un consumidor de datos. Para obtener más información sobre las identidades administradas, consulte [Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md). Para obtener más información sobre los roles y los permisos necesarios para compartir datos, consulte [Roles y requisitos](concepts-roles-permissions.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).