---
title: CA permitidas para habilitar HTTPS personalizado en Azure Front Door
description: Si utiliza su propio certificado para habilitar HTTPS en un dominio personalizado, debe utilizar una entidad de certificación permitida (CA) para crearlo.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: sharadag
ms.openlocfilehash: 611f5730afed4c3a84b81d6acfd33b633c532bbc
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874677"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Entidades de certificación permitidas para habilitar HTTPS personalizado en Azure Front Door

Para un dominio personalizado de Azure Front Door, al [habilitar la característica HTTPS mediante su propio certificado](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), debe usar una entidad de certificación (CA) permitida para crear el certificado TLS/SSL. De lo contrario, si usa una entidad de certificación no permitida o un certificado autofirmado, la solicitud se rechazará.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
