---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 2e90d218aa6dc90746ba0e928fb3393f0bdb5e5a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "68966382"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Si el almacén de datos está configurado de una de las siguientes maneras, debe configurar un [entorno de ejecución de integración autohospedado](../articles/data-factory/create-self-hosted-integration-runtime.md) para conectarse a este almacén de datos:

- El almacén de datos se encuentra dentro de una red local, dentro de Azure Virtual Network o dentro de Amazon Virtual Private Cloud.
- El almacén de datos es un servicio administrado de datos en la nube en el que el acceso está restringido a direcciones IP permitidas por las reglas del firewall.
