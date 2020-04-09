---
title: Compatibilidad de Azure IoT Hub con redes virtuales
description: Cómo usar el patrón de conectividad de redes virtuales con IoT Hub
services: iot-hub
author: rezasherafat
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: rezas
ms.openlocfilehash: 34f66c13b0e7eb7092332a48744f9abfd8f0db80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501440"
---
# <a name="iot-hub-support-for-virtual-networks"></a>Compatibilidad de IoT Hub con redes virtuales

En este artículo se presenta el patrón de conectividad de red virtual y se explica cómo configurar una experiencia de conectividad privada para una instancia de IoT Hub a través de una red virtual de Azure propiedad del cliente.

> [!NOTE]
> Las características de IoT Hub que se describen en este artículo están disponibles actualmente para las instancias de IoT Hub [creadas con Managed Service Identity](#create-an-iot-hub-with-managed-service-identity) en las siguientes regiones: Este de EE. UU., Centro y Sur de EE. UU. y Oeste de EE. UU. 2.


## <a name="introduction"></a>Introducción

De forma predeterminada, los nombres de host de IoT Hub se asignan a un punto de conexión público con una dirección IP enrutable públicamente a través de Internet. Como se muestra en la ilustración siguiente, este punto de conexión público de IoT Hub se comparte entre varios concentradores propiedad de distintos clientes y es accesible para los dispositivos IoT a través de redes de área extensa, así como redes locales.

De forma similar, las distintas características de IoT Hub, entre las que se incluyen las de [enrutamiento de mensajes](./iot-hub-devguide-messages-d2c.md), [carga de archivos](./iot-hub-devguide-file-upload.md) e [importación/exportación masiva de dispositivos](./iot-hub-bulk-identity-mgmt.md), requieren conectividad de IoT Hub a un recurso de Azure propiedad del cliente a través de su punto de conexión público. Como se muestra a continuación, estas rutas de conectividad constituyen colectivamente el tráfico de salida de IoT Hub a los recursos del cliente.
![Punto de conexión público de IoT Hub](./media/virtual-network-support/public-endpoint.png)


Por varios motivos, es posible que los clientes deseen restringir la conectividad a sus recursos de Azure (incluido IoT Hub) a través de una red virtual de su propiedad que ellos mismos controlan. Estas razones incluyen las siguientes:

* Presentación de niveles adicionales de seguridad a través del aislamiento de nivel de red para su instancia de IoT Hub, para lo cual se evita la exposición de conectividad al concentrador a través de la red pública de Internet.

* Habilitación de una experiencia de conectividad privada desde los recursos de la red local, lo que garantiza que sus datos y tráfico se transmiten directamente a la red troncal de Azure.

* Prevención de ataques de exfiltración de redes locales confidenciales. 

* Seguimiento de los patrones de conectividad generales de Azure establecidos mediante [puntos de conexión privados](../private-link/private-endpoint-overview.md).


En este artículo se describe cómo lograr estos objetivos con [puntos de conexión privados](../private-link/private-endpoint-overview.md) para la conectividad de entrada a IoT Hub, como el uso de la excepción de servicios propios de confianza de Azure para la conectividad de salida de IoT Hub a otros recursos de Azure.


## <a name="ingress-connectivity-to-iot-hub-using-private-endpoints"></a>Conectividad de entrada a IoT Hub con puntos de conexión privados

Un punto de conexión privado es una dirección IP privada asignada en una red virtual propiedad del cliente a través de la cual se puede acceder a un recurso de Azure. Si tiene un punto de conexión privado para su instancia de IoT Hub, podrá permitir que los servicios que operan en la red virtual accedan a IoT Hub sin necesidad de enviar el tráfico al punto de conexión público de IoT Hub. Del mismo modo, los dispositivos que operan en su entorno local pueden usar la [red privada virtual (VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) o el emparejamiento privado de [ExpressRoute](https://azure.microsoft.com/services/expressroute/) para obtener conectividad a su red virtual en Azure y, posteriormente, a su IoT Hub (a través del punto de conexión privado). Como resultado, los clientes que deseen restringir la conectividad a los puntos de conexión públicos de su instancia de IoT Hub (o quizás bloquearlo por completo) pueden lograrlo usando las [reglas de firewall de IoT Hub](./iot-hub-ip-filtering.md) a la vez que conservan la conectividad a su concentrador mediante el punto de conexión privado.

> [!NOTE]
> El objetivo principal de esta configuración es para los dispositivos de una red local. No se recomienda esta configuración para los dispositivos implementados en una red de área extensa.

![Punto de conexión público de IoT Hub](./media/virtual-network-support/virtual-network-ingress.png)

Antes de continuar, asegúrese de que se cumplen los requisitos previos siguientes:

* La instancia de IoT Hub debe aprovisionarse con [Managed Service Identity](#create-an-iot-hub-with-managed-service-identity).

* La instancia de IoT Hub debe aprovisionarse en una de las [regiones admitidas](#regional-availability-private-endpoints).

* Ha aprovisionado una red virtual de Azure con una subred en la que se creará el punto de conexión privado. Consulte [Creación de una red virtual mediante la CLI de Azure](../virtual-network/quick-create-cli.md) para obtener más información.

* En el caso de los dispositivos que operan en redes locales, configure la [red privada virtual (VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) o el emparejamiento privado de [ExpressRoute](https://azure.microsoft.com/services/expressroute/) en su red virtual de Azure.


### <a name="regional-availability-private-endpoints"></a>Disponibilidad regional (puntos de conexión privados)

Los puntos de conexión privados que se admiten en IoT Hub creados en las siguientes regiones:

* Este de EE. UU.

* Centro-sur de EE. UU.

* Oeste de EE. UU. 2


### <a name="set-up-a-private-endpoint-for-iot-hub-ingress"></a>Configuración de un punto de conexión privado para la entrada de IoT Hub

Para configurar un punto de conexión privado, siga estos pasos:

1. Ejecute el siguiente comando de la CLI de Azure para volver a registrar el proveedor de Azure IoT Hub con su suscripción:

    ```azurecli-interactive
    az provider register --namespace Microsoft.Devices --wait --subscription  <subscription-name>
    ```

2. Vaya a la pestaña **Conexiones de punto de conexión privado** en el portal de IoT Hub (esta pestaña solo está disponible para las instancias de IoT Hub de las [regiones admitidas](#regional-availability-private-endpoints)) y haga clic en el signo **+** para agregar un nuevo punto de conexión privado.

3. Proporcione la suscripción, el grupo de recursos, el nombre y la región donde crear el nuevo punto de conexión privado (lo ideal es que el punto de conexión privado se cree en la misma región que el concentrador; consulte la [sección de disponibilidad regional](#regional-availability-private-endpoints) para obtener más detalles).

4. Haga clic en **Siguiente: Recurso** y proporcione la suscripción para el recurso de IoT Hub, y seleccione **"Microsoft.Devices/IotHubs"** como tipo de recurso, su nombre de IoT Hub como **recurso** e **iotHub** como subrecurso de destino.

5. Haga clic en **Siguiente: Configuración** y proporcione la red virtual y la subred donde se creará el punto de conexión privado. Si lo desea, seleccione la opción para la integración con la zona DNS privada de Azure.

6. Haga clic en **Siguiente: Etiquetas** y, de manera opcional, proporcione las etiquetas del recurso.

7. Haga clic en **Revisar y crear** para crear el recurso de punto de conexión privado.


### <a name="pricing-private-endpoints"></a>Precios (puntos de conexión privados)

Para más información sobre los precios, consulte [Precios de Azure Private Link](https://azure.microsoft.com/pricing/details/private-link).


## <a name="egress-connectivity-from-iot-hub-to-other-azure-resources"></a>Conectividad de salida de IoT Hub a otros recursos de Azure

IoT Hub necesita acceso a Azure Blob Storage, Event Hubs, recursos de Service Bus para el [enrutamiento de mensajes](./iot-hub-devguide-messages-d2c.md), [carga de archivos](./iot-hub-devguide-file-upload.md) e [importación/exportación masiva de dispositivos](./iot-hub-bulk-identity-mgmt.md), que normalmente proporciona el punto de conexión público de los recursos. En el caso de que enlace su cuenta de almacenamiento, la instancia de Event Hubs o el recurso de Service Bus a una red virtual, la configuración recomendada bloqueará la conectividad al recurso de forma predeterminada. Por lo tanto, esto impedirá las funcionalidades de IoT Hub que requieran acceso a esos recursos.

Para mitigar esta situación, debe habilitar la conectividad del recurso de IoT Hub a la cuenta de almacenamiento, la instancia de Event Hubs o los recursos de Service Bus a través de la opción de **servicios de confianza de Azure**.

Los requisitos previos son los siguientes:

* La instancia de IoT Hub debe aprovisionarse en una de las [regiones admitidas](#regional-availability-trusted-microsoft-first-party-services).

* La instancia de IoT Hub debe tener asignada una instancia de Managed Service Identity en el momento del aprovisionamiento del concentrador. Siga las instrucciones sobre cómo [crear un concentrador con Managed Service Identity](#create-an-iot-hub-with-managed-service-identity).


### <a name="regional-availability-trusted-microsoft-first-party-services"></a>Disponibilidad regional (servicios de confianza de Microsoft)

La excepción de servicios de confianza de Azure para derivar las restricciones de firewall a Azure Storage, las instancias de Event Hubs y los recursos de Service Bus solo se admite para las instancias de IoT Hub en las siguientes regiones:

* Este de EE. UU.

* Centro-sur de EE. UU.

* Oeste de EE. UU. 2


### <a name="pricing-trusted-microsoft-first-party-services"></a>Precios (servicios de confianza de Microsoft)

La característica de excepción de servicios de confianza de Microsoft es gratuita para las instancias de IoT Hub de las [regiones admitidas](#regional-availability-trusted-microsoft-first-party-services). Los cargos por las cuentas de almacenamiento aprovisionadas, las instancias de Event Hubs o los recursos de Service Bus se aplican por separado.


### <a name="create-an-iot-hub-with-managed-service-identity"></a>Creación de una instancia de IoT Hub con Managed Service Identity

Se puede asignar una instancia de Managed Service Identity a su concentrador en el momento del aprovisionamiento de recursos (esta característica no se admite actualmente para los concentradores existentes), lo que requiere que IoT Hub use TLS 1.2 como versión mínima. Para ello, debe usar la plantilla de recursos de ARM siguiente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Devices/IotHubs",
      "apiVersion": "2020-03-01",
      "name": "<provide-a-valid-resource-name>",
      "location": "<any-of-supported-regions>",
      "identity": {
        "type": "SystemAssigned"
      },
      "properties": {
        "minTlsVersion": "1.2"
      },
      "sku": {
        "name": "<your-hubs-SKU-name>",
        "tier": "<your-hubs-SKU-tier>",
        "capacity": 1
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-02-01",
      "name": "updateIotHubWithKeyEncryptionKey",
      "dependsOn": [
        "<provide-a-valid-resource-name>"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "0.9.0.0",
          "resources": [
            {
              "type": "Microsoft.Devices/IotHubs",
              "apiVersion": "2020-03-01",
              "name": "<provide-a-valid-resource-name>",
              "location": "<any-of-supported-regions>",
              "identity": {
                "type": "SystemAssigned"
              },
              "properties": {
                "minTlsVersion": "1.2"
              },
              "sku": {
                "name": "<your-hubs-SKU-name>",
                "tier": "<your-hubs-SKU-tier>",
                "capacity": 1
              }
            }
          ]
        }
      }
    }
  ]
}
```

Después de sustituir los valores del recurso `name`, `location`, `SKU.name` y `SKU.tier`, puede usar la CLI de Azure para implementar el recurso en un grupo de recursos existente mediante:

```azurecli-interactive
az group deployment create --name <deployment-name> --resource-group <resource-group-name> --template-file <template-file.json>
```

Una vez creado el recurso, puede recuperar la instancia de Managed Service Identity asignada al concentrador mediante la CLI de Azure:

```azurecli-interactive
az resource show --resource-type Microsoft.Devices/IotHubs --name <iot-hub-resource-name> --resource-group <resource-group-name>
```

Después de aprovisionar IoT Hub con una instancia de Managed Service Identity, siga las instrucciones de la sección correspondiente para configurar puntos de conexión de enrutamiento para [cuentas de almacenamiento](#egress-connectivity-to-storage-account-endpoints-for-routing), instancias de [Event Hubs](#egress-connectivity-to-event-hubs-endpoints-for-routing) y recursos de [Service Bus](#egress-connectivity-to-service-bus-endpoints-for-routing), o para configurar la [carga de archivos](#egress-connectivity-to-storage-accounts-for-file-upload) y la [importación/exportación masiva de dispositivos](#egress-connectivity-to-storage-accounts-for-bulk-device-importexport).


### <a name="egress-connectivity-to-storage-account-endpoints-for-routing"></a>Conectividad de salida a los puntos de conexión de la cuenta de almacenamiento para el enrutamiento

IoT Hub se pueden configurar para enrutar mensajes a una cuenta de almacenamiento propiedad del cliente. Para permitir que la funcionalidad de enrutamiento acceda a una cuenta de almacenamiento mientras se aplican las restricciones de firewall, IoT Hub debe tener una instancia de Managed Service Identity (consulte [Creación de un concentrador con Managed Service Identity](#create-an-iot-hub-with-managed-service-identity)). Después de aprovisionar Managed Service Identity, realice los pasos siguientes para conceder el permiso de RBAC a la identidad de recursos del concentrador para acceder a su cuenta de almacenamiento.

1. En Azure Portal, vaya a la pestaña **Control de acceso (IAM)** de la cuenta de almacenamiento y haga clic en **Agregar** en la sección **Agregar una asignación de roles**.

2. Seleccione **Colaborador de datos de Storage Blob** como **rol**, el **usuario, grupo o entidad de servicio de Azure AD** en **Asignando acceso a** y seleccione el nombre del recurso de IoT Hub en la lista desplegable. Haga clic en el botón **Save** (Guardar).

3. Vaya a la pestaña **Firewalls y redes virtuales** de la cuenta de almacenamiento y habilite la opción **Allow access from selected networks** (Permitir acceso desde redes seleccionadas). En la lista **Excepciones**, active la casilla situada junto a **Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento**. Haga clic en el botón **Save** (Guardar).

4. En la página de recursos de IoT Hub, vaya a la pestaña **Enrutamiento de mensajes**.

5. Vaya a la sección **Puntos de conexión personalizados** y haga clic en **Agregar**. Seleccione **Almacenamiento** como el tipo de punto de conexión.

6. En la página que aparece, proporcione un nombre para el punto de conexión, seleccione el contenedor que quiere usar en Blob Storage, y proporcione la codificación y el formato de nombre de archivo. Seleccione **Asignado por el sistema** como **Tipo de autenticación** para su punto de conexión de almacenamiento. Haga clic en el botón **Crear**.

Ahora el punto de conexión de almacenamiento personalizado está configurado para usar la identidad asignada por el sistema del concentrador y tiene permiso para acceder al recurso de almacenamiento a pesar de las restricciones de firewall. Ahora puede usar este punto de conexión para configurar una regla de enrutamiento.


### <a name="egress-connectivity-to-event-hubs-endpoints-for-routing"></a>Conectividad de salida a los puntos de conexión de Event Hubs para el enrutamiento

IoT Hub se pueden configurar para enrutar mensajes a un espacio de nombres de Event Hubs propiedad del cliente. Para permitir que la funcionalidad de enrutamiento acceda a un recurso de Event Hubs mientras se aplican las restricciones de firewall, IoT Hub debe tener una instancia de Managed Service Identity (consulte [Creación de un concentrador con Managed Service Identity](#create-an-iot-hub-with-managed-service-identity)). Después de aprovisionar Managed Service Identity, realice los pasos siguientes para conceder el permiso de RBAC a la identidad de recursos del concentrador para acceder a su instancia de Event Hubs.

1. En Azure Portal, vaya a la pestaña **Control de acceso (IAM)** de su instancia de Event Hubs y haga clic en **Agregar** en la sección **Agregar una asignación de roles**.

2. Seleccione **Remitente de los datos de Azure Event Hubs** como **rol**, el **usuario, grupo o entidad de servicio de Azure AD** en **Asignando acceso a** y seleccione el nombre del recurso de IoT Hub en la lista desplegable. Haga clic en el botón **Save** (Guardar).

3. Vaya a la pestaña **Firewalls y redes virtuales** de su instancia de Event Hubs y habilite la opción **Allow access from selected networks** (Permitir acceso desde redes seleccionadas). En la lista **Excepciones**, active la casilla situada junto a **Allow trusted Microsoft services to access event hubs** (Permitir que los servicios de Microsoft de confianza accedan a esta instancia de Event Hubs). Haga clic en el botón **Save** (Guardar).

4. En la página de recursos de IoT Hub, vaya a la pestaña **Enrutamiento de mensajes**.

5. Vaya a la sección **Puntos de conexión personalizados** y haga clic en **Agregar**. Seleccione **Event Hubs** como el tipo de punto de conexión.

6. En la página que aparece, proporcione un nombre para el punto de conexión, seleccione el espacio de nombres y la instancia de Event Hubs, y haga clic en el botón **Crear**.

Ahora el punto de conexión de Event Hubs personalizado está configurado para usar la identidad asignada por el sistema del concentrador y tiene permiso para acceder al recurso de Event Hubs a pesar de las restricciones de firewall. Ahora puede usar este punto de conexión para configurar una regla de enrutamiento.


### <a name="egress-connectivity-to-service-bus-endpoints-for-routing"></a>Conectividad de salida a los puntos de conexión de Service Bus para el enrutamiento

IoT Hub se pueden configurar para enrutar mensajes a un espacio de nombres de Service Bus propiedad del cliente. Para permitir que la funcionalidad de enrutamiento acceda a un recurso de Service Bus mientras se aplican las restricciones de firewall, IoT Hub debe tener una instancia de Managed Service Identity (consulte [Creación de un concentrador con Managed Service Identity](#create-an-iot-hub-with-managed-service-identity)). Después de aprovisionar Managed Service Identity, realice los pasos siguientes para conceder el permiso de RBAC a la identidad de recursos del concentrador para acceder a su instancia de Service Bus.

1. En Azure Portal, vaya a la pestaña **Control de acceso (IAM)** de su instancia de Service Bus y haga clic en **Agregar** en la sección **Agregar una asignación de roles**.

2. Seleccione **Remitente de los datos de Azure Service Bus** como **rol**, el **usuario, grupo o entidad de servicio de Azure AD** en **Asignando acceso a** y seleccione el nombre del recurso de IoT Hub en la lista desplegable. Haga clic en el botón **Save** (Guardar).

3. Vaya a la pestaña **Firewalls y redes virtuales** de su instancia de Service Bus y habilite la opción **Allow access from selected networks** (Permitir acceso desde redes seleccionadas). En la lista **Excepciones**, active la casilla situada junto a **Allow trusted Microsoft services to access this service bus** (Permitir que los servicios de Microsoft de confianza accedan a esta instancia de Service Bus). Haga clic en el botón **Save** (Guardar).

4. En la página de recursos de IoT Hub, vaya a la pestaña **Enrutamiento de mensajes**.

5. Vaya a la sección **Puntos de conexión personalizados** y haga clic en **Agregar**. Seleccione **Cola de Service Bus** o **Tema de Service Bus** (según corresponda) como el tipo de punto de conexión.

6. En la página que aparece, proporcione un nombre para el punto de conexión, y seleccione el espacio de nombres y la cola o el tema de Service Bus (según corresponda). Haga clic en el botón **Crear**.

Ahora el punto de conexión de Service Bus personalizado está configurado para usar la identidad asignada por el sistema del concentrador y tiene permiso para acceder al recurso de Service Bus a pesar de las restricciones de firewall. Ahora puede usar este punto de conexión para configurar una regla de enrutamiento.


### <a name="egress-connectivity-to-storage-accounts-for-file-upload"></a>Conectividad de salida a las cuentas de almacenamiento para la carga de archivos

La característica de carga de archivos de IoT Hub permite a los dispositivos cargar archivos en una cuenta de almacenamiento propiedad del cliente. Para permitir que la carga de archivos funcione, tanto los dispositivos como IoT Hub deben tener conectividad con la cuenta de almacenamiento. Si se aplican restricciones de firewall a la cuenta de almacenamiento, los dispositivos deben usar cualquiera de los mecanismos de la cuenta de almacenamiento admitida (como [puntos de conexión privados](../private-link/create-private-endpoint-storage-portal.md), [puntos de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) o [configuración directa de firewall](../storage/common/storage-network-security.md)) para obtener conectividad. Del mismo modo, si se aplican restricciones de firewall en la cuenta de almacenamiento, IoT Hub se debe configurar para acceder al recurso de almacenamiento a través de la excepción de servicios de confianza de Microsoft. Con este propósito, IoT Hub debe tener una instancia de Managed Service Identity (consulte [Creación de un concentrador con Managed Service Identity](#create-an-iot-hub-with-managed-service-identity)). Después de aprovisionar Managed Service Identity, realice los pasos siguientes para conceder el permiso de RBAC a la identidad de recursos del concentrador para acceder a su cuenta de almacenamiento.

1. En Azure Portal, vaya a la pestaña **Control de acceso (IAM)** de la cuenta de almacenamiento y haga clic en **Agregar** en la sección **Agregar una asignación de roles**.

2. Seleccione **Colaborador de datos de Storage Blob** como **rol**, el **usuario, grupo o entidad de servicio de Azure AD** en **Asignando acceso a** y seleccione el nombre del recurso de IoT Hub en la lista desplegable. Haga clic en el botón **Save** (Guardar).

3. Vaya a la pestaña **Firewalls y redes virtuales** de la cuenta de almacenamiento y habilite la opción **Allow access from selected networks** (Permitir acceso desde redes seleccionadas). En la lista **Excepciones**, active la casilla situada junto a **Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento**. Haga clic en el botón **Save** (Guardar).

4. En la página de recursos de IoT Hub, vaya a la pestaña **Carga de archivos**.

5. En la página que se muestra, seleccione el contenedor que quiere usar en Blob Storage, y establezca los valores que desee en **Configuración de notificación de archivos**, **TTL de SAS**, **TTL predeterminado** y **Número máximo de entregas**. Seleccione **Asignado por el sistema** como **Tipo de autenticación** para su punto de conexión de almacenamiento. Haga clic en el botón **Crear**.

Ahora el punto de conexión de almacenamiento para la carga de archivos está configurado para usar la identidad asignada por el sistema del concentrador y tiene permiso para acceder al recurso de almacenamiento a pesar de las restricciones de firewall.


### <a name="egress-connectivity-to-storage-accounts-for-bulk-device-importexport"></a>Conectividad de salida a las cuentas de almacenamiento para la importación/exportación masiva de dispositivos

IoT Hub admite la funcionalidad para [importar/exportar](./iot-hub-bulk-identity-mgmt.md) información de dispositivos de forma masiva desde y hacia una instancia de Storage Blob proporcionada por el cliente. Para permitir que la característica de importación/exportación masiva funcione, tanto los dispositivos como IoT Hub deben tener conectividad con la cuenta de almacenamiento.

Esta funcionalidad requiere conectividad de IoT Hub a la cuenta de almacenamiento. Para acceder a un recurso de Service Bus mientras se aplican las restricciones de firewall, IoT Hub debe tener una instancia de Managed Service Identity (consulte [Creación de un concentrador con Managed Service Identity](#create-an-iot-hub-with-managed-service-identity)). Después de aprovisionar Managed Service Identity, realice los pasos siguientes para conceder el permiso de RBAC a la identidad de recursos del concentrador para acceder a su instancia de Service Bus.

1. En Azure Portal, vaya a la pestaña **Control de acceso (IAM)** de la cuenta de almacenamiento y haga clic en **Agregar** en la sección **Agregar una asignación de roles**.

2. Seleccione **Colaborador de datos de Storage Blob** como **rol**, el **usuario, grupo o entidad de servicio de Azure AD** en **Asignando acceso a** y seleccione el nombre del recurso de IoT Hub en la lista desplegable. Haga clic en el botón **Save** (Guardar).

3. Vaya a la pestaña **Firewalls y redes virtuales** de la cuenta de almacenamiento y habilite la opción **Allow access from selected networks** (Permitir acceso desde redes seleccionadas). En la lista **Excepciones**, active la casilla situada junto a **Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento**. Haga clic en el botón **Save** (Guardar).

Ahora puede usar la API de REST de Azure IoT para [crear trabajos de importación y exportación](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjobs) a fin de obtener información sobre cómo usar la funcionalidad de importación y exportación masiva. Tenga en cuenta que tendrá que proporcionar el objeto `storageAuthenticationType="identityBased"` en el cuerpo de la solicitud y usar `inputBlobContainerUri="https://..."` y `outputBlobContainerUri="https://..."` como las direcciones URL de entrada y salida de la cuenta de almacenamiento, respectivamente.


Los SDK de Azure IoT Hub también admiten esta funcionalidad en el administrador del Registro del cliente del servicio. El fragmento de código siguiente muestra cómo iniciar un trabajo de importación o exportación al usar el SDK de C#.

```csharp
// Call an import job on the IoT Hub
JobProperties importJob = 
await registryManager.ImportDevicesAsync(
  JobProperties.CreateForImportJob(inputBlobContainerUri, outputBlobContainerUri, null, StorageAuthenticationType.IdentityBased), 
  cancellationToken);

// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = 
await registryManager.ExportDevicesAsync(
    JobProperties.CreateForExportJob(outputBlobContainerUri, true, null, StorageAuthenticationType.IdentityBased),
    cancellationToken);
```


Para usar esta versión limitada por región de los SDK de Azure IoT con compatibilidad con redes virtuales para #C, Java y Node.js:

1. Cree una variable de entorno denominada `EnableStorageIdentity` y establezca su valor en `1`.

2. Descargue el SDK:  [Java](https://aka.ms/vnetjavasdk) | [C#](https://aka.ms/vnetcsharpsdk) | [Node.js](https://aka.ms/vnetnodesdk)
 
Para Python, descargue nuestra versión limitada de GitHub.

1. Vaya a la [página de la versión de GitHub](https://aka.ms/vnetpythonsdk).

2. Descargue el archivo siguiente, que encontrará en la parte inferior de la página de la versión, bajo el encabezado denominado **assets**.
    > *azure_iot_hub-2.2.0_limited-py2.py3-none-any.whl*

3. Abra un terminal y navegue hasta la carpeta que contiene el archivo descargado.

4. Ejecute el siguiente comando para instalar el SDK de servicio de Python con compatibilidad con redes virtuales:
    > pip install ./azure_iot_hub-2.2.0_limited-py2.py3-none-any.whl


## <a name="next-steps"></a>Pasos siguientes

Use los vínculos siguientes para obtener más información sobre las características de IoT Hub:

* [Enrutamiento de mensajes](./iot-hub-devguide-messages-d2c.md)
* [Carga de archivos](./iot-hub-devguide-file-upload.md)
* [Importación/exportación masiva de dispositivos](./iot-hub-bulk-identity-mgmt.md) 
