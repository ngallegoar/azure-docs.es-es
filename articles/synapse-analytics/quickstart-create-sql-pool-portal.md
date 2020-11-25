---
title: 'Inicio rápido: Creación de un grupo de SQL dedicado (versión preliminar) mediante Azure Portal'
description: Siga los pasos de esta guía para crear un grupo de SQL dedicado mediante Azure Portal.
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: b558e34d3f5d224515eee669dfa7b8f2c6bbb3cd
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94541018"
---
# <a name="quickstart-create-a-dedicated-sql-pool-preview-using-the-azure-portal"></a>Inicio rápido: Creación de un grupo de SQL dedicado (versión preliminar) mediante Azure Portal

Azure Synapse Analytics ofrece varios motores de análisis que le ayudarán a ingerir, transformar, modelar y analizar sus datos. Un grupo de SQL dedicado ofrece funcionalidades de proceso y almacenamiento basadas en T-SQL. Después de crear un grupo de SQL dedicado en el área de trabajo de Synapse, los datos se pueden cargar, modelar, procesar y entregar para tener información para los análisis más rápidamente.

En este inicio rápido aprenderá a crear un grupo de SQL dedicado en un área de trabajo de Synapse mediante Azure Portal.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita antes de empezar](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)
- [Área de trabajo de Synapse](./quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

## <a name="navigate-to-the-synapse-workspace"></a>Desplazamiento al área de trabajo de Synapse

1. Vaya al área de trabajo de Synapse en la que se va a crear el grupo de SQL dedicado. Para ello, escriba el nombre del servicio (o el nombre del recurso directamente) en la barra de búsqueda.
![Barra de búsqueda de Azure Portal en la que se ha escrito "Áreas de trabajo de Synapse".](media/quickstart-create-sql-pool/create-sql-pool-00a.png). 
1. En la lista de áreas de trabajo, escriba el nombre (o una parte del nombre) del área que desea abrir. En este ejemplo, se usará un área de trabajo denominado **contosoanalytics**.
![Lista de áreas de trabajo de Synapse filtradas en la que se muestran las que contienen el nombre Contoso.](media/quickstart-create-sql-pool/create-sql-pool-00b.png)

## <a name="create-new-dedicated-sql-pool"></a>Creación de un grupo de SQL dedicado

1. En el área de trabajo de Synapse en la que desea crear el grupo de SQL dedicado, seleccione el comando **New dedicado SQL pool** (Nuevo grupo de SQL dedicado) en la barra superior.
![Información general sobre el área de trabajo de Synapse con un recuadro rojo alrededor del comando para crear un nuevo grupo de SQL dedicado.](media/quickstart-create-sql-pool/create-sql-pool-portal-01.png)
2. Escriba la siguiente información en la pestaña **Datos básicos**.

    | Configuración | Valor sugerido | Descripción |
    | :------ | :-------------- | :---------- |
    | **Nombre del grupo de SQL dedicado** | Cualquier nombre válido | Nombre del grupo de SQL dedicado. |
    | **Nivel de rendimiento** | DW100c | Establézcalo en el menor tamaño para reducir los costos de este inicio rápido. |

  
    ![Flujo de creación del grupo de SQL dedicado: pestaña de datos básicos.](media/quickstart-create-sql-pool/create-sql-pool-portal-02.png)

    > [!IMPORTANT]
    > Tenga en cuenta que existen limitaciones específicas para los nombres que los grupos de SQL dedicados pueden usar. Los nombres no pueden contener caracteres especiales, deben tener 15 caracteres como máximo, no contener palabras reservadas y ser únicos en el área de trabajo.

3. Seleccione **Siguiente: Configuración adicional**.
4. Seleccione **None** (Ninguno) para aprovisionar el grupo de SQL dedicado sin datos. Deje seleccionada la intercalación predeterminada.

    Si desea restaurar el grupo de SQL dedicado desde un punto de restauración, seleccione el **punto de restauración**. Para obtener más información sobre cómo realizar una restauración, vea [ Restauración de un grupo de SQL dedicado](backuprestore/restore-sql-pool.md)

![Flujo de creación del grupo de SQL dedicado: pestaña de configuración adicional.](media/quickstart-create-sql-pool/create-sql-pool-portal-03.png)

5. Seleccione **Revisar + crear**.
6. Asegúrese de que los detalles son correctos en función de lo que se especificó anteriormente. Seleccione **Crear**.
![Flujo de creación del grupo de SQL dedicado: pestaña de revisión de la configuración.](media/quickstart-create-sql-pool/create-sql-pool-portal-04.png)

7. Llegados a este punto, se iniciará el flujo de aprovisionamiento de recursos.
 ![Captura de pantalla que muestra la página "Se completó la implementación".](media/quickstart-create-sql-pool/create-sql-pool-portal-06.png)

8. Una vez completado el aprovisionamiento, al desplazarse al área de trabajo se mostrará una nueva entrada para el grupo de SQL dedicado recién creado.
 ![Flujo de creación del grupo de SQL: aprovisionamiento de recursos.](media/quickstart-create-sql-pool/create-sql-pool-studio-27.png)


Una vez que se cree el grupo de SQL dedicado, estará disponible en el área de trabajo para la carga de datos, el procesamiento de secuencias, la lectura del lago, etc.

## <a name="clean-up-resources"></a>Limpieza de recursos

Siga los pasos que se indican a continuación para eliminar el grupo de SQL dedicado del área de trabajo.
> [!WARNING]
> Al eliminar un grupo de SQL dedicado, se quitan del área de trabajo el motor de análisis y los datos almacenados en la base de datos del grupo de SQL dedicado eliminado. Ya no será posible conectarse al grupo de SQL dedicado y todas las consultas, canalizaciones y cuadernos que lean o escriban en este grupo de SQL dedicado dejarán de funcionar.

Si quiere eliminar el grupo de SQL dedicado, siga estos pasos:

1. Vaya a la hoja de grupos de SQL en la hoja del área de trabajo.
1. Seleccione el grupo de SQL dedicado que se va a eliminar (en este caso, **contosowdw**).
1. Una vez seleccionado, seleccione **Delete** (Eliminar).
1. Confirme la eliminación y haga clic en el botón **Delete** (Eliminar)![. Información general del grupo de SQL dedicado: se resalta la confirmación de eliminación.](media/quickstart-create-sql-pool/create-sql-pool-portal-11.png)
1. Cuando el proceso se complete correctamente, el grupo de SQL dedicado dejará de aparecer en los recursos del área de trabajo.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Quickstart: Creación de un grupo de Apache Spark sin servidor en Synapse Studio mediante herramientas web](quickstart-apache-spark-notebook.md).
- Consulte [Quickstart: Creación de un grupo de Apache Spark sin servidor mediante Azure Portal](quickstart-create-apache-spark-pool-portal.md).
