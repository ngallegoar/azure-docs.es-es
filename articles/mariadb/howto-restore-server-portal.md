---
title: Copia de seguridad y restauración en Azure Database for MariaDB mediante Azure Portal
description: En este artículo se describe cómo restaurar un servidor en Azure Database for MariaDB mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/27/2020
ms.openlocfilehash: fa8ead8daa202f5747c134a62fbd43bcdf2af0d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369259"
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-mariadb-using-the-azure-portal"></a>Copia de seguridad y restauración de un servidor en Azure Database for MariaDB mediante Azure Portal

## <a name="backup-happens-automatically"></a>Las copias de seguridad se realizan automáticamente
Periódicamente, se realizan copias de seguridad de los servidores de Azure Database for MariaDB para habilitar las características de restauración. Con esta característica, puede restaurar el servidor y todas sus bases de datos en un servidor nuevo a un momento dado anterior.

## <a name="prerequisites"></a>Prerrequisitos
Para completar esta guía, necesita:
- Un [servidor y base de datos de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="set-backup-configuration"></a>Configuración de copia de seguridad

Elija la configuración del servidor para copias de seguridad con redundancia local o con redundancia geográfica en el momento de crear el servidor, en la ventana **Plan de tarifa**.

> [!NOTE]
> Después de crear un servidor, no se puede cambiar el tipo de redundancia elegido, redundancia geográfica o redundancia local.
>

Al crear un servidor mediante Azure Portal, en la ventana **Plan de tarifa** se selecciona **Redundancia local** o **Redundancia geográfica** para las copias de seguridad del servidor. En esta ventana se selecciona también el **Período de retención de copia de seguridad**, es decir, durante cuánto tiempo se almacenarán las copias de seguridad del servidor.

   ![Plan de tarifa: elija la redundancia de las copias de seguridad](./media/howto-restore-server-portal/pricing-tier.png)

Para más información acerca de cómo establecer estos valores durante la creación, consulte la [guía de inicio rápido del servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md).

Para cambiar el período de retención de copia de seguridad en un servidor, siga estos pasos:
1. Inicie sesión en el [Portal de Azure](https://portal.azure.com/).

2. Seleccione un servidor de Azure Database for MariaDB. Esta acción abre la página **Información general**.

3. Seleccione **Plan de tarifa** en el menú, en **Configuración**. Con el control deslizante puede cambiar el **Período de retención de copia de seguridad** entre 7 y 35 días.
En la captura de pantalla siguiente, se ha aumentado a 35 días.
![Aumento del período de retención de copia de seguridad](./media/howto-restore-server-portal/3-increase-backup-days.png)

4. Haga clic en **Aceptar** para confirmar el cambio.

El período de retención de copia de seguridad rige durante cuánto tiempo se puede realizar una restauración a un momento dado, porque se basa en las copias de seguridad disponibles. La restauración a un momento dado se describe con más detalle en la sección siguiente. 

## <a name="point-in-time-restore"></a>Restauración a un momento dado
Azure Database for MariaDB permite restaurar el servidor a un momento dado y en una nueva copia del servidor. Puede usar este nuevo servidor para recuperar los datos o hacer que las aplicaciones cliente apunten a este nuevo servidor.

Por ejemplo, si una tabla se quitó accidentalmente a mediodía de hoy, podría restaurar al momento justo antes de mediodía y recuperar la tabla y los datos que faltan desde esa copia nueva del servidor. La restauración a un momento dado se realiza en el nivel del servidor, no en el nivel de base de datos.

Los siguientes pasos restauran el servidor de ejemplo a un momento dado:
1. En Azure Portal, seleccione el servidor de Azure Database for MariaDB. 

2. En la barra de herramientas de la página **Información general** del servidor, seleccione **Restaurar**.

   ![Azure Database for MariaDB - Información general - Botón Restaurar](./media/howto-restore-server-portal/2-server.png)

3. Rellene el formulario Restaurar con la información necesaria:

   ![Azure Database for MariaDB - Información sobre restauración](./media/howto-restore-server-portal/3-restore.png)
   - **Punto de restauración**: seleccione el momento al que desea restaurar.
   - **Servidor de destino:** : proporcione un nombre para el nuevo servidor.
   - **Ubicación**: no se puede seleccionar la región. De manera predeterminada, es el mismo que el del servidor de origen.
   - **Plan de tarifa**: estos parámetros no se pueden cambiar al realizar una restauración a un momento dado. Es el mismo que el del servidor de origen. 

4. Haga clic en **Aceptar** para restaurar el servidor a un momento dado. 

5. Una vez finalizada la restauración, busque el nuevo servidor que se crea para comprobar que los datos se restauraron del modo esperado.


El nuevo servidor creado mediante restauración a un momento dado tiene el mismo nombre de inicio de sesión y contraseña de administrador del servidor que el servidor existente tenía en ese momento dado. Puede cambiar la contraseña en la página **Información general** del nuevo servidor.

El servidor creado durante una restauración no tiene los puntos de conexión de servicio VNet que existían en el servidor original. Estas reglas deben configurarse por separado para este nuevo servidor. Se restauran las reglas de firewall del servidor original.

## <a name="geo-restore"></a>Restauración geográfica

Si ha configurado el servidor para copias de seguridad con redundancia geográfica, se puede crear un nuevo servidor a partir de la copia de seguridad de ese servidor existente. Este nuevo servidor puede crearse en cualquier región en la que Azure Database for MariaDB esté disponible.  

1. Seleccione **Bases de datos** > **Azure Database for MariaDB**. También puede escribir **MariaDB** en el cuadro de búsqueda para encontrar el servicio.

   ![La opción Azure Database for MariaDB](./media/howto-restore-server-portal/2_navigate-to-mariadb.png)

2. En el menú desplegable **Seleccionar origen** del formulario, elija **Copia de seguridad**. Esta acción carga una lista de servidores que tienen habilitadas copias de seguridad con redundancia geográfica. Seleccione una de las copias de seguridad como origen del nuevo servidor.
   ![Seleccionar origen: Azure Backup y lista de copias de seguridad con redundancia geográfica](./media/howto-restore-server-portal/2-georestore.png)

   > [!NOTE]
   > Al crear por primera vez un servidor, puede que no esté disponible para la restauración geográfica inmediatamente. Los metadatos pueden tardar unas horas en rellenarse.
   >

3. Rellene el resto del formulario con sus preferencias. Puede seleccionar cualquier valor en **Ubicación**. Después de seleccionar la ubicación, seleccione un valor en **Plan de tarifa**. De forma predeterminada, se muestran los parámetros del servidor existente que se va a restaurar. Puede hacer clic en **Aceptar** sin realizar ningún cambio para heredar esa configuración. O bien, puede cambiar los valores de **Generación de procesos** (si está disponible en la región que haya elegido), **Núcleos virtuales**, **Período de retención de copia de seguridad** y **Opciones de redundancia de copia de seguridad**. No se permite cambiar el **Plan de tarifa** (Básico, Uso general o Memoria optimizada) ni el tamaño de **Almacenamiento** durante la restauración.

El nuevo servidor creado mediante restauración geográfica tiene el mismo nombre de inicio de sesión y contraseña de administrador del servidor que el servidor existente tenía cuando se inició la restauración. La contraseña se puede cambiar en la página **Información general** del nuevo servidor.

El servidor creado durante una restauración no tiene los puntos de conexión de servicio VNet que existían en el servidor original. Estas reglas deben configurarse por separado para este nuevo servidor. Se restauran las reglas de firewall del servidor original.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre las [copias de seguridad](concepts-backup.md) del servicio.
- Más información sobre las [réplicas](concepts-read-replicas.md).
- Más información sobre las opciones de [continuidad del negocio](concepts-business-continuity.md).
