---
title: CA permitidas para habilitar HTTPS personalizado en Azure CDN
description: Si utiliza su propio certificado para habilitar HTTPS en un dominio personalizado, debe utilizar una entidad de certificación permitida (CA) para crearlo.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 7b71611d43bc2d4de4c3e609462906c44fba0443
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919981"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Entidades de certificación permitidas para habilitar HTTPS personalizado en Azure CDN

Debe cumplir requisitos de certificado específicos cuando [habilite la característica HTTPS mediante su propio certificado](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#ssl-certificates) para un dominio personalizado de Azure Content Delivery Network (CDN). El perfil **Azure CDN Estándar de Microsoft** requiere un certificado de una de las entidades de certificación (CA) aprobadas en la lista siguiente. Si se utiliza un certificado de una entidad de certificación no aprobada o si se usa un certificado autofirmado, la solicitud se rechazará. Los perfiles **Azure CDN Estándar de Verizon** y **Azure CDN Prémium de Verizon** aceptan cualquier certificado válido de cualquier entidad de certificación válida.

> [!NOTE]
> La opción de usar su propio certificado para habilitar la característica HTTPS del dominio personalizado *no* está disponible para los perfiles **Azure CDN Estándar de Akamai**. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

