---
title: Uso de un volumen basado en Azure Files en una aplicación Service Fabric Mesh
description: Aprenda a almacenar el estado en una aplicación Azure Service Fabric Mesh montando un volumen basado en Azure Files en un servicio mediante la CLI de Azure.
author: dkkapur
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 5bb7ab6c861d958f6811ca852363c59cfced3940
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76718827"
---
# <a name="mount-an-azure-files-based-volume-in-a-service-fabric-mesh-application"></a>Montaje de un volumen basado en Azure Files en una aplicación Service Fabric Mesh 

En este artículo se describe cómo montar un volumen basado en Azure Files en un servicio de una aplicación Service Fabric Mesh.  El controlador de volumen de Azure Files es un controlador de volumen de Docker que se utiliza para montar un recurso compartido de Azure Files en un contenedor, que se utiliza para conservar el estado del servicio. Los volúmenes proporcionan almacenamiento de archivos de propósito general y permiten leer y escribir archivos mediante las API de archivos de E/S de disco normal.  Para obtener más información sobre los volúmenes y las opciones para almacenar datos de la aplicación, lea el artículo sobre cómo [almacenar el estado](service-fabric-mesh-storing-state.md).

Para montar un volumen en un servicio, cree un recurso de volumen en la aplicación Service Fabric Mesh y, a continuación, haga referencia a ese volumen en el servicio.  La declaración del recurso de volumen y la referencia a él en el recurso del servicio se pueden realizar en [archivos de recursos basados en YAML](#declare-a-volume-resource-and-update-the-service-resource-yaml) o en la [plantilla de implementación basada en JSON](#declare-a-volume-resource-and-update-the-service-resource-json). Antes de montar el volumen, primero cree una cuenta de almacenamiento de Azure y un [recurso compartido de archivos en Azure Files](/azure/storage/files/storage-how-to-create-file-share).

## <a name="prerequisites"></a>Prerrequisitos
> [!NOTE]
> **Problema conocido con la implementación en la máquina de desarrollo de Windows RS5:** Hay un error abierto con el cmdlet de PowerShell New-SmbGlobalMapping en máquinas de Windows RS5 que impide el montaje de volúmenes AzureFile. A continuación se muestra un error de ejemplo que se produce cuando el volumen basado en AzureFile se está montando en el equipo de desarrollo local.
```
Error event: SourceId='System.Hosting', Property='CodePackageActivation:counterService:EntryPoint:131884291000691067'.
There was an error during CodePackage activation.System.Fabric.FabricException (-2147017731)
Failed to start Container. ContainerName=sf-2-63fc668f-362d-4220-873d-85abaaacc83e_6d6879cf-dd43-4092-887d-17d23ed9cc78, ApplicationId=SingleInstance_0_App2, ApplicationName=fabric:/counterApp. DockerRequest returned StatusCode=InternalServerError with ResponseBody={"message":"error while mounting volume '': mount failed"}
```
La solución para el problema es 1) ejecutar el comando siguiente como administrador de PowerShell y 2) reiniciar el equipo.
```powershell
PS C:\WINDOWS\system32> Mofcomp c:\windows\system32\wbem\smbwmiv2.mof
```

Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar este artículo. 

Para usar la CLI de Azure localmente con este artículo, asegúrese de que `az --version` devuelve al menos `azure-cli (2.0.43)`.  Instale (o actualice) el módulo de extensión de la CLI de Azure Service Fabric Mesh siguiendo estas [instrucciones](service-fabric-mesh-howto-setup-cli.md).

Para iniciar sesión en Azure y configurar la suscripción:

```azurecli
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-storage-account-and-file-share-optional"></a>Creación de una cuenta de almacenamiento y un recurso compartido de archivos (opcional)
El montaje de un volumen de Azure Files requiere una cuenta de almacenamiento y un recurso compartido de archivos.  Puede usar una cuenta de almacenamiento de Azure y un recurso compartido de archivos existentes o bien crear recursos:

```azurecli-interactive
az group create --name myResourceGroup --location eastus

az storage account create --name myStorageAccount --resource-group myResourceGroup --location eastus --sku Standard_LRS --kind StorageV2

$current_env_conn_string=$(az storage account show-connection-string -n myStorageAccount -g myResourceGroup --query 'connectionString' -o tsv)

az storage share create --name myshare --quota 2048 --connection-string $current_env_conn_string
```

## <a name="get-the-storage-account-name-and-key-and-the-file-share-name"></a>Obtener el nombre y la clave de la cuenta de almacenamiento y el nombre del recurso compartido de archivos
En las secciones siguientes el nombre de la cuenta de almacenamiento, la clave de la cuenta de almacenamiento y el nombre del recurso compartido de archivos se denominan `<storageAccountName>`, `<storageAccountKey>` y `<fileShareName>`. 

Enumere las cuentas de almacenamiento y obtenga el nombre de la cuenta de almacenamiento con el recurso compartido de archivos que desea utilizar:
```azurecli-interactive
az storage account list
```

Obtenga el nombre del recurso compartido de archivos:
```azurecli-interactive
az storage share list --account-name <storageAccountName>
```

Obtenga la clave de la cuenta de almacenamiento ("key1"):
```azurecli-interactive
az storage account keys list --account-name <storageAccountName> --query "[?keyName=='key1'].value"
```

También puede encontrar estos valores en [Azure Portal](https://portal.azure.com):
* `<storageAccountName>`: en **Cuentas de almacenamiento**, el nombre de la cuenta de almacenamiento utilizada para crear el recurso compartido de archivos.
* `<storageAccountKey>` -Seleccione la cuenta de almacenamiento en **Cuentas de almacenamiento** y después seleccione **Claves de acceso** y utilice el valor de **key1**.
* `<fileShareName>` - Seleccione la cuenta de almacenamiento en **Cuentas de almacenamiento** y después seleccione **Archivos**. El nombre que se usa es el del recurso compartido de archivos que ha creado.

## <a name="declare-a-volume-resource-and-update-the-service-resource-json"></a>Declaración de un recurso de volumen y actualización del recurso de servicio (JSON)

Agregue parámetros para los valores `<fileShareName>`, `<storageAccountName>` y `<storageAccountKey>` que puede encontrar en un paso anterior. 

Cree un recurso de volumen como un elemento del mismo nivel del recurso de aplicación. Especifique un nombre y el proveedor ("SFAzureFile" para usar el volumen basado en Azure Files). En `azureFileParameters`, especifique los parámetros para los valores `<fileShareName>`, `<storageAccountName>` y `<storageAccountKey>` que puede encontrar en un paso anterior.

Para montar el volumen en el servicio, agregue `volumeRefs` al elemento del servicio `codePackages`.  `name` es el identificador de recurso para el volumen o un parámetro de plantilla de implementación para el recurso de volumen, y el nombre del volumen se declara en el archivo de recursos volume.yaml.  `destinationPath` es el directorio local en el que se montará el volumen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "defaultValue": "EastUS",
      "type": "String",
      "metadata": {
        "description": "Location of the resources."
      }
    },
    "fileShareName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Azure Files file share that provides the volume for the container."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Azure storage account that contains the file share."
      }
    },
    "storageAccountKey": {
      "type": "securestring",
      "metadata": {
        "description": "Access key for the Azure storage account that contains the file share."
      }
    },
    "stateFolderName": {
      "type": "string",
      "defaultValue": "TestVolumeData",
      "metadata": {
        "description": "Folder in which to store the state. Provide an empty value to create a unique folder for each container to store the state. A non-empty value will retain the state across deployments, however if more than one applications are using the same folder, the counter may update more frequently."
      }
    }
  },
  "resources": [
    {
      "apiVersion": "2018-09-01-preview",
      "name": "VolumeTest",
      "type": "Microsoft.ServiceFabricMesh/applications",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/networks/VolumeTestNetwork",
        "Microsoft.ServiceFabricMesh/volumes/testVolume"
      ],
      "properties": {
        "services": [
          {
            "name": "VolumeTestService",
            "properties": {
              "description": "VolumeTestService description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "VolumeTestService",
                  "image": "volumetestservice:dev",
                  "volumeRefs": [
                    {
                      "name": "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'testVolume')]",
                      "destinationPath": "C:\\app\\data"
                    }
                  ],
                  "environmentVariables": [
                    {
                      "name": "ASPNETCORE_URLS",
                      "value": "http://+:20003"
                    },
                    {
                      "name": "STATE_FOLDER_NAME",
                      "value": "[parameters('stateFolderName')]"
                    }
                  ],
                  ...
                }
              ],
              ...
            }
          }
        ],
        "description": "VolumeTest description."
      }
    },
    {
      "apiVersion": "2018-09-01-preview",
      "name": "testVolume",
      "type": "Microsoft.ServiceFabricMesh/volumes",
      "location": "[parameters('location')]",
      "dependsOn": [],
      "properties": {
        "description": "Azure Files storage volume for the test application.",
        "provider": "SFAzureFile",
        "azureFileParameters": {
          "shareName": "[parameters('fileShareName')]",
          "accountName": "[parameters('storageAccountName')]",
          "accountKey": "[parameters('storageAccountKey')]"
        }
      }
    }
    ...
  ]
}
```

## <a name="declare-a-volume-resource-and-update-the-service-resource-yaml"></a>Declaración de un recurso de volumen y actualización del recurso de servicio (YAML)

Agregue un nuevo archivo *volume.yaml* en el directorio *Recursos de aplicación* de la aplicación.  Especifique un nombre y el proveedor ("SFAzureFile" para usar el volumen basado en Azure Files). `<fileShareName>`, `<storageAccountName>` y `<storageAccountKey>` son los valores que puede encontrar en un paso anterior.

```yaml
volume:
  schemaVersion: 1.0.0-preview2
  name: testVolume
  properties:
    description: Azure Files storage volume for counter App.
    provider: SFAzureFile
    azureFileParameters: 
        shareName: <fileShareName>
        accountName: <storageAccountName>
        accountKey: <storageAccountKey>
```

Actualice el archivo *service.yaml* en el directorio *Recursos de servicio* para montar el volumen en el servicio.  Agregue el elemento `volumeRefs` al elemento `codePackages`.  `name` es el identificador de recurso para el volumen o un parámetro de plantilla de implementación para el recurso de volumen, y el nombre del volumen se declara en el archivo de recursos volume.yaml.  `destinationPath` es el directorio local en el que se montará el volumen.

```yaml
## Service definition ##
application:
  schemaVersion: 1.0.0-preview2
  name: VolumeTest
  properties:
    services:
      - name: VolumeTestService
        properties:
          description: VolumeTestService description.
          osType: Windows
          codePackages:
            - name: VolumeTestService
              image: volumetestservice:dev
              volumeRefs:
                - name: "[resourceId('Microsoft.ServiceFabricMesh/volumes', 'testVolume')]"
                  destinationPath: C:\app\data
              endpoints:
                - name: VolumeTestServiceListener
                  port: 20003
              environmentVariables:
                - name: ASPNETCORE_URLS
                  value: http://+:20003
                - name: STATE_FOLDER_NAME
                  value: TestVolumeData
              resources:
                requests:
                  cpu: 0.5
                  memoryInGB: 1
          replicaCount: 1
          networkRefs:
            - name: VolumeTestNetwork
```

## <a name="next-steps"></a>Pasos siguientes

- Compruebe la aplicación de ejemplo del volumen de Azure Files en [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Para obtener más información sobre el modelo de recursos de Service Fabric, consulte el [modelo de recursos de Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
- Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).
