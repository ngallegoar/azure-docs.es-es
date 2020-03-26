---
title: Qué es Azure Blockchain Tokens
description: Azure Blockchain Tokens es una plataforma como servicio (PaaS) para la emisión y administración de tokens.
ms.date: 11/04/2019
ms.topic: overview
ms.reviewer: brendal
ms.openlocfilehash: e7c106ffee8ea06c7c0ce738aa8be0aba96a38ed
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79215093"
---
# <a name="what-is-azure-blockchain-tokens"></a>¿Qué es Azure Blockchain Tokens?

[!INCLUDE [Preview note](./includes/preview.md)]

Azure Blockchain Tokens es una plataforma como servicio (PaaS) para la emisión y administración estandarizadas de tokens en los libros de cadenas de bloques de Azure.

Mediante Azure Blockchain Tokens, puede crear tokens estandarizados para su solución de cadena de bloques mediante una plantilla de tokens generada previamente. También puede crear su propia plantilla de tokens mediante el servicio. Una vez creada, use Azure Blockchain Tokens para conectarse y emitir los tokens en una cadena de bloques. Una vez emitidos, puede administrar los tokens en varias redes de cadenas de bloques.

## <a name="templates"></a>Plantillas

Use Azure Blockchain Tokens para seleccionar una plantilla de tokens generada previamente o para crear su propia plantilla de tokens. Azure Blockchain Tokens admite la composición de la plantilla de tokens que le permite crear su propia plantilla de tokens basada en comportamientos admitidos. Las plantillas de tokens se pueden usar para la mayoría de las soluciones de cadena de bloques, ya que se asignan a los tokens que se usan con más frecuencia. Puede empezar con una plantilla, personalícela e implementa los tokens de la solución.

Para obtener más información sobre Azure Blockchain Tokens, consulte [Plantillas de Azure Blockchain Tokens](templates.md).

## <a name="management"></a>Administración

Azure Blockchain Tokens proporciona administración de Azure Portal y API para conectarse a una red de cadenas de bloques existente. Actualmente, puede conectarse a [Azure Blockchain Service](../service/overview.md) u otra cadena de bloques de la familia Ethereum.

Una vez conectado a una o varias redes de cadenas de bloques, puede usar las API de Azure Blockchain Tokens para emitir y administrar tokens para su uso en la solución de cadena de bloques. Con las API, puede integrar la administración de tokens en su lógica y aplicaciones empresariales. Por ejemplo, puede usar la API de REST para administrar tokens en lugar de administrar tokens directamente en la cadena de bloques.

## <a name="blockchains-and-accounts"></a>Cadenas de bloques y cuentas

Azure Blockchain Tokens proporciona administración de Azure Portal y API para crear nuevos grupos y nuevas cuentas de cadenas de bloques en redes de cadenas de bloques conectadas. Puede crear nuevas cuentas directamente en las redes conectadas, Azure Blockchain Tokens administra las claves privadas de la cuenta en su nombre. Mediante el uso de grupos, puede agrupar distintas cuentas de cadena de bloques de varias redes y administrar el control de acceso a través de los grupos.

Para obtener más información sobre la administración de cuentas de Azure Blockchain Tokens, consulte [Azure Blockchain Tokens account management](account-management.md) (Administración de cuentas de Azure Blockchain Tokens).

## <a name="token-taxonomy-framework"></a>Marco de taxonomía de tokens

Azure Blockchain Tokens se ha creado sobre una base con estándares denominada marco de taxonomía de tokens (TTF). TTF es un conjunto de entregas creado a partir del grupo de trabajo de [Token Taxonomy Initiative](https://entethalliance.org/participate/token-taxonomy-initiative/) (TTI). El grupo de trabajo de TTI define una taxonomía empresarial para los tokens y sus comportamientos que se pueden aplicar en todos los libros principales, incluidos Ethereum, Quorum, Corda y Hyperledger Fabric. El objetivo del grupo de trabajo es crear un marco que estandarice el uso de tokens desde una perspectiva empresarial para impulsar la simplificación y democratizar el desarrollo basado en tokens. Al permitir que el sector defina estos tokens y su comportamiento al nivel empresarial, la implementación detallada de los tokens se abstrae de la lógica de negocios que manipula los tokens.

## <a name="support-and-feedback"></a>Soporte y comentarios

Para leer noticias acerca de Azure Blockchain, visite el [blog de Azure Blockchain](https://azure.microsoft.com/blog/topics/blockchain/), que le permitirá mantenerse al día sobre las ofertas de servicio de Blockchain y le proporcionará información del equipo de ingeniería de Azure Blockchain.

Para obtener acceso a la versión preliminar de Azure Blockchain Tokens, [póngase en contacto con el equipo de Azure Blockchain Tokens](https://aka.ms/PreviewForm).

Para proporcionar comentarios sobre el producto o solicitar nuevas características, realice una publicación o vote para una idea a través del [Foro de comentarios de Azure para Blockchain](https://aka.ms/blockchainuservoice).

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de las [plantillas de Azure Blockchain Tokens](templates.md) disponibles.
