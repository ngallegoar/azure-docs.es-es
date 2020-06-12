---
title: Administración de paquetes con el almacén de paquetes de Azure-SSIS Integration Runtime
description: Aprenda a administrar paquetes con el almacén de paquetes de Azure-SSIS Integration Runtime.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 05/25/2020
ms.openlocfilehash: fa1ab60ae5690bd3782e4c849ca7cb5c29d640c2
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84198872"
---
# <a name="manage-packages-with-azure-ssis-integration-runtime-package-store"></a>Administración de paquetes con el almacén de paquetes de Azure-SSIS Integration Runtime

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Para migrar mediante lift-and-shift las cargas de trabajo locales de SQL Server Integration Services (SSIS) a la nube, puede aprovisionar Azure-SSIS Integration Runtime (IR) en Azure Data Factory (ADF). Para obtener más información, vea [Aprovisionamiento de Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure). Azure-SSIS Integration Runtime admite:

- La ejecución de paquetes implementados en el catálogo de SSIS (SSISDB) hospedados por un servidor de Azure SQL Database o por Instancia administrada (modelo de implementación de proyectos)
- La ejecución de paquetes implementados en el sistema de archivos, en Azure Files o en una base de datos de SQL Server (MSDB) hospedados por Instancia administrada de Azure SQL (modelo de implementación de paquetes)

Al usar el modelo de implementación de paquetes, puede elegir si quiere aprovisionar Azure-SSIS IR con almacenes de paquetes que proporcionan una capa de administración de paquetes encima del sistema de archivos, de Azure Files o de MSDB hospedada en Instancia administrada de Azure SQL. El almacén de paquetes de Azure-SSIS IR permite importar, exportar, eliminar y ejecutar paquetes, así como supervisar y detener los paquetes en ejecución mediante SQL Server Management Studio (SSMS) de forma similar al [almacén de paquetes de SSIS heredado](https://docs.microsoft.com/sql/integration-services/service/package-management-ssis-service?view=sql-server-2017). 

## <a name="connect-to-azure-ssis-ir"></a>Conexión a Azure-SSIS IR

Una vez que se ha aprovisionado Azure-SSIS IR, puede conectarse a este para examinar los almacenes de paquetes en SSMS.

![Conexión a Azure-SSIS IR](media/azure-ssis-integration-runtime-package-store/ssms-package-store-connect.png)

En la ventana **Explorador de objetos** de SSMS, seleccione **Azure-SSIS Integration Runtime** en el menú desplegable **Conectar**. Después, inicie sesión en Azure y seleccione la suscripción y las instancias de ADF y Azure-SSIS IR pertinentes a las que están asociados los almacenes de paquetes. Aparecerá Azure-SSIS IR con los nodos **Paquetes en ejecución** y **Paquetes almacenados** situados debajo. Expanda el nodo **Paquetes almacenados** para ver debajo los almacenes de paquetes. Expanda los almacenes de paquetes para ver debajo las carpetas y los paquetes. Es posible que se le pida que escriba las credenciales de acceso para los almacenes de paquetes, en caso de que SSMS no se pueda conectar a ellos automáticamente. Por ejemplo, si expande un almacén de paquetes encima de MSDB, es posible que se le pida que se conecte primero a Instancia administrada de Azure SQL.

![Conexión a Instancia administrada de Azure SQL](media/azure-ssis-integration-runtime-package-store/ssms-package-store-connect2.png)

## <a name="manage-folders-and-packages"></a>Administración de carpetas y paquetes

Al examinar Azure-SSIS IR en SSMS, puede hacer clic con el botón derecho en cualquier almacén de paquetes, carpeta o paquete para que aparezca un menú emergente y seleccionar **Nueva carpeta**, **Importar paquete**, **Exportar paquete**, **Eliminar** o **Actualizar**.

   ![Administración de carpetas y paquetes](media/azure-ssis-integration-runtime-package-store/ssms-package-store-manage.png)

   *  Seleccione **Nueva carpeta** para crear una carpeta para los paquetes importados.

   *  Seleccione **Importar paquete** para importar paquetes del **Sistema de archivos**, **SQL Server** (MSDB) o el **Almacén de paquetes de SSIS** heredado en el almacén de paquetes.

      ![Importación de paquete](media/azure-ssis-integration-runtime-package-store/ssms-package-store-import.png)

      En función de la **Ubicación del paquete** desde la que se va a importar, seleccione el valor pertinente de **Servidor**/**Tipo de autenticación**, escriba las credenciales de acceso si es necesario, seleccione la **Ruta de acceso del paquete** y escriba el nuevo **Nombre del paquete**. Al importar paquetes, no se puede cambiar su nivel de protección. Para cambiarlo, use SQL Server Data Tools (SSDT) o la utilidad de línea de comandos `dtutil`.

   *  Seleccione **Exportar paquete** para exportar paquetes del almacén de paquetes al **Sistema de archivos**, **SQL Server** (MSDB) o el **Almacén de paquetes de SSIS** heredado.

      ![Exportación de paquetes](media/azure-ssis-integration-runtime-package-store/ssms-package-store-export.png)

      En función de la **Ubicación del paquete** a la que se va a exportar, seleccione el valor pertinente de **Servidor**/**Tipo de autenticación**, escriba las credenciales de acceso si es necesario y seleccione la **Ruta de acceso del paquete**. Al exportar paquetes, si están cifrados, escriba las contraseñas para descifrarlos primero. Después, puede cambiar su nivel de protección, por ejemplo, para no guardar datos confidenciales o para cifrarlo todo, para cifrar la información confidencial con una clave de usuario o para cifrar la contraseña.

   *  Seleccione **Eliminar** para eliminar los paquetes o las carpetas del almacén de paquetes.

   *  Seleccione **Actualizar** para mostrar los paquetes y las carpetas recién agregados al almacén de paquetes.

## <a name="execute-packages"></a>Ejecución de paquetes

Al examinar Azure-SSIS IR en SSMS, puede hacer clic con el botón derecho en cualquier paquete almacenado para que aparezca un menú emergente y seleccionar **Ejecutar paquete**.  Se abrirá el cuadro de diálogo **Utilidad de ejecución de paquetes**, donde podrá configurar las ejecuciones de paquetes en Azure-SSIS IR como actividades de tipo Ejecutar paquete de SSIS en canalizaciones de ADF.

![Páginas 1 y 2 de Utilidad de ejecución de paquetes](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute.png)

![Páginas 3 y 4 de Utilidad de ejecución de paquetes](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute2.png)

Las páginas **General**, **Configuraciones**, **Opciones de ejecución** y **Registro** del cuadro de diálogo **Utilidad de ejecución de paquetes** se corresponden con la pestaña **Configuración** de la actividad Ejecutar paquete de SSIS, donde se puede especificar la contraseña de cifrado del paquete, la información de acceso del archivo de configuración del paquete, las credenciales o propiedades de ejecución del paquete y la información de acceso de la carpeta de registro.  La página **Establecer valores** del cuadro de diálogo **Utilidad de ejecución de paquetes** se corresponde con la pestaña **Invalidaciones de propiedad** de la actividad Ejecutar paquete de SSIS, donde se pueden especificar las propiedades del paquete que se van a invalidar. Para obtener información, vea [Ejecución de un paquete de SSIS mediante la actividad Ejecutar paquete de SSIS en canalizaciones de ADF](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Al seleccionar el botón **Ejecutar** del cuadro de diálogo **Utilidad de ejecución de paquetes**, se generará y se desencadenará automáticamente una nueva canalización de ADF con la actividad Ejecutar paquete de SSIS. Si ya existe una canalización de ADF con la misma configuración de ejecución de paquetes, se volverá a ejecutar y no se generará una nueva canalización. La canalización de ADF y la actividad Ejecutar paquete de SSIS se denominarán `Pipeline_SSMS_YourPackageName_HashString` y `Activity_SSMS_YourPackageName`, respectivamente.

![Botón de la Utilidad de ejecución de paquetes](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute3.png)

![Ejecutar una actividad de paquete SSIS](media/azure-ssis-integration-runtime-package-store/ssis-activity-package-store.png)

## <a name="monitor-and-stop-running-packages"></a>Supervisión y detención de los paquetes en ejecución

Al examinar Azure-SSIS IR en SSMS, puede expandir el nodo **Paquetes en ejecución** para ver debajo los paquetes que se están ejecutando.  Haga clic con el botón derecho en cualquiera de ellos para que aparezca un menú emergente y seleccione **Detener** o **Actualizar**.

   ![Supervisión y detención de los paquetes en ejecución](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor.png)

   *  Seleccione **Detener** para cancelar la canalización de ADF que se está ejecutando actualmente y que ejecuta el paquete como una actividad Ejecutar paquete de SSIS.

   *  Seleccione **Actualizar** para mostrar los paquetes en ejecución recientemente en los almacenes de paquetes.

## <a name="monitor-azure-ssis-ir-and-edit-package-stores"></a>Supervisión de Azure-SSIS IR y edición de los almacenes de paquetes

Al examinar Azure-SSIS IR en SSMS, puede hacer clic con el botón derecho en él para que aparezca un menú emergente y seleccionar **Go to Azure Data Factory portal** (Ir al portal de Azure Data Factory) o **Actualizar**.

   ![Ir al portal de ADF](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor2.png)

   *  Seleccione **Go to Azure Data Factory portal** (Ir al portal de Azure Data Factory) para abrir la página **Integration runtimes** (Entornos de ejecución de integración) del centro de supervisión de ADF, donde puede supervisar Azure-SSIS IR. En el icono **PACKAGE STORES** (ALMACENES DE PAQUETES), puede ver el número de almacenes de paquetes que están conectados a Azure-SSIS IR.  Al seleccionar ese número, aparecerá una ventana emergente donde puede editar los servicios vinculados de ADF que almacenan la información de acceso de los almacenes de paquetes.

      ![Edición de los almacenes de paquetes](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor3.png)

   *  Seleccione **Actualizar** para mostrar las carpetas o paquetes recién agregados a los almacenes de paquetes y los paquetes en ejecución en los almacenes de paquetes.

## <a name="next-steps"></a>Pasos siguientes

Puede volver a ejecutar o editar las canalizaciones de ADF generadas automáticamente con actividades de tipo Ejecutar paquete de SSIS, o bien crear otras nuevas en el portal de ADF. Para obtener información, vea [Ejecución de un paquete de SSIS mediante la actividad Ejecutar paquete de SSIS en canalizaciones de ADF](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).