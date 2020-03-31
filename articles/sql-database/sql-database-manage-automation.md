---
title: Administración de bases de datos con Azure Automation
description: Obtenga información acerca de cómo puede usarse el servicio Azure Automation para administrar bases de datos de Azure SQL a escala.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 9d826a75f05cf2031565f89e21d7f3667ecc8f17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822803"
---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Administración de bases de datos de Azure SQL mediante Azure Automation

Esta guía le ofrece el servicio Azure Automation y cómo se puede usar para simplificar la administración de las bases de datos de Azure SQL.

## <a name="what-is-azure-automation"></a>¿Qué es Azure Automation?

[Azure Automation](https://azure.microsoft.com/services/automation/) es un servicio de Azure para simplificar la administración en la nube mediante la automatización de procesos. Mediante Azure Automation, se pueden automatizar las tareas de ejecución prolongada, manuales, propensas a errores y que se repiten con frecuencia para aumentar la confiabilidad, la eficiencia y el valioso tiempo para su organización.

Azure Automation proporciona un motor de ejecución de flujo de trabajo altamente confiable y de alta disponibilidad con escalado para satisfacer sus necesidades a medida que crece la organización. En Azure Automation, los sistemas de terceros pueden interrumpir los procesos manualmente o en intervalos programados para que las tareas se realicen justo cuando sea necesario.

Reduzca la sobrecarga operativa y libere al personal de TI/DevOps para concentrarse en el trabajo que proporciona valor al negocio trasladando las tareas de administración en la nube para que se ejecuten automáticamente mediante Azure Automation.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>¿Cómo puede Azure Automation ayudar a administrar bases de datos de Azure SQL?

Azure SQL Database puede administrarse en Azure Automation mediante los [cmdlets de PowerShell de Azure SQL Database](https://docs.microsoft.com/powershell/module/servicemanagement/azure/#sql) que están disponibles en las [herramientas de Azure PowerShell](/powershell/azure/overview). Azure Automation tiene estos cmdlets de PowerShell para Azure SQL Database disponibles directamente para que pueda realizar todas las tareas de administración de su base de datos SQL dentro del servicio. También puede emparejar estos cmdlets en Azure Automation con los cmdlets para otros servicios de Azure, para automatizar tareas complejas entre los servicios de Azure y sistemas de terceros.

Azure Automation también tiene la capacidad de comunicarse con servidores SQL Server directamente, mediante la emisión de comandos SQL con PowerShell.

La [Galería de runbooks de Azure Automation](https://azure.microsoft.com/blog/20../../introducing-the-azure-automation-runbook-gallery/) contiene una gran variedad de runbooks de la comunidad y el equipo de producto para empezar a automatizar la administración de bases de datos de Azure SQL, otros servicios de Azure y sistemas de terceros. Los runbooks de la Galería incluyen:

- [Ejecución de consultas SQL en una Base de datos de SQL Server](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
- [Escalación vertical (arriba o abajo) de una base de datos de Azure SQL en una programación](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
- [Truncamiento de una tabla SQL si su base de datos se aproxima al tamaño máximo](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
- [Indexación de tablas en una base de datos de Azure SQL si están muy fragmentadas](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido los aspectos básicos de Azure Automation y cómo se puede usar para administrar bases de datos de Azure SQL, siga estos vínculos para obtener más información acerca de Azure Automation.

- [Información general sobre Azure Automation](../automation/automation-intro.md)
- [Mi primer runbook](../automation/automation-first-runbook-graphical.md)
- [Azure Automation: el agente SQL en la nube](https://azure.microsoft.com/blog/20../../azure-automation-your-sql-agent-in-the-cloud/) 