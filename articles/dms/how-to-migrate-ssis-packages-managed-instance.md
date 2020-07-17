---
title: Migración de paquetes de SQL Server Integration Services a una instancia administrada de Azure SQL Database
titleSuffix: Azure Database Migration Service
description: Obtenga información sobre cómo migrar paquetes y proyectos de SQL Server Integration Services (SSIS) a Instancia administrada de Azure SQL mediante Azure Database Migration Service o Data Migration Assistant.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 13fcda03f3ccd0c28e1dce594c46b8c0d9449054
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086575"
---
# <a name="migrate-sql-server-integration-services-packages-to-an-azure-sql-managed-instance"></a>Migración de paquetes de SQL Server Integration Services a Instancia administrada de Azure SQL
Si usa SQL Server Integration Services (SSIS) y quiere migrar sus proyectos o paquetes de SSIS desde la SSISDB de origen, que está hospedada en SQL Server, a la SSISDB de destino, que está hospedada en Instancia administrada de Azure SQL, puede usar Azure Database Migration Service.

Si la versión de SSIS que usa es anterior a 2012 o utiliza tipos de almacenes de paquetes que no sean SSISDB, antes de migrar los proyectos o paquetes de SSIS, es preciso que los convierta, para lo que debe usar el Asistente para la conversión de proyectos de Integration Services, que también se puede iniciar desde SSMS. Para obtener más información, vea el artículo [Para convertir un proyecto al modelo de implementación de proyectos](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Actualmente, Azure Database Migration Service (DMS) no admite Azure SQL Database como un destino de migración. Para volver a implementar los proyectos o paquetes de SSIS en Azure SQL Database, consulte el artículo [Reimplementación de paquetes de SQL Server Integration Services a Azure SQL Database](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

En este artículo aprenderá a:
> [!div class="checklist"]
>
> * Evaluar los proyectos o paquetes SSIS de origen.
> * Migrar de paquetes o proyectos SSIS a Azure.

## <a name="prerequisites"></a>Requisitos previos

Para completar estos pasos, necesitará lo siguiente:

* Crear una instancia de Microsoft Azure Virtual Network para Azure Database Migration Service mediante el modelo de implementación de Azure Resource Manager, que proporciona conectividad de sitio a sitio a los servidores de origen local mediante [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) o [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Para más información, consulte el artículo [Topologías de red para migraciones de Instancia administrada de SQL mediante Azure Database Migration Service]( https://aka.ms/dmsnetworkformi). Para más información sobre la creación de una red virtual, consulte la documentación de [Virtual Network](https://docs.microsoft.com/azure/virtual-network/)y, especialmente, los artículos de inicio rápido con detalles paso a paso.
* Asegurarse de que las reglas del grupo de seguridad de red de la red virtual no bloqueen los siguientes puertos de comunicación de entrada a Azure Database Migration Service: 443, 53, 9354, 445, 12000. Para más información sobre el filtrado del tráfico con grupos de seguridad de red para redes virtuales, vea el artículo [Filtrado del tráfico de red con grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* Configurar [Firewall de Windows para acceder al motor de base de datos de origen](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access?view=sql-server-2017).
* Abrir Firewall de Windows para permitir que Azure Database Migration Service acceda a la instancia de SQL Server de origen que, de manera predeterminada, es el puerto TCP 1433.
* Si se ejecutan varias instancias con nombre de SQL Server con puertos dinámicos, puede ser conveniente habilitar el servicio SQL Browser y permitir el acceso al puerto UDP 1434 mediante los firewalls para que Azure Database Migration Service pueda conectarse a una instancia con nombre en el servidor de origen.
* Si va a usar un dispositivo de firewall frente a las bases de datos de origen, puede que sea necesario agregar reglas de firewall para permitir que Azure Database Migration Service acceda a las bases de datos de origen para realizar la migración, así como archivos a través del puerto SMB 445.
* Instancia administrada de SQL para hospedar SSISDB. Si necesita crearla, siga los pasos que se describen en el artículo [Creación de una Instancia administrada de Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* Asegurarse de que los inicios de sesión que se usan para conectar la instancia de SQL Server de origen y la instancia administrada de destino son miembros del rol de servidor sysadmin.
* Para comprobar que SSIS está aprovisionado en Azure Data Factory (ADF) y contiene una instancia de Azure-SSIS Integration Runtime (IR) con la SSISDB de destino hospedada por una Instancia administrada de SQL (como se describe en el artículo [Creación de la instancia de Azure-SSIS Integration Runtime en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)).

## <a name="assess-source-ssis-projectspackages"></a>Evaluar los proyectos o paquetes SSIS de origen

Mientras que la evaluación de la SSISDB de origen aún no está integrada en Database Migration Assistant (DMA), los proyectos o paquetes de SSIS se evaluarán o validarán a medida que se vuelvan a implementar en la SSISDB de destino hospedada en una Instancia administrada de Azure SQL.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registro del proveedor de recursos Microsoft.DataMigration

1. Inicie sesión en Azure Portal, seleccione **Todos los servicios** y, después, **Suscripciones**.

    ![Mostrar suscripciones en el portal](media/how-to-migrate-ssis-packages-mi/portal-select-subscriptions.png)

2. Seleccione la suscripción en la que quiere crear la instancia de Azure Database Migration Service y después seleccione **Proveedores de recursos**.

    ![Mostrar los proveedores de recursos](media/how-to-migrate-ssis-packages-mi/portal-select-resource-provider.png)

3. Busque la migración y después, a la derecha de **Microsoft.DataMigration**, seleccione **Registrar**.

    ![Registro del proveedor de recursos](media/how-to-migrate-ssis-packages-mi/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Creación de una instancia de Azure Database Migration Service

1. En Azure Portal, seleccione **+ Crear un recurso**, busque **Azure Database Migration Service** y, a continuación, seleccione **Azure Database Migration Service** en la lista desplegable.

     ![Azure Marketplace](media/how-to-migrate-ssis-packages-mi/portal-marketplace.png)

2. En la pantalla **Azure Database Migration Service**, seleccione **Crear**.

    ![Creación de una instancia de Azure Database Migration Service](media/how-to-migrate-ssis-packages-mi/dms-create1.png)

3. En la pantalla **Crear el servicio de migración**, especifique un nombre para el servicio, la suscripción y un grupo de recursos nuevo o existente.

4. Seleccione la ubicación en la que desea crear la instancia de DMS.

5. Seleccione una red virtual existente o cree una.

    La red virtual proporciona a Azure Database Migration Service acceso al servidor SQL Server de origen y a la Instancia administrada de Azure SQL de destino.

    Para más información sobre cómo crear una red virtual en Azure Portal, consulte el artículo [Creación de una red virtual mediante Azure Portal](https://aka.ms/DMSVnet).

    Para obtener detalles adicionales, consulte el artículo [Topologías de red para migraciones a Azure SQL Managed Instance Database con Azure Database Migration Service](https://aka.ms/dmsnetworkformi).

6. Seleccione un plan de tarifa.

    Para más información sobre los costos y planes de tarifa, vea la [página de precios](https://aka.ms/dms-pricing).

    ![Creación de servicio DMS](media/how-to-migrate-ssis-packages-mi/dms-create-service2.png)

7. Seleccione **Crear** para crear el servicio.

## <a name="create-a-migration-project"></a>Creación de un proyecto de migración

Después de crear una instancia del servicio, búsquela en Azure Portal, ábrala y cree un proyecto de migración.

1. En Azure Portal, seleccione **Todos los servicios**, busque Azure Database Migration Service y, luego, elija **Azure Database Migration Services**.

    ![Búsqueda de todas las instancias de Azure Database Migration Service](media/how-to-migrate-ssis-packages-mi/dms-search.png)

2. En la pantalla **Azure Database Migration Service**, busque el nombre de la instancia que ha creado y selecciónela.

3. Seleccione **+ New Migration Project** (+ Nuevo proyecto de migración).

4. En la pantalla **Nuevo proyecto de migración**, especifique el nombre del proyecto; en el cuadro de texto **Tipo de servidor de origen**, seleccione **SQL Server**; en el cuadro de texto **Tipo de servidor de destino**, seleccione **Instancia administrada de Azure SQL** y, finalmente, en **Elegir el tipo de actividad**, seleccione **Migración de paquetes SSIS**.

   ![Creación de proyecto DMS](media/how-to-migrate-ssis-packages-mi/dms-create-project2.png)

5. Seleccione **Crear** para crear el proyecto.

## <a name="specify-source-details"></a>Especificación de los detalles de origen

1. En la pantalla **Detalles del origen de la migración**, especifique los detalles de conexión de SQL Server de origen.

2. Si no ha instalado ningún certificado de confianza en el servidor, seleccione la casilla **Certificado de servidor de confianza**.

    Si no hay ningún certificado de confianza instalado, SQL Server genera un certificado autofirmado cuando se inicia la instancia. Este certificado se usa para cifrar las credenciales de las conexiones del cliente.

    > [!CAUTION]
    > Las conexiones TLS cifradas mediante un certificado autofirmado no proporcionan una gran seguridad. Son susceptibles de sufrir ataques de tipo "Man in the middle". No debe confiar en TLS con certificados autofirmados en un entorno de producción, ni en servidores conectados a Internet.

   ![Detalles del origen](media/how-to-migrate-ssis-packages-mi/dms-source-details1.png)

3. Seleccione **Guardar**.

## <a name="specify-target-details"></a>Especificación de los detalles de destino

1. En la pantalla **Detalles del destino de la migración**, especifique los detalles de conexión del destino.

     ![Detalles de destino](media/how-to-migrate-ssis-packages-mi/dms-target-details2.png)

2. Seleccione **Guardar**.

## <a name="review-the-migration-summary"></a>Examen del resumen de la migración

1. En la pantalla **Migration summary** (Resumen de migración), en el cuadro de texto **Nombre de actividad**, especifique un nombre para la actividad de migración.

2. En **SSIS project(s) and environment(s) overwrite option** (Opción de sobrescritura de entornos y proyectos de SSIS), especifique si desea sobrescribir u omitir los entornos y proyectos de SSIS existentes.

    ![Resumen del proyecto de migración](media/how-to-migrate-ssis-packages-mi/dms-project-summary2.png)

3. Revise y compruebe los detalles relacionados con el proyecto de migración.

## <a name="run-the-migration"></a>Ejecución de la migración

* Seleccione **Ejecutar migración**.

## <a name="next-steps"></a>Pasos siguientes

* Revise las directrices de migración en la [Guía de migración de bases de datos](https://datamigration.microsoft.com/).
