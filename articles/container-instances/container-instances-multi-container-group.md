---
title: 'Tutorial: Implementación de un grupo de varios contenedores: plantilla'
description: En este tutorial, obtendrá información sobre cómo implementar un grupo de contenedores con varios contenedores en Azure Container Instances mediante una plantilla de Azure Resource Manager con la CLI de Azure.
ms.topic: article
ms.date: 04/03/2019
ms.custom: mvc
ms.openlocfilehash: d2b4e20520cad28c5d62118f6c9d10fcc43ac89e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533632"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-resource-manager-template"></a>Tutorial: Implementación de un grupo con varios contenedores con una plantilla de Resource Manager

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [Resource Manager](container-instances-multi-container-group.md)

Azure Container Instances admite la implementación de varios contenedores en un solo host mediante un [grupo de contenedores](container-instances-container-groups.md). Un grupo de contenedores es útil cuando se crea un sidecar de aplicación para el registro, la supervisión o cualquier otra configuración donde un servicio necesite un segundo proceso asociado.

En este tutorial, seguirá los pasos para ejecutar una configuración de sidecar de dos contenedores sencilla mediante la implementación de una plantilla de Azure Resource Manager con la CLI de Azure. Aprenderá a:

> [!div class="checklist"]
> * Configurar una plantilla de grupo de varios contenedores
> * Implementación del grupo de contenedores
> * Ver los registros de los contenedores

Una plantilla de Resource Manager se puede adaptar rápidamente para escenarios en los que es necesario implementar recursos de un servicio de Azure adicionales (por ejemplo, un recurso compartido de Azure Files o una red virtual) con el grupo de contenedores. 

> [!NOTE]
> Los grupos de varios contenedores están restringidos actualmente a los contenedores Linux. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-template"></a>Configuración de una plantilla

Para empezar, copie el siguiente código JSON en un nuevo archivo denominado `azuredeploy.json`. En Azure Cloud Shell, puede usar Visual Studio Code para crear el archivo en el directorio de trabajo:

```
code azuredeploy.json
```

Esta plantilla de Resource Manager define un grupo de contenedores con dos contenedores, una dirección IP pública y dos puertos expuestos. El primer contenedor del grupo ejecuta una aplicación web accesible desde Internet. El segundo contenedor, el sidecar, realiza una solicitud HTTP a la aplicación web principal a través de la red local del grupo.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

Para usar un registro de imagen de contenedor privado, agregue un objeto al documento JSON con el formato siguiente. Para ver una implementación de ejemplo de esta configuración, consulte el documento [Referencia de plantilla de Resource Manager de ACI][template-reference].

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>Implementación de la plantilla

Para crear un grupo de recursos, use el comando [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implemente la plantilla con el comando [az group deployment create][az-group-deployment-create].

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
```

Al cabo de unos segundos, debe recibir una respuesta inicial de Azure.

## <a name="view-deployment-state"></a>Visualización del estado de la implementación

Para ver el estado de la implementación, use el siguiente comando [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Si desea ver la aplicación en ejecución, vaya a su dirección IP en el explorador. Por ejemplo, la dirección IP es `52.168.26.124` en esta salida de ejemplo:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-container-logs"></a>Visualización de registros de contenedores

Visualice la salida del registro de un contenedor con el comando [az container logs][az-container-logs]. El argumento `--container-name` especifica el contenedor del que se van a extraer registros. En este ejemplo, se especifica el contenedor `aci-tutorial-app`.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Salida:

```bash
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Para ver los registros para el contenedor sidecar, ejecute un comando similar en el que se especifique el contenedor `aci-tutorial-sidecar`.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Salida:

```bash
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Como puede ver, el sidecar realiza periódicamente una solicitud HTTP a la aplicación web principal a través de la red local del grupo para asegurarse de que se está ejecutando. Este ejemplo de sidecar se podría ampliar para desencadenar una alerta si recibe un código de respuesta HTTP distinto de `200 OK`.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha usado una plantilla de Azure Resource Manager para implementar un grupo de varios contenedores en Azure Container Instances. Ha aprendido a:

> [!div class="checklist"]
> * Configurar una plantilla de grupo de varios contenedores
> * Implementación del grupo de contenedores
> * Ver los registros de los contenedores

Para obtener ejemplos de plantilla adicionales, vea [Plantillas de Azure Resource Manager para Azure Container Instances](container-instances-samples-rm.md).

También puede especificar un grupo de varios contenedores mediante un [archivo YAML](container-instances-multi-container-yaml.md). Dada la naturaleza más concisa del formato YAML, la implementación con un archivo YAML es una buena opción cuando la implementación solo incluye instancias de contenedor.


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
