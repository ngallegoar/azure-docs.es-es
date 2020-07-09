---
title: Configuración de zonas de búsqueda inversa para la comprobación de un banner de SMTP
titlesuffix: Azure Virtual Network
description: Describe cómo configurar zonas de búsqueda inversa para la comprobación de un banner de SMTP en Azure
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: dcscontentpm
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c8ffadb8d54db0c2a99dc12e45b5990155a0505e
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135065"
---
# <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Configuración de zonas de búsqueda inversa para la comprobación de un banner de SMTP

Este artículo describe cómo usar una zona inversa en Azure DNS y crear un registro DNS inverso (PTR) para la comprobación del banner de SMTP.

## <a name="symptom"></a>Síntoma

Si hospeda un servidor SMTP en Microsoft Azure, puede que reciba el siguiente mensaje de error al enviar o recibir un mensaje de servidores de correo remotos:

**554: no hay ningún registro PTR**

## <a name="solution"></a>Solución

Para una dirección IP virtual de Azure, los registros inversos se crean en zonas de dominio propiedad de Microsoft y no en las zonas de dominios personalizados.

Para configurar registros PTR en zonas propiedad de Microsoft, use la propiedad -ReverseFqdn en el recurso PublicIpAddress. Para obtener más información, consulte [Configuración de DNS inversos para servicios hospedados en Azure](../dns/dns-reverse-dns-for-azure-services.md).

Al configurar registros PTR, asegúrese de que la dirección IP y el FQDN inverso pertenecen a la suscripción. Si intenta establecer un FQDN inverso que no pertenece a la suscripción, recibirá el siguiente mensaje de error:

```output
Set-AzPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership:
                    
1) ReverseFqdn matches fqdn of any public ip resource under the subscription;
2) ReverseFqdn resolves to the fqdn (through CName records chain) of any public ip resource under the subscription;
3) It resolves to the ip address (through CName and A records chain) of a static public ip resource under the subscription.
```

Si cambia manualmente el banner de SMTP para que coincida con el FQDN inverso predeterminado, el servidor de correo remoto puede seguir generando errores porque es posible que espere que el host del banner de SMTP coincida con el registro MX para el dominio.
