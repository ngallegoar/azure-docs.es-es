---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/15/2020
ms.author: cephalin
ms.openlocfilehash: 0fccb1ed092190f88d197c9bf2cd17678a4d1731
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649029"
---
Los componentes de plataforma de App Service, incluidas las máquinas virtuales de Azure, el almacenamiento, las conexiones de red, las plataformas web y las características de administración e integración se protegen y refuerzan activamente. App Service realiza exhaustivas comprobaciones de cumplimiento de forma continua para asegurarse de que:

- Los recursos de aplicación estén [protegidos](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) de los recursos de Azure de otros clientes.
- [Las instancias de máquina virtual y el software en tiempo de ejecución se actualicen periódicamente](../articles/app-service/overview-patch-os-runtime.md) para abordar puntos vulnerables recién detectados. 
- La comunicación de secretos (por ejemplo, cadenas de conexión) entre su aplicación y otros recursos de Azure (por ejemplo, [SQL Database](https://azure.microsoft.com/services/sql-database/)) permanezca dentro de Azure y no cruce ningún límite de la red. Los secretos siempre se cifren al guardarlos.
- Todas las comunicaciones que se realicen con las características de conectividad de App Service, como la [conexión híbrida](../articles/app-service/app-service-hybrid-connections.md), se cifren. 
- Todas las conexiones con herramientas de administración remota como Azure PowerShell, la CLI de Azure, los SDK de Azure o las API REST, se cifren.
- La administración ininterrumpida de amenazas proteja la infraestructura y la plataforma frente a malware, ataques por denegación de servicio distribuido (DDoS), ataques de tipo "Man in the middle" (MITM) y otras amenazas.

Para más información sobre la seguridad de la infraestructura y la plataforma en Azure, consulte [Centro de confianza de Microsoft Azure](https://azure.microsoft.com/overview/trusted-cloud/).