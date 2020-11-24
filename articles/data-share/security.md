---
title: Información general de seguridad para Azure Data Share
description: Información general de seguridad para Azure Data Share
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: 1fdf026e9271ef6eb30c2b4ca96a04880b65be75
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578100"
---
# <a name="security-overview-for-azure-data-share"></a>Información general de seguridad para Azure Data Share

En este artículo se proporciona una información general de la seguridad del servicio de Azure Data Share.

## <a name="security-overview"></a>Introducción a la seguridad

Azure Data Share aprovecha la seguridad subyacente que ofrece Azure para proteger los datos tanto en reposo como en tránsito. Los datos se cifran en reposo, si el almacén de datos subyacente admite esta opción. Los datos también se cifran en tránsito mediante TLS 1.2. Los metadatos de un recurso compartido de datos también se cifran tanto en reposo como en tránsito. Azure Data Share no almacena el contenido de los datos del cliente que se esté compartiendo.

Azure Data Share aprovecha la identidad administrada (anteriormente conocida como MSI) para tener acceso a los almacenes de datos que sirven para el uso compartido de datos. No hay intercambio de credenciales entre un proveedor de datos y un consumidor de datos. Para obtener más información sobre las identidades administradas, consulte [Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md). Para obtener más información sobre los roles y los permisos necesarios para compartir datos, consulte [Roles y requisitos](concepts-roles-permissions.md).

Se pueden establecer controles de acceso a Azure Data Share en el nivel de recursos para asegurarse de que solo se accede con autorización. 

## <a name="share-data-from-or-to-data-stores-with-firewall-enabled"></a>Datos compartidos de almacenes de datos o con estos con el firewall habilitado
Para compartir datos con cuentas de almacenamiento o de estas con el firewall activado, debe habilitar **Permitir servicios de Microsoft de confianza** en la cuenta de almacenamiento. Consulte [Configuración de redes virtuales y firewalls de Azure Storage](
https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services) para más información.


## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).
