---
title: Implementación de un servicio web
titleSuffix: ML Studio (classic) - Azure
description: Aprenda a convertir un experimento de entrenamiento en un experimento predictivo, a prepararlo para su implementación y a implementarlo como servicio web de Azure Machine Learning Studio (clásico).
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 01/06/2017
ms.openlocfilehash: 6c81e50560de69f7702e852d4602680fde7f01f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218139"
---
# <a name="deploy-an-azure-machine-learning-studio-classic-web-service"></a>Implementación de un servicio web de Azure Machine Learning Studio (clásico)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Machine Learning Studio (clásico) permite compilar y probar una solución de análisis predictivo. Luego, puede implementar la solución como un servicio web.

Los servicios web de Machine Learning Studio (clásico) proporcionan una interfaz entre una aplicación y un modelo de puntuación de flujo de trabajo de Machine Learning Studio (clásico). Una aplicación externa puede comunicarse con un modelo de puntuación de flujo de trabajo de Machine Learning Studio (clásico) en tiempo real. Una llamada a un servicio web de Machine Learning Studio (clásico) devuelve resultados de predicción a una aplicación externa. Para realizar una llamada a un servicio web Machine Learning, es necesario pasar una clave de API que se creó al implementar el servicio web. Un servicio web de Machine Learning Studio (clásico) se basa en REST, una conocida opción de arquitectura para proyectos de programación web.

Azure Machine Learning Studio (clásico) tiene dos tipos de servicios web:

* Servicio de solicitud-respuesta (RRS): Un servicio de baja latencia y alta escalabilidad que puntúa un registro de datos.
* Servicio de ejecución por lotes (BES): Un servicio asincrónico que puntúa un lote de registros de datos.

La entrada de BES es similar a la entrada de datos que se emplea en RRS. La diferencia principal radica en que BES lee un bloque de registros de varios orígenes, como Azure Blob Storage, Azure Table Storage, Azure SQL Database, HDInsight (consultas de Hive) y orígenes de HTTP.

Desde un punto de vista general, implementará el modelo en tres pasos:

* **[Crear un experimento de entrenamiento]** : en Studio (clásico), puede entrenar y probar un modelo de análisis predictivo con datos de entrenamiento que suministre, mediante un gran conjunto de algoritmos de aprendizaje automático integrados.
* **[Convertirlo en un experimento predictivo]** : una vez que se ha entrenado el modelo con datos existentes y está listo para usarse con el objetivo de puntuar nuevos datos, debe prepararlo y simplificarlo para realizar predicciones.
* **Implementarlo** como un **[Servicio web nuevo]** o un **[servicio web clásico]** : al implementar el experimento predictivo como un servicio web de Azure, los usuarios pueden enviar datos al modelo y recibir las predicciones de este.

## <a name="create-a-training-experiment"></a>Crear un experimento de entrenamiento

Para entrenar un modelo de análisis predictivo, usará Azure Machine Learning Studio (clásico) para crear un experimento de entrenamiento en el que incluir varios módulos para cargar los datos de entrenamiento, preparar los datos necesarios, aplicar algoritmos de aprendizaje automático y evaluar los resultados. Puede iterar en un experimento y probar algoritmos distintos de aprendizaje automático para comparar y evaluar los resultados.

El proceso de creación y administración de experimentos de entrenamiento se trata más detenidamente en otros artículos. Para obtener más información, consulte estos artículos:

* [Creación de un experimento sencillo en Azure Machine Learning Studio (clásico)](create-experiment.md)
* [Desarrollo de una solución predictiva con Azure Machine Learning Studio (clásico)](tutorial-part1-credit-risk.md)
* [Importar datos de entrenamiento en Azure Machine Learning Studio (clásico)](import-data.md)
* [Administrar iteraciones de experimentos en Azure Machine Learning Studio (clásico)](manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Convertir un experimento de entrenamiento en experimento predictivo

Una vez que haya entrenado el modelo, estará listo para convertir el experimento de entrenamiento en un experimento predictivo para puntuar datos nuevos.

Al efectuar la conversión a un experimento predictivo, estará preparando el modelo entrenado para implementarlo como servicio web de puntuación. Los usuarios del servicio web pueden enviar datos de entrada a su modelo y este les devolverá los resultados de predicción. Cuando efectúe la conversión a un experimento predictivo, tenga en cuenta la forma en que prevé que los demás usen el modelo.

Para convertir su experimento de entrenamiento en un experimento predictivo, haga clic en **Ejecutar** en la parte inferior del lienzo de experimento, haga clic en **Set Up Web Service** (Configurar servicio web) y luego en **Predictive Web Service** (Servicio web predictivo).

![Convertir en experimento de puntuación](./media/publish-a-machine-learning-web-service/figure-1.png)

Para obtener más información sobre cómo realizar esta conversión, consulte [Preparación del modelo de implementación en Azure Machine Learning Studio](convert-training-experiment-to-scoring-experiment.md).

A continuación, se describe el proceso de implementación de un experimento predictivo como servicio web nuevo. También puede implementarlo como servicio web clásico.

## <a name="deploy-it-as-a-new-web-service"></a>Implementación como un nuevo servicio web

Una vez preparado el experimento predictivo, puede implementarlo como un nuevo servicio web de Azure (basado en Resource Manager). Mediante el servicio web, los usuarios pueden enviar datos a su modelo y el modelo devolverá las predicciones.

Para implementar un experimento predictivo, haga clic en la opción **Ejecutar** de la parte inferior del lienzo del experimento. Cuando el experimento haya terminado de ejecutarse, haga clic en **Deploy Web Service** (Implementar servicio web) y seleccione **Deploy Web Service [New]** (Implementar servicio web [nuevo]).  Se abre la página de implementación del portal de servicios web de Machine Learning Studio (clásico).

> [!NOTE] 
> Para implementar un nuevo servicio web, debe tener permisos suficientes en la suscripción en la que lo implementa. Para obtener más información, consulte [Administración de un servicio web mediante el portal Servicios web Azure Machine Learning](manage-new-webservice.md). 

### <a name="web-service-portal-deploy-experiment-page"></a>Página de implementación de experimentos del portal del servicio web

En la página de implementación de experimentos, escriba un nombre para el servicio web.
Seleccione un plan de tarifa. Si ya tiene uno, puede seleccionarlo; si no, debe crear uno nuevo para el servicio.

1. En el menú desplegable **Price Plan** (Plan de precios), seleccione un plan existente o elija la opción **Select new plan** (Seleccionar nuevo plan).
2. En **Nombre del plan**, escriba un nombre que identifique el plan en la factura.
3. Seleccione uno de los **niveles de planes mensuales**. Los niveles de los planes predeterminados son los de la región predeterminada, y el servicio web se implementa en dicha región.

Haga clic en las páginas **Implementar** e **Inicio rápido** del servicio web que se abre.

A través de la página Inicio rápido del servicio web podrá acceder a las tareas más comunes que se realizarán después de crear un servicio web nuevo, así como instrucciones sobre cómo hacerlo. A partir de ahí, puede acceder fácilmente a las páginas Prueba y Consume (Consumo).

<!-- ![Deploy the web service](./media/publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-new-web-service"></a>Prueba del servicios web nuevo

Para probar el nuevo servicio web, haga clic en la opción **Test web service** (Probar servicio web) de las tareas comunes. En la página Prueba puede probar el servicio web como servicio de solicitud-respuesta (RRS) o de ejecución por lotes (BES).

En la página de pruebas RRS se muestran las entradas, las salidas y los parámetros globales definidos para el experimento. Para probar el servicio web, puede escribir manualmente los valores adecuados de las entradas o bien introducir un archivo con formato de valores separados por comas (CSV) que contenga los valores de prueba.

Para realizar pruebas RRS, en el modo de vista de lista, escriba los valores adecuados de las entradas y haga clic en **Test Request-Response**(Probar solicitud-respuesta). Los resultados de predicción se mostrarán en la columna de salida de la izquierda.

![Escritura de los valores adecuados para probar el servicio web](./media/publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Para realizar pruebas BES, haga clic en **Batch**. En la página de pruebas por lotes, haga clic en la opción Examinar de la entrada y seleccione un archivo CSV que contenga los valores de ejemplo adecuados. Si no dispone de un archivo CSV y ha creado el experimento predictivo con Machine Learning Studio (clásico), puede descargar el conjunto de datos para el experimento predictivo y utilizarlo.

Para ello, abra Machine Learning Studio (clásico). Abra el experimento predictivo y haga clic con el botón derecho en la entrada del experimento. En el menú contextual, seleccione **conjunto de datos** y, después, haga clic en **Descargar**.

![Descarga del conjunto de datos en el lienzo de Studio (clásico)](./media/publish-a-machine-learning-web-service/figure-7-mls-download.png)

Haga clic en **Probar**. El estado del trabajo de ejecución por lotes se muestra a la derecha de **Test Batch Jobs** (Probar trabajos por lotes).

![Prueba del trabajo de ejecución por lotes con el portal de servicios web](./media/publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/publish-a-machine-learning-web-service/figure-3.png)-->

En la página **CONFIGURACIÓN**, puede cambiar la descripción y el título, actualizar la clave de la cuenta de almacenamiento y habilitar los datos de ejemplo para el servicio web.

![Configuración del servicio web](./media/publish-a-machine-learning-web-service/figure-8-arm-configure.png)

### <a name="access-your-new-web-service"></a>Acceso al servicio web nuevo

Cuando implementa el servicio web desde Machine Learning Studio (clásico), puede enviar datos al servicio y recibir respuestas mediante programación.

La página **Consume** (Consumo) proporciona toda la información que necesita para acceder al servicio web. Por ejemplo, la clave de API se ofrece para permitir el acceso autorizado al servicio.

Para más información sobre el acceso a un servicio web de Machine Learning Studio (clásico), consulte [Cómo consumir un servicio web Azure Machine Learning Studio (clásico)](consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Administración del servicio web nuevo

Puede administrar los nuevos servicios web mediante el portal de servicios web de Machine Learning Studio (clásico). En la [página principal del portal](https://services.azureml.net/), haga clic en **Servicios web**. En la página de servicios web, puede eliminar o copiar servicios. Para supervisar un servicio concreto, haga clic en el servicio y, después, haga clic en **Panel**. Para supervisar los trabajos por lotes asociados con el servicio web, haga clic en **Batch Request Log**(Registro de solicitudes por lotes).

### <a name="deploy-your-new-web-service-to-multiple-regions"></a><a id="multi-region"></a> Implementación del nuevo servicio web en varias regiones

Puede implementar fácilmente un nuevo servicio web en varias regiones sin necesidad de disponer de varias suscripciones o áreas de trabajo.

Los precios dependen de la región, así que debe definir un plan de facturación para cada región en la que implementará el servicio web.

#### <a name="create-a-plan-in-another-region"></a>Creación de un plan en otra región

1. Inicie sesión en el [portal de servicios web de Microsoft Azure Machine Learning](https://services.azureml.net/).
2. Haga clic en la opción de menú **Planes** .
3. En la página de información general de Planes, haga clic en **Nuevo**.
4. En el menú desplegable **Suscripción** , seleccione la suscripción en que residirá el nuevo plan.
5. En el menú desplegable **Región** , seleccione una región para el nuevo plan. Las opciones de planes de la región seleccionada se mostrarán en la sección de la página **Plan Options** (Opciones de planes).
6. En el menú desplegable **Grupo de recursos** , seleccione un grupo de recursos para el plan. Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](../../azure-resource-manager/management/overview.md).
7. En **Nombre del plan** , escriba el nombre del plan.
8. En **Plan Options**(Opciones de planes), haga clic en el nivel de facturación del nuevo plan.
9. Haga clic en **Crear**.

#### <a name="deploy-the-web-service-to-another-region"></a>Implementación del servicio web en otra región

1. En la página de servicios web de Microsoft Azure Machine Learning, haga clic en la opción de menú **Web Services** (Servicios web).
2. Seleccione el servicio web que se va a implementar en una nueva región.
3. Haga clic en **Copiar**.
4. En **Nombre de servicio web**, escriba un nombre nuevo del servicio web.
5. En **Descripción del servicio web**, escriba una descripción del servicio web.
6. En el menú desplegable **Suscripción** , seleccione la suscripción en que residirá el servicio web nuevo.
7. En el menú desplegable **Grupo de recursos** , seleccione un grupo de recursos para el servicio web. Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](../../azure-resource-manager/management/overview.md).
8. En el menú desplegable **Región** , seleccione la región en la que se va a implementar el servicio web.
9. En el menú desplegable **Cuenta de almacenamiento** , seleccione la cuenta de almacenamiento en la que se va a almacenar el servicio web.
10. En el menú desplegable **Price Plan** (Plan de precios), seleccione un plan en la región que seleccionó en el paso 8.
11. Haga clic en **Copiar**.

## <a name="deploy-it-as-a-classic-web-service"></a>Implementación como servicio web clásico

Ahora que ha preparado el experimento predictivo suficientemente, puede implementarlo como servicio web de Azure clásico. Mediante el servicio web, los usuarios pueden enviar datos a su modelo y el modelo devolverá las predicciones.

Para implementar el experimento predictivo, haga clic en **Ejecutar** en la parte inferior del lienzo del experimento y luego haga clic en **Deploy Web Service** (Implementar servicio web). El servicio web está configurado y se colocará en el panel del servicio web.

![Implementación del servicio web desde Studio (clásico)](./media/publish-a-machine-learning-web-service/figure-2.png)

### <a name="test-your-classic-web-service"></a>Prueba del servicio web clásico

Puede probar el servicio web en el portal del servicio web de Machine Learning Studio (clásico) o en Machine Learning Studio (clásico).

Para probar el servicio web de respuesta de solicitudes, haga clic en el botón **Probar** del panel del servicio web. Aparecerá un cuadro de diálogo que le pide los datos de entrada del servicio. Estas son las columnas esperadas del experimento puntuación. Escriba un conjunto de datos y haga clic en **Aceptar**. Los resultados generados por el servicio web se muestran en la parte inferior del panel.

Puede hacer clic en el vínculo de versión preliminar **Test** (Probar) para probar el servicio en el portal de servicios web de Azure Machine Learning Studio (clásico), como se mostró anteriormente en la sección de nuevo servicio web.

Para probar el servicio de ejecución por lotes, haga clic en el vínculo de vista previa **Probar**. En la página de pruebas por lotes, haga clic en la opción Examinar de la entrada y seleccione un archivo CSV que contenga los valores de ejemplo adecuados. Si no dispone de un archivo CSV y ha creado el experimento predictivo con Machine Learning Studio (clásico), puede descargar el conjunto de datos para el experimento predictivo y utilizarlo.

![Prueba del servicio web](./media/publish-a-machine-learning-web-service/figure-3.png)

En la página **CONFIGURACIÓN** puede cambiar el nombre para mostrar del servicio y darle una descripción. El nombre y la descripción se muestran en [Azure Portal](https://portal.azure.com/), donde se administran los servicios web.

Puede dar una descripción para los datos de entrada, los de salida y los parámetros del servicio web escribiendo una cadena para cada columna en **INPUT SCHEMA** (ESQUEMA DE ENTRADA), **OUTPUT SCHEMA** (ESQUEMA DE SALIDA) y **WEB SERVICE PARAMETER** (PARÁMETRO DE SERVICIO WEB). Estas descripciones se utilizan en la documentación de código de ejemplo proporcionada para el servicio web.

Puede habilitar el registro para diagnosticar cualquier error que vea al acceder al servicio web. Para más información, consulte [Habilitación del registro para los servicios web de Machine Learning Studio (clásico)](web-services-logging.md).

![Habilitación del registro en el portal de servicios web](./media/publish-a-machine-learning-web-service/figure-4.png)

También puede configurar los puntos de conexión del servicio web en el portal Servicios web Azure Machine Learning de una forma parecida al procedimiento mostrado anteriormente en la sección Servicio web nuevo. Las opciones son diferentes: puede agregar o cambiar la descripción del servicio, habilitar el registro y habilitar datos de ejemplo para las pruebas.

### <a name="access-your-classic-web-service"></a>Acceso al servicio web clásico

Cuando implemente el servicio web desde Azure Machine Learning Studio (clásico), puede enviar datos al servicio y recibir respuestas mediante programación.

El panel proporciona toda la información que necesita para tener acceso a su servicio web. Por ejemplo, la clave de API se proporciona para permitir el acceso autorizado al servicio, y las páginas de ayuda de API sirven para ayudarle a empezar a escribir el código.

Para más información sobre el acceso a un servicio web de Machine Learning Studio (clásico), consulte [Cómo consumir un servicio web Azure Machine Learning Studio (clásico)](consume-web-services.md).

### <a name="manage-your-classic-web-service"></a>Administración del servicio web clásico

Hay varias acciones que puede realizar para supervisar un servicio web. Puede actualizarlo y eliminarlo. También puede agregar puntos de conexión adicionales para un servicio web clásico aparte del punto de conexión predeterminado que se crea cuando se implementa.

Para más información, consulte [Administración de un área de trabajo de Azure Machine Learning Studio (clásico)](manage-workspace.md) y [Administración de un servicio web mediante el portal de servicios web de Azure Machine Learning Studio (clásico)](manage-new-webservice.md).

## <a name="update-the-web-service"></a>Actualizar el servicio web
Puede realizar cambios en el servicio web, como actualizar el modelo con datos de entrenamiento adicionales y volver a implementarlo, sobrescribiendo el servicio web original.

Para actualizar el servicio web, abra el experimento predictivo original que usó para implementar el servicio web y haga una copia modificable haciendo clic en **GUARDAR COMO**. Realice los cambios y haga clic en **Publicar servicio web**.

Como ya ha implementado este experimento antes, se le preguntará si quiere sobrescribir (servicio web clásico) o actualizar (servicio web nuevo) el servicio existente. Al hacer clic en **SÍ** o **Actualizar**, el servicio web existente se detendrá y, en su lugar, se implementará el nuevo experimento predictivo.

> [!NOTE]
> Si ha realizado cambios de configuración en el servicio web original, como, por ejemplo, escribir un nuevo nombre para mostrar o una descripción, necesitará escribir esos valores de nuevo.

Una opción para actualizar el servicio web es volver a entrenar el modelo mediante programación. Para más información, consulte [Nuevo entrenamiento de modelos de Machine Learning Studio (clásico) mediante programación](/azure/machine-learning/studio/retrain-machine-learning-model).

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre cómo funciona la implementación, consulte [Progreso de un modelo de Machine Learning Studio (clásico) de un experimento a un servicio web aplicado](model-progression-experiment-to-web-service.md).

* Para más información sobre cómo obtener el modelo listo para implementar, consulte [Preparación del modelo de implementación en Azure Machine Learning Studio (clásico)](convert-training-experiment-to-scoring-experiment.md).

* Hay varias maneras de usar la API de REST y acceder al servicio web. Consulte [Cómo consumir un servicio web de Azure Machine Learning Studio (clásico)](consume-web-services.md).

<!-- internal links -->
[Crear un experimento de entrenamiento]: #create-a-training-experiment
[Convertirlo en un experimento predictivo]: #convert-the-training-experiment-to-a-predictive-experiment
[Servicio web nuevo]: #deploy-it-as-a-new-web-service
[Servicio web clásico]: #deploy-it-as-a-classic-web-service
[New]: #deploy-it-as-a-new-web-service
[classic]: #deploy-the-predictive-experiment-as-a-classic-web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
