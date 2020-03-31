---
title: Acceso a registros de consultas lentos de Azure Database for MariaDB mediante Azure Portal
description: En este artículo se describe cómo configurar los registros de consultas lentos en Azure Database for MariaDB, y acceder a ellos, mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 89bdd209315445519c35f3ef2c2f1ad2555106ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79531403"
---
# <a name="configure-and-access-slow-query-logs-from-the-azure-portal"></a>Configuración y acceso a los registros de consultas lentas en Azure Portal

Se pueden configurar, enumerar y descargar [registros de consulta lentos de Azure Database for MariaDB](concepts-server-logs.md) desde Azure Portal.

## <a name="prerequisites"></a>Prerrequisitos
Los pasos descritos en este artículo requieren que tenga un [servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md).

## <a name="configure-logging"></a>registro
Configure el acceso al registro de consultas lentas. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

2. Seleccione un servidor de Azure Database for MariaDB.

3. En la sección **Supervisión** de la barra lateral, seleccione **Registros de servidor**. 
   ![Captura de pantalla de las opciones de registros de servidor](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. Seleccione **Haga clic aquí para habilitar los registros y configurar los parámetros** para ver los parámetros del servidor.

5. Cambie los parámetros que quiera ajustar, y cambie el estado de **slow_query_log** a **ON**. Todos los cambios que realice en esta sesión se resaltan en color púrpura. 

   Después de cambiar los parámetros, seleccione **Guardar**. O bien puede descartar los cambios.

   ![Captura de pantalla de las opciones de los parámetros del servidor](./media/howto-configure-server-logs-portal/3-save-discard.png)

Desde la página **Parámetros de servidor**, puede volver a la lista de los registros cerrando la página.

## <a name="view-list-and-download-logs"></a>Visualización de lista y descarga de registros
Una vez que comienza el registro, puede ver una lista de los registros de consultas lentas disponibles y descargar archivos de registro individuales. 

1. Abra Azure Portal.

2. Seleccione un servidor de Azure Database for MariaDB.

3. En la sección **Supervisión** de la barra lateral, seleccione **Registros de servidor**. La página muestra una lista de los archivos de registro.

   ![Captura de pantalla de la página Registros de servidor con una lista de registros resaltados](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > La convención de nomenclatura del registro es **mysql-slow-< nombre de su servidor>-yyyymmddhh.log**. La fecha y hora que se utilizan en el nombre de archivo indican el momento en que se emitió el registro. Los archivos de registro se rotan cada 24 horas o 7,5 GB, lo que ocurra primero.

4. Si es necesario, utilice el cuadro de búsqueda para encontrar rápidamente un registro específico en función de la fecha y hora. La búsqueda se encuentra en el nombre del registro.

5. Para descargar los archivos de registro individuales, seleccione el icono de flecha hacia abajo que hay junto a cada archivo de registro en la fila de tabla.

   ![Captura de pantalla de la página Registros de servidor, con el icono de flecha hacia abajo resaltado](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>Configuración de registros de diagnósticos

1. En la sección **Supervisión** de la barra lateral, seleccione **Configuración de diagnóstico** > **Add diagnostic settings** (Agregar configuración de diagnóstico).

   ![Captura de pantalla de las opciones de diagnóstico](./media/howto-configure-server-logs-portal/add-diagnostic-setting.png)

1. Proporcione un nombre de configuración de diagnóstico.

1. Especifique a qué receptores de datos se enviarán los registros de consultas lentas (cuenta de almacenamiento, centro de eventos o área de trabajo de Log Analytics).

1. Seleccione **MySqlSlowLogs** como tipo de registro.
![Captura de pantalla de las opciones de configuración de diagnóstico](./media/howto-configure-server-logs-portal/configure-diagnostic-setting.png)

1. Una vez que haya configurado los receptores de datos a los que canalizar los registros de consultas lentas, seleccione **Guardar**.
![Captura de pantalla de las opciones de configuración de diagnóstico, con la opción Guardar resaltada](./media/howto-configure-server-logs-portal/save-diagnostic-setting.png)

1. Acceda a los registros de consultas lentas explorándolos en los receptores de datos que configuró. Los registros pueden tardar hasta 10 minutos en aparecer.

## <a name="next-steps"></a>Pasos siguientes
- Consulte el artículo de [configuración y acceso a los registros de consultas lentas con la CLI de Azure](howto-configure-server-logs-cli.md) para más información sobre cómo descargar registros de consultas lentas mediante programación.
- Más información sobre los [registros de consultas lentas](concepts-server-logs.md) en Azure Database for MariaDB.
- Para obtener más información sobre las definiciones de parámetros y el registro, consulte la documentación de MariaDB sobre [registros](https://mariadb.com/kb/en/library/slow-query-log-overview/).