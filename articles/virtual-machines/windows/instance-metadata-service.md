---
title: Azure Instance Metadata Service para Windows
description: Obtenga información sobre Azure Instance Metadata Service y cómo proporciona información sobre las instancias de máquina virtual que se ejecutan actualmente en Windows.
services: virtual-machines
author: KumariSupriya
manager: paulmey
ms.service: virtual-machines
ms.subservice: monitoring
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 03/30/2020
ms.author: sukumari
ms.reviewer: azmetadatadev
ms.openlocfilehash: cc0d47807101a3cfbb26e7ea69cc7d117d3f9b31
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96435236"
---
# <a name="azure-instance-metadata-service"></a>Servicio de metadatos de instancia de Azure

Azure Instance Metadata Service (IMDS) le proporciona información sobre las instancias de máquina virtual que se ejecutan actualmente. Puede usarlo para administrar y configurar las máquinas virtuales.
Esta información incluye las SKU, almacenamiento, configuraciones de red y próximos eventos de mantenimiento. Para obtener una lista completa de los datos que están disponibles, consulte las [API de metadatos](#metadata-apis).


IMDS está disponible para ejecutar instancias de máquinas virtuales (VM) e instancias de un conjunto de escalado de máquinas virtuales. Todas las API admiten VM creadas o administradas mediante [Azure Resource Manager](/rest/api/resources/). Solo los puntos de conexión de red y atestiguados admiten las VM creadas mediante el modelo de implementación clásica. El punto de conexión de atestiguados solo lo hace solo de forma limitada.

IMDS es un punto de conexión de REST que está disponible en una dirección IP conocida y no enrutable (`169.254.169.254`). Solo puede obtener acceso a ella desde la VM. La comunicación entre la VM e IMDS nunca sale del host.
Puede sugerir a sus clientes HTTP que omitan los servidores proxy web dentro de la VM al consultar a IMDS y que consideren que `169.254.169.254` es lo mismo que [`168.63.129.16`](../../virtual-network/what-is-ip-address-168-63-129-16.md).

## <a name="security"></a>Seguridad

El punto de conexión de IMDS solo es accesible desde la instancia de máquina virtual en ejecución en una dirección IP no enrutable. Además, el servicio rechaza cualquier solicitud que tenga un encabezado `X-Forwarded-For`.
Asimismo, las solicitudes tienen que incluir también un encabezado `Metadata: true` para garantizar que la solicitud actual esté dirigida directamente y no como parte de un redireccionamiento accidental.

> [!IMPORTANT]
> Recuerde que IMDS no es un canal para datos confidenciales. El punto de conexión está abierto a todos los procesos de la VM. La información expuesta a través de este servicio se debe considerar como información compartida con todas las aplicaciones que se ejecutan dentro de la VM.

## <a name="usage"></a>Uso

### <a name="access-azure-instance-metadata-service"></a>Acceso a Azure Instance Metadata Service

Para obtener acceso a IMDS, cree una VM desde [Azure Resource Manager](/rest/api/resources/) o [Azure Portal](https://portal.azure.com) y siga los ejemplos que tiene a continuación.
Para obtener más ejemplos, consulte los [Ejemplos de Azure Instance Metadata](https://github.com/microsoft/azureimds).

Puede usar este código de ejemplo para recuperar todos los metadatos de una instancia. Para obtener acceso a un origen de datos específico, consulte la sección [API de metadatos](#metadata-apis). 

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance?api-version=2020-09-01 | ConvertTo-Json
```
> [!NOTE]
> La marca `-NoProxy` solo está disponible en PowerShell 6 o una versión posterior. Puede omitir la marca si no tiene una configuración de proxy.

**Respuesta**

> [!NOTE]
> La respuesta es una cadena JSON. Canalice la consulta REST a través del cmdlet `ConvertTo-Json` para la impresión con sangría.

```json
{
    "compute": {
        "azEnvironment": "AZUREPUBLICCLOUD",
        "isHostCompatibilityLayerVm": "true",
        "licenseType":  "Windows_Client",
        "location": "westus",
        "name": "examplevmname",
        "offer": "Windows",
        "osProfile": {
            "adminUsername": "admin",
            "computerName": "examplevmname",
            "disablePasswordAuthentication": "true"
        },
        "osType": "linux",
        "placementGroupId": "f67c14ab-e92c-408c-ae2d-da15866ec79a",
        "plan": {
            "name": "planName",
            "product": "planProduct",
            "publisher": "planPublisher"
        },
        "platformFaultDomain": "36",
        "platformUpdateDomain": "42",
        "publicKeys": [{
                "keyData": "ssh-rsa 0",
                "path": "/home/user/.ssh/authorized_keys0"
            },
            {
                "keyData": "ssh-rsa 1",
                "path": "/home/user/.ssh/authorized_keys1"
            }
        ],
        "publisher": "RDFE-Test-Microsoft-Windows-Server-Group",
        "resourceGroupName": "macikgo-test-may-23",
        "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/virtualMachines/examplevmname",
        "securityProfile": {
            "secureBootEnabled": "true",
            "virtualTpmEnabled": "false"
        },
        "sku": "Windows-Server-2012-R2-Datacenter",
        "storageProfile": {
            "dataDisks": [{
                "caching": "None",
                "createOption": "Empty",
                "diskSizeGB": "1024",
                "image": {
                    "uri": ""
                },
                "lun": "0",
                "managedDisk": {
                    "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
                    "storageAccountType": "Standard_LRS"
                },
                "name": "exampledatadiskname",
                "vhd": {
                    "uri": ""
                },
                "writeAcceleratorEnabled": "false"
            }],
            "imageReference": {
                "id": "",
                "offer": "UbuntuServer",
                "publisher": "Canonical",
                "sku": "16.04.0-LTS",
                "version": "latest"
            },
            "osDisk": {
                "caching": "ReadWrite",
                "createOption": "FromImage",
                "diskSizeGB": "30",
                "diffDiskSettings": {
                    "option": "Local"
                },
                "encryptionSettings": {
                    "enabled": "false"
                },
                "image": {
                    "uri": ""
                },
                "managedDisk": {
                    "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
                    "storageAccountType": "Standard_LRS"
                },
                "name": "exampleosdiskname",
                "osType": "Linux",
                "vhd": {
                    "uri": ""
                },
                "writeAcceleratorEnabled": "false"
            }
        },
        "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
        "tags": "baz:bash;foo:bar",
        "version": "15.05.22",
        "vmId": "02aab8a4-74ef-476e-8182-f6d2ba4166a6",
        "vmScaleSetName": "crpteste9vflji9",
        "vmSize": "Standard_A3",
        "zone": ""
    },
    "network": {
        "interface": [{
            "ipv4": {
               "ipAddress": [{
                    "privateIpAddress": "10.144.133.132",
                    "publicIpAddress": ""
                }],
                "subnet": [{
                    "address": "10.144.133.128",
                    "prefix": "26"
                }]
            },
            "ipv6": {
                "ipAddress": [
                 ]
            },
            "macAddress": "0011AAFFBB22"
        }]
    }
}
```

### <a name="data-output"></a>Salida de datos

De forma predeterminada, IMDS devuelve datos en formato JSON (`Content-Type: application/json`). Sin embargo, las distintas API pueden devolver los datos en formatos diferentes si se solicita.
En la tabla siguiente se enumeran otros formatos de datos que las API pueden admitir.

API | Formato predeterminado de datos | Otros formatos
--------|---------------------|--------------
/attested | json | None
/identity | json | None
/instance | json | text
/scheduledevents | json | None

Para acceder a un formato de respuesta que no sea el predeterminado, especifique el formato solicitado como un parámetro de cadena de consulta en la solicitud. Por ejemplo:

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

> [!NOTE]
> En el caso de los nodos hoja de `/metadata/instance`, el elemento `format=json` no funciona. Para estas consultas, `format=text` debe especificarse explícitamente ya que el formato predeterminado es JSON.

### <a name="version"></a>Versión

IMDS tiene varias versiones y es obligatorio especificar la versión de la API en la solicitud HTTP.

Las versiones de la API admitidas son: 
- 2017-03-01
- 2017-04-02
- 2017-08-01 
- 2017-10-01
- 2017-12-01 
- 2018-02-01
- 2018-04-02
- 2018-10-01
- 01-02-2019
- 2019-03-11
- 2019-04-30
- 2019-06-01
- 2019-06-04
- 2019-08-01
- 2019-08-15
- 2019-11-01
- 2020-06-01
- 2020-07-15
- 01-09-2020
- 2020-10-01

> [!NOTE]
> Es posible que la versión 2020-10-01 todavía no esté disponible en todas las regiones.

A medida que se agreguen versiones más recientes, todavía se podrá acceder a las versiones anteriores por motivos de compatibilidad si los scripts tienen dependencias en formatos de datos específicos.

Cuando no especifique la versión, obtendrá un error con una lista de las versiones admitidas más recientes.

> [!NOTE]
> La respuesta es una cadena JSON. En el ejemplo siguiente se indica la condición de error que se devuelve cuando no se especifica la versión. La respuesta se ha impreso con sangría para mejorar la legibilidad.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance
```

**Respuesta**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2020-10-01",
        "2020-09-01",
        "2020-07-15"
    ]
}
```

## <a name="metadata-apis"></a>API de metadatos

IMDS contiene varias API que representan distintos orígenes de datos.

API | Descripción | Versión introducida
----|-------------|-----------------------
/attested | Consulte [Datos atestiguados](#attested-data) | 2018-10-01
/identity | Consulte el artículo sobre cómo [obtener un token de acceso](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01
/instance | Vea [API de instancia](#instance-api). | 2017-04-02
/scheduledevents | Consulte [Eventos programados](scheduled-events.md) | 2017-08-01

## <a name="instance-api"></a>API de instancia

La API de instancia expone los metadatos importantes de las instancias de máquina virtual, como la máquina virtual, la red y el almacenamiento. Puede obtener acceso a las siguientes categorías a través de `instance/compute`:

data | Descripción | Versión introducida
-----|-------------|-----------------------
azEnvironment | Entorno de Azure en el que se ejecuta la VM. | 2018-10-01
customData | Esta funcionalidad actualmente está deshabilitada. | 01-02-2019
isHostCompatibilityLayerVm | Identifica si la VM se ejecuta en el nivel de compatibilidad del host. | 2020-06-01
licenseType | Tipo de licencia para la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit). Tenga en cuenta que esta opción solo está presente en las VM habilitadas para AHB. | 01-09-2020
ubicación | Región de Azure en la que se ejecuta la VM. | 2017-04-02
name | el nombre de la máquina virtual. | 2017-04-02
offer | Ofrece información para la imagen de máquina virtual. Esta opción solo está presente en las imágenes que se implementan desde la galería de imágenes de Azure. | 2017-04-02
osProfile.adminUsername | Especifica el nombre de la cuenta de administrador. | 2020-07-15
osProfile.computerName | Especifica el nombre del equipo. | 2020-07-15
osProfile.disablePasswordAuthentication | Especifica si se deshabilita la autenticación de contraseña. Tenga en cuenta que esto solo está presente en las VM Linux. | 2020-10-01
osType | Linux o Windows. | 2017-04-02
placementGroupId | [Grupo de selección de ubicación](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) de su conjunto de escalado de máquinas virtuales. | 2017-08-01
plan | [Plan](/rest/api/compute/virtualmachines/createorupdate#plan) que contiene el nombre, el producto y el editor de una VM si es una imagen de Azure Marketplace. | 2018-04-02
platformUpdateDomain |  [Dominio de actualización](../manage-availability.md) en el que se ejecuta la VM. | 2017-04-02
platformFaultDomain | [Dominio de error](../manage-availability.md) en el que se ejecuta la VM. | 2017-04-02
provider | Proveedor de la VM. | 2018-10-01
publicKeys | [Colección de claves públicas](/rest/api/compute/virtualmachines/createorupdate#sshpublickey) asignada a la VM y a las rutas de acceso. | 2018-04-02
publisher | Editor de la imagen de VM. | 2017-04-02
resourceGroupName | [Grupo de recursos](../../azure-resource-manager/management/overview.md) de la VM. | 2017-08-01
resourceId | Id. [completo](/rest/api/resources/resources/getbyid) del recurso. | 2019-03-11
sku | SKU específica de la imagen de VM. | 2017-04-02
securityProfile.secureBootEnabled | Identifica si el arranque seguro UEFI está habilitado en la VM. | 2020-06-01
securityProfile.virtualTpmEnabled | Identifica si el Módulo de plataforma segura (TPM) virtual está habilitado en la VM. | 2020-06-01
storageProfile | Consulte el [Perfil de almacenamiento](#storage-metadata). | 2019-06-01
subscriptionId | Opciones de suscripción de Azure para la VM. | 2017-08-01
etiquetas | [Etiquetas](../../azure-resource-manager/management/tag-resources.md) para la VM.  | 2017-08-01
tagsList | Etiquetas con formato de matriz de JSON para facilitar el análisis mediante programación.  | 2019-06-04
version | Versión de la imagen de VM. | 2017-04-02
vmId | [Identificador único](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) para la VM. | 2017-04-02
vmScaleSetName | [Nombre del conjunto de escalado de máquinas virtuales](../../virtual-machine-scale-sets/overview.md) de su conjunto de escalado de máquinas virtuales. | 2017-12-01
vmSize | Consulte el [Tamaño de VM](../sizes.md). | 2017-04-02
zona | [Zona de disponibilidad](../../availability-zones/az-overview.md) de la VM. | 2017-12-01

### <a name="sample-1-track-a-vm-running-on-azure"></a>Muestra 1: seguimiento de una VM que se ejecuta en Azure

Como proveedor de servicios, es posible que necesite hacer el seguimiento de la cantidad de VM que ejecutan su software o que tenga agentes que deban hacer el seguimiento de la unidad de la VM. Para poder obtener un id. único para una VM, use el campo `vmId` de IMDS.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Respuesta**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="sample-2-placement-of-different-data-replicas"></a>Ejemplo 2: ubicación de diferentes réplicas de datos

Para ciertos escenarios, la ubicación de las distintas réplicas de datos es de máxima importancia. Por ejemplo, para la [ubicación de réplicas de HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) o la ubicación de contenedores a través de un [orquestador](https://kubernetes.io/docs/user-guide/node-selection/), debe saber en qué `platformFaultDomain` y `platformUpdateDomain` se ejecuta la VM.
También puede usar las [zonas de disponibilidad](../../availability-zones/az-overview.md) para las instancias para tomar estas decisiones.
Puede consultar estos datos directamente a través de IMDS.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Respuesta**

```text
0
```

### <a name="sample-3-get-more-information-about-the-vm-during-support-case"></a>Ejemplo 3: obtención de más información sobre la VM durante el caso de soporte técnico

Como proveedor de servicios, es posible que reciba una llamada de soporte técnico en la que le gustaría tener más información sobre la VM. En este caso, pedirle al cliente que comparta los metadatos de proceso puede serle útil.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance/compute?api-version=2020-09-01
```

**Respuesta**

> [!NOTE]
> La respuesta es una cadena JSON. La respuesta de ejemplo siguiente se ha impreso correctamente para mejorar la legibilidad.

```json
{
    "azEnvironment": "AZUREPUBLICCLOUD",
    "isHostCompatibilityLayerVm": "true",
    "licenseType":  "Windows_Client",
    "location": "westus",
    "name": "examplevmname",
    "offer": "Windows",
    "osProfile": {
        "adminUsername": "admin",
        "computerName": "examplevmname",
        "disablePasswordAuthentication": "true"
    },
    "osType": "linux",
    "placementGroupId": "f67c14ab-e92c-408c-ae2d-da15866ec79a",
    "plan": {
        "name": "planName",
        "product": "planProduct",
        "publisher": "planPublisher"
    },
    "platformFaultDomain": "36",
    "platformUpdateDomain": "42",
    "publicKeys": [{
            "keyData": "ssh-rsa 0",
            "path": "/home/user/.ssh/authorized_keys0"
        },
        {
            "keyData": "ssh-rsa 1",
            "path": "/home/user/.ssh/authorized_keys1"
        }
    ],
    "publisher": "RDFE-Test-Microsoft-Windows-Server-Group",
    "resourceGroupName": "macikgo-test-may-23",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/virtualMachines/examplevmname",
    "securityProfile": {
        "secureBootEnabled": "true",
        "virtualTpmEnabled": "false"
    },
    "sku": "Windows-Server-2012-R2-Datacenter",
    "storageProfile": {
        "dataDisks": [{
            "caching": "None",
            "createOption": "Empty",
            "diskSizeGB": "1024",
            "image": {
                "uri": ""
            },
            "lun": "0",
            "managedDisk": {
                "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
                "storageAccountType": "Standard_LRS"
            },
            "name": "exampledatadiskname",
            "vhd": {
                "uri": ""
            },
            "writeAcceleratorEnabled": "false"
        }],
        "imageReference": {
            "id": "",
            "offer": "UbuntuServer",
            "publisher": "Canonical",
            "sku": "16.04.0-LTS",
            "version": "latest"
        },
        "osDisk": {
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "diskSizeGB": "30",
            "diffDiskSettings": {
                "option": "Local"
            },
            "encryptionSettings": {
                "enabled": "false"
            },
            "image": {
                "uri": ""
            },
            "managedDisk": {
                "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
                "storageAccountType": "Standard_LRS"
            },
            "name": "exampleosdiskname",
            "osType": "Linux",
            "vhd": {
                "uri": ""
            },
            "writeAcceleratorEnabled": "false"
        }
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "baz:bash;foo:bar",
    "version": "15.05.22",
    "vmId": "02aab8a4-74ef-476e-8182-f6d2ba4166a6",
    "vmScaleSetName": "crpteste9vflji9",
    "vmSize": "Standard_A3",
    "zone": ""
}
```

### <a name="sample-4-get-the-azure-environment-where-the-vm-is-running"></a>Ejemplo 4: obtención del entorno de Azure donde se está ejecutando la VM

Azure tiene varias nubes soberanas, como [Azure Government](https://azure.microsoft.com/overview/clouds/government/). En ocasiones, necesitará el entorno de Azure para tomar algunas decisiones acerca del tiempo de ejecución. En el siguiente ejemplo se muestra cómo lograr este comportamiento.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Respuesta**

```text
AzurePublicCloud
```

Aquí se enumeran las nubes y los valores del entorno de Azure.

 Nube   | Entorno de Azure
---------|-----------------
[Todas las regiones globales de Azure disponibles con carácter general](https://azure.microsoft.com/regions/)     | AzurePublicCloud
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | AzureUSGovernmentCloud
[Azure China 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)         | AzureChinaCloud
[Azure Alemania](https://azure.microsoft.com/overview/clouds/germany/)                    | AzureGermanCloud

## <a name="network-metadata"></a>Metadatos de red 

Los metadatos de red forman parte de la API de instancia. Las siguientes categorías de redes están disponibles a través del punto de conexión `instance/network`.

data | Descripción | Versión introducida
-----|-------------|-----------------------
ipv4/privateIpAddress | Dirección IPv4 local de la VM. | 2017-04-02
ipv4/publicIpAddress | Dirección IPv4 pública de la VM. | 2017-04-02
subnet/address | Dirección de subred de la VM. | 2017-04-02
subnet/prefix | Prefijo de la subred. Ejemplo: 24 | 2017-04-02
ipv6/ipAddress | Dirección IPv6 local de la VM. | 2017-04-02
macAddress | Dirección MAC de la VM. | 2017-04-02

> [!NOTE]
> Todas las respuestas de las API son cadenas JSON. Todas las respuestas de ejemplo siguientes se han imprimido correctamente para mejorar la legibilidad.

#### <a name="sample-1-retrieve-network-information"></a>Muestra 1: recuperación de información de red

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance/network?api-version=2017-08-01
```

**Respuesta**

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="sample-2-retrieve-public-ip-address"></a>Ejemplo 2: recuperación de la dirección IP pública

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

## <a name="storage-metadata"></a>Metadatos de almacenamiento

Los metadatos de almacenamiento forman parte de la API de instancia en el punto de conexión `instance/compute/storageProfile`.
Proporciona detalles sobre los discos de almacenamiento asociados a la máquina virtual. 

El perfil de almacenamiento de una VM se divide en tres categorías: referencia de imagen, disco del sistema operativo y discos de datos.

El objeto de referencia de imagen contiene la información siguiente sobre la imagen del sistema operativo:

data    | Descripción
--------|-----------------
id      | Id. de recurso
offer   | Oferta de la plataforma o imagen
publisher | Editor de la imagen
sku     | SKU de la imagen
version | Versión de la plataforma o imagen

El objeto de disco del sistema operativo contiene la siguiente información sobre el disco del sistema operativo que usa la VM:

data    | Descripción
--------|-----------------
caching | Requisitos de almacenamiento en caché
createOption | Información sobre cómo se ha creado la máquina virtual
diffDiskSettings | Configuración de discos efímeros
diskSizeGB | Tamaño del disco en GB
encryptionSettings | Configuración de cifrado para el disco
imagen   | Disco duro virtual de la imagen de usuario de origen
managedDisk | Parámetros de disco administrado
name    | Nombre del disco
osType  | Tipo de sistema operativo incluido en el disco
vhd     | Disco duro virtual
writeAcceleratorEnabled | Si `writeAccelerator` está habilitado o no en el disco

La matriz de discos de datos contiene una lista de discos de datos conectados a la máquina virtual. Cada objeto de disco de datos contiene la información siguiente:

data    | Descripción
--------|-----------------
caching | Requisitos de almacenamiento en caché
createOption | Información sobre cómo se ha creado la máquina virtual
diffDiskSettings | Configuración de discos efímeros
diskSizeGB | Tamaño del disco en GB
imagen   | Disco duro virtual de la imagen de usuario de origen
lun     | Número de unidad lógica del disco
managedDisk | Parámetros de disco administrado
name    | Nombre del disco
vhd     | Disco duro virtual
writeAcceleratorEnabled | Si `writeAccelerator` está habilitado o no en el disco

El ejemplo siguiente muestra cómo consultar la información de almacenamiento de la máquina virtual.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance/compute/storageProfile?api-version=2019-06-01
```

**Respuesta**

> [!NOTE]
> La respuesta es una cadena JSON. La respuesta de ejemplo siguiente se ha impreso correctamente para mejorar la legibilidad.

```json
{
    "dataDisks": [
      {
        "caching": "None",
        "createOption": "Empty",
        "diskSizeGB": "1024",
        "image": {
          "uri": ""
        },
        "lun": "0",
        "managedDisk": {
          "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
          "storageAccountType": "Standard_LRS"
        },
        "name": "exampledatadiskname",
        "vhd": {
          "uri": ""
        },
        "writeAcceleratorEnabled": "false"
      }
    ],
    "imageReference": {
      "id": "",
      "offer": "UbuntuServer",
      "publisher": "Canonical",
      "sku": "16.04.0-LTS",
      "version": "latest"
    },
    "osDisk": {
      "caching": "ReadWrite",
      "createOption": "FromImage",
      "diskSizeGB": "30",
      "diffDiskSettings": {
        "option": "Local"
      },
      "encryptionSettings": {
        "enabled": "false"
      },
      "image": {
        "uri": ""
      },
      "managedDisk": {
        "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
        "storageAccountType": "Standard_LRS"
      },
      "name": "exampleosdiskname",
      "osType": "Linux",
      "vhd": {
        "uri": ""
      },
      "writeAcceleratorEnabled": "false"
    }
}
```

## <a name="vm-tags"></a>Etiquetas de máquina virtual

Las etiquetas de VM se incluyen en la API de instancia en el punto de conexión `instance/compute/tags`.
Es posible que se hayan aplicado etiquetas a las VM de Azure para organizarlas de forma lógica en una taxonomía. Puede recuperar las etiquetas asignadas a una VM mediante la siguiente solicitud.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/instance/compute/tags?api-version=2018-10-01&format=text"
```

**Respuesta**

```text
Department:IT;Environment:Test;Role:WebRole
```

El campo `tags` es una cadena con las etiquetas delimitadas por puntos y coma. Esta salida puede ser un problema si se usan puntos y coma en las propias etiquetas. Si se escribe un analizador para extraer mediante programación las etiquetas, debe basarse en el campo `tagsList`. El campo `tagsList` es una matriz JSON sin delimitadores y, por tanto, es más fácil de analizar.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/instance/compute/tagsList?api-version=2019-06-04
```

**Respuesta**

```json
[
  {
    "name": "Department",
    "value": "IT"
  },
  {
    "name": "Environment",
    "value": "Test"
  },
  {
    "name": "Role",
    "value": "WebRole"
  }
]
```

## <a name="attested-data"></a>Datos atestiguados

IMDS le ofrece la garantía de que los datos proporcionados proceden de Azure. Microsoft firma parte de esta información, por lo que puede confirmar que una imagen de Azure Marketplace es la que se está ejecutando en Azure.

### <a name="sample-1-get-attested-data"></a>Muestra 1: obtención de los datos atestiguados

> [!NOTE]
> Todas las respuestas de las API son cadenas JSON. Las siguientes respuestas de ejemplo se han impreso correctamente para facilitar su lectura.

**Solicitud**

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"
```

> [!NOTE]
> Debido al mecanismo de almacenamiento en caché de IMDS, es posible que se devuelva un valor `nonce` previamente almacenado en caché.

`Api-version` es un campo obligatorio. Consulte la [sección de uso](#usage) para ver las versiones de API compatibles.
`Nonce` es una cadena opcional de 10 dígitos. Si no se proporciona, IMDS devuelve la marca de tiempo de la Hora universal coordinada actual en su lugar.

**Respuesta**

```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

El blob de firma es una versión con la firma [pkcs7](https://aka.ms/pkcs7) del documento. Contiene el certificado que se usa para firmar, junto con los detalles específicos de una VM determinada. 

En el caso de las VM creadas con Azure Resource Manager, esto incluye `vmId`, `sku`, `nonce`, `subscriptionId` y `timeStamp` para la creación y expiración del documento, así como la información del plan sobre la imagen. La información del plan solo se rellena para las imágenes de Azure Marketplace. 

En el caso de las VM creadas con el modelo de implementación clásico, solo se garantiza que se rellene `vmId`. Puede extraer el certificado de la respuesta y usarlo para confirmar que la respuesta es válida y procede de Azure.

El documento contiene estos campos:

data | Descripción | Versión introducida
-----|-------------|-----------------------
licenseType | Tipo de licencia para la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit). Tenga en cuenta que esta opción solo está presente en las VM habilitadas para AHB. | 01-09-2020
valor de seguridad | Una cadena que se puede proporcionar de manera opcional con la solicitud. Si no se ha proporcionado ningún elemento `nonce`, se usa la marca de tiempo de la Hora universal coordinada actual. | 2018-10-01
plan | El [plan de imagen de Azure Marketplace](/rest/api/compute/virtualmachines/createorupdate#plan). Contiene el identificador de plan (nombre), la oferta o imagen de producto (producto) y el identificador de publicador (publicador). | 2018-10-01
timestamp/createdOn | Esta es la marca de tiempo de la Hora universal coordinada para el momento en que se creó el documento firmado. | 2018-20-01
timestamp/expiresOn | Esta es la marca de tiempo de la Hora universal coordinada para el momento en que expire el documento firmado. | 2018-10-01
vmId |  [Identificador único](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) para la VM. | 2018-10-01
subscriptionId | Suscripción de Azure para la VM. | 2019-04-30
sku | SKU específica de la imagen de VM. | 2019-11-01

### <a name="sample-2-validate-that-the-vm-is-running-in-azure"></a>Ejemplo 2: validación de que la VM se ejecuta en Azure

Los proveedores de Azure Marketplace quieren asegurarse de que su software tiene licencia para ejecutarse solo en Azure. Si alguien copia el disco duro virtual en un entorno local, el proveedor debe ser capaz de detectarlo. A través de IMDS, estos proveedores pueden obtener datos firmados que garantizan que la respuesta es solo de Azure.

```powershell
# Get the signature
$attestedDoc = Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri http://169.254.169.254/metadata/attested/document?api-version=2020-09-01
# Decode the signature
$signature = [System.Convert]::FromBase64String($attestedDoc.signature)
```

Compruebe que la firma proviene de Microsoft Azure y si hay errores en la cadena de certificados.

```powershell
# Get certificate chain
$cert = [System.Security.Cryptography.X509Certificates.X509Certificate2]($signature)
$chain = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Chain
$chain.Build($cert)
# Print the Subject of each certificate in the chain
foreach($element in $chain.ChainElements)
{
    Write-Host $element.Certificate.Subject
}

# Get the content of the signed document
Add-Type -AssemblyName System.Security
$signedCms = New-Object -TypeName System.Security.Cryptography.Pkcs.SignedCms
$signedCms.Decode($signature);
$content = [System.Text.Encoding]::UTF8.GetString($signedCms.ContentInfo.Content)
Write-Host "Attested data: " $content
$json = $content | ConvertFrom-Json
# Do additional validation here
```

> [!NOTE]
> Debido al mecanismo de almacenamiento en caché de IMDS, es posible que se devuelva un valor `nonce` previamente almacenado en caché.

Se puede comparar el elemento `nonce` del documento firmado si proporcionó un parámetro `nonce` en la solicitud inicial.

> [!NOTE]
> El certificado para la nube pública y cada nube soberana será distinto.

Nube | Certificado
------|------------
[Todas las regiones globales de Azure disponibles con carácter general](https://azure.microsoft.com/regions/) | *.metadata.azure.com
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)          | *.metadata.azure.us
[Azure China 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)     | *.metadata.azure.cn
[Azure Alemania](https://azure.microsoft.com/overview/clouds/germany/)                | *.metadata.microsoftazure.de

> [!NOTE]
> Es posible que los certificados no tengan una coincidencia exacta de `metadata.azure.com` para la nube pública. Por lo tanto, la validación de la certificación debe permitir un nombre común de cualquier subdominio `.metadata.azure.com`.

En aquellos casos en los que el certificado intermedio no se puede descargar debido a las restricciones de red durante la validación, es posible anclarlo. Tenga en cuenta que Azure sustituye los certificados, ya que es una práctica estándar de PKI. Debe actualizar los certificados anclados cuando se produzca la sustitución. Cada vez que planee un cambio para actualizar el certificado intermedio, se actualizará el blog de Azure y se notificará a los clientes de Azure. 

Puede encontrar los certificados intermedios en el [repositorio de PKI](https://www.microsoft.com/pki/mscorp/cps/default.htm). Los certificados intermedios para cada una de las regiones pueden ser diferentes.

> [!NOTE]
> El certificado intermedio para Azure China 21Vianet es de la entidad de certificación raíz global de DigiCert en lugar de Baltimore.
Además, si había anclado los certificados intermedios para Azure China como parte del cambio de la entidad de la cadena raíz, los certificados intermedios tendrán que actualizarse.

## <a name="failover-clustering-in-windows-server"></a>Clústeres de conmutación por error de Windows Server

Cuando se consulta IMDS con los clústeres de conmutación por error, a veces es necesario agregar una ruta a la tabla de rutas. A continuación, se indica cómo puede hacerlo.

1. Abra un símbolo del sistema con privilegios de administrador.

1. Ejecute el siguiente comando y anote la dirección de la interfaz de red de destino (`0.0.0.0`) en la tabla de rutas IPv4.

```bat
route print
```

> [!NOTE]
> La salida de ejemplo siguiente es de una VM de Windows Server que tiene un clúster de conmutación por error habilitado. Para simplificar las cosas, la salida solo contiene la tabla de rutas IPv4.

```text
IPv4 Route Table
===========================================================================
Active Routes:
Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0         10.0.1.1        10.0.1.10    266
         10.0.1.0  255.255.255.192         On-link         10.0.1.10    266
        10.0.1.10  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.15  255.255.255.255         On-link         10.0.1.10    266
        10.0.1.63  255.255.255.255         On-link         10.0.1.10    266
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
      169.254.0.0      255.255.0.0         On-link     169.254.1.156    271
    169.254.1.156  255.255.255.255         On-link     169.254.1.156    271
  169.254.255.255  255.255.255.255         On-link     169.254.1.156    271
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     169.254.1.156    271
  255.255.255.255  255.255.255.255         On-link         10.0.1.10    266
```

Ejecute el siguiente comando y use la dirección de la interfaz de red de destino (`0.0.0.0`) que es (`10.0.1.10`) en el ejemplo.

```bat
route add 169.254.169.254/32 10.0.1.10 metric 1 -p
```

## <a name="managed-identity"></a>Identidad administrada

Se puede habilitar una identidad administrada que haya asignado el sistema en la VM. También puede asignar una o varias identidades administradas que haya asignado el usuario a la VM.
A continuación, puede solicitar tokens para identidades administradas desde IMDS. Estos tokens se pueden usar para autenticarse con otros servicios de Azure, como Azure Key Vault.

Para ver pasos detallados sobre cómo habilitar esta característica, consulte cómo [adquirir un token de acceso](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="scheduled-events"></a>Eventos programados
Puede obtener el estado de los eventos programados mediante IMDS. A continuación, el usuario puede especificar un conjunto de acciones para que se ejecuten en estos eventos. Para obtener más información, consulte [Eventos programados](scheduled-events.md). 

## <a name="regional-availability"></a>Disponibilidad regional

El servicio está disponible con carácter general en todas las nubes de Azure.

## <a name="sample-code-in-different-languages"></a>Código de ejemplo en diferentes lenguajes

En la tabla siguiente se muestran ejemplos de llamada a IMDS mediante el uso de diferentes idiomas en la VM:

Idioma      | Ejemplo
--------------|----------------
C++ (Windows) | https://github.com/Microsoft/azureimds/blob/master/IMDSSample-windows.cpp
C#            | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
Go            | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Java          | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
NodeJS        | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
Perl          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
PowerShell    | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Puppet        | https://github.com/keirans/azuremetadata
Python        | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
Ruby          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb
Visual Basic  | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.vb

## <a name="errors-and-debugging"></a>Errores y depuración

Si no se encuentra un elemento de datos o una solicitud con formato incorrecto, IMDS devuelve errores HTTP estándar. Por ejemplo:

Código de estado HTTP | Motivo
-----------------|-------
200 OK |
400 - Solicitud incorrecta | Falta el encabezado `Metadata: true` o el parámetro `format=json` al consultar un nodo hoja.
404 No encontrado  | El elemento solicitado no existe.
405 Método no permitido | Solo se admiten solicitudes `GET`.
410 Ya no existe | Vuelva a intentarlo en un rato, durante un máximo de 70 segundos.
429 Demasiadas solicitudes | La API es compatible actualmente con un máximo de cinco consultas por segundo.
500 Error de servicio     | Vuelva a intentarlo más tarde.

### <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**Obtengo el error `400 Bad Request, Required metadata header not specified`. ¿Qué significa?**

IMDS requiere que el encabezado `Metadata: true` se transmita en la solicitud. Transmitir este encabezado en la llamada de REST le permite obtener acceso a IMDS.

**¿Por qué no recibo información de proceso de mi máquina virtual?**

Actualmente IMDS solo admite instancias creadas con Azure Resource Manager.

**He creado mi VM mediante Azure Resource Manager hace algún tiempo. ¿Por qué no veo la información de metadatos del proceso?**

Si creó la VM después de septiembre del 2016, agregue una [etiqueta](../../azure-resource-manager/management/tag-resources.md) para empezar a ver los metadatos del proceso. Si creó la VM antes de septiembre de 2016, agregue extensiones o discos de datos en la instancia de VM (o quítelos) para actualizar los metadatos.

**¿Por qué no veo todos los datos rellenados para una nueva versión?**

Si creó la VM después de septiembre del 2016, agregue una [etiqueta](../../azure-resource-manager/management/tag-resources.md) para empezar a ver los metadatos del proceso. Si creó la VM antes de septiembre de 2016, agregue extensiones o discos de datos en la instancia de VM (o quítelos) para actualizar los metadatos.

**¿Por qué recibo el error `500 Internal Server Error` o `410 Resource Gone`?**

Vuelva a intentar la solicitud. Para obtener más información, consulte [Administración de errores transitorios](/azure/architecture/best-practices/transient-faults). Si el problema persiste, cree un problema de soporte técnico en Azure Portal para la VM.

**¿Esto funciona en las instancias del conjunto de escalado de máquinas virtuales?**

Sí, IMDS está disponible para las instancias del conjunto de escalado de máquinas virtuales.

**Actualicé mis etiquetas en el conjunto de escalado de máquinas virtuales, pero no aparecen en las instancias, a diferencia de las VM de instancia única. ¿Estoy haciendo algo mal?**

Actualmente, las etiquetas para los conjuntos de escalado de máquinas virtuales solo se muestran a la VM durante el reinicio, el restablecimiento de una imagen o el cambio de un disco en la instancia.

**¿Por qué se agotó el tiempo de espera de la solicitud de mi llamada al servicio?**

Las llamadas de metadatos se deben hacer desde la dirección IP principal asignada a la tarjeta de red principal de la máquina virtual. Además, si ha cambiado las rutas, debe haber una ruta para la dirección 169.254.169.254/32 en la tabla de rutas local de la VM.
   * <details>
        <summary>Comprobación de la tabla de rutas</summary>

        1. Vuelque la tabla de rutas local y busque la entrada de IMDS. Por ejemplo:
            ```console
            > route print
            IPv4 Route Table
            ===========================================================================
            Active Routes:
            Network Destination        Netmask          Gateway       Interface  Metric
                      0.0.0.0          0.0.0.0      172.16.69.1      172.16.69.7     10
                    127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
                    127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
              127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
                168.63.129.16  255.255.255.255      172.16.69.1      172.16.69.7     11
              169.254.169.254  255.255.255.255      172.16.69.1      172.16.69.7     11
            ... (continues) ...
            ```
        1. compruebe que existe una ruta para `169.254.169.254` y anote la interfaz de red correspondiente (por ejemplo, `172.16.69.7`).
        1. Vuelque la configuración de la interfaz, busque aquella interfaz que se corresponda con la que se menciona en la tabla de rutas y anote la dirección MAC (física).
            ```console
            > ipconfig /all
            ... (continues) ...
            Ethernet adapter Ethernet:

               Connection-specific DNS Suffix  . : xic3mnxjiefupcwr1mcs1rjiqa.cx.internal.cloudapp.net
               Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
               Physical Address. . . . . . . . . : 00-0D-3A-E5-1C-C0
               DHCP Enabled. . . . . . . . . . . : Yes
               Autoconfiguration Enabled . . . . : Yes
               Link-local IPv6 Address . . . . . : fe80::3166:ce5a:2bd5:a6d1%3(Preferred)
               IPv4 Address. . . . . . . . . . . : 172.16.69.7(Preferred)
               Subnet Mask . . . . . . . . . . . : 255.255.255.0
            ... (continues) ...
            ```
        1. Confirme que la interfaz corresponde a la NIC principal y la dirección IP principal de la máquina virtual. Para encontrar la NIC y la dirección IP principales, consulte la configuración de red en Azure Portal o búsquela con la CLI de Azure. Anote las direcciones IP públicas y privadas (y la dirección MAC, si usa la CLI). Aquí tiene un ejemplo de la CLI de PowerShell:
            ```powershell
            $ResourceGroup = '<Resource_Group>'
            $VmName = '<VM_Name>'
            $NicNames = az vm nic list --resource-group $ResourceGroup --vm-name $VmName | ConvertFrom-Json | Foreach-Object { $_.id.Split('/')[-1] }
            foreach($NicName in $NicNames)
            {
                $Nic = az vm nic show --resource-group $ResourceGroup --vm-name $VmName --nic $NicName | ConvertFrom-Json
                Write-Host $NicName, $Nic.primary, $Nic.macAddress
            }
            # Output: wintest767 True 00-0D-3A-E5-1C-C0
            ```
        1. Si no coinciden, actualice la tabla de rutas para que la NIC y la dirección IP principales estén dirigidas.
    </details>

## <a name="support"></a>Soporte técnico

Si no puede obtener una respuesta de metadatos después de varios intentos, puede crear un problema de soporte técnico en Azure Portal.
En **Tipo de problema**, seleccione **Administración**. En **Categoría**, seleccione **Instance Metadata Service**.

![Captura de pantalla de la compatibilidad con Instance Metadata Service](./media/instance-metadata-service/InstanceMetadata-support.png)

## <a name="next-steps"></a>Pasos siguientes

[Obtener un token de acceso para la VM](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md)

[Eventos programados](scheduled-events.md)
