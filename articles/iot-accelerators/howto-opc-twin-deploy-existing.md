---
title: Implementación de un módulo de OPC Twin en un proyecto existente de Azure | Microsoft Docs
description: En este artículo se explica cómo implementar OPC Twin en un proyecto existente. También puede aprender a solucionar errores de implementación.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 0a4866db463dcf4ff17b2f7b25806806a700e8fa
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92071649"
---
# <a name="deploy-opc-twin-to-an-existing-project"></a>Implementación de OPC Twin en un proyecto existente

> [!IMPORTANT]
> Mientras actualizamos este artículo, consulte el artículo sobre [Azure Industrial IoT](https://azure.github.io/Industrial-IoT/), donde encontrará el contenido más actualizado.

El módulo OPC Twin se ejecuta en IoT Edge y proporciona varios servicios perimetrales para los servicios de registro y OPC Twin.

El microservicio OPC Twin facilita la comunicación entre los operadores de fábrica y los dispositivos de servidor de OPC UA en la fábrica a través de un módulo de IoT Edge de OPC Twin. El microservicio expone servicios de OPC UA (examinar, leer, escribir y ejecutar) mediante su API REST. 

El microservicio de registro de dispositivos de OPC UA proporciona acceso a las aplicaciones registradas de OPC UA y sus puntos de conexión. Los operadores y administradores pueden registrar y anular el registro de nuevas aplicaciones de OPC UA y examinar los existentes, incluidos sus puntos de conexión. Además de la administración de aplicaciones y puntos de conexión, el servicio de registro también cataloga los módulos registrados de IoT Edge de OPC Twin. La API del servicio le ofrece el control de la funcionalidad del módulo perimetral; por ejemplo, iniciar o detener la detección de servidores (servicios de análisis) o activar nuevos gemelos de punto de conexión a los que se puede acceder mediante el microservicio de OPC Twin.

El núcleo del módulo es la identidad de supervisor. El supervisor administra el gemelo del punto de conexión, que corresponde a los puntos de conexión de servidor de OPC UA que se activan mediante la API de registro de OPC UA correspondiente. Este gemelo de punto de conexión traduce el código JSON de OPC UA recibido del microservicio de OPC Twin que se ejecuta en la nube a mensajes binarios de OPC UA, que se envían a través de un canal seguro con estado al punto de conexión administrado. El supervisor también proporciona servicios de detección que envían eventos de detección de dispositivo para el servicio de incorporación de dispositivos OPC UA para su procesamiento y estos eventos generan actualizaciones del registro de OPC UA.  En este artículo se muestra cómo implementar el módulo de OPC Twin en un proyecto existente.

> [!NOTE]
> Para obtener más información sobre las instrucciones y los detalles de la implementación, vea el [repositorio](https://github.com/Azure/azure-iiot-opc-twin-module) de GitHub.

## <a name="prerequisites"></a>Requisitos previos

Asegúrese de que tiene las extensiones de PowerShell y [AzureRM PowerShell](/powershell/azure/azurerm/install-azurerm-ps) instaladas. Si aún no lo ha hecho, clone este repositorio de GitHub. Ejecute los siguientes comandos en PowerShell:

```powershell
git clone --recursive https://github.com/Azure/azure-iiot-components.git
cd azure-iiot-components
```

## <a name="deploy-industrial-iot-services-to-azure"></a>Implementar servicios industriales de IoT en Azure

1. En una sesión PowerShell, ejecute:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy.cmd
    ```

2. Siga las indicaciones para asignar un nombre al grupo de recursos de la implementación y un nombre al sitio web.   El script implementa los microservicios y sus dependencias de la plataforma de Azure en el grupo de recursos de la suscripción a Azure.  El script también registra una aplicación en el inquilino de Azure Active Directory (AAD) para admitir la autenticación basada en OAUTH.  La implementación puede tardar varios minutos.  Un ejemplo de lo que vería una vez implementada correctamente la solución:

   ![Implementación de OPC Twin de IoT en un proyecto existente](media/howto-opc-twin-deploy-existing/opc-twin-deploy-existing1.png)

   La salida incluye la dirección URL del punto de conexión público. 

3. Una vez que el script se complete correctamente, seleccione si quiere guardar el archivo `.env`.  Necesita el archivo de entorno `.env` si quiere conectarse al punto de conexión en la nube con herramientas como la consola o implementar módulos para el desarrollo y la depuración.

## <a name="troubleshooting-deployment-failures"></a>Solución de problemas con errores de implementación

### <a name="resource-group-name"></a>Definición de un nombre de grupo de recursos

Asegúrese de usar un nombre de grupo de recursos corto y sencillo.  El nombre se usa también para denominar recursos y debe cumplir los requisitos de nomenclatura de los recursos.  

### <a name="website-name-already-in-use"></a>El nombre del sitio web ya está en uso

Es posible que el nombre del sitio web ya esté en uso.  Si experimenta este error, deberá usar un nombre de aplicación diferente.

### <a name="azure-active-directory-aad-registration"></a>Registro de Azure Active Directory (AAD)

El script de implementación intenta registrar dos aplicaciones de AAD en Azure Active Directory.  En función de sus derechos para el inquilino de AAD seleccionado, podría producirse en error de implementación. Hay dos opciones:

1. Si ha elegido un inquilino de AAD de una lista de inquilinos, reinicie el script y elija otro de la lista.
2. Como alternativa, implemente un inquilino de AAD privado en otra suscripción, reinicie el script y selecciónelo para usarlo.

> [!WARNING]
> No continúe nunca sin autenticación.  Si decide hacerlo, cualquier persona puede acceder a los puntos de conexión de OPC Twin desde Internet sin autenticar.   Siempre puede elegir la [opción de implementación "local"](howto-opc-twin-deploy-dependencies.md) para probar.

## <a name="deploy-an-all-in-one-industrial-iot-services-demo"></a>Implementar una demostración de servicios de IoT industriales integrados

En lugar de los servicios y las dependencias, puede implementar una demostración integrada.  La demostración integrada contiene tres servidores OPC UA, el módulo OPC Twin, todos los microservicios y una aplicación web de ejemplo.  Tiene una finalidad de demostración.

1. Asegúrese de que tiene un clon del repositorio (consulte más arriba). Abra un símbolo del sistema de PowerShell en la raíz del repositorio y ejecute:

    ```powershell
    set-executionpolicy -ExecutionPolicy Unrestricted -Scope Process
    .\deploy -type demo
    ```

2. Siga las indicaciones para asignar un nombre al grupo de recursos y al sitio web.  Una vez que se haya implementado correctamente, el script mostrará la dirección URL del punto de conexión de la aplicación web.

## <a name="deployment-script-options"></a>Opciones del script de implementación

El script usa los parámetros siguientes:

```powershell
-type
```

Tipo de implementación (VM, local, demostración)

```powershell
-resourceGroupName
```

Puede ser el nombre de un grupo de recursos nuevo o existente.

```powershell
-subscriptionId
```

Opcional: identificador de suscripción en que se implementarán los recursos.

```powershell
-subscriptionName
```

O el nombre de la suscripción.

```powershell
-resourceGroupLocation
```

Opcional: ubicación del grupo de recursos. Si se especifica, intentará crear un nuevo grupo de recursos en esta ubicación.

```powershell
-aadApplicationName
```

Nombre de la aplicación de AAD en que se registra.

```powershell
-tenantId
```

Inquilino de AAD que se usará.

```powershell
-credentials
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido cómo implementar OPC Twin en un proyecto existente, este es el siguiente paso que se le sugiere:

> [!div class="nextstepaction"]
> [Protección de la comunicación entre OPC UA Client y OPC UA PLC](howto-opc-vault-secure.md)