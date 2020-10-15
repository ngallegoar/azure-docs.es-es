---
title: Introducción a TmaxSoft OpenFrame en Azure Virtual Machines
description: Rehospede las cargas de trabajo del sistema central IBM z/OS con el entorno TmaxSoft OpenFrame en Azure Virtual Machines (VM).
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.openlocfilehash: 408e0166e52af9efd3d4c64f1b29bddcfc1cca4c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "61485542"
---
# <a name="get-started-with-tmaxsoft-openframe-on-azure"></a>Introducción a TmaxSoft OpenFrame en Azure

Aproveche sus recursos de sistema central existentes y muévalos a Microsoft Azure con TmaxSoft OpenFrame. Esta solución de rehospedaje popular crea un entorno de emulación en Azure, que le permite migrar aplicaciones rápidamente. No es necesario volver a formatear nada.

## <a name="openframe-rehosting-environment"></a>Entorno de rehospedaje OpenFrame

Configure un entorno OpenFrame en Azure para cargas de trabajo de desarrollo, demostraciones, pruebas o producción. Como se muestra en la figura siguiente, OpenFrame incluye varios componentes que crean el entorno de emulación del sistema central en Azure. Por ejemplo, los servicios en línea de OpenFrame reemplazan el middleware del sistema central, como IBM Customer Information Control System (CICS). OpenFrame Batch, con su componente TJES, reemplaza el subsistema de entrada de trabajo (JES) del sistema central de IBM. 

![Proceso de rehospedaje de OpenFrame](media/openframe-01.png)

> [!NOTE]
> Para ejecutar el entorno OpenFrame en Azure, debe tener una licencia de producto válida o una licencia de prueba de TmaxSoft.

## <a name="openframe-components"></a>Componentes de OpenFrame

Los siguientes componentes forman parte del entorno OpenFrame en Azure:

- **Herramientas de migración**, que incluyen OFMiner, una solución que analiza los recursos de los sistemas centrales y después los migra a Azure.
- **Compiladores**, incluidos OFCOBOL, un compilador que interpreta los programas COBOL del sistema central; OFPLI, que interpreta los programas PL/I del sistema central; y OFASM, un compilador que interpreta los programas de ensamblador de sistema central.
- Componentes de **front-end**, que incluyen la solución para usuarios de Java Enterprise (JEUS), un servidor de aplicaciones web certificado para Java Enterprise Edition 6.OFGW y el componente de puerta de enlace de OpenFrame que proporciona un cliente de escucha 3270.
- Entorno de **aplicación**. OpenFrame Base es el middleware que administra todo el sistema. El servidor OpenFrame del tipo C (OSC) reemplaza el middleware del sistema central y el CICS de IBM.
- **Base de datos relacional**, como Tibero (la que se muestra), Oracle Database, Microsoft SQL Server, IBM Db2 o MySQL. Las aplicaciones de OpenFrame utilizan la conectividad abierta de bases de datos (ODBC) para comunicarse con la base de datos.
- **Seguridad** mediante TACF, un módulo de servicio que controla el acceso del usuario a los sistemas y recursos. 
- **OFManager**, una solución que proporciona funciones de administración y operación de OpenFrame en el entorno web.

![Arquitectura de OpenFrame](media/openframe-02.png)

## <a name="next-steps"></a>Pasos siguientes

- [Install TmaxSoft OpenFrame on Azure](./install-openframe-azure.md) (Instalación de TmaxSoft OpenFrame en Azure)
