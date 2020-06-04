---
title: Implementación de Azure SQL Edge (versión preliminar) mediante Azure Portal
description: Información sobre cómo implementar Azure SQL Edge (versión preliminar) mediante Azure Portal
keywords: implementación de SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 43359b66ba747dba7b3294d022a2c1aa2a3e624c
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84233237"
---
# <a name="deploy-azure-sql-edge-preview"></a>Implementación de Azure SQL Edge (versión preliminar) 

Azure SQL Edge (versión preliminar) es un motor de base de datos relacional optimizado para implementaciones de IoT y Azure IoT Edge. Proporciona funcionalidades para crear una capa de procesamiento y almacenamiento de datos de alto rendimiento para las aplicaciones y soluciones de IoT. Esta guía de inicio rápido le muestra cómo comenzar a crear un módulo de Azure SQL Edge a través de Azure IoT Edge mediante Azure Portal.

## <a name="before-you-begin"></a>Antes de empezar

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/).
* Inicie sesión en [Azure Portal](https://portal.azure.com/).
* Cree un [Azure IoT Hub](../iot-hub/iot-hub-create-through-portal.md).
* Registre un [dispositivo IoT Edge desde Azure Portal](../iot-edge/how-to-register-device-portal.md).
* Prepare el dispositivo IoT Edge para [implementar un módulo de IoT Edge desde Azure Portal](../iot-edge/how-to-deploy-modules-portal.md).

> [!NOTE]
> Para implementar una máquina virtual Linux de Azure como un dispositivo IoT Edge, consulte esta [guía de inicio rápido](../iot-edge/quickstart-linux.md).

## <a name="deploy-sql-edge-module-from-azure-marketplace"></a>Implementación del módulo de SQL Edge desde Azure Marketplace

Azure Marketplace es un mercado de aplicaciones y servicios en línea en el que puede examinar una amplia gama de aplicaciones y soluciones empresariales que están certificadas y optimizadas para ejecutarse en Azure, incluyendo los [módulos de IoT Edge](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules). Azure SQL Edge se puede implementar en un dispositivo perimetral a través de Marketplace.

1. Busque el módulo de Azure SQL Edge en Azure Marketplace.<br><br>

   ![SQL Edge en Marketplace](media/deploy-portal/find-offer-marketplace.png)

2. Elija el plan de software que mejor se adapte a sus requisitos y haga clic en **Crear**. <br><br>

   ![Elegir el plan de software correcto](media/deploy-portal/pick-correct-plan.png)

3. En la página de dispositivos de destino para el módulo de IoT Edge, especifique los detalles siguientes y, a continuación, haga clic en **Crear**

   |**Campo**  |**Descripción**  |
   |---------|---------|
   |Suscripción  |  La suscripción de Azure en la que se creó el IoT Hub |
   |IoT Hub   |  Nombre del IoT Hub en el que está registrado el dispositivo IoT Edge y, a continuación, seleccione la opción "Implementar en un dispositivo"|
   |Nombre de dispositivo IoT Edge  |  Nombre del dispositivo IoT Edge en el que se implementará SQL Edge |

4. En la página **Establecer módulos**, vaya a la sección sobre módulos de implementación y haga clic en **Configurar** en el módulo de SQL Edge. 

5. En el panel de **Módulos personalizados de IoT Edge**, especifique los valores que desee para las variables de entorno o personalice las opciones de creación y las propiedades deseadas para el módulo. Para obtener una lista completa de las variables de entorno admitidas, consulte [variables de entorno de un contenedor de SQL Server](/sql/linux/sql-server-linux-configure-environment-variables/).

   |**Parámetro**  |**Descripción**|
   |---------|---------|
   | Nombre | Nombre del módulo. |
   |SA_PASSWORD  | Especifica una contraseña segura para la cuenta de administrador de SQL Edge. |
   |MSSQL_LCID   | Establece el identificador de idioma que se usará para SQL Server. Por ejemplo, 1036 es francés. |
   |MSSQL_COLLATION | Establece la intercalación predeterminada para SQL Server. Esta configuración invalida la asignación predeterminada de identificador de idioma (LCID) a la intercalación. |

   > [!NOTE]
   > No cambie ni actualice el **URI de la imagen** o la configuración de **ACCEPT_EULA** del módulo.

6. En el panel de **Módulos personalizados de IoT Edge**, actualice el valor deseado de las opciones de creación del contenedor para el **puerto del host**. Si necesita implementar más de un módulo de SQL DB Edge, asegúrese de actualizar la opción de montajes para crear un nuevo origen y par de destino para el volumen persistente. Para obtener más información sobre los montajes y el volumen, consulte el artículo sobre cómo [usar volúmenes](https://docs.docker.com/storage/volumes/) en la documentación de Docker. 

   ```json
       {
         "HostConfig": {
           "Binds": [
             "sqlvolume:/sqlvolume"
           ],
           "PortBindings": {
             "1433/tcp": [
               {
                 "HostPort": "1433"
               }
             ]
           },
           "Mounts": [
             {
               "Type": "volume",
               "Source": "sqlvolume",
               "Target": "/var/opt/mssql"
             }
           ]
         },
         "Env": [
           "MSSQL_AGENT_ENABLED=TRUE",
           "MSSQL_PID=Developer"
         ]
       }
   ```

7. En el panel de **módulos personalizados de IoT Edge**, actualice las *propiedades deseadas del módulo gemelo establecido* para que incluyan la ubicación del paquete SQL y la información del trabajo de análisis de streaming. Estos dos campos son opcionales y deben usarse si quiere implementar el módulo de SQL Edge con una base de datos y un trabajo de streaming.

   ```json
       {
         "properties.desired":
         {
           "SqlPackage": "<Optional_DACPAC_ZIP_SAS_URL>",
           "ASAJobInfo": "<Optional_ASA_Job_ZIP_SAS_URL>"
         }
       }
   ```

8. En el panel de **Módulos personalizados de IoT Edge**, establezca *Directiva de reinicio* en siempre y *Estado deseado* en ejecución.
9. En el panel de **Módulos personalizados de IoT Edge**, haga clic en **Guardar**.
10. En la página **Establecer módulos**, haga clic en **Siguiente**.
11. En la sección **Especificar ruta (opcional)** de la página **Establecer módulos**, especifique las rutas para la comunicación entre módulo y módulo o entre módulo y IoT Edge Hub; consulte [Implementación de módulos y establecimiento de rutas en IoT Edge](../iot-edge/module-composition.md).
12. Haga clic en **Next**.
13. Haga clic en **Enviar**.

Con esta guía de inicio rápido, ha implementado un módulo de SQL Edge en un dispositivo IoT Edge.

## <a name="next-steps"></a>Pasos siguientes

- [Aprendizaje automático e inteligencia artificial con ONNX en SQL Edge](onnx-overview.md).
- [Creación de una solución de IoT de un extremo a otro con SQL Edge mediante IoT Edge](tutorial-deploy-azure-resources.md).