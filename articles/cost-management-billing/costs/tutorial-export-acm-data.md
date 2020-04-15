---
title: 'Tutorial: Creación y administración de datos exportados desde Azure Cost Management'
description: En este artículo se muestra cómo crear y administrar datos de Azure Cost Management para utilizarlos en sistemas externos.
author: bandersmsft
ms.author: banders
ms.date: 03/24/2020
ms.topic: tutorial
ms.service: cost-management-billing
ms.reviewer: adwise
ms.custom: seodec18
ms.openlocfilehash: f0a1515816fe7a9e1d79f69655e6bf21725a0b5d
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877956"
---
# <a name="tutorial-create-and-manage-exported-data"></a>Tutorial: Creación y administración de datos exportados

Si lee el tutorial de análisis de costos, estará familiarizado con la descarga manual de los datos de Cost Management. Sin embargo, puede crear una tarea periódica para exportar automáticamente los datos de Cost Management en el almacenamiento de Azure con una periodicidad diaria, semanal o mensual. Los datos exportados están en formato CSV y contienen toda la información recopilada por Cost Management. A continuación, puede usar los datos exportados en el almacenamiento de Azure con sistemas externos y combinarlos con sus propios datos personalizados. También puede usar los datos exportados en un sistema externo, como un panel u otro sistema financiero.

Vea el vídeo sobre [cómo programar exportaciones a almacenamiento con Azure Cost Management](https://www.youtube.com/watch?v=rWa_xI1aRzo) para conocer más sobre la creación de una exportación programada de los datos de sus costos de Azure a Azure Storage. Para ver otros vídeos, visite el [canal de YouTube de Cost Management](https://www.youtube.com/c/AzureCostManagement).

>[!VIDEO https://www.youtube.com/embed/rWa_xI1aRzo]

Los ejemplos de este tutorial le guiarán durante la exportación de los datos de administración de costos y la comprobación de que los datos se exporten correctamente.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una exportación diaria
> * Verificación de la recopilación de los datos

## <a name="prerequisites"></a>Prerrequisitos
La exportación de datos está disponible para varios tipos de cuenta de Azure, entre las que se incluyen las de los clientes con [Contrato Enterprise](https://azure.microsoft.com/pricing/enterprise-agreement/) y [Contrato de cliente de Microsoft](get-started-partners.md). Para ver la lista completa de tipos de cuenta compatibles, consulte [Understand Cost Management data](understand-cost-mgt-data.md) (Información sobre los datos de Cost Management). Para la exportación de datos por usuario y grupo, se admiten los siguientes permisos o ámbitos de Azure por suscripción. Para más información sobre los ámbitos, consulte [Descripción y uso de ámbitos](understand-work-scopes.md).

- Propietario: puede crear, modificar o eliminar exportaciones programadas para una suscripción.
- Colaborador: puede crear, modificar o eliminar sus propias exportaciones programadas. Puede modificar el nombre de las exportaciones programadas creadas por otros usuarios.
- Lector: puede programar las exportaciones para las que tenga permiso.

Para las cuentas de Azure Storage:
- Se necesitan permisos de escritura para cambiar la cuenta de Storage configurada, independientemente de los permisos en la exportación.
- La cuenta de Azure Storage debe configurarse para el almacenamiento de blobs o archivos.

Si su suscripción es nueva, no podrá usar inmediatamente las características de Cost Management. Para poder hacerlo deberán transcurrir un máximo de 48 horas.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com/).

## <a name="create-a-daily-export"></a>Creación de una exportación diaria

Para crear o ver una exportación de datos o programar una exportación, abra el ámbito deseado en Azure Portal y seleccione **Análisis de costos** en el menú. Por ejemplo, vaya a **Suscripciones**, seleccione una suscripción de la lista y **Análisis de costos**, en el menú. En la parte superior de la página Análisis de costos,seleccione **Exportar** y elija una opción de exportación. Por ejemplo, haga clic en **Programar exportación**.  

> [!NOTE]
> - Además de las suscripciones, puede crear exportaciones en grupo de recursos, cuentas, departamentos e inscripciones. Para más información sobre los ámbitos, consulte [Descripción y uso de ámbitos](understand-work-scopes.md).
>- Cuando haya iniciado sesión como asociado en el ámbito de la cuenta de facturación o en el inquilino de un cliente, podrá exportar los datos a una cuenta de Azure Storage vinculada a su cuenta de almacenamiento de asociado. Sin embargo, debe tener una suscripción activa en el inquilino de CSP.
>


Seleccione **Agregar**, escriba un nombre para la exportación y seleccione la opción **Exportación diaria de costos desde ese mes hasta la fecha actual**. Seleccione **Next** (Siguiente).

![Nuevo ejemplo de exportación que muestra el tipo de exportación](./media/tutorial-export-acm-data/basics_exports.png)

Especifique la suscripción de su cuenta de almacenamiento de Azure y, luego, seleccione la cuenta de almacenamiento.  Especifique el contenedor de almacenamiento y la ruta del directorio al que desea que vaya el archivo de exportación. Seleccione **Next** (Siguiente).

![Nuevo ejemplo de exportación que muestra los detalles de la cuenta de almacenamiento](./media/tutorial-export-acm-data/storage_exports.png)

Revise los detalles de la exportación y seleccione **Crear**.

La exportación nueva aparece en la lista de exportaciones. De forma predeterminada, se habilitan las nuevas exportaciones. Si quiere deshabilitar o eliminar una exportación programada, seleccione cualquier elemento de la lista y, después, **Deshabilitar** o **Eliminar**.

Al principio la exportación puede tardar una o dos horas en ejecutarse. Sin embargo, pueden pasar hasta cuatro horas antes de que se muestren los datos en los archivos exportados.

### <a name="export-schedule"></a>Programación de exportaciones

Las exportaciones programadas se ven afectadas por la hora y el día de la semana en que se crearon inicialmente. Al crear una exportación programada, esta se ejecuta con la misma frecuencia las veces siguientes. Por ejemplo, para un conjunto de exportación con frecuencia diaria cada mes hasta la fecha, la exportación se ejecuta todos los días. Pasa lo mismo con una exportación semanal, se ejecuta cada semana, el mismo día en el que fue programada. No se garantiza el tiempo de entrega exacto de la exportación y los datos exportados están disponibles en un plazo de cuatro horas a partir del tiempo de ejecución.
Cada exportación crea un nuevo archivo, por lo que las exportaciones anteriores no se sobrescriben.

Existen dos tipos de opciones de exportación:

**Exportación diaria de los costos del mes hasta la fecha de**: la exportación inicial se ejecuta inmediatamente. Las exportaciones posteriores se ejecutan el siguiente día y a la misma hora que la exportación inicial. Se agregan los datos más recientes de las exportaciones diarias anteriores.

**Personalizado**: le permite programar exportaciones semanales y mensuales con las opciones de semana hasta la fecha y de mes hasta la fecha. *La exportación inicial se ejecutará inmediatamente.*

Si tiene una suscripción de pago por uso, MSDN o Visual Studio, es posible que el período de facturación no esté alineado con el mes natural. En el caso de esos tipos de suscripciones y grupos de recursos, puede crear una exportación que se alinee con el período de su factura o con los meses naturales. Para crear una exportación que esté alineada con el mes de facturación, navegue a **Personalizado** y, luego, seleccione **Período de facturación hasta la fecha**.  Para crear una exportación que se alinee con el mes natural, seleccione **Mes hasta la fecha**.
>
>

![La pestaña Exportación nueva - Básico que muestra una selección de semana hasta la fecha personalizada semanalmente](./media/tutorial-export-acm-data/tutorial-export-schedule-weekly-week-to-date.png)

## <a name="verify-that-data-is-collected"></a>Verificación de la recopilación de los datos

Puede verificar fácilmente que los datos de Cost Management se están recopilando y consultar el archivo CSV exportado mediante el Explorador de Azure Storage.

En la lista de exportaciones, seleccione el nombre de la cuenta de almacenamiento. En la página de la cuenta de almacenamiento, seleccione Abrir en el explorador. Si ve un cuadro de confirmación, seleccione **Sí** para abrir el archivo en el Explorador de Azure Storage.

![Página de la cuenta de almacenamiento que muestra información de ejemplo y un vínculo a Abrir en el Explorador](./media/tutorial-export-acm-data/storage-account-page.png)

En el Explorador de Storage, vaya al contenedor que quiera abrir y seleccione la carpeta correspondiente al mes actual. Se muestra una lista de archivos CSV. Seleccione uno y **Open** (Abrir).

![Información de ejemplo que se muestra en el Explorador de almacenamiento](./media/tutorial-export-acm-data/storage-explorer.png)

El archivo se abre con el programa o la aplicación que se ha establecido para abrir las extensiones de archivos CSV. Este es un ejemplo en Excel.

![Datos CSV exportados de ejemplo que se muestran en Excel](./media/tutorial-export-acm-data/example-export-data.png)


## <a name="access-exported-data-from-other-systems"></a>Acceso a los datos exportados desde otros sistemas

Uno de los propósitos de exportar los datos de Cost Management es tener acceso a los datos desde sistemas externos. Puede usar un sistema de paneles u otro sistema financiero. Estos sistemas pueden variar considerablemente, por lo que no es práctico mostrar un ejemplo.  Sin embargo, puede comenzar accediendo a sus datos desde aplicaciones en [Introducción a Azure Storage](../../storage/common/storage-introduction.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Creación de una exportación diaria
> * Verificación de la recopilación de los datos

Vaya al siguiente tutorial para optimizar y mejorar la eficiencia mediante la identificación de recursos inactivos e infrautilizados.

> [!div class="nextstepaction"]
> [Consulta de las recomendaciones sobre optimización y medidas](tutorial-acm-opt-recommendations.md)
