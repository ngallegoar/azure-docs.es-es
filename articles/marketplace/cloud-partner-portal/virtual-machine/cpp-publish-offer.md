---
title: Publicar una oferta de máquina virtual en Azure Marketplace
description: Se enumeran los pasos necesarios para publicar una oferta de máquina virtual existente en Azure Marketplace.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: dsindona
ms.openlocfilehash: c35f721131b997dcfdb0f23a91a39329168b757c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277351"
---
# <a name="publish-a-virtual-machine-offer"></a>Publicación de una oferta de máquina virtual

 El último paso, después de haber definido la oferta en el portal y creado los recursos técnicos asociados, consiste en enviar la oferta para la publicación. En el diagrama siguiente se describen los pasos principales del proceso de publicación para el "lanzamiento":

![Pasos de publicación para la oferta de máquina virtual](./media/publishvm_013.png)

En la tabla siguiente se describen estos pasos y se proporciona una estimación del tiempo máximo para su finalización:
<!-- we need to tell them that if an offer seems stuck in a step, to know that they should file a support ticket (link to support ticket doc) -->


|  **Paso de publicación**           | **Time**    | **Descripción**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Validar requisitos previos         | 15 minutos   | Se validan la información y la configuración de la oferta.                        |
| Validación de la versión de prueba (opcional) | 2 horas | Si ha seleccionado habilitar Versión de prueba, Microsoft valida la configuración de la versión de prueba, su implementación y la replicación a través de las regiones seleccionadas. |
| Certificación                  | 3 días | El equipo de certificación de Azure analiza la oferta. En este paso se llevan a cabo exámenes de virus, malware, cumplimiento de seguridad y problemas de seguridad. Si se detecta un problema, se proporcionan comentarios. |
| Aprovisionamiento                   | 4 días   | La oferta de máquina virtual se replica en los sistemas de producción de Marketplace.               |
| Empaquetado y registro de la generación de clientes potenciales | \< 1 hora  | Los recursos técnicos de la oferta se empaquetan para uso del cliente y se configuran los sistemas del cliente potencial. |
|  Aprobación del publicador             |  -        | Revisión final del publicador y confirmación antes del lanzamiento de la oferta. Puede implementar la oferta en las suscripciones seleccionadas (en los pasos de información de la oferta) para comprobar que cumple todos los requisitos.  |
| Aprovisionamiento                   | 4 días | La oferta de máquina virtual terminada se replica en las regiones y los sistemas de producción de Marketplace. | 
| En vivo                           | 4 días | La oferta de máquina virtual se lanza, se replica en las regiones necesarias y se pone a disposición del público. |
|  |  |

Espere hasta 16 días a que el proceso se complete.  Después de completar estos pasos de publicación, la oferta de máquina virtual se mostrará en [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). 

