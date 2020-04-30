---
title: 'Inicio rápido: Biblioteca cliente de Azure Key Vault para Node.js (v4)'
description: Obtenga información sobre cómo crear, recuperar y eliminar secretos desde una instancia de Azure Key Vault mediante la biblioteca cliente de Node.js
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/20/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: 609076cd204457bb7c952c91cd249b1aece58022
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81421459"
---
# <a name="quickstart-azure-key-vault-client-library-for-nodejs-v4"></a>Inicio rápido: Biblioteca cliente de Azure Key Vault para Node.js (v4)

Introducción a la biblioteca cliente de Azure Key Vault para Node.js. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas.

Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Use la biblioteca cliente de Key Vault para Node.js para:

- Aumentar la seguridad y el control sobre claves y contraseñas
- Crear e importar claves de cifrado en minutos
- Reducir la latencia con escala en la nube y redundancia global
- Simplificar y automatizar tareas para certificados TLS/SSL
- Utilizar módulos HSM con certificación FIPS 140-2 nivel 2

[Documentación de referencia de la API](/javascript/api/overview/azure/key-vault?view=azure-node-latest) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault) | [Paquete (npm)](https://www.npmjs.com/package/@azure/keyvault-secrets)

## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Node.js](https://nodejs.org) actual del sistema operativo.
- [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) o [Azure PowerShell](/powershell/azure/overview)

En esta guía de inicio rápido se supone que está ejecutando la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) en una ventana de terminal de Linux.

## <a name="setting-up"></a>Instalación

### <a name="install-the-package"></a>Instalar el paquete

En la ventana de la consola, instale la biblioteca de secretos de Azure Key Vault para Node.js.

```console
npm install @azure/keyvault-secrets
```

Para este inicio rápido, deberá instalar también el paquete azure.identity:

```console
npm install @azure/identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Creación de un grupo de recursos y de un almacén de claves

En este inicio rápido se usa un almacén de claves de Azure creado previamente. Puede crear un almacén de claves siguiendo los pasos descritos en el [inicio rápido de CLI de Azure](quick-create-cli.md), [inicio rápido de Azure PowerShell](quick-create-powershell.md) o [inicio rápido de Azure Portal](quick-create-portal.md). Como alternativa, puede ejecutar simplemente los siguientes comandos de la CLI de Azure.

> [!Important]
> Cada almacén de claves debe tener un nombre único. Reemplace <nombre-almacén de claves-único> por el nombre del almacén de claves en los ejemplos siguientes.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

### <a name="create-a-service-principal"></a>Creación de una entidad de servicio

La manera más sencilla de autenticar una aplicación basada en la nube es con una identidad administrada. Consulte [Uso de identidades administradas de App Service para acceder a Azure Key Vault](../general/managed-identity.md) para más información. Sin embargo, para simplificar, en este inicio rápido se va a crear una aplicación de consola. La autenticación de una aplicación de escritorio con Azure requiere el uso de una entidad de servicio y una directiva de control de acceso.

Cree una entidad de servicio con el comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) de la CLI de Azure:

```azurecli
az ad sp create-for-rbac -n "http://mySP" --sdk-auth
```

Esta operación devolverá una serie de pares clave-valor. 

```azurecli
{
  "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
  "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
  "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
  "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Tome nota de los valores de clientId y clientSecret, ya que los usaremos en el paso [Establecimiento de variables de entorno](#set-environmental-variables) a continuación.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Acceso de la entidad de servicio al almacén de claves

Cree una directiva de acceso para el almacén de claves que conceda permiso a la entidad de servicio pasando clientId al comando [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy). Proporcione a la entidad de servicio los permisos get, list y set para las claves y los secretos.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

#### <a name="set-environmental-variables"></a>Establecimiento de variables de entorno

El método DefaultAzureCredential de nuestra aplicación depende de tres variables de entorno: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET` y `AZURE_TENANT_ID`. Establezca estas variables en los valores clientId, clientSecret y tenantId que anotó en el paso [Creación de una entidad de servicio](#create-a-service-principal) anterior con el formato `export VARNAME=VALUE`. (Esto solo establece las variables para el shell actual y los procesos creados desde el shell. Para agregar de forma permanente estas variables a su entorno, edite el archivo `/etc/environment `). 

También tendrá que guardar el nombre del almacén de claves como una variable de entorno llamada `KEY_VAULT_NAME`.

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````

## <a name="object-model"></a>Modelo de objetos

La biblioteca cliente de Azure Key Vault para Node.js le permite administrar las claves y los recursos relacionados, como certificados y secretos. Los ejemplos de código siguientes le mostrarán cómo crear un cliente y cómo establecer, recuperar y eliminar un secreto.

Toda la aplicación de consola está disponible en https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app.

## <a name="code-examples"></a>Ejemplos de código

### <a name="add-directives"></a>Adición de directivas

Agregue las siguientes directivas al principio del código:

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { SecretClient } = require("@azure/keyvault-secrets");
```

### <a name="authenticate-and-create-a-client"></a>Autenticación y creación de un cliente

La autenticación en el almacén de claves y la creación de un cliente de almacén de claves depende de las variables de entorno del paso [Establecimiento de variables de entorno](#set-environmental-variables) anterior y del [constructor SecretClient](/javascript/api/@azure/keyvault-secrets/secretclient?view=azure-node-latest#secretclient-string--tokencredential--pipelineoptions-). 

El nombre del almacén de claves se expande al URI del almacén de claves, con el formato `https://<your-key-vault-name>.vault.azure.net`. 

```javascript
const keyVaultName = process.env["KEY_VAULT_NAME"];
const KVUri = "https://" + keyVaultName + ".vault.azure.net";

const credential = new DefaultAzureCredential();
const client = new SecretClient(KVUri, credential);
```

### <a name="save-a-secret"></a>Almacenamiento de un secreto

Ahora que la aplicación se ha autenticado, puede colocar un secreto en el almacén de claves mediante el [método client.setSecret](/javascript/api/@azure/keyvault-secrets/secretclient?view=azure-node-latest#setsecret-string--string--setsecretoptions-). Esto requiere establecer un nombre para el secreto; en este ejemplo, usaremos "mySecret".  

```javascript
await client.setSecret(secretName, secretValue);
```

Puede comprobar que el secreto se ha establecido con el comando [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show):

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Recuperación de un secreto

Ahora puede recuperar el valor previamente establecido con el [método client.getSecret](/javascript/api/@azure/keyvault-secrets/secretclient?view=azure-node-latest#getsecret-string--getsecretoptions-).

```javascript
const retrievedSecret = await client.getSecret(secretName);
 ```

El secreto se guarda ahora como `retrievedSecret.value`.

### <a name="delete-a-secret"></a>eliminar un secreto

Por último, vamos a eliminar el secreto del almacén de claves con el [método client.beginDeleteSecret](/javascript/api/@azure/keyvault-secrets/secretclient?view=azure-node-latest#begindeletesecret-string--begindeletesecretoptions-).

```javascript
await client.beginDeleteSecret(secretName)
```

Puede comprobar que el secreto ya no está con el comando [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show):

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, puede usar la CLI de Azure o Azure PowerShell para quitar el almacén de claves y el grupo de recursos correspondiente.

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="sample-code"></a>Código de ejemplo

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { SecretClient } = require("@azure/keyvault-secrets");

const readline = require('readline');

function askQuestion(query) {
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
    });

    return new Promise(resolve => rl.question(query, ans => {
        rl.close();
        resolve(ans);
    }))
}

async function main() {

  const keyVaultName = process.env["KEY_VAULT_NAME"];
  const KVUri = "https://" + keyVaultName + ".vault.azure.net";

  const credential = new DefaultAzureCredential();
  const client = new SecretClient(KVUri, credential);

  const secretName = "mySecret";
  var secretValue = await askQuestion("Input the value of your secret > ");

  console.log("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...");
  await client.setSecret(secretName, secretValue);

  console.log("Done.");

  console.log("Forgetting your secret.");
  secretValue = "";
  console.log("Your secret is '" + secretValue + "'.");

  console.log("Retrieving your secret from " + keyVaultName + ".");

  const retrievedSecret = await client.getSecret(secretName);

  console.log("Your secret is '" + retrievedSecret.value + "'.");
  console.log("Deleting your secret from " + keyVaultName + " ...");

  await client.beginDeleteSecret(secretName);

  console.log("Done.");

}

main()

```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un almacén de claves y ha almacenado un secreto en él. Para más información sobre Key Vault y cómo integrarlo con las aplicaciones, continúe con los artículos siguientes.

- Lea una [introducción a Azure Key Vault](../general/overview.md).
- Consulte la [guía del desarrollador de Azure Key Vault](../general/developers-guide.md).
- Consulte los [procedimientos recomendados de Azure Key Vault](../general/best-practices.md).