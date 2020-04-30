---
title: Implementación de recursos con una plantilla y la CLI de Azure
description: Use Azure Resource Manager y la CLI de Azure para implementar recursos en Azure. Los recursos se definen en una plantilla de Resource Manager.
ms.topic: conceptual
ms.date: 04/20/2020
ms.openlocfilehash: 8ee15699a085178add05137be895fe6b660b715b
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685699"
---
# <a name="deploy-resources-with-arm-templates-and-azure-cli"></a>Implementación de recursos con plantillas de ARM y la CLI de Azure

En este artículo, se explica el uso de la CLI de Azure con plantillas de Azure Resource Manager (ARM) para implementar recursos en Azure. Si no está familiarizado con los conceptos de implementación y administración de las soluciones de Azure, vea [Información general sobre plantillas](overview.md).

Los comandos de implementación cambiaron en la CLI de Azure, versión 2.2.0. Los ejemplos de este artículo requieren la CLI de Azure, versión 2.2.0 o posterior.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

Si no tiene instalada la CLI de Azure, puede usar [Cloud Shell](#deploy-template-from-cloud-shell).

## <a name="deployment-scope"></a>Ámbito de la implementación

La implementación puede tener como destino un grupo de recursos, una suscripción, un grupo de administración o un inquilino. En la mayoría de los casos, la implementación tendrá como destino un grupo de recursos. Para aplicar directivas y asignaciones de roles en un ámbito mayor, use implementaciones de suscripción, de grupo de administración o de inquilino. Al implementar en una suscripción, puede crear un grupo de recursos e implementar recursos en él.

Según el ámbito de la implementación, usará comandos diferentes.

Para implementar en un **grupo de recursos**, use [az deployment group create](/cli/azure/deployment/group?view=azure-cli-latest#az-deployment-group-create):

```azurecli-interactive
az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>
```

Para implementar en una **suscripción**, use [az deployment sub create](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-create):

```azurecli-interactive
az deployment sub create --location <location> --template-file <path-to-template>
```

Para más información sobre las implementaciones en el nivel de suscripción, consulte [Creación de grupos de recursos y otros recursos en el nivel de suscripción](deploy-to-subscription.md).

Para implementar en un **grupo de administración**, use [az deployment mg create](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-create):

```azurecli-interactive
az deployment mg create --location <location> --template-file <path-to-template>
```

Para obtener más información sobre las implementaciones de nivel de grupo de administración, consulte [Creación de recursos en el nivel de grupo de administración](deploy-to-management-group.md).

Para implementar en un **inquilino**, use [az deployment tenant create](/cli/azure/deployment/tenant?view=azure-cli-latest#az-deployment-tenant-create):

```azurecli-interactive
az deployment tenant create --location <location> --template-file <path-to-template>
```

Para obtener más información sobre las implementaciones a nivel de inquilino, consulte [Creación de recursos en el nivel de inquilino](deploy-to-tenant.md).

Los ejemplos de este artículo usan las implementaciones del grupo de recursos.

## <a name="deploy-local-template"></a>Implementar una plantilla local

Al implementar recursos en Azure, siga estos pasos:

1. Inicio de sesión en la cuenta de Azure.
2. Cree un grupo de recursos que actúe como contenedor para los recursos implementados. El nombre del grupo de recursos solo puede incluir caracteres alfanuméricos, puntos, guiones bajos, guiones y paréntesis. Puede tener hasta 90 caracteres. No puede terminar en punto.
3. Implemente en el grupo de recursos la plantilla que define los recursos que se van a crear.

Una plantilla puede incluir parámetros que le permiten personalizar la implementación. Por ejemplo, puede proporcionar valores que están diseñados para un entorno concreto (como desarrollo, prueba y producción). La plantilla de ejemplo define un parámetro para la SKU de la cuenta de almacenamiento.

En el ejemplo siguiente se crea un grupo de recursos y se implementa una plantilla desde la máquina local:

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

La implementación puede demorar unos minutos en completarse. Cuando termine, verá un mensaje que incluye el resultado:

```output
"provisioningState": "Succeeded",
```

## <a name="deploy-remote-template"></a>Implementación de una plantilla remota

En lugar de almacenar las plantillas de ARM en el equipo local, quizás prefiera almacenarlas en una ubicación externa. Puede almacenar plantillas en un repositorio de control de código fuente (por ejemplo, GitHub). O bien, puede almacenarlas en una cuenta de Azure Storage para el acceso compartido en su organización.

Para implementar una plantilla externa, use el parámetro **template-uri**. Use el identificador URI en el ejemplo para implementar la plantilla de ejemplo de GitHub.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

En el ejemplo anterior, se requiere un identificador URI accesible públicamente para la plantilla, que funciona con la mayoría de los escenarios porque la plantilla no debe incluir datos confidenciales. Si tiene que especificar datos confidenciales (por ejemplo, una contraseña de administrador), pase ese valor como un parámetro seguro. Sin embargo, si no desea que la plantilla sea accesible públicamente, puede almacenarla en un contenedor de almacenamiento privado para protegerla. Para más información sobre la implementación de una plantilla que requiere un token de Firma de acceso compartido (SAS), consulte [Implementación de una plantilla privada con el token de SAS](secure-template-with-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../../includes/resource-manager-cloud-shell-deploy.md)]

En Cloud Shell, use los comandos siguientes:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az deployment group create --resource-group examplegroup \
  --template-uri <copied URL> \
  --parameters storageAccountType=Standard_GRS
```

## <a name="parameters"></a>Parámetros

Para pasar valores de parámetros, puede usar parámetros en línea o un archivo de parámetros.

### <a name="inline-parameters"></a>Parámetros en línea

Para pasar parámetros en línea, proporcione los valores en `parameters`. Por ejemplo, para pasar una cadena y una matriz a una plantilla en un shell de Bash, use:

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Si usa la CLI de Azure con el símbolo del sistema de Windows (CMD) o PowerShell, pase la matriz con el formato `exampleArray="['value1','value2']"`.

También puede obtener el contenido del archivo y proporcionar ese contenido como un parámetro en línea.

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file demotemplate.json \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Obtener un valor de parámetro de un archivo es útil cuando se necesita proporcionar valores de configuración. Por ejemplo, puede proporcionar [valores de cloud-init para una máquina virtual Linux](../../virtual-machines/linux/using-cloud-init.md).

El formato de arrayContent.json es:

```json
[
    "value1",
    "value2"
]
```

### <a name="parameter-files"></a>Archivos de parámetros

En lugar de pasar parámetros como valores en línea en el script, quizá le resulte más fácil usar un archivo JSON que contiene los valores de parámetro. El archivo de parámetros debe ser un archivo local. No se admiten los archivos de parámetros externos con la CLI de Azure.

Para más información sobre el archivo de parámetro, consulte [Creación de un archivo de parámetros de Resource Manager](parameter-files.md).

Para pasar un archivo de parámetros local, use `@` para especificar un archivo local denominado storage.parameters.json.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

## <a name="handle-extended-json-format"></a>Control del formato JSON extendido

Para poder implementar una plantilla con cadenas o comentarios multilínea utilizando la versión 2.3.0 u otra anterior de la CLI de Azure, debe usar el conmutador `--handle-extended-json-format`.  Por ejemplo:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

## <a name="test-a-template-deployment"></a>Prueba de una implementación de plantilla

Para probar los valores de parámetro y de plantilla sin implementar realmente ningún recurso, use [az deployment group validate](/cli/azure/group/deployment).

```azurecli-interactive
az deployment group validate \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters @storage.parameters.json
```

Si no se detectan errores, el comando devuelve información sobre la implementación de prueba. En concreto, tenga en cuenta que el valor **error** es null.

```output
{
  "error": null,
  "properties": {
      ...
```

Si se detecta un error, el comando devuelve un mensaje de error. Por ejemplo, si se pasa un valor incorrecto para la SKU de la cuenta de almacenamiento, se devuelve el error siguiente:

```output
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Si la plantilla tiene un error de sintaxis, el comando devuelve un error que indica que la plantilla no se pudo analizar. El mensaje indica el número de línea y la posición del error de análisis.

```output
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para revertir a una implementación correcta cuando se produce un error, consulte [Revertir en caso de error a una implementación correcta](rollback-on-error.md).
- Para especificar cómo controlar los recursos que existen en el grupo de recursos, pero que no están definidos en la plantilla, consulte [Modos de implementación de Azure Resource Manager](deployment-modes.md).
- Para entender cómo definir parámetros en la plantilla, consulte [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](template-syntax.md).
- Para obtener sugerencias para resolver los errores de implementación más comunes, consulte [Solución de errores comunes de implementación de Azure con Azure Resource Manager](common-deployment-errors.md).
- Para más información sobre la implementación de una plantilla que requiere un token de SAS, vea [Implementación de una plantilla privada con el token de SAS](secure-template-with-sas-token.md).
- Para el lanzamiento seguro del servicio en más de una región, consulte [Azure Deployment Manager](deployment-manager-overview.md).
