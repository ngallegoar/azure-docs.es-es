---
title: 'PowerShell: Habilitación del cifrado de datos transparente con BYOK: Instancia administrada de Azure SQL Database '
description: Aprenda a configurar una Instancia administrada de Azure SQL Database para comenzar a usar Cifrado de datos transparente (TDE) con BYOK para cifrado en reposo mediante PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 11/05/2019
ms.openlocfilehash: ddffda5229c9c0d33c563e3ae7b4a884f0f92dff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73691395"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Administración del Cifrado de datos transparente en una instancia administrada con la propia clave desde Azure Key Vault

Este ejemplo de script de PowerShell configura el cifrado de datos transparente (TDE) con una clave administrada por el cliente para Instancia administrada de Azure SQL, mediante una clave de Azure Key Vault. Con frecuencia esto se denomina escenario de Bring Your Own Key para TDE. Para más información sobre el cifrado de datos transparente con compatibilidad con una clave administrada por el cliente, consulte [BYOK de TDE con Azure SQL](../transparent-data-encryption-byok-azure-sql.md).

## <a name="prerequisites"></a>Prerrequisitos

- Una instancia administrada existente. Consulte [Use PowerShell para crear una instancia administrada de Azure SQL Database](sql-database-create-configure-managed-instance-powershell.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Tanto el uso local de PowerShell como el de Azure Cloud Shell requiere AZ PowerShell 2.3.2 o cualquier versión posterior. Si necesita realizar la actualización, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps) o ejecute el script de ejemplo siguiente para instalar el módulo en el usuario actual:

`Install-Module -Name Az -AllowClobber -Scope CurrentUser`

Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

## <a name="sample-scripts"></a>Muestras de scripts

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.ps1 "Set up BYOK TDE for SQL Managed Instance")]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/powershell/azure/overview).

Encontrará más ejemplos de scripts de PowerShell de SQL Database en los [scripts de PowerShell de Azure SQL Database](../sql-database-powershell-samples.md).
