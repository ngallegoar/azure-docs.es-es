---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 0b03957178af0578d2c6cd91d7377c93f413cec3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84629539"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Si el almacén de datos está configurado de una de las siguientes maneras, debe configurar un [entorno de ejecución de integración autohospedado](../articles/data-factory/create-self-hosted-integration-runtime.md) para conectarse al almacén de datos:

- El almacén de datos se encuentra dentro de una red local, dentro de una red virtual de Azure o dentro de Amazon Virtual Private Cloud.
- El almacén de datos es un servicio administrado de datos en la nube en el que el acceso está restringido a direcciones IP permitidas por las reglas del firewall.
