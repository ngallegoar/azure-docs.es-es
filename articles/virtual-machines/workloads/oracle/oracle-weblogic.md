---
title: Aplicaciones de Azure de WebLogic Server | Microsoft Docs
description: Obtenga información sobre cómo ejecutar Oracle WebLogic Server en Microsoft Azure.
services: virtual-machines-linux
documentationcenter: ''
author: edburns
manager: gwallace
tags: azure-resource-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2020
ms.author: edburns
ms.openlocfilehash: 9ba4e3a66ae8ef71b39aa26fd54048381237c2fa
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83664053"
---
# <a name="oracle-weblogic-server-azure-applications"></a>Aplicaciones de Azure de Oracle WebLogic Server

## <a name="oracle-weblogic-server-is-a-scalable-enterprise-ready-java-ee-application-server"></a>Oracle WebLogic Server es un servidor de aplicaciones de Java EE escalable y listo para la empresa.

Oracle WebLogic Server es el primer servidor de aplicaciones de la plataforma Java nativo de nube del mundo para desarrollar e implementar aplicaciones empresariales distribuidas de varios niveles. Las ofertas de Azure WebLogic Server le permiten adoptar la informática en la nube al proporcionar una mayor variedad de opciones y flexibilidad para la migración de WebLogic, incluida la migración mediante "lift-and-shift" de las aplicaciones de Java EE a Azure Cloud con el mínimo esfuerzo y el máximo impacto. Las ofertas le permiten iniciar rápidamente sus aplicaciones empresariales mediante el aprovisionamiento automático de la red virtual, los recursos de Linux y almacenamiento, la instalación de WebLogic Server, la configuración de seguridad con un grupo de seguridad de red, el equilibrio de carga con Azure App Gateway, la autenticación con Azure Active Directory y la simplificación de la conectividad de base de datos.

Hay cuatro ofertas disponibles para satisfacer distintos escenarios: un solo nodo sin un servidor de administración, un solo nodo con un servidor de administración, un clúster y un clúster dinámico.  No dude en probarlas, ya que las ofertas están disponibles de forma gratuita.

_Estas ofertas son de tipo "traiga su propia licencia"_ . Se supone que ya ha adquirido las licencias adecuadas con Oracle y que tiene licencias adecuadas para ejecutar ofertas en Microsoft Azure.

_Si desea trabajar estrechamente en los escenarios de migración con el equipo de ingeniería que desarrolla estas ofertas, solo tiene que pulsar el botón [PONERSE EN CONTACTO CONMIGO](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview)_ en [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/oracle.oraclelinux-wls-cluster?tab=Overview). Los administradores de programas, los arquitectos y los ingenieros se pondrán en contacto con usted en breve para iniciar la colaboración.

### <a name="oracle-weblogic-server-single-node"></a>Nodo único de Oracle WebLogic Server

Esta oferta aprovisiona una única máquina virtual e instala Oracle WebLogic Server en esta. No crea un dominio ni inicia el servidor de administración. Resulta útil en escenarios con una configuración de dominio muy personalizada.

### <a name="oracle-weblogic-server-with-admin-server"></a>Oracle WebLogic Server con servidor de administración

Esta oferta aprovisiona una única máquina virtual e instala Oracle WebLogic Server en esta. Crea un dominio e inicia el servidor de administración, lo que le permite administrar el dominio.

### <a name="oracle-weblogic-server-cluster"></a>Clúster de Oracle WebLogic Server

Esta oferta crea un clúster de alta disponibilidad de máquinas virtuales de Oracle WebLogic Server. El servidor de administración y todos los servidores administrados se inician de forma predeterminada, lo que le permite administrar el dominio.

### <a name="oracle-weblogic-server-dynamic-cluster"></a>Clúster dinámico de Oracle WebLogic Server

Esta oferta crea un clúster dinámico escalable y de alta disponibilidad de máquinas virtuales de Oracle WebLogic Server. El servidor de administración y todos los servidores administrados se inician de forma predeterminada, lo que le permite administrar el dominio.

## <a name="next-steps"></a>Pasos siguientes

Explore las ofertas en Azure Marketplace.

> [!div class="nextstepaction"]
> [Nodo único de Oracle WebLogic Server](https://portal.azure.com/#create/oracle.20191001-arm-oraclelinux-wls20191001-arm-oraclelinux-wls)

> [!div class="nextstepaction"]
> [Oracle WebLogic Server con servidor de administración](https://portal.azure.com/#create/oracle.20191009-arm-oraclelinux-wls-admin20191009-arm-oraclelinux-wls-admin)

> [!div class="nextstepaction"]
> [Clúster de Oracle WebLogic Server](https://portal.azure.com/#create/oracle.20191007-arm-oraclelinux-wls-cluster20191007-arm-oraclelinux-wls-cluster)

> [!div class="nextstepaction"]
> [Clúster dinámico de Oracle WebLogic Server](https://portal.azure.com/#create/oracle.20191021-arm-oraclelinux-wls-dynamic-cluster20191021-arm-oraclelinux-wls-dynamic-cluster)
