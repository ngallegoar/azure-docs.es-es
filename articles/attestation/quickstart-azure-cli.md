---
title: Configuración de Azure Attestation con la CLI de Azure
description: Instalación y configuración de un proveedor de atestación mediante la CLI de Azure.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: quickstart
ms.date: 11/20/2020
ms.author: mbaldwin
ms.openlocfilehash: dee9e7596c0a30301d9e0453ef22a6dfe9541522
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96020949"
---
# <a name="quickstart-set-up-azure-attestation-with-azure-cli"></a>Inicio rápido: Configuración de Azure Attestation con la CLI de Azure

Comience a usar Azure Attestation mediante la CLI de Azure para configurar la atestación.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

## <a name="get-started"></a>Introducción

1. Use el siguiente comando para iniciar sesión en Azure:

   ```azurecli
   az login
   ```

1. Si es necesario, cambie a la suscripción para Azure Attestation:

   ```azurecli
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

1. Registre el proveedor de recursos Microsoft.Atestation en la suscripción con el comando [az provider register](/cli/azure/provider#az_provider_register):

   ```azurecli
   az provider register --name Microsoft.Attestation
   ```

   Para más información sobre los proveedores de recursos de Azure y cómo configurarlos y administrarlos, consulte [Tipos y proveedores de recursos de Azure](../azure-resource-manager/management/resource-providers-and-types.md).

   > [!NOTE]
   > Solo tiene que registrar un proveedor de recursos una vez para una suscripción.

1. Cree un grupo de recursos para el proveedor de atestación. Puede colocar otros recursos de Azure en el mismo grupo de recursos, incluida una máquina virtual con una instancia de aplicación cliente. Ejecute el comando [az group create](/cli/azure/group#az_group_create) para crear un grupo de recursos o use un grupo de recursos existente:

   ```azurecli
   az group create --name attestationrg --location uksouth
   ```

## <a name="create-and-manage-an-attestation-provider"></a>Creación y administración de un proveedor de atestación

A continuación se incluyen comandos que puede usar para crear y administrar el proveedor de atestación:

1. Ejecute el comando [az attestation create](/cli/azure/ext/attestation/attestation#ext_attestation_az_attestation_create) para crear un proveedor de atestación:

   ```azurecli
   az attestation create --resource-group attestationrg --name attestationProvider --location uksouth \
      --attestation-policy SgxDisableDebugMode --certs-input-path C:\test\policySignersCertificates.pem
   ```

   El parámetro **--certs-input-path** especifica un conjunto de claves de firma de confianza. Si especifica un nombre de archivo para este parámetro, el proveedor de atestación solo debe configurarse con directivas en formato JWT firmado. En caso contrario, la directiva se puede configurar con un formato de texto o un formato JWT sin firma. Para más información acerca de JWT, consulte [Conceptos básicos](basic-concepts.md). Para ver ejemplos de certificados, consulte [Ejemplos de un certificado de firmante de directivas de atestación](policy-signer-examples.md).

1. Ejecute el comando [az attestation show](/cli/azure/ext/attestation/attestation#ext_attestation_az_attestation_show) para recuperar propiedades del proveedor de atestación como status y AttestURI:

   ```azurecli
   az attestation show --resource-group attestationrg --name attestationProvider
   ```

   Este comando muestra valores como la siguiente salida:

   ```output
   Id:/subscriptions/MySubscriptionID/resourceGroups/MyResourceGroup/providers/Microsoft.Attestation/attestationProviders/MyAttestationProvider
   Location: MyLocation
   ResourceGroupName: MyResourceGroup
   Name: MyAttestationProvider
   Status: Ready
   TrustModel: AAD
   AttestUri: https://MyAttestationProvider.us.attest.azure.net
   Tags:
   TagsTable:
   ```

Puede eliminar un proveedor de atestación mediante el comando [az attestation delete](/cli/azure/ext/attestation/attestation#ext_attestation_az_attestation_delete):

```azurecli
az attestation delete --resource-group attestationrg --name attestationProvider
```

## <a name="policy-management"></a>Administración de directivas

Para administrar directivas, un usuario de Azure AD necesita los siguientes permisos para `Actions`:

- `Microsoft.Attestation/attestationProviders/attestation/read`
- `Microsoft.Attestation/attestationProviders/attestation/write`
- `Microsoft.Attestation/attestationProviders/attestation/delete`

Estos permisos se pueden asignar a un usuario de AD por medio de un rol como `Owner` (permisos comodín), `Contributor` (permisos comodín) o `Attestation Contributor` (permisos específicos solo para Azure Attestation).  

Para leer directivas, un usuario de Azure AD necesita el siguiente permiso para `Actions`:

- `Microsoft.Attestation/attestationProviders/attestation/read`

Este permiso se puede asignar a un usuario de Azure AD por medio de un rol como `Reader` (permisos comodín) o `Attestation Reader` (permisos específicos solo para Azure Attestation).

Use los comandos que se describen aquí para proporcionar la administración de directivas para un proveedor de atestación, un TEE a la vez.

El comando [az attestation policy show](/cli/azure/ext/attestation/attestation/policy#ext_attestation_az_attestation_policy_show) devuelve la directiva actual para el TEE especificado:

```azurecli
az attestation policy show --resource-group attestationrg --name attestationProvider --tee SgxEnclave
```

> [!NOTE]
> El comando muestra la directiva en el formato de texto y JWT.

Estos son los tipos de TEE que se admiten:

- `CyResComponent`
- `OpenEnclave`
- `SgxEnclave`
- `VSMEnclave`

Use el comando [az attestation policy set](/cli/azure/ext/attestation/attestation/policy#ext_attestation_az_attestation_policy_set) para establecer una nueva directiva para el TEE especificado.

```azurecli
az attestation policy set --resource-group attestationrg --name attestationProvider --tee SgxEnclave \
   --new-attestation-policy newAttestationPolicyname
```

La directiva de atestación en formato JWT debe contener una notificación denominada `AttestationPolicy`. Una directiva firmada debe estar firmada con una clave que se corresponda con cualquiera de los firmantes de directivas de atestación existentes.

Para obtener ejemplos de directivas, consulte [Ejemplos de una directiva de atestación](policy-examples.md).

El comando [az attestation policy reset](/cli/azure/ext/attestation/attestation/policy#ext_attestation_az_attestation_policy_reset) establece una nueva directiva para el TEE especificado.

```azurecli
az attestation policy reset --resource-group attestationrg --name attestationProvider --tee SgxEnclave \
   --policy-jws "eyJhbGciOiJub25lIn0.."
```

## <a name="policy-signer-certificates-management"></a>Administración de certificados de firmante de directivas

Use los siguientes comandos para administrar los firmantes de directivas de atestación para un proveedor de atestación:

```azurecli
az attestation signer list --resource-group attestationrg --name attestationProvider

az attestation signer add --resource-group attestationrg --name attestationProvider \
   --signer "eyAiYWxnIjoiUlMyNTYiLCAie..."

az attestation signer remove --resource-group attestationrg --name attestationProvider \
   --signer "eyAiYWxnIjoiUlMyNTYiLCAie..."
```

Un firmante de directivas de atestación es un JWT firmado con una notificación denominada `maa-policyCertificate`. El valor de la notificación es un JWK que contiene la clave de firma de confianza que se va a agregar. El JWT debe estar firmado con una clave privada que se corresponda con cualquiera de los firmantes de directivas de atestación existentes. Para más información acerca de JWT y JWK, consulte [Conceptos básicos](basic-concepts.md).

Toda la manipulación semántica del firmante de directivas de atestación debe realizarse fuera de la CLI de Azure. En lo que se refiere a la CLI de Azure, es una cadena simple.

Para ver ejemplos de certificados, consulte [Ejemplos de un certificado de firmante de directivas de atestación](policy-signer-examples.md).

## <a name="next-steps"></a>Pasos siguientes

- [Creación y firma de una directiva de atestación](author-sign-policy.md)
- [Implementar la atestación con un enclave de SGX mediante ejemplos de código](/samples/browse/?expanded=azure&terms=attestation)
