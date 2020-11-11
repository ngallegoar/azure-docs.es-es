---
title: Auditoría y administración del cumplimiento de directivas
titleSuffix: Azure Machine Learning
description: Conozca Azure Policy y aprenda a utilizar directivas integradas en Azure Machine Learning para asegurarse de que las áreas de trabajo se ajustan a sus requisitos.
author: jhirono
ms.author: jhirono
ms.date: 09/15/2020
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.openlocfilehash: 47a97bb8fbe52d3dde84afd77997b179a5c9248d
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93325498"
---
# <a name="audit-and-manage-azure-machine-learning-using-azure-policy"></a>Auditoría y administración de Azure Machine Learning mediante Azure Policy

[Azure Policy](../governance/policy/index.yml) es una herramienta de gobernanza que le permite tener la seguridad de que los recursos de Azure son compatibles con las directivas. Con Azure Machine Learning, puede asignar las siguientes directivas:

* **Clave administrada por el cliente** : audite o exija si las áreas de trabajo deben usar una clave administrada por el cliente.
* **Vínculo privado** : audite si las áreas de trabajo usan un punto de conexión privado para comunicarse con una red virtual.

Se pueden establecer directivas en ámbitos diferentes, por ejemplo, en el nivel de suscripción o de grupo de recursos. Para más información, consulte la [documentación de Azure Policy](../governance/policy/overview.md).

## <a name="built-in-policies"></a>Directivas integradas

Azure Machine Learning proporciona un conjunto de directivas que puede usar en escenarios comunes con esta solución. Estas definiciones de directiva se pueden asignar a la suscripción existente o utilizar como base para crear sus propias definiciones personalizadas. Para ver una lista completa de las directivas integradas de Azure Machine Learning, consulte [Directivas integradas de Azure Machine Learning](../governance/policy/samples/built-in-policies.md#machine-learning).

Para ver las definiciones de directivas integradas relacionadas con Azure Machine Learning, siga estos pasos:

1. Vaya a __Azure Policy__ en [Azure Portal](https://portal.azure.com).
1. Seleccione __Definiciones__.
1. En __Tipo__ , seleccione _Integrada_ y, en __Categoría__ , seleccione __Machine Learning__.

Desde aquí, puede seleccionar definiciones de directivas para verlas. Al ver una definición, puede usar el vínculo __Asignar__ para asignar la directiva a un ámbito específico y configurar los parámetros de la directiva. Para más información, consulte [Asignación de una directiva: Portal](../governance/policy/assign-policy-portal.md).

También puede asignar directivas mediante [Azure PowerShell](../governance/policy/assign-policy-powershell.md), la [CLI de Azure](../governance/policy/assign-policy-azurecli.md) y [plantillas](../governance/policy/assign-policy-template.md).

## <a name="workspaces-encryption-with-customer-managed-key"></a>Cifrado de las áreas de trabajo con claves administradas por el cliente

Controla si las áreas de trabajo deben cifrarse con una clave administrada por el cliente (CMK) o mediante una clave administrada por Microsoft para cifrar las métricas y los metadatos. Para más información sobre el uso de CMK, consulte la sección [Azure Cosmos DB](concept-enterprise-security.md#azure-cosmos-db) del artículo sobre seguridad de la empresa.

Para configurar esta directiva, establezca el parámetro de efecto en __audit__ o __deny__. Si se establece en __audit__ , se pueden crear áreas de trabajo sin CMK y se creará un evento de advertencia en el registro de actividad.

Si la directiva se establece en __deny__ , no podrá crear un área de trabajo a menos que especifique una CMK. Si se intenta crear un área de trabajo sin una CMK, se producirá un error similar a `Resource 'clustername' was disallowed by policy` y se creará un error en el registro de actividad. Como parte de este error, también se devuelve el identificador de la directiva.

## <a name="workspaces-should-use-private-link"></a>Las áreas de trabajo deben usar un vínculo privado

Controla si las áreas de trabajo deben usar Azure Private Link para comunicarse con Azure Virtual Network. Para más información sobre el uso del vínculo privado, consulte [Configuración del vínculo privado para un área de trabajo](how-to-configure-private-link.md).

Para configurar esta directiva, establezca el parámetro de efecto en __audit__. Si crea un área de trabajo sin usar el vínculo privado, se crea un evento de advertencia en el registro de actividad.

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de Azure Policy](../governance/policy/overview.md)
* [Directivas integradas de Azure Machine Learning](policy-reference.md)
* [Uso de directivas de seguridad con Azure Security Center](../security-center/tutorial-security-policy.md)