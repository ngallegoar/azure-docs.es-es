---
title: Administración remota de Windows a través de HTTPS para Azure | Azure Marketplace
description: Se explica cómo configurar una máquina virtual basada en Windows hospedada en Azure para que pueda administrarse de forma remota con PowerShell.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: dsindona
ms.openlocfilehash: 88941b334a9c218365bd2d97046dbb6c32d50f35
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82142764"
---
# <a name="windows-remote-management-over-https"></a>Administración remota de Windows a través de HTTPS

> [!IMPORTANT]
> A partir del 13 de abril de 2020, comenzaremos a trasladar la administración de las ofertas de máquinas virtuales de Azure al Centro de partners. Después de la migración, las ofertas se crearán y administrarán en el Centro de partners. Siga las instrucciones que se indican en [Creación de una oferta de máquina virtual de Azure](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) para administrar las ofertas migradas.

En esta sección se explica cómo configurar una VM basada en Windows hospedada en Azure para que pueda administrarse e implementarse de forma remota con PowerShell.  Para habilitar la comunicación remota de PowerShell, la VM de destino debe exponer un punto de conexión HTTPS de Administración remota de Windows (WinRM).  Para más información sobre la comunicación remota de PowerShell, consulte [Running Remote Commands](https://docs.microsoft.com/powershell/scripting/learn/remoting/running-remote-commands) (Ejecución de comandos remotos).  Para más información sobre WinRM, consulte [Administración remota de Windows](https://docs.microsoft.com/windows/desktop/WinRM/portal).

Si crea una VM usando uno de los enfoques "clásicos" de Azure —el portal de Azure Service Manager o [Azure Service Management API](https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100)) en desuso, se configura automáticamente con un punto de conexión de WinRM.  Sin embargo, si crea una VM mediante cualquiera de los siguientes enfoques "modernos" de Azure, la VM *no* se configurará para WinRM a través de HTTPS.

- Mediante [Azure Portal](https://portal.azure.com/), normalmente a partir de una base aprobada, tal como se describe en la sección [Creación de un disco duro virtual compatible con Azure](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-vhd)
- [Mediante las plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/ps-template)
- Mediante el shell de comandos de Azure PowerShell o de la CLI de Azure.  Para obtener ejemplos, consulte [Guía de inicio rápido: Creación de una máquina virtual Windows en Azure con PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell) y [Guía de inicio rápido: Creación de una máquina virtual Linux con la CLI de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli).

Este punto de conexión de WinRM también se requiere para ejecutar el kit de herramientas de certificación para la incorporación de la VM, como se describe en [Certificar la imagen de máquina virtual](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-certify-vm).

En cambio, normalmente las VM Linux se administran de manera remota mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure) o comandos de Linux desde una consola SSH.  Azure también proporciona varios métodos alternativos para [ejecutar scripts en la VM Linux](https://docs.microsoft.com/azure/virtual-machines/linux/run-scripts-in-vm).  Para escenarios más complejos, hay una serie de soluciones de integración y automatización disponibles para VM basadas en Windows o Linux.


## <a name="configure-and-deploy-with-winrm"></a>Configuración e implementación con WinRM

El punto de conexión de WinRM para una VM basada en Windows puede configurarse durante dos fases distintas de su desarrollo:

- Durante la creación, durante la implementación de una VM en un disco duro virtual existente.  Este es el enfoque preferido para las ofertas nuevas.  Este enfoque requiere la creación de un certificado de Azure, mediante plantillas de Azure Resource Manager proporcionadas, y ejecutar scripts de PowerShell personalizados.
- Después de la implementación, en una VM existente hospedada en Azure.  Use este enfoque si ya tiene una solución de VM implementada en Azure y tiene que habilitar la Administración remota de Windows para ella.  Este enfoque requiere cambios manuales en Azure Portal y la ejecución de un script en la VM de destino.


## <a name="next-steps"></a>Pasos siguientes
Si va a crear una nueva VM, puede habilitar WinRM durante la [implementación de la VM desde sus VHD](./cpp-deploy-vm-vhd.md).  En caso contrario, WinRM se puede habilitar en una VM existente.
