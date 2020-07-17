---
title: Administrar una instancia de Azure Blockchain Service con la CLI de Azure
description: Administración de Azure Blockchain Service con la CLI de Azure
ms.date: 11/22/2019
ms.topic: how-to
ms.reviewer: janders
ms.openlocfilehash: fc00bedee5ff55033a1d65c6d5d6bfa766f0f01e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85208168"
---
# <a name="manage-azure-blockchain-service-using-azure-cli"></a>Administrar una instancia de Azure Blockchain Service con la CLI de Azure

Además de Azure Portal, puede usar la CLI de Azure para administrar los miembros de la cadena de bloques y los nodos de transacción para Azure Blockchain Service.

Asegúrese de que ha instalado la versión más reciente de la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) y que ha iniciado sesión en una cuenta de Azure con `az login`.

En los ejemplos siguientes, reemplace el ejemplo de `<parameter names>` con sus propios valores.

## <a name="create-blockchain-member"></a>Creación de un miembro de cadena de bloques

En el ejemplo se crea un miembro de cadena de bloques en Azure Blockchain Service que ejecuta el protocolo de libro de contabilidad de Quorum en un nuevo consorcio.

```azurecli
az resource create \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --is-full-object \
                     --properties '{ "location":"<myBlockchainLocation>", "properties": {"password":"<myStrongPassword>", "protocol":"Quorum","consortium":"<myConsortiumName>", "consortiumManagementAccountPassword":"<myConsortiumManagementAccountPassword>", "firewallRules":[{"ruleName":"<myRuleName>","startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]}, "sku":{"name":"<skuName>"}}'
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos de Azure Blockchain Service. |
| **name** | Un nombre único que identifica al miembro de cadena de bloques de Azure Blockchain Service. El nombre se usa como dirección de punto de conexión público. Por ejemplo, `myblockchainmember.blockchain.azure.com`. |
| **ubicación** | Región de Azure donde se crea el miembro de cadena de bloques. Por ejemplo, `eastus`. Elija la ubicación más cercana a los usuarios o a sus otras aplicaciones de Azure. |
| **password** | La contraseña de la cuenta del miembro. La contraseña de la cuenta del miembro se usa para autenticarse en el punto de conexión público del miembro de cadena de bloques mediante autenticación básica. La contraseña debe cumplir tres de los cuatro requisitos siguientes: debe tener una longitud de entre 12 y 72 caracteres, un carácter en minúscula, un carácter en mayúscula, un número y un carácter especial que no sea el signo de número (#), porcentaje (%), coma (,), asterisco (*), comilla inversa (\`), comilla doble ("), comilla simple ('), guion (-) y punto y coma (;).|
| **protocolo** | La versión preliminar pública admite Quorum. |
| **consortium** | Nombre del consorcio al que unirse o que se va a crear. |
| **consortiumManagementAccountPassword** | La contraseña de administración del consorcio. La contraseña se usa para unirse a un consorcio. |
| **ruleName** | Nombre de la regla para poner en la lista blanca un intervalo de direcciones IP. Parámetro opcional para reglas de firewall.|
| **startIpAddress** | Inicio del intervalo de direcciones IP para la creación de listas de permitidos. Parámetro opcional para reglas de firewall. |
| **endIpAddress** | Fin del intervalo de direcciones IP para la creación de listas de permitidos. Parámetro opcional para reglas de firewall. |
| **skuName** | Tipo de nivel. Use S0 para Estándar y B0 para Básico. |

## <a name="change-blockchain-member-password"></a>Cambiar la contraseña del miembro de la cadena de bloques

En el ejemplo se cambia la contraseña de un miembro de la cadena de bloques.

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.password='<myStrongPassword>' \
                     --remove properties.consortiumManagementAccountAddress
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos de Azure Blockchain Service. |
| **name** | Nombre que identifica al miembro de Azure Blockchain Service. |
| **password** | La contraseña de la cuenta del miembro. La contraseña debe cumplir tres de los cuatro requisitos siguientes: debe tener una longitud de entre 12 y 72 caracteres, un carácter en minúscula, un carácter en mayúscula, un número y un carácter especial que no sea el signo de número (#), porcentaje (%), coma (,), asterisco (*), comilla inversa (\`), comilla doble ("), comilla simple ('), guion (-) y punto y coma (;). |

## <a name="create-transaction-node"></a>Creación del nodo de transacción

Cree un nodo de transacción en un miembro de la cadena de bloques existente. Al agregar nodos de transacción, puede aumentar el aislamiento de la seguridad y distribuir la carga. Por ejemplo, podría tener un punto de conexión del nodo de transacción para diferentes aplicaciones cliente.

```azurecli
az resource create \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --is-full-object \
                     --properties '{"location":"<myRegion>", "properties":{"password":"<myStrongPassword>", "firewallRules":[{"ruleName":"<myRuleName>", "startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]}}'
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos de Azure Blockchain Service. |
| **name** | Nombre del miembro de la cadena de bloques de Azure Blockchain Service que también incluye el nuevo nombre del nodo de transacción. |
| **ubicación** | Región de Azure donde se crea el miembro de cadena de bloques. Por ejemplo, `eastus`. Elija la ubicación más cercana a los usuarios o a sus otras aplicaciones de Azure. |
| **password** | Contraseña del nodo de transacción. La contraseña debe cumplir tres de los cuatro requisitos siguientes: debe tener una longitud de entre 12 y 72 caracteres, un carácter en minúscula, un carácter en mayúscula, un número y un carácter especial que no sea el signo de número (#), porcentaje (%), coma (,), asterisco (*), comilla inversa (\`), comilla doble ("), comilla simple ('), guion (-) y punto y coma (;). |
| **ruleName** | Nombre de la regla para poner en la lista blanca un intervalo de direcciones IP. Parámetro opcional para reglas de firewall. |
| **startIpAddress** | Inicio del intervalo de direcciones IP para la creación de listas de permitidos. Parámetro opcional para reglas de firewall. |
| **endIpAddress** | Fin del intervalo de direcciones IP para la creación de listas de permitidos. Parámetro opcional para reglas de firewall.|

## <a name="change-transaction-node-password"></a>Cambio de la contraseña del nodo de transacción

En el ejemplo se cambia la contraseña de un nodo de transacción

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.password='<myStrongPassword>'
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos en el que se encuentran los recursos de Azure Blockchain Service. |
| **name** | Nombre del miembro de la cadena de bloques de Azure Blockchain Service que también incluye el nuevo nombre del nodo de transacción. |
| **password** | Contraseña del nodo de transacción. La contraseña debe cumplir tres de los cuatro requisitos siguientes: debe tener una longitud de entre 12 y 72 caracteres, un carácter en minúscula, un carácter en mayúscula, un número y un carácter especial que no sea el signo de número (#), porcentaje (%), coma (,), asterisco (*), comilla inversa (\`), comilla doble ("), comilla simple ('), guion (-) y punto y coma (;). |

## <a name="change-consortium-management-account-password"></a>Cambio de la contraseña de la cuenta de administración del consorcio

La cuenta de administración del consorcio se usa para administrar la pertenencia al consorcio. Cada miembro se identifica de forma única con una cuenta de administración del consorcio, cuya contraseña se puede cambiar con el siguiente comando.

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.consortiumManagementAccountPassword='<myConsortiumManagementAccountPassword>' \
                     --remove properties.consortiumManagementAccountAddress
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos donde se crean los recursos de Azure Blockchain Service. |
| **name** | Nombre que identifica al miembro de Azure Blockchain Service. |
| **consortiumManagementAccountPassword** | La contraseña de la cuenta de administración del consorcio La contraseña debe cumplir tres de los cuatro requisitos siguientes: debe tener una longitud de entre 12 y 72 caracteres, un carácter en minúscula, un carácter en mayúscula, un número y un carácter especial que no sea el signo de número (#), porcentaje (%), coma (,), asterisco (*), comilla inversa (\`), comilla doble ("), comilla simple ('), guion (-) y punto y coma (;). |
  
## <a name="update-firewall-rules"></a>Actualización de las reglas de firewall

```azurecli
az resource update \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers \
                     --set properties.firewallRules='[{"ruleName":"<myRuleName>", "startIpAddress":"<myStartIpAddress>", "endIpAddress":"<myEndIpAddress>"}]' \
                     --remove properties.consortiumManagementAccountAddress
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos en el que se encuentran los recursos de Azure Blockchain Service. |
| **name** | Nombre del miembro de la cadena de bloques de Azure Blockchain Service. |
| **ruleName** | Nombre de la regla para poner en la lista blanca un intervalo de direcciones IP. Parámetro opcional para reglas de firewall.|
| **startIpAddress** | Inicio del intervalo de direcciones IP para la creación de listas de permitidos. Parámetro opcional para reglas de firewall.|
| **endIpAddress** | Fin del intervalo de direcciones IP para la creación de listas de permitidos. Parámetro opcional para reglas de firewall.|

## <a name="list-api-keys"></a>Enumeración de las claves de API

Las claves de API se pueden usar para acceder al nodo de forma similar al nombre de usuario y la contraseña. Hay dos claves de API que admiten la rotación de claves. Para enumerar las claves de API, utilice el comando siguiente.

```azurecli
az resource invoke-action \
                            --resource-group <myResourceGroup> \
                            --name <myMemberName>/transactionNodes/<myTransactionNode> \
                            --action "listApiKeys" \
                            --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos en el que se encuentran los recursos de Azure Blockchain Service. |
| **name** | Nombre del miembro de la cadena de bloques de Azure Blockchain Service que también incluye el nuevo nombre del nodo de transacción. |

## <a name="regenerate-api-keys"></a>Regeneración de claves de API

Para generar claves de API, utilice el comando siguiente.

```azurecli
az resource invoke-action \
                            --resource-group <myResourceGroup> \
                            --name <myMemberName>/transactionNodes/<myTransactionNode> \
                            --action "regenerateApiKeys" \
                            --resource-type Microsoft.Blockchain/blockchainMembers \
                            --request-body '{"keyName":"<keyValue>"}'
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos en el que se encuentran los recursos de Azure Blockchain Service. |
| **name** | Nombre del miembro de la cadena de bloques de Azure Blockchain Service que también incluye el nuevo nombre del nodo de transacción. |
| **keyName** | Reemplace \<keyValue\> por key1 o key2. |

## <a name="delete-a-transaction-node"></a>Eliminar un nodo de transacción

En el ejemplo se elimina un nodo de transacción del miembro de la cadena de bloques.

```azurecli
az resource delete \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName>/transactionNodes/<myTransactionNode> \
                     --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos en el que se encuentran los recursos de Azure Blockchain Service. |
| **name** | Nombre del miembro de la cadena de bloques de Azure Blockchain Service que también incluye el nombre del nodo de transacción que se va a eliminar. |

## <a name="delete-a-blockchain-member"></a>Eliminación de un miembro de cadena de bloques

En el ejemplo se elimina un miembro de cadena de bloques.

```azurecli
az resource delete \
                     --resource-group <myResourceGroup> \
                     --name <myMemberName> \
                     --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parámetro | Descripción |
|---------|-------------|
| **resource-group** | Nombre del grupo de recursos en el que se encuentran los recursos de Azure Blockchain Service. |
| **name** | Nombre del miembro de cadena de bloques de Azure Blockchain Service que se va a eliminar. |

## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="grant-access-for-azure-ad-user"></a>Concesión de acceso a un usuario de Azure AD

```azurecli
az role assignment create \
                            --role <role> \
                            --assignee <assignee> \
                            --scope /subscriptions/<subId>/resourceGroups/<groupName>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>
```

| Parámetro | Descripción |
|---------|-------------|
| **role** | Nombre del rol de Azure AD. |
| **assignee** | Identificador de usuario de Azure AD. Por ejemplo: `user@contoso.com` |
| **scope** | Ámbito de la asignación de roles. Puede ser un miembro de cadena de bloques o de nodo de transacción. |

**Ejemplo**:

Conceda acceso al nodo al usuario de Azure AD para un **miembro** de la cadena de bloques:

```azurecli
az role assignment create \
                            --role 'myRole' \
                            --assignee user@contoso.com \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

**Ejemplo**:

Conceda acceso al nodo al usuario de Azure AD para un **nodo de transacción** de la cadena de bloques:

```azurecli
az role assignment create \
                            --role 'MyRole' \
                            --assignee user@contoso.com \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1/transactionNodes/contosoTransactionNode1
```

### <a name="grant-node-access-for-azure-ad-group-or-application-role"></a>Concesión de acceso al nodo a un rol de aplicación o grupo de Azure AD

```azurecli
az role assignment create \
                            --role <role> \
                            --assignee-object-id <assignee_object_id>
```

| Parámetro | Descripción |
|---------|-------------|
| **role** | Nombre del rol de Azure AD. |
| **assignee-object-id** | Identificador de la aplicación o del grupo de Azure AD. |
| **scope** | Ámbito de la asignación de roles. Puede ser un miembro de cadena de bloques o de nodo de transacción. |

**Ejemplo**:

Conceda acceso al nodo para el **rol de aplicación**.

```azurecli
az role assignment create \
                            --role 'myRole' \
                            --assignee-object-id 22222222-2222-2222-2222-222222222222 \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

### <a name="remove-azure-ad-node-access"></a>Retirada del acceso al nodo de Azure AD

```azurecli
az role assignment delete \
                            --role <myRole> \
                            --assignee <assignee> \
                            --scope /subscriptions/mySubscriptionId/resourceGroups/<myResourceGroup>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>/transactionNodes/<myTransactionNode>
```

| Parámetro | Descripción |
|---------|-------------|
| **role** | Nombre del rol de Azure AD. |
| **assignee** | Identificador de usuario de Azure AD. Por ejemplo: `user@contoso.com` |
| **scope** | Ámbito de la asignación de roles. Puede ser un miembro de cadena de bloques o de nodo de transacción. |

## <a name="next-steps"></a>Pasos siguientes

Aprenda cómo [configurar los nodos de transacción de Azure Blockchain Service con Azure Portal](configure-transaction-nodes.md).
