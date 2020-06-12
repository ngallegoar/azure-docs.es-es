---
title: 'Ejemplo de la CLI: Habilitación del cifrado de datos transparente con BYOK: Instancia administrada de Azure SQL'
description: Aprenda a configurar una Instancia administrada de Azure SQL Database para comenzar a usar Cifrado de datos transparente (TDE) con BYOK para cifrado en reposo mediante PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: azurecli
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 11/05/2019
ms.openlocfilehash: 271f68a57a4490032e318da57b4b37aac09a1410
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "84019355"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Administración del Cifrado de datos transparente en una instancia administrada con la propia clave desde Azure Key Vault

Este ejemplo de script de la CLI de Azure configura el cifrado de datos transparente (TDE) con una clave administrada por el cliente para Instancia administrada de Azure SQL, mediante una clave de Azure Key Vault. Con frecuencia esto se denomina escenario de Bring Your Own Key para TDE. Para más información sobre el cifrado de datos transparente con compatibilidad con una clave administrada por el cliente, consulte [BYOK de TDE con Azure SQL](../../azure-sql/database/transparent-data-encryption-byok-overview.md).

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

### <a name="prerequisites"></a>Requisitos previos

Para ver una instancia administrada existente, consulte el artículo en el que se explica cómo [usar la CLI de Azure para crear una instancia administrada de Azure SQL](sql-database-create-configure-managed-instance-cli.md).

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Ejecute el script.

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.sh "Set up BYOK TDE for SQL Managed Instance")]

### <a name="clean-up-deployment"></a>Limpieza de la implementación

Use el siguiente comando para quitar el grupo de recursos y todos los recursos que tenga asociados.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Referencia de ejemplo

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| | |
|---|---|
| [az sql db](/cli/azure/sql/db) | Comandos de base de datos. |
| [az sql failover-group](/cli/azure/sql/failover-group) | Comandos de grupo de conmutación por error. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de la CLI de SQL Database en la [documentación de Azure SQL Database](../../azure-sql/database/az-cli-script-samples-content-guide.md).
