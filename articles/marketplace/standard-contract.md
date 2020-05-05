---
title: Contrato estándar | Azure Marketplace
description: Contrato estándar para Azure Marketplace y AppSource
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: dsindona
ms.openlocfilehash: facb500299f7196e0e1387c3a7267a917d13a3a6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81681448"
---
# <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Contrato estándar para el Marketplace comercial de Microsoft

Para simplificar el proceso de adquisición para los clientes y reducir la complejidad legal para los proveedores de software, Microsoft ofrece un Contrato estándar para el Marketplace comercial de Microsoft con el fin de ayudar a facilitar una transacción en Marketplace. En lugar de crear términos y condiciones personalizados, los anunciantes del Marketplace comercial pueden ofrecer su software en virtud del Contrato estándar, que los clientes solo tienen que revisar y aceptar una vez. El Contrato estándar puede encontrarse aquí: [https://go.microsoft.com/fwlink/?linkid=2041178](https://go.microsoft.com/fwlink/?linkid=2041178).

Los términos y condiciones de una oferta se definen al crearla en el Centro de partners. Puede optar por usar el Contrato estándar para el Marketplace comercial de Microsoft en lugar de proporcionar sus propios términos y condiciones personalizados.

>[!Note]
>Después de publicar una oferta con el Contrato estándar para el Marketplace comercial de Microsoft, no puede usar sus propios términos y condiciones personalizados. Solo puede elegir una de las dos opciones. Puede ofrecer su solución con el Contrato estándar *o* con sus propios términos y condiciones. Si quiere modificar los términos del Contrato estándar, puede hacerlo a través de las modificaciones del Contrato estándar.

## <a name="standard-contract-amendments"></a>Modificaciones del Contrato estándar

Las modificaciones del Contrato estándar permiten a los anunciantes seleccionar el Contrato estándar por motivos de simplicidad y personalizar los términos para su producto o empresa. Los clientes solo necesitan revisar las modificaciones del contrato, si ya han revisado y aceptado el Contrato estándar de Microsoft.

Hay dos tipos de modificaciones para los anunciantes de Marketplace comercial:

* Modificaciones universales: Estas modificaciones se aplican universalmente al Contrato estándar para todos los clientes. Las modificaciones universales se muestran a cada cliente de la oferta en el flujo de compra. Los clientes deben aceptar los términos del Contrato estándar y la modificación antes de que puedan usar su oferta.

* Modificaciones personalizadas: Estas son modificaciones especiales al Contrato estándar que están dirigidas a cliente únicamente a clientes específicos a través de los Id. de inquilino de Azure. Los anunciantes pueden elegir al inquilino al que desean dirigirse. Solo los clientes del inquilino recibirán los términos con modificaciones personalizadas en el flujo de compra de la oferta.  Los clientes deben aceptar los términos del Contrato estándar y las modificaciones antes de que puedan usar su oferta.

>[!Note]
>Estos dos tipos de modificaciones se superponen entre sí. Los clientes específicos con modificaciones personalizadas también obtendrán la modificación universal al Contrato estándar durante la compra.

Puede aprovechar el Contrato estándar del Marketplace comercial de Microsoft para los siguientes tipos de ofertas:  Las aplicaciones de Azure (plantillas de solución y aplicaciones administradas), máquinas virtuales, contenedores, aplicaciones de contenedor, módulos de IoT Edge y SaaS.

## <a name="customer-experience"></a>Experiencia del cliente

Durante la experiencia de búsqueda en Azure Marketplace o AppSource, los clientes podrán ver los términos asociados con la oferta en la forma de un Contrato estándar para el Marketplace comercial de Microsoft y todas las modificaciones universales.

![Experiencia de búsqueda del cliente de Azure Portal.](media/marketplace-publishers-guide/azure-discovery-process.png)

Durante el proceso de compra en Azure Portal, los clientes podrán ver los términos asociados con la oferta en la forma de un Contrato estándar para el Marketplace comercial de Microsoft y todas las modificaciones universales o específicas de un inquilino.

![Experiencia de compra para el cliente de Azure Portal.](media/marketplace-publishers-guide/azure-purchase-process.png)

## <a name="api"></a>API

Los clientes pueden usar Get-AzureRmMarketplaceTerms para recuperar los términos de una oferta y aceptarlos. El Contrato estándar y las modificaciones asociadas se devolverán en la salida del cmdlet.
