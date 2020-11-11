---
title: Protección de centros virtuales de Azure mediante Check Point Cloudguard Connect
description: Información sobre el uso de Check Point CloudGuard Connect para proteger centros virtuales de Azure
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 10/30/2020
ms.author: victorh
ms.openlocfilehash: 3c61dc689d19e1a7d6f9b6dbefae846e9458d750
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2020
ms.locfileid: "93146865"
---
# <a name="secure-virtual-hubs-using-check-point-cloudguard-connect"></a>Protección de centros virtuales con Check Point Cloudguard Connect

Check Point CloudGuard Connect es un asociado de seguridad de confianza en Azure Firewall Manager. Protege las conexiones distribuidas globalmente de sucursal a Internet (B2I) o de red virtual a Internet (V2I) con opciones avanzadas de prevención de amenazas. 

Con una sencilla configuración en Azure Firewall Manager, puede enrutar las conexiones de sucursal y de red virtual a Internet mediante la propuesta de seguridad como servicio (SECaaS) de CloudGuard Connect. El tráfico se protege mientras está en tránsito desde el centro de conectividad hasta el servicio en la nube de Check Point en túneles VPN con IPsec.

Al habilitar la sincronización automática en el portal de Check Point, cualquier recurso marcado como *protegido* en Azure Portal se protege automáticamente. No tiene que administrar los recursos dos veces; basta con que elija protegerlos una vez en Azure Portal.

Check Point engloba varios servicios de seguridad bajo un solo paraguas. El tráfico de seguridad integrado se descifra una vez y se inspecciona en un solo paso. El control de aplicaciones, el filtrado de direcciones URL y el reconocimiento de contenido (DLP) fuerzan un uso web seguro y protegen los datos. El sistema de prevención de intrusiones y el antivirus protegen a los usuarios frente a vulnerabilidades de red conocidas. La opción contra robots bloquea las conexiones a los servidores de comando y control, y le avisa si un host se infecta.

La emulación de amenazas (espacio seguro) protege a los usuarios frente a amenazas desconocidas y de día cero. Check Point Sandblast Zero-Day Protection es una tecnología de espacio seguro hospedada en la nube que rápidamente pone los archivos en cuarentena y los inspecciona. Se ejecuta en un espacio aislado virtual para detectar comportamientos malintencionados antes de acceder a la red. Evita las amenazas antes de que se produzcan los daños para ahorrar tiempo valioso al personal a la hora de responder a las amenazas. 

## <a name="deployment-example"></a>Ejemplo de implementación

Descubra en el siguiente vídeo cómo implementar Check Point CloudGuard Connect como asociado de seguridad de confianza de Azure.

> [!VIDEO https://www.youtube.com/embed/C8AuN76DEmU]

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de un proveedor de seguridad asociado](deploy-trusted-security-partner.md)