---
title: Administración de protocolos y cifrados en Azure API Management | Microsoft Docs
description: Obtenga información sobre cómo administrar protocolos (TLS) y cifrados (DES) en Azure API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/29/2019
ms.author: apimpm
ms.openlocfilehash: 7f87389016286c9f1b91abc77155be9e94005371
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335866"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Administración de protocolos y cifrados en Azure API Management

Azure API Management admite varias versiones del protocolo TLS para los lados cliente y back-end, así como el cifrado 3DES.

Esta guía muestra cómo administrar la configuración de los protocolos y cifrados en una instancia de Azure API Management.

![Administración de protocolos y cifrados en APIM](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

## <a name="prerequisites"></a>Prerrequisitos

Para seguir los pasos de este artículo, debe tener:

* Una instancia de API Management

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>Cómo administrar protocolos TLS y el cifrado 3DES

1. Vaya a la instancia de **API Management** en Azure Portal.
2. Seleccione **Configuración del protocolo** en el menú.  
3. Habilite o deshabilite los protocolos o los cifrados que quiera.
4. Haga clic en **Save**(Guardar). Los cambios se aplicarán en el plazo de una hora.  

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [TLS (Seguridad de la capa de transporte)](https://docs.microsoft.com/dotnet/framework/network-programming/tls).
* Consulte más [vídeos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) sobre la administración de API.