---
title: Requisitos del sistema de Microsoft Azure Data Box | Microsoft Docs
description: Obtenga información sobre requisitos del sistema importantes tanto de Azure Data Box como de los clientes que se conectan a Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 10/02/2020
ms.author: alkohli
ms.openlocfilehash: 5f1623ef4dde59e816e3afe5a5f5894c49469580
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91767882"
---
# <a name="azure-data-box-system-requirements"></a>Requisitos del sistema de Azure Data Box

En este artículo se describen requisitos del sistema importantes tanto de Microsoft Azure Data Box como de los clientes de que se conectan a Data Box. Se recomienda leer detenidamente la información antes de implementar Data Box y volver a consultarla según sea necesario durante la implementación y el funcionamiento.

Los requisitos del sistema incluyen:

* **Requisitos de software:** en el caso de los hosts que se conectan a Data Box, se describen los sistemas operativos, protocolos de transferencia de archivos, cuentas de almacenamiento, tipos de almacenamiento y exploradores compatibles con la interfaz de usuario web local.
* **Requisitos de red:** en el caso de Data Box, se describen los requisitos de los puertos y la conexión de red para un funcionamiento óptimo de Data Box.


## <a name="software-requirements"></a>Requisitos de software

Entre los requisitos de software se incluyen los sistemas operativos, protocolos de transferencia de archivos, cuentas de almacenamiento, tipos de almacenamiento y exploradores compatibles con la interfaz de usuario web local.

### <a name="supported-operating-systems-for-clients"></a>Sistemas operativos compatibles para clientes

[!INCLUDE [data-box-supported-os-clients](../../includes/data-box-supported-os-clients.md)]


### <a name="supported-file-transfer-protocols-for-clients"></a>Protocolos de transferencia de archivos compatibles para clientes

[!INCLUDE [data-box-supported-file-systems-clients](../../includes/data-box-supported-file-systems-clients.md)]

> [!IMPORTANT] 
> No se admite la conexión a recursos compartidos de Data Box a través de REST para los pedidos de exportación. 

### <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

[!INCLUDE [data-box-supported-storage-accounts](../../includes/data-box-supported-storage-accounts.md)]

### <a name="supported-storage-types"></a>Tipos de almacenamiento compatibles

[!INCLUDE [data-box-supported-storage-types](../../includes/data-box-supported-storage-types.md)]

### <a name="supported-web-browsers"></a>Exploradores web compatibles

[!INCLUDE [data-box-supported-web-browsers](../../includes/data-box-supported-web-browsers.md)]

## <a name="networking-requirements"></a>Requisitos de red

El centro de datos debe tener una red de alta velocidad. Es muy recomendable tener una conexión de 10 GbE como mínimo. Si no hay disponible una conexión 10 GbE, se puede usar un vínculo de datos de 1 GbE para copiar datos, pero la velocidad de copia se verá afectada.

### <a name="port-requirements"></a>Requisitos de puerto

En la siguiente tabla se enumeran los puertos que deben abrirse en el firewall para permitir el tráfico de SMB o NFS. En esta tabla, *dentro* (*entrante*) hace referencia a la dirección desde la que el cliente entrante solicita acceso al dispositivo. *Fuera* (o *saliente*) hace referencia a la dirección en la que el dispositivo de Data Box envía datos externamente después de la implementación: por ejemplo, saliente a Internet.

[!INCLUDE [data-box-port-requirements](../../includes/data-box-port-requirements.md)]


## <a name="next-steps"></a>Pasos siguientes

* [Implementación de Azure Data Box](data-box-deploy-ordered.md)
