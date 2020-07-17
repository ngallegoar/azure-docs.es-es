---
title: Guía de administración de costos de Azure Lab Services
description: Entienda las distintas formas de ver los costos de Lab Services.
author: rbest
ms.author: rbest
ms.date: 06/26/2020
ms.topic: article
ms.openlocfilehash: fbbaf4a3646260fc09467e214b82fd0213415635
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445311"
---
# <a name="cost-management-for-azure-lab-services"></a>Administración de costos de Azure Lab Services

La administración de costos se puede dividir en dos áreas distintas: estimación de costos y análisis de costos.  La estimación de costos se realiza al configurar el laboratorio a fin de asegurarse de que la estructura inicial de este se ajuste al presupuesto previsto.  El análisis de costos normalmente se realiza al final del mes a fin de analizar los costos y determinar las acciones necesarias para el mes siguiente.

## <a name="estimating-the-lab-costs"></a>Estimación de los costos del laboratorio

Cada panel de laboratorio tiene una sección **Costos y facturación** que presenta una estimación aproximada de lo que va a costar el laboratorio ese mes.  La estimación de costos resume el uso de horas con el número máximo de usuarios por el costo estimado por hora.  Para obtener la estimación más precisa, configure el laboratorio, incluida la [programación](how-to-create-schedules.md), y el panel refleja el costo estimado.  

Es posible que esta estimación no refleje todos los costos posibles, ya que algunos recursos no están incluidos.  El costo de preparación de la plantilla no se refleja en la estimación de costos.  La cantidad de tiempo necesaria para crear la plantilla puede variar considerablemente. El costo de ejecutar la plantilla es el mismo que el costo de laboratorio general por hora. Los costos de una [galería de imágenes compartidas](how-to-use-shared-image-gallery.md) no se incluyen en el panel del laboratorio, ya que una galería puede compartirse entre varios laboratorios.  Por último, las horas dedicadas al inicio de una máquina por parte del creador del laboratorio están excluidas de esta estimación.

> [!div class="mx-imgBorder"]
> ![Panel de estimación de costos](./media/cost-management-guide/dashboard-cost-estimation.png)

## <a name="analyzing-previous-months-usage"></a>Análisis del uso de los meses anteriores

El análisis de costos sirve para revisar el uso de los meses anteriores a fin de ayudar a determinar cualquier ajuste necesario para el laboratorio.  El desglose de los costos del pasado puede encontrarse en el [análisis de costos de la suscripción](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis).  En Azure Portal, puede escribir "Suscripciones" en el campo de búsqueda superior y seleccionar la opción Suscripciones.  

> [!div class="mx-imgBorder"]
> ![Búsqueda de suscripción](./media/cost-management-guide/subscription-search.png)

Seleccione la suscripción concreta que se va a revisar.

> [!div class="mx-imgBorder"]
> ![Selección de suscripción](./media/cost-management-guide/subscription-select.png)

 Seleccione "Análisis de costos" en el panel de la izquierda, en **Administración de costos**.

 > [!div class="mx-imgBorder"]
> ![Análisis de costos de una suscripción](./media/cost-management-guide/subscription-cost-analysis.png)

Este panel permite un análisis de costos en profundidad, incluida la posibilidad de exportar a otros tipos de archivo según una programación.  La administración de costos tiene numerosas capacidades; para obtener más información, vea [¿Qué es Administración de costos y la facturación?](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)

Si se filtra por el tipo de recurso `microsoft.labservices/labaccounts`, solo se muestra el costo asociado a Lab Services.

## <a name="understanding-the-usage"></a>Reconocimiento del uso

A continuación se muestra un ejemplo del análisis de costos.

> [!div class="mx-imgBorder"]
> ![Análisis de costos de una suscripción](./media/cost-management-guide/cost-analysis.png)

Hay seis columnas de forma predeterminada: Recurso, Tipo de recurso, Ubicación, Nombre del grupo de recursos, Etiquetas y Costo.  La columna Recurso contiene la información sobre la cuenta de laboratorio, el nombre del laboratorio y la máquina virtual.  Las filas con Cuenta de laboratorio / Nombre del laboratorio / valor predeterminado son el costo del laboratorio, y se pueden ver en la segunda y la tercera filas.  Las máquinas virtuales usadas tienen un costo en Cuenta de laboratorio / Nombre del laboratorio / valor predeterminado / nombre de la máquina virtual.  En este ejemplo, al sumar la primera fila y la segunda que comienzan por "aaalab/dockerlab", se obtiene el costo total del laboratorio "dockerlab" de la cuenta de laboratorio "aaalab".

Para obtener información sobre la galería de imágenes compartidas, cambie el tipo de recurso a `Microsoft.Compute/Galleries`, lo que da el costo general de la galería de imágenes.  Es posible que las galerías de imágenes compartidas no se muestren en los costos en función de dónde estén almacenadas.

> [!NOTE]
> La galería de imágenes compartidas está conectada a la cuenta de laboratorio.  Eso significa que varios laboratorios pueden usar la misma imagen.

## <a name="separating-costs"></a>Separación de costos

Algunas universidades han usado la cuenta de laboratorio y el grupo de recursos como formas de separar las distintas clases.  Cada clase tiene su propia cuenta de laboratorio y su propio grupo de recursos. En el panel Análisis de costos, agregue un filtro basado en el nombre del grupo de recursos con el nombre del grupo de recursos adecuado para la clase para que solo se vean los costos de esa clase.  Esto permite una demarcación más clara entre las distintas clases al visualizar los costos.  La característica de [exportación programada](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data) del análisis de costos permite descargar los costos de cada clase en archivos independientes.

## <a name="managing-costs"></a>Administración de los costos

En función del tipo de clase, hay formas de administrar los costos a fin de reducir el número de máquinas virtuales en ejecución sin que haya un alumno presente.

### <a name="auto-shutdown-on-disconnect"></a>Apagado automático al desconectar

Durante la creación del laboratorio, el propietario de este puede establecer que las máquinas virtuales del laboratorio se [apaguen cuando la conexión RDP a la máquina virtual se desconecte](how-to-enable-shutdown-disconnect.md).  Esta configuración minimiza los casos en que el alumno se desconecta pero se olvida de detener la máquina virtual.

### <a name="quota-vs-scheduled-time"></a>Cuota frente a hora programada

Entender el [tiempo de cuota](classroom-labs-concepts.md#quota) frente a la [hora programada](classroom-labs-concepts.md#schedules) permite al propietario del laboratorio configurar este de la manera que mejor se ajuste a las necesidades del profesor y los alumnos.  La hora programada es una hora establecida a la que todas las máquinas virtuales de los alumnos se han iniciado y están disponibles para conectarse.  Normalmente, la programación se usa cuando todos los alumnos tienen su propia máquina virtual y están siguiendo las instrucciones del profesor a una hora establecida durante el día, como las horas de clase.  La desventaja es que todas las máquinas virtuales de los alumnos se han iniciado y están acumulando costos, aunque un alumno no inicie sesión en la máquina virtual.  El tiempo de cuota es el tiempo asignado a cada alumno, que este puede usar como decida, y que se suele usar para el estudio independiente. Las máquinas virtuales no se inician hasta que el alumno inicia la máquina virtual.  

Un laboratorio puede usar el tiempo de cuota, la hora programada o una combinación de ambos. Si una clase no necesita la hora programada, use solo el tiempo de cuota para un uso más eficaz de las máquinas virtuales.

### <a name="scheduled-event---stop-only"></a>Evento programado: solo detención

En la programación puede agregar un tipo de evento de solo detención, que detiene todas las máquinas a una hora determinada.  Algunos propietarios de laboratorio han establecido un evento de solo detención cada día a medianoche para reducir el costo y el uso de cuota si un alumno olvida apagar la máquina virtual que está usando.  El inconveniente de este tipo de evento es que todas las máquinas virtuales se apagan, aunque el alumno esté usando la máquina virtual.

### <a name="other-costs-related-to-labs"></a>Otros costos relacionados con los laboratorios 

Hay costos que no se incluyen en Lab Services, pero que se pueden asociar a un servicio de laboratorio.  Puede haber una galería de imágenes compartidas conectada a los laboratorios, pero que no aparezca en los costos de los servicios a pesar de tener costos.  Para ayudar a reducir los costos generales, debe quitar las imágenes no usadas de la galería, ya que las imágenes tienen un costo de almacenamiento heredado.  Los laboratorios pueden tener conexiones con otros recursos de Azure mediante una red virtual (VNet). Al quitar un laboratorio, debe quitar la red virtual y los demás recursos.

## <a name="conclusion"></a>Conclusión

Es de esperar que la información anterior le permita entender mejor los costos de uso y cómo usar las herramientas proporcionadas para reducir el exceso de costos.
