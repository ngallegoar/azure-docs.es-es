---
title: 'Datos de configuración a escala: Azure Automation'
description: Aprenda a configurar datos a escala para la configuración de estado en Azure Automation.
keywords: dsc,powershell,configuration,setup
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.date: 08/08/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1a77a366ee8e06b2d8c47eb3b47eeaf9ae809598
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76028289"
---
# <a name="configuration-data-at-scale"></a>Datos de configuración a escala

> Se aplica a: Windows PowerShell 5.1

Administrar cientos o miles de servidores puede ser un reto.
De los comentarios de los clientes se deduce que el aspecto más difícil es realmente administrar los [datos de configuración](/powershell/scripting/dsc/configurations/configdata).
Organizar la información en construcciones lógicas como la ubicación, el tipo y el entorno.

> [!NOTE]
> En este artículo se hace referencia a una solución que se mantiene en la comunidad de código abierto.
> La compatibilidad solo está disponible en forma de colaboración en GitHub, no con Microsoft.

## <a name="community-project-datum"></a>Proyecto de la comunidad: Datum

Para resolver este reto, se ha creado una solución mantenida por la comunidad llamada [Datum](https://github.com/gaelcolas/Datum).
Datum se basa en las grandes ideas de otras plataformas de administración de la configuración e implementa el mismo tipo de solución para DSC de PowerShell.
La información se [organiza en archivos de texto](https://github.com/gaelcolas/Datum#3-intended-usage) basados en ideas lógicas.
Algunos ejemplos serían:

- Configuración que se debe aplicar globalmente
- Configuración que se debe aplicar a todos los servidores de una ubicación
- Configuración que se debe aplicar a todos los servidores de bases de datos
- Configuración de un solo servidor

Esta información se organiza en el formato de archivo que prefiera (JSON, YAML o PSD1).
A continuación, se proporcionan cmdlets para generar archivos de datos de configuración mediante la [consolidación de la información](https://github.com/gaelcolas/Datum#datum-tree) de cada archivo en una vista única de un servidor o rol de servidor.

Una vez que se han generado los archivos de datos, puede usarlos con [scripts de configuración de DSC](/powershell/scripting/dsc/configurations/write-compile-apply-configuration) para generar archivos MOF y [cargarlos en Azure Automation](/azure/automation/tutorial-configure-servers-desired-state#create-and-upload-a-configuration-to-azure-automation).
A continuación, registre los servidores desde una [ubicación local](/azure/automation/automation-dsc-onboarding#physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azure-including-aws-ec2-instances) o [en Azure](/azure/automation/automation-dsc-onboarding#azure-virtual-machines) para extraer las configuraciones.

Para probar Datum, visite la [Galería de PowerShell](https://www.powershellgallery.com/packages/datum/) y descargue la solución o haga clic en "Project Site" (Sitio del proyecto) para ver la [documentación](https://github.com/gaelcolas/Datum#2-getting-started--concepts).

## <a name="next-steps"></a>Pasos siguientes

- [Información general sobre la configuración de estado deseado de Windows PowerShell](/powershell/scripting/dsc/overview/overview)
- [Recursos de DSC](/powershell/scripting/dsc/resources/resources)
- [Configuración del administrador de configuración local](/powershell/scripting/dsc/managing-nodes/metaconfig)
