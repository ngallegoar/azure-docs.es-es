---
title: CA permitidas para habilitar HTTPS personalizado en Azure CDN
description: Si utiliza su propio certificado para habilitar HTTPS en un dominio personalizado, debe utilizar una entidad de certificación permitida (CA) para crearlo.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/18/2018
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 29b6cb25e021e86ce6663b4db5c89217aaf70a37
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84887399"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Entidades de certificación permitidas para habilitar HTTPS personalizado en Azure CDN

Debe cumplir requisitos de certificado específicos cuando [habilite la característica HTTPS mediante su propio certificado](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) para un dominio personalizado de Azure Content Delivery Network (CDN). El perfil **Azure CDN Estándar de Microsoft** requiere un certificado de una de las entidades de certificación (CA) aprobadas en la lista siguiente. Si se utiliza un certificado de una entidad de certificación no aprobada o si se usa un certificado autofirmado, la solicitud se rechazará. Los perfiles **Azure CDN Estándar de Verizon** y **Azure CDN Prémium de Verizon** aceptan cualquier certificado válido de cualquier entidad de certificación válida.

> [!NOTE]
> La opción de usar su propio certificado para habilitar la característica HTTPS del dominio personalizado *no* está disponible para los perfiles **Azure CDN Estándar de Akamai**. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

