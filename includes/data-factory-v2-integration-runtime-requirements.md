---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 07/13/2020
ms.author: jingwang
ms.openlocfilehash: 994b8285999bfa52e2aea9a57ad832aefddcfb76
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544690"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Si el almacén de datos se encuentra en una red local, una red virtual de Azure o una nube privada virtual de Amazon, debe configurar un [entorno de ejecución de integración autohospedado](../articles/data-factory/create-self-hosted-integration-runtime.md) para conectarse a él.

Si el almacén de datos es un servicio administrado de datos en la nube en el que el acceso está restringido a direcciones IP permitidas por las reglas del firewall, puede usar el entorno de ejecución de integración de Azure y agregar [sus direcciones IP](../articles/data-factory/azure-integration-runtime-ip-addresses.md) a la lista de direcciones permitidas. 

Consulte [Estrategias de acceso a datos](../articles/data-factory/data-access-strategies.md) para más información sobre los mecanismos de seguridad de red que admite Data Factory para acceder a los almacenes de datos en general.
