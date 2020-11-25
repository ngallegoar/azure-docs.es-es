---
title: Impacto de la actualización de enero de 2021 de Azure Stack Edge | Microsoft Docs
description: En este artículo se describe el impacto de la administración de roles de IoT Edge en dispositivos de Azure Stack Edge después de la instalación de la actualización de enero de 2021.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 10/26/2020
ms.author: alkohli
ms.openlocfilehash: f16f33e9aadcc01427602a1bd81f81cb0710e4dd
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578746"
---
# <a name="iot-edge-role-management-changes-for-your-azure-stack-edge-device"></a>Cambios en la administración de roles de IoT Edge para el dispositivo de Azure Stack Edge

En el caso de la administración de roles de Azure IoT Edge para el dispositivo de Azure Stack Edge, use la versión más reciente de la API, el SDK y Azure PowerShell, que se lanzará en enero de 2021. 

En este artículo se describe en detalle los cambios que debe realizar al usar esta versión más reciente.

La actualización de enero de 2021 solo estará disponible para los dispositivos de Azure Stack Edge: GPU, Azure Stack Edge Pro R y Azure Stack Edge Mini R. La información de este artículo se aplica solo a estos dispositivos.

> [!NOTE]
> No es obligatorio que actualice a la versión de enero de 2021. Si decide seguir usando la versión actual, no se producirá ningún efecto en la administración de roles de IoT Edge. Sin embargo, para aprovechar las nuevas características y reducir los riesgos de seguridad, se recomienda instalar la versión más reciente. 

## <a name="iot-edge-role-management-changes"></a>Cambios en la administración de roles de IoT Edge

Después de instalar la actualización opcional de enero de 2021 en el dispositivo de Azure Stack Edge, es preciso usar la versión más reciente de los cmdlets de API, SDK y PowerShell para la administración de roles de IoT Edge.

Los cambios siguientes solo son necesarios si se aplica la actualización de enero de 2021:

- Si actualmente usa la API de administración de roles versión&nbsp;2019-08-01, actualice a la versión de la API que se publicará en enero de 2021. 
- Si actualmente usa la administración de roles mediante el SDK versión&nbsp;1.0.0, actualice a la versión de la API que se publicará en enero de 2021.
- Si usa la administración de roles con los cmdlets de Azure PowerShell (versión preliminar) como, por ejemplo, `Get-AzStackEdgeRole`, `New-AzStackEdgeRole`, `Set-AzStackEdgeRole` o `Remove-AzStackEdgeRole`, espere a los nuevos cmdlets que se publicarán en febrero de 2021.

## <a name="api-usage"></a>Uso de la API

Si actualmente realiza la administración de roles de IoT Edge mediante la API, debe usar la nueva versión de esta (2020-12-01) que se publicará más adelante. Si usa la versión de Role API actual, después de instalar la próxima versión del software del dispositivo, debe volver a los roles PUT, GET y DELETE de Kubernetes seguidos por PUT IoT Addon API.

### <a name="for-the-put-method"></a>Para el método PUT

#### <a name="the-current-http-request"></a>Solicitud HTTP actual 

- Las llamadas a la API se realizan en este identificador URI: "https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01"

- El cuerpo de la solicitud es como este:

    ```json
    {
        "kind": "IOT",
        "properties": {
            "hostPlatform": "Linux",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotDevice;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "348586569999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotEdge;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "1245475856069999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "shareMappings": [],
            "roleStatus": "Enabled"
        }
    }
    ```

#### <a name="the-upcoming-http-request"></a>La próxima solicitud HTTP 

- Las llamadas de la API al rol de Kubernetes se realizan en el siguiente identificador URI: 

  'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1?api-version=2020-12-01'

    El cuerpo de la solicitud presentará un aspecto similar a este:

    ```json
    {
        "kind": "Kubernetes",
        "properties": {
            "hostPlatform": "Linux",
            "kubernetesClusterInfo": {
                "version": "v1.17.3"
            },
            "kubernetesRoleResources": {
                "storage": {
                    "endpoints": []
                },
                "compute": {
                    "vmProfile": "DS1_v2"
                }
            }
        }
    }
    ```

- Las llamadas de la API al complemento de IoT Edge se realizan en el siguiente identificador URI: 

   'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01'

    El cuerpo de la solicitud presentará un aspecto similar a este:

    ```json
    {
        "kind": "IoT",
        "properties": {
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotDevice;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "348586569999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotEdge;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "1245475856069999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            }
        }
    }
    ```


### <a name="for-the-get-method"></a>Para el método GET

#### <a name="the-current-http-response"></a>La respuesta HTTP actual

- Las llamadas a la API se realizan en el siguiente identificador URI: 

   'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01'

- El cuerpo de la respuesta es como este:

    ```json
        "kind": "IOT",
        "properties": {
            "hostPlatform": "Linux",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "shareMappings": [],
            "roleStatus": "Enabled"
        },
        "id": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1",
        "name": "IoTRole1",
        "type": "dataBoxEdgeDevices/roles"
    }    
    ```

#### <a name="the-upcoming-http-response"></a>La próxima respuesta HTTP

- Las llamadas a la API se realizan en el siguiente identificador URI:  

   'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01'

- El cuerpo de la respuesta es como este: 

    ```json
    {
        "kind": "IoT",
        "properties": {
            "provisioningState": "Creating",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "version": "0.1.0-beta10"
        },
        "id": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/res1/roles/kubernetesRole/addons/iotName",
        "name": " iotName",
        "type": "Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/addon",
    }
    ```

### <a name="for-the-delete-method"></a>Para el método DELETE

### <a name="the-current-api-calls"></a>Las llamadas API actuales

Las llamadas a la API se realizan en el siguiente identificador URI:  

'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01'

### <a name="the-upcoming-api-calls"></a>Las próximas llamadas API

Las llamadas a la API se realizan en el siguiente identificador URI:  

'https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01'

## <a name="sdk-usage"></a>Uso del SDK

Si usa el SDK después de haber instalado la actualización de enero de 2021, deberá cambiar la forma en que se configura el rol de IoT Edge como se muestra en el ejemplo siguiente. Después, deberá descargar e instalar el próximo paquete NuGet para pasar al nuevo SDK, como se indica aquí.

### <a name="the-current-sdk-sample"></a>El ejemplo actual del SDK

```csharp
var iotRoleStatus = "Enabled";
var iotHostPlatform = "Linux";
var id = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/iotrole"; 
var name = "iotrole";
var type = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/role";
var iotRoleName = "iotrole";
var ioTDeviceDetails = new IoTDeviceInfo(...);
var ioTEdgeDeviceDetails = new IoTDeviceInfo(...);
var ioTEdgeAgentInfo = new IoTEdgeAgentInfo(...);
var shareMappings = new List<MountPointMap>(...);

var role = new IoTRole(roleStatus, 
    hostPlatform, 
    shareMappings, 
    ioTDeviceDetails, 
    ioTEdgeDeviceDetails, 
    ioTEdgeAgentInfo, 
    id, 
    name, 
    type);

DataBoxEdgeManagementClient.Roles.CreateOrUpdate(deviceName, iotRoleName, role, resourceGroup);
```


### <a name="the-new-sdk-sample"></a>El nuevo ejemplo del SDK

```csharp
var k8sRoleStatus = "Enabled";
var k8sHostPlatform = "Linux";
var k8sId = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/KubernetesRole"; 
var k8sRoleName = "KubernetesRole";
var k8sClusterVersion = "v1.17.3"; //Final values will be updated here around January 2021
var k8sVmProfile = "DS1_v2"; //Final values will be updated here around January 2021
var type = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/role";
var k8sRole = new KubernetesRole(
    roleStatus,
    hostPlatform,
    shareMappings,
    k8sClusterVersion,
    k8sVmProfile,
    k8sId,
    k8sRoleName,
    type
);
DataBoxEdgeManagementClient.Roles.CreateOrUpdate(deviceName, k8sRoleName, k8sRole, resourceGroup); //Final usage will be updated here around January 2021

var ioTId = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/KubernetesRole/addons/iotaddon";
var ioTAddonName = "iotaddon";
var ioTAddonType = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/addons";
var addon = new IoTAddon(
    ioTDeviceDetails, 
    ioTEdgeDeviceDetails, 
    ioTEdgeAgentInfo, 
    ioTId, 
    ioTAddonName, 
    ioTAddonType);
DataBoxEdgeManagementClient.AddOns.CreateOrUpdate(deviceName, k8sRoleName, addonName, addon, resourceGroup); //Final usage will be updated here around January 2021
```

## <a name="cmdlet-usage"></a>Uso de cmdlets

Si actualmente usa el cmdlet `Get-AzStackEdgeRole`, `New-AzStackEdgeRole`, `Set-AzStackEdgeRole` o `Remove-AzStackEdgeRole`, deberá esperar a la versión nueva que está prevista para febrero de 2021.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**Uso de Azure Stack Edge Pro: FPGA. ¿Afecta la actualización de enero de 2021 al modelo de FPGA?**

No. La actualización de enero de 2021 solo se aplica a estos dispositivos de Azure Stack Edge Pro: FPGA, Azure Stack Edge Pro R y Azure Stack Edge Mini R. Azure Stack Edge Pro: FPGA no resulta afectado por esta actualización y no requiere ningún cambio en la administración de roles de IoT Edge.

**Después de actualizar Azure Stack Edge Pro: GPU al nuevo software del dispositivo en enero de 2021, ¿se ven afectados los servicios existentes?**

No. Los servicios configurados no se verán afectados después de instalar la actualización del dispositivo de enero de 2021.

**¿Cuáles son los cambios de alto nivel de la API, el SDK o el cmdlet de administración de IoT Edge?**

IoT Edge es un complemento del rol de Kubernetes, lo que implica que primero debe asegurarse de que Kubernetes esté configurado y, a continuación, realizar la configuración de IoT Edge.


## <a name="next-steps"></a>Pasos siguientes

- [Aprenda a aplicar las actualizaciones](azure-stack-edge-gpu-install-update.md)

