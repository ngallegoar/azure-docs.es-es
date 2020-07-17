---
title: Filtrado de FQDN de Azure Firewall en reglas de red (versión preliminar)
description: Cómo usar el filtrado de FQDN de Azure Firewall en reglas de red
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: aef6fb52aaad3dd20db63f1d9023745169e84fcb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568283"
---
# <a name="fqdn-filtering-in-network-rules-preview"></a>Filtrado de FQDN en reglas de red (versión preliminar)

> [!IMPORTANT]
> El filtrado de FQDN en las reglas de red se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Un nombre de dominio completo (FQDN) representa un nombre de dominio de un host. Un nombre de dominio se asocia a una o varias direcciones IP. Puede permitir o bloquear los FQDN y las etiquetas de FQDN en las reglas de la aplicación. Con la configuración personalizada de DNS y proxy DNS, también puede usar el filtrado de FQDN en las reglas de red.

## <a name="how-it-works"></a>Funcionamiento

Azure Firewall traduce el FQDN a una o más direcciones IP mediante su configuración de DNS, y realiza el procesamiento de las reglas en función de Azure DNS o de una configuración de DNS personalizada.

Para usar FQDN en las reglas de red, debe habilitar el proxy DNS. Si no habilita el proxy DNS, el procesamiento de reglas confiables estará en riesgo. Cuando el tráfico DNS está habilitado, se dirige a Azure Firewall, donde puede configurar el servidor DNS personalizado. A continuación, el firewall y los clientes usan el mismo servidor DNS configurado. Si el proxy DNS no está habilitado, Azure Firewall puede generar una respuesta diferente, ya que el cliente y el firewall pueden usar servidores distintos para la resolución de nombres. El filtrado de FQDN en las reglas de red puede ser incorrecto o incoherente si el cliente y el firewall reciben respuestas de DNS diferentes.

Puede optar por invalidar este requisito si reconoce el riesgo antes de seleccionar **Guardar** en la colección de reglas.

## <a name="next-steps"></a>Pasos siguientes

[Configuración DNS de Azure Firewall](dns-settings.md)
