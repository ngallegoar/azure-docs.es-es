---
title: Problemas conocidos/limitaciones de migración con el uso del modo híbrido
description: Obtenga información sobre los problemas conocidos y las limitaciones de migración mediante el uso del servicio Azure Database Migration Service en modo híbrido.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: troubleshooting
ms.date: 02/20/2020
ms.openlocfilehash: 819fe5ced6c91819c817065305a31fca456ea5c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91291850"
---
# <a name="known-issuesmigration-limitations-with-using-hybrid-mode"></a>Problemas conocidos/limitaciones de migración con el uso del modo híbrido

Los problemas conocidos y las limitaciones asociadas al uso del servicio Azure Database Migration Service en modo híbrido se describen en las secciones siguientes.

## <a name="installer-fails-to-authenticate"></a>El instalador no se puede autenticar

Después de cargar el certificado en AdApp, hay un retraso de hasta un par de minutos antes de poder autenticarse en Azure. El instalador intentará volver a intentarlo con algún retraso, pero es posible que el retraso de propagación sea mayor que el reintento, de modo que verá el mensaje **FailedToGetAccessTokenException**. Si el certificado se cargó en el elemento AdApp correcto y se proporcionó el valor de AppId correcto en dmsSettings.json, intente ejecutar de nuevo el comando de instalación.

## <a name="service-offline-after-successful-installation"></a>Servicio "sin conexión" después de una instalación correcta

Si el servicio se muestra como sin conexión después de que el proceso de instalación se complete correctamente, pruebe a usar los pasos siguientes.

1. En Azure Portal, en la instancia de Azure Database Migration Service, vaya a la pestaña de configuración **Híbrida** y verifique que el trabajo esté registrado. Para ello, compruebe la cuadrícula de los trabajos registrados.

    El estado de este trabajo debe ser **En línea**, pero puede mostrarse como **Sin conexión** si hay un problema.

2. En el equipo de trabajo, compruebe el estado del servicio mediante la ejecución del siguiente comando de PowerShell:

    ```
    Get-Service Scenario*
    ```

    Este comando proporciona el estado del servicio de Windows que ejecuta el trabajo. Solo debe haber un resultado. Si el trabajo está detenido, puede intentar reiniciarlo mediante el siguiente comando de PowerShell:

    ```
    Start-Service Scenario*
    ```

    También puede comprobar el servicio en la interfaz de usuario de servicios de Windows.

3. Si el servicio de Windows cambia entre los estados En ejecución y Detenido, significa que el trabajador detectó problemas al iniciarse. Compruebe los registros de Hybrid Worker de Azure Database Migration Service para determinar el problema.

    - Los registros del proceso de instalación se almacenan en la carpeta "logs" dentro de la carpeta desde la que se ejecutó el archivo ejecutable del instalador.
    - Los registros de Hybrid Worker de Azure Database Migration Service se almacenan en la carpeta **WorkerLogs** de la carpeta en la que está instalado el trabajo. La ubicación predeterminada de los archivos de registro de Hybrid Worker es **C:\Archivos de Files\DatabaseMigrationServiceHybrid\WorkerLogs**.

## <a name="using-your-own-signed-certificate"></a>Uso de su propio certificado firmado

El certificado que genera la acción GenerateCert es un certificado autofirmado, que es posible que no se pueda aceptar en función de las directivas de seguridad internas. En lugar de usar este certificado, puede proporcionar su propio certificado y proporcionar la huella digital en dmsSettings.json. Este certificado deberá cargarse en el elemento AdApp e instalarse en el equipo en el que va a instalar el servicio Hybrid Worker de Azure Database Migration Service. A continuación, instale este certificado con la clave privada en el almacén de certificados de la máquina local.

## <a name="running-the-worker-service-as-a-low-privilege-account"></a>Ejecución del servicio de trabajo como una cuenta con pocos privilegios

De forma predeterminada, el servicio Hybrid Worker de Azure Database Migration Service se ejecuta como la cuenta del sistema local. Puede cambiar la cuenta usada para este servicio siempre y cuando la cuenta que use tenga permisos de red. Para cambiar la cuenta de servicio "ejecutar como", use el siguiente proceso.

1. Detenga el servicio, ya sea a través de los servicios de Windows o mediante el comando Stop-Service de PowerShell.

2. Actualice el servicio para que use otra cuenta de inicio de sesión.

3. En certmgr para certificados del equipo local, asigne a la nueva cuenta permisos de clave privada para los certificados **DMS Hybrid App Key** (Clave de la aplicación híbrida de DMS) y **DMS Scenario Engine Key Pair** (Par de claves del motor de escenario de DMS).

    a. Abra Certmgr para ver las claves siguientes:

    - DMS Hybrid App Key (Clave de aplicación híbrida de DMS)
    - DMS Hybrid Worker Setup Key (Clave de configuración de Hybrid Worker de DMS)
    - DMS Scenario Engine Key Pair (Par de claves del motor de escenario de DMS)

    b. Haga clic con el botón derecho en la entrada **DMS Hybrid App Key** (Clave de la aplicación híbrida de DMS), seleccione **Todas las tareas** y, a continuación, seleccione **Manage Private Keys** (Administrar claves privadas).

    c. En la pestaña **Seguridad**, seleccione **Agregar**y, a continuación, escriba el nombre de la cuenta.

    d. Siga los mismos pasos para conceder el permiso de clave privada de la nueva cuenta al certificado **DMS Scenario Engine Key Pair** (Par de claves del motor de escenario de DMS).

## <a name="unregistering-the-worker-manually"></a>Anulación del registro de trabajo manual

Si ya no tiene acceso al equipo de trabajo, puede anular el registro del trabajo y volver a usar la instancia de Azure Database Migration Service siguiendo los pasos siguientes:

1. En Azure Portal, vaya a la instancia de Azure Database Migration Service y, después, navegue a la página de configuración **Híbrida**.

   La entrada de trabajo se muestra en la lista con el estado **Sin conexión**.

2. En el extremo derecho de la lista de entradas de trabajo, seleccione el botón de puntos suspensivos y, a continuación, **Anular el registro**.

## <a name="addressing-issues-for-specific-migration-scenarios"></a>Solución de problemas de escenarios de migración específicos

En las secciones siguientes se describen problemas específicos de cada escenario relacionados con el uso del modo híbrido de Azure Database Migration Service para realizar una migración en línea.

### <a name="online-migrations-to-azure-sql-managed-instance"></a>Migraciones en línea a Instancia administrada de Azure SQL

**Uso elevado de la CPU**

**Problema:** En el caso de las migraciones en línea a instancia administrada de SQL Database, el equipo que ejecuta Hybrid Worker detectará un uso elevado de la CPU si hay demasiadas copias de seguridad o si las copias de seguridad son demasiado grandes.

**Mitigación**: Para mitigar este problema, use copias de seguridad comprimidas, divida la migración para que use varios recursos compartidos o escale verticalmente el equipo que ejecuta Hybrid Worker.
