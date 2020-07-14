---
title: Control de acceso basado en rol en Azure Cosmos DB
description: Obtenga información sobre cómo Azure Cosmos DB proporciona protección de bases de datos con la integración de Active Directory (RBAC).
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: mjbrown
ms.openlocfilehash: cbb97dd260e5aee53595afc24e577ce08334e2b2
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2020
ms.locfileid: "86027025"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Control de acceso basado en rol en Azure Cosmos DB

Azure Cosmos DB proporciona un control de acceso basado en role (RBAC) integrado para escenarios de administración comunes en Azure Cosmos DB. Un usuario que tiene un perfil en Azure Active Directory puede asignar estos roles RBAC a usuarios, grupos, entidades de servicio o identidades administradas para conceder o denegar el acceso a recursos y operaciones en los recursos de Azure Cosmos DB. Las asignaciones de roles están dirigidas únicamente al acceso al plano de control, que incluye el acceso a las cuentas, bases de datos, contenedores y ofertas de Azure Cosmos (rendimiento).

## <a name="built-in-roles"></a>Roles integrados

Los siguientes son los roles integrados compatibles que se admiten en Azure Cosmos DB:

|**Rol integrado**  |**Descripción**  |
|---------|---------|
|[Colaborador de cuenta de DocumentDB](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Puede administrar cuentas de Azure Cosmos DB.|
|[Lector de cuentas de Cosmos DB](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Puede leer los datos de cuentas de Azure Cosmos DB.|
|[Operador de copias de seguridad de Cosmos](../role-based-access-control/built-in-roles.md#cosmosbackupoperator)|Puede enviar una solicitud de restauración para una base de datos de Azure Cosmos o un contenedor.|
|[Operador de Cosmos DB](../role-based-access-control/built-in-roles.md#cosmos-db-operator)|Puede aprovisionar cuentas, bases de datos y contenedores de Azure Cosmos, pero no puede acceder a las claves necesarias para acceder a los datos.|

> [!IMPORTANT]
> La compatibilidad con RBAC en Azure Cosmos DB se aplica solo a las operaciones del plano de control. Las operaciones del plano de datos se protegen mediante las claves maestras o tokens de recursos. Para más información, consulte [Protección del acceso a los datos de Azure Cosmos DB](secure-access-to-data.md).

## <a name="identity-and-access-management-iam"></a>Administración de identidad y acceso (IAM)

El panel **Control de acceso (IAM)** de Azure Portal se utiliza para configurar el control de acceso basado en rol en los recursos de Azure Cosmos. Los roles se aplican a usuarios, grupos, entidades de servicio e identidades administradas en Active Directory. Puede usar roles integrados o personalizados para usuarios y grupos. En la captura de pantalla siguiente se muestra la integración de Active Directory (RBAC) con control de acceso (IAM) en Azure Portal:

:::image type="content" source="./media/role-based-access-control/database-security-identity-access-management-rbac.png" alt-text="Control de acceso (IAM) en Azure Portal: demostración de la seguridad de base de datos":::

## <a name="custom-roles"></a>Roles personalizados

Además de los roles integrados, los usuarios también pueden crear [roles personalizados](../role-based-access-control/custom-roles.md) en Azure y aplicar estos roles a las entidades de servicio en todas las suscripciones dentro de su inquilino de Active Directory. Los roles personalizados proporcionan a los usuarios una forma de crear definiciones de roles RBAC con un conjunto personalizado de operaciones de los proveedores de recursos. Para saber qué operaciones están disponibles para crear roles personalizados para Azure Cosmos DB, consulte [Operaciones del proveedor de recursos de Azure Cosmos DB](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb).

## <a name="preventing-changes-from-cosmos-sdk"></a>Evitar cambios en el SDK de Cosmos

> [!WARNING]
> La habilitación de esta característica puede tener un impacto peligroso en la aplicación. Lea detenidamente antes de habilitar esta característica.

El proveedor de Azure Cosmos DB se puede bloquear para evitar cambios en recursos desde clientes que se conectan mediante claves de cuenta (es decir, aplicaciones que se conectan a través del SDK de Cosmos). También se incluyen los cambios realizados desde Azure Portal. Esto puede ser deseable para los usuarios que quieren mayores grados de control y gobernanza para entornos de producción, habilitar características como bloqueos de recursos y habilitar registros de diagnóstico para las operaciones del plano de control. Los clientes que se conecten a través del SDK de Cosmos DB no podrán cambiar ninguna propiedad de las cuentas, las bases de datos, los contenedores y el rendimiento de Cosmos. Las operaciones que implican la lectura y la escritura de datos en contenedores de Cosmos no se ven afectadas.

Cuando se establecen, los cambios en cualquier recurso solo se pueden realizar desde un usuario con el rol de RBAC adecuado y credenciales de Azure Active Directory, incluidas las instancias de Managed Service Identity.

### <a name="check-list-before-enabling"></a>Lista de comprobación anterior a la habilitación

Esta configuración impedirá que se realicen cambios en cualquier recurso de Cosmos desde cualquier cliente que se conecte mediante claves de cuenta, como cualquier SDK de Cosmos DB, cualquier herramienta que se conecte a través de claves de cuenta o desde Azure Portal. Para evitar problemas o errores de aplicaciones después de habilitar esta característica, compruebe si las aplicaciones o los usuarios de Azure Portal realizan alguna de las siguientes acciones antes de habilitar esta característica; por ejemplo:

- Cualquier cambio en la cuenta de Cosmos, incluidas las propiedades o la adición o eliminación de regiones.

- Crear o eliminar recursos secundarios, como bases de datos y contenedores. Esto incluye recursos para otras API, como Cassandra, MongoDB, Gremlin y recursos de tabla.

- Actualizar el rendimiento de los recursos de nivel de base de datos o de contenedor.

- Modificar las propiedades de los contenedores, incluida la directiva de índice, TTL y las claves únicas.

- Modificar procedimientos almacenados, desencadenadores o funciones definidas por el usuario.

Si las aplicaciones (o usuarios mediante Azure Portal) realizan alguna de estas acciones, deberán migrarse para ejecutarse mediante [Plantillas ARM](manage-sql-with-resource-manager.md), [PowerShell](manage-with-powershell.md), [CLI de Azure](manage-with-cli.md), [REST](/rest/api/cosmos-db-resource-provider/) o [Biblioteca de administración de Azure](https://github.com/Azure-Samples/cosmos-management-net). Tenga en cuenta que la administración de Azure está disponible en [varios idiomas](https://docs.microsoft.com/azure/?product=featured#languages-and-tools).

### <a name="set-via-arm-template"></a>Definición mediante una plantilla de ARM

Para establecer esta propiedad mediante una plantilla de ARM, actualice la plantilla existente o exporte una nueva plantilla para la implementación actual y, a continuación, incluya `"disableKeyBasedMetadataWriteAccess": true` en las propiedades de los recursos de databaseAccounts. A continuación se muestra un ejemplo básico de una plantilla de Azure Resource Manager con este valor de propiedad.

```json
{
    {
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "name": "[variables('accountName')]",
      "apiVersion": "2020-04-01",
      "location": "[parameters('location')]",
      "kind": "GlobalDocumentDB",
      "properties": {
        "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
        "locations": "[variables('locations')]",
        "databaseAccountOfferType": "Standard",
        "disableKeyBasedMetadataWriteAccess": true
        }
    }
}
```

> [!IMPORTANT]
> Asegúrese de incluir las otras propiedades de la cuenta y los recursos secundarios al volver a implementar esta propiedad. No implemente esta plantilla tal cual o se restablecerán todas las propiedades de la cuenta.

### <a name="set-via-azure-cli"></a>Definir mediante la CLI de Azure

Para habilitar el uso de la CLI de Azure, use el siguiente comando:

```azurecli-interactive
az cosmosdb update  --name [CosmosDBAccountName] --resource-group [ResourceGroupName]  --disable-key-based-metadata-write-access true

```

### <a name="set-via-powershell"></a>Definir mediante PowerShell

Para habilitar el uso de Azure PowerShell, use el siguiente comando:

```azurepowershell-interactive
Update-AzCosmosDBAccount -ResourceGroupName [ResourceGroupName] -Name [CosmosDBAccountName] -DisableKeyBasedMetadataWriteAccess true
```

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es el control de acceso basado en rol de Azure (RBAC)?](../role-based-access-control/overview.md)
- [Roles personalizados en los recursos de Azure](../role-based-access-control/custom-roles.md)
- [Operaciones del proveedor de recursos de Azure Cosmos DB](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)
