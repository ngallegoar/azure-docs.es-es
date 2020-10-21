---
title: Introducción a Azure Portal | Microsoft Docs
description: Azure Portal es una interfaz gráfica de usuario que puede usarse para administrar los servicios de Azure. Aprenda a navegar y buscar recursos en Azure Portal.
services: azure-portal
keywords: portal
author: mgblythe
ms.author: mblythe
ms.date: 12/20/2019
ms.topic: overview
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 24b249d1098788e518f5eda2d1b0d421d4e5e6bb
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92072397"
---
# <a name="azure-portal-overview"></a>Introducción a Azure Portal

En este artículo se presenta Azure Portal, se identifican elementos de la página del portal y se le ayuda a familiarizarse con la experiencia de administración de Azure Portal.

## <a name="what-is-the-azure-portal"></a>¿Qué es Azure Portal?

Azure Portal es una consola unificada basada en web que proporciona una alternativa a las herramientas de línea de comandos. Con Azure Portal, puede administrar su suscripción a Azure mediante una interfaz gráfica de usuario. Puede compilar, administrar y supervisar todo, desde aplicaciones web sencillas hasta complejas implementaciones en la nube. Cree paneles personalizados para una vista organizada de recursos. Configure opciones de accesibilidad para una experiencia óptima.

Azure Portal está diseñado para proporcionar resistencia y disponibilidad continua. Tiene una presencia en cada centro de datos de Azure. Esta configuración hace que Azure Portal sea resistente a errores individuales de centros de datos y evita que la red se ralentice al estar cerca de los usuarios. Azure Portal no deja de actualizarse y no requiere tiempo de inactividad para las actividades de mantenimiento.

## <a name="azure-portal-menu"></a>Menú de Azure Portal

Puede seleccionar el modo predeterminado del menú del portal. Se puede acoplar o puede actuar como un panel flotante.

Cuando el menú del portal está en modo flotante, se oculta hasta que se necesita. Seleccione el icono de menú para abrir o cerrar el menú.

![Menú de Azure Portal en modo flotante](./media/azure-portal-overview/azure-portal-overview-portal-menu-flyout.png)

Si selecciona el modo acoplado para el menú del portal, siempre está visible. Puede contraer el menú para proporcionar más espacio de trabajo.

![Menú de Azure Portal en modo acoplado](./media/azure-portal-overview/azure-portal-overview-portal-menu-expandcollapse.png)

## <a name="azure-home"></a>Inicio de Azure

Como nuevo suscriptor a servicios de Azure, lo primero que ve nada más [iniciar sesión en el portal](https://portal.azure.com) es **Inicio de Azure**. En esta página se compilan recursos que le ayudan a sacar el máximo partido a su suscripción a Azure. Hemos incluido vínculos a cursos en línea gratuitos, documentación, servicios principales y sitios útiles para mantenerse al día y administrar el cambio para su organización. Para obtener acceso de forma rápida y sencilla al trabajo en curso, también mostramos una lista de sus últimos recursos visitados. No puede personalizar esta página, pero puede elegir si desea ver **Inicio de Azure** o **Panel de Azure** como su vista predeterminada. Al iniciar sesión por primera vez, aparece un aviso en la parte superior de la página donde puede guardar su preferencia.

![Captura de pantalla que muestra el selector de vistas predeterminado](./media/azure-portal-overview/azure-portal-default-view.png)

Tanto el menú de Azure Portal como la vista predeterminada de Azure se pueden cambiar en **Configuración del portal**. Si cambia la selección, el cambio se aplica de inmediato.

![Captura de pantalla que muestra el selector de vistas predeterminado](./media/azure-portal-overview/azure-portal-overview-portal-settings-menu-home.png)

## <a name="azure-dashboard"></a>Panel de Azure

Los paneles proporcionan una vista enfocada de los recursos de su suscripción que más le importan. Le hemos facilitado un panel predeterminado para empezar. Puede personalizar este panel para reunir los recursos que más usa en una sola vista. Los cambios que realice en la vista predeterminada solo afectarán a su experiencia. Sin embargo, puede crear paneles adicionales para su propio uso o publicar paneles personalizados y compartirlos con otros usuarios de su organización. Para obtener más información, consulte [Creación y uso compartido de paneles en Azure Portal](../azure-portal/azure-portal-dashboards.md).

## <a name="getting-around-the-portal"></a>Desplazamiento por el portal

Es útil para entender el diseño básico del portal y cómo interactuar con él. Aquí, presentaremos los componentes de la interfaz de usuario y parte de la terminología que empleamos para proporcionar instrucciones. Para un recorrido más detallado por el portal, consulte la lección del curso [Desplazarse por el portal](/learn/modules/tour-azure-portal/3-navigate-the-portal).

El menú de Azure Portal y el encabezado de página son elementos globales que están siempre presentes. Estas características persistentes son el "shell" para la interfaz de usuario asociada a cada característica o servicio individual y el encabezado proporciona acceso a los controles globales. La página de configuración (a veces denominada "hoja") de un recurso también puede tener un menú para ayudarle a moverse entre características.

En la siguiente ilustración se etiquetan los elementos básicos de Azure Portal, cada uno de los cuales se describe en la siguiente tabla.

![Captura de pantalla que muestra la clave y la vista del portal en pantalla completa a elementos de interfaz de usuario](./media/azure-portal-overview/azure-portal-overview-portal-callouts.png)

![Captura de pantalla en la que se muestra el menú del portal expandido](./media/azure-portal-overview/azure-portal-overview-portal-menu-callouts.png)

|Clave|Descripción
|:---:|---|
|1|Encabezado de página. Aparece en la parte superior de todas las páginas del portal e incluye elementos globales.|
|2| Búsqueda global. Use la barra de búsqueda para encontrar rápidamente un recurso específico, un servicio o documentación.|
|3|Controles globales. Como todos los elementos globales, estas características persisten en todo el portal e incluyen: Cloud Shell, filtro de suscripciones, notificaciones, configuración del portal, ayuda y soporte técnico y envíenos comentarios.|
|4|Su cuenta. Vea información sobre su cuenta, cambie directorios, cierre sesión o inicie sesión con otra cuenta.|
|5|Menú del portal. El menú del portal es un elemento global que ayuda a desplazarse entre servicios. A veces se le conoce como barra lateral; el modo del menú del portal se puede cambiar en **Configuración del portal**.|
|6|Menú de recursos. Muchos servicios incluyen un menú de recursos que ayuda a administrar el servicio. Es posible que vea referencias a este elemento como panel izquierdo.|
|7|Barra de comandos. Los controles de la barra de comandos se basan en su enfoque actual.|
|8|Panel de trabajo.  Muestra los detalles sobre el recurso que se encuentra actualmente en el foco.|
|9|Ruta de navegación. Puede usar los vínculos de la ruta de navegación para retroceder un nivel en su flujo de trabajo.|
|10|Control maestro para crear un nuevo recurso en la suscripción actual. Expanda o abra el menú del portal para buscar **+ Crear un recurso**. Busque o examine Azure Marketplace para el tipo de recurso que desea crear.|
|11|Su lista de favoritos. Vea [Adición, eliminación y ordenación de favoritos](../azure-portal/azure-portal-add-remove-sort-favorites.md) para aprender a personalizar la lista.|

## <a name="get-started-with-services"></a>Introducción a los servicios

Si es un suscriptor nuevo, tendrá que crear un recurso antes de que haya algo para administrar. Seleccione **+ Crear un recurso** para ver los servicios disponibles en Azure Marketplace. Aquí encontrará aplicaciones y servicios de cientos de proveedores, todos certificados para ejecutarse en Azure.

Se han rellenado previamente sus Favoritos en la barra lateral con vínculos a servicios usados con frecuencia.  Para ver todos los servicios disponibles, seleccione **Todos los servicios** en la barra lateral.

> [!TIP]
> La forma más rápida de buscar un recurso, servicio o documentación es usar *Search* en el encabezado global. Use los vínculos de la ruta de navegación para volver a las páginas anteriores.
>
Vea este vídeo para una demostración del uso de la búsqueda global en Azure Portal.


> [!VIDEO https://www.youtube.com/embed/nZ7WwTZcQbo]

[How to use global search in the Azure portal](https://www.youtube.com/watch?v=nZ7WwTZcQbo) (La búsqueda global en Azure Portal)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre dónde ejecutar Azure Portal en [Exploradores y dispositivos compatibles](../azure-portal/azure-portal-supported-browsers-devices.md).
* Permanezca conectado en los desplazamientos con [Azure Mobile App](https://azure.microsoft.com/features/azure-portal/mobile-app/).
* Incorpórese y configure el entorno de nube con el [Centro de inicio rápido de Azure](../azure-portal/azure-portal-quickstart-center.md).