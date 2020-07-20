---
title: Configuración de Azure SQL Edge (versión preliminar)
description: Información sobre la configuración de Azure SQL Edge (versión preliminar).
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: c38bb6100665cc9456b66608660bdca520b934c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84636247"
---
# <a name="configure-azure-sql-edge-preview"></a>Configuración de Azure SQL Edge (versión preliminar)

Azure SQL Edge admite la configuración a través de una de las dos opciones siguientes:

- Variables de entorno
- Un archivo mssql.conf colocado en la carpeta /var/opt/mssql

> [!NOTE]
> La configuración de variables de entorno invalida la configuración especificada en el archivo mssql.conf.

## <a name="configure-by-using-environment-variables"></a>Configuración con variables de entorno

Azure SQL Edge expone varias variables de entorno diferentes que se pueden usar para configurar el contenedor de SQL Edge. Estas variables de entorno son un subconjunto de las disponibles para SQL Server en Linux. Para más información acerca de las variables de entorno de SQL Server en Linux, consulte [Variables de entorno](/sql/linux/sql-server-linux-configure-environment-variables/).

La siguiente variable de entorno de SQL Server en Linux no se admite en Azure SQL Edge. Si se define, esta variable de entorno se omitirá durante la inicialización del contenedor.

| Variable de entorno | Descripción |
|-----|-----|
| **MSSQL_ENABLE_HADR** | Habilite el grupo de disponibilidad. Por ejemplo, **1** significa que está habilitado y **0**, deshabilitado. |

> [!IMPORTANT]
> La variable de entorno **MSSQL_PID** para SQL Edge solo acepta **Premium** y **Desarrollador** como valores válidos. Azure SQL Edge no admite la inicialización mediante una clave de producto.

> [!NOTE]
> Descargue los [términos de licencia del software de Microsoft](https://go.microsoft.com/fwlink/?linkid=2128283) para Azure SQL Edge.

### <a name="specify-the-environment-variables"></a>Especificación de las variables de entorno

Especifique las variables de entorno de SQL Edge al implementar el servicio mediante [Azure Portal](deploy-portal.md). Puede agregarlas en la sección **Variables de entorno** de la implementación del módulo o como parte de las **Opciones de creación del contenedor**.

Agregue valores en **Variables de entorno**.

![Establecimiento mediante la lista de variables de entorno](media/configure/set-environment-variables.png)

Agregue valores en **Opciones de creación del contenedor**.

![Establecimiento mediante las opciones de creación del contenedor](media/configure/set-environment-variables-using-create-options.png)

## <a name="configure-by-using-an-mssqlconf-file"></a>Configuración mediante un archivo mssql.conf

Azure SQL Edge no incluye la [utilidad de configuración mssql-conf](/sql/linux/sql-server-linux-configure-mssql-conf/), como sí hace SQL Server en Linux. Debe configurar manualmente el archivo mssql.conf y colocarlo en la unidad de almacenamiento persistente asignada a la carpeta /var/opt/mssql/ en el módulo SQL Edge. Al implementar SQL Edge desde Azure Marketplace, esta asignación se especifica como la opción **Mounts** en las **Opciones de creación del contenedor**.

```json
    {
        "Mounts": [
          {
            "Type": "volume",
            "Source": "sqlvolume",
            "Target": "/var/opt/mssql"
          }
        ]
      }
    }
```

Las siguientes opciones de mssql.conf no son aplicables a SQL Edge:

|Opción|Descripción|
|:---|:---|
|**Comentarios del cliente** | Elija si SQL Server envía comentarios a Microsoft. |
|**Perfil de correo electrónico de base de datos** | Establezca el perfil de correo electrónico de base de datos predeterminado para SQL Server en Linux. |
|**Alta disponibilidad** | Habilite los grupos de disponibilidad. |
|**Microsoft DTC (Coordinador de transacciones distribuidas)** | Configure y solucione problemas de MSDTC en Linux. No se admiten opciones de configuración relacionadas con transacciones distribuidas adicionales para SQL Edge. Para más información sobre estas opciones de configuración adicionales, consulte [Configuración de MSDTC](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#msdtc). |
|**CLUF de MLServices** | Acepte los CLUF de R y Python de los paquetes de Azure Machine Learning. Solo se aplica a SQL Server 2019.|
|**outboundnetworkaccess** |Habilite el acceso de red saliente para las extensiones de R, Python y Java de [Machine Learning Services](/sql/linux/sql-server-linux-setup-machine-learning/).|

El siguiente archivo mssql.conf de ejemplo funciona con SQL Edge. Para más información sobre el formato del archivo mssql.conf, consulte [Formato de mssql.conf](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf#mssql-conf-format).

```ini
[EULA]
accepteula = Y

[coredump]
captureminiandfull = true
coredumptype = full

[filelocation]
defaultbackupdir = /var/opt/mssql/backup/
defaultdatadir = /var/opt/mssql/data/
defaultdumpdir = /var/opt/mssql/data/
defaultlogdir = /var/opt/mssql/log/

[language]
lcid = 1033

[memory]
memorylimitmb = 6144

[sqlagent]
errorlogfile = /var/opt/mssql/log/sqlagentlog.log
errorlogginglevel = 7

[traceflag]
traceflag0 = 3604
traceflag1 = 3605
traceflag2 = 1204
```

## <a name="next-steps"></a>Pasos siguientes

- [Conexión a Azure SQL Edge](connect.md)
- [Creación de una solución de IoT global con SQL Edge](tutorial-deploy-azure-resources.md)
