---
title: Investigación de incidentes y alertas en Azure Security Center | Microsoft Docs
description: Este documento le ayuda a utilizar la característica de investigación de Azure Security Center para investigar alertas e incidentes de seguridad.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: a8e894a9-8781-4749-ae8f-8c8e01658566
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2018
ms.author: memildin
ms.openlocfilehash: ef1684f11349696c22a290c90469a8d94f922e58
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603995"
---
# <a name="investigate-incidents-and-alerts-in-azure-security-center-retired"></a>Investigación de incidentes y alertas en Azure Security Center (retirado)
Este documento le ayuda a utilizar la característica de investigación (versión preliminar) de Azure Security Center para investigar alertas e incidentes de seguridad.

> [!NOTE]
> La experiencia de investigación de alertas e incidentes (versión preliminar) se retiró el 31 de julio de 2019. Para obtener más información y servicios alternativos, consulte [Retirada de las características de Security Center (julio de 2019)](security-center-features-retirement-july2019.md#menu_investigate).

## <a name="what-is-investigation-in-security-center"></a>¿Qué es la investigación en Security Center?
La característica de investigación de Security Center le permite evaluar la causa principal de un posible [incidente de seguridad](https://docs.microsoft.com/azure/security-center/security-center-incident), comprender su ámbito y realizar su seguimiento.

Lo que se pretende es facilitar el proceso de investigación mediante la vinculación de todas las entidades ([alertas de seguridad](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), usuarios, equipos e incidentes) que están implicadas en los incidentes que se investigan.  Para lograr esto, Security Center correlaciona los datos de interés con las entidades implicadas y expone esta correlación mediante un gráfico dinámico que le ayuda a desplazarse por los objetos y visualizar la información de interés.


> [!NOTE]
> * Las [alertas personalizadas](security-center-custom-alert.md) no son compatibles con la característica de investigación de Security Center.
> * La investigación solo se admite para alertas basadas en datos recopilados de servidores Windows.


## <a name="how-investigation-works"></a>¿Cómo funciona la investigación?
La investigación está compuesta por un gráfico que ocupa el área central del panel de investigación. El gráfico siempre se centra en una entidad específica y presenta las entidades que están relacionadas con él. Una entidad podría ser una alerta de seguridad, un usuario, un equipo o un incidente.

![Map](./media/security-center-investigation/security-center-investigation-fig1.png)

El usuario puede desplazarse de una entidad a otra haciendo clic en ella en el gráfico. El gráfico se centraliza automáticamente en la entidad seleccionada y sus entidades relacionadas. Las entidades que ya no son de interés se pueden quitar del gráfico.

### <a name="investigation-path"></a>Ruta de acceso de investigación
Mientras el usuario se desplaza a distintas entidades, la ruta de acceso de investigación ayuda a mantener el seguimiento del contexto de la investigación y permite navegar rápidamente. El incidente que contiene los resultados de la investigación siempre es el incidente del extremo izquierdo en la ruta de acceso de investigación.

![Path](./media/security-center-investigation/security-center-investigation-fig2.png)

### <a name="general-information"></a>Información general
Cuando una entidad se presenta en el gráfico, las pestañas muestran información adicional sobre ella. La pestaña **Información** presenta información general sobre la entidad de diversas fuentes disponibles.

![Información general](./media/security-center-investigation/security-center-investigation-fig3.png)

La pestaña Información muestra información de interés para el incidente seleccionado en el mapa. El incidente es un contenedor que incluye los resultados de una investigación. Cada investigación se realiza en el contexto de un incidente.

Un incidente solo se crea cuando un usuario hace clic en el botón **Iniciar investigación** para una alerta específica. La funcionalidad básica disponible para el investigador es marcar las entidades, por ejemplo, como usuario, equipo o alerta. Cuando una entidad está marcada como relacionada, se proporciona una razón. A partir de este momento en adelante, esta entidad aparece directamente debajo del incidente en el gráfico y en la lista de entidades de incidentes.

### <a name="entities"></a>Entidades

La pestaña **Entidades** muestra todas las entidades relacionadas agrupadas por tipo. Resulta de utilidad en dos casos: cuando hay demasiadas entidades que presentar en el gráfico y cuando los nombres de las entidades son demasiado largos y es más fácil examinarlos en forma de tabla.

![Entidades](./media/security-center-investigation/security-center-investigation-fig4.png)

### <a name="search"></a>Search

La pestaña **Búsqueda** presenta todos los tipos de registro que están disponibles para la entidad. Para cada tipo de registro, puede ver el número de registros que están disponibles. Al hacer clic en cada tipo de registro, irá a la pantalla de búsqueda. En la pantalla de búsqueda, puede refinar la búsqueda y usar las distintas características de búsqueda, como el establecimiento de alertas. En la versión actual, la pestaña de búsqueda solo está disponible para entidades de usuarios y equipos.

![Search](./media/security-center-investigation/security-center-investigation-fig5.png)

### <a name="exploration"></a>Exploración

La pestaña **Exploración** permite al investigador examinar los datos relacionados con diversos problemas relativos a la entidad. Por ejemplo, cuando se investiga una máquina, en la pestaña de exploración se presenta la lista de procesos ejecutados en ella. En algunos casos, la pestaña Exploración presenta datos que podrían indicar un problema sospechoso. Los investigadores pueden examinar los datos en la pestaña o abrirlos en la pantalla de búsqueda para examinar grandes conjuntos de datos y para usar opciones de búsqueda avanzadas, como el filtrado y la exportación a Excel.

![Exploración](./media/security-center-investigation/security-center-investigation-fig6.png)

### <a name="timeline"></a>Escala de tiempo

La mayoría de los datos que se presentan en el gráfico y las distintas pestañas son de interés en un período de tiempo específico. Este ámbito de tiempo se establece mediante el selector de ámbitos de tiempo del lado superior izquierdo del gráfico. El investigador tiene varios métodos para seleccionar el ámbito de tiempo.

![Escala de tiempo](./media/security-center-investigation/security-center-investigation-fig7.png)

Los elementos siguientes dependen del ámbito de tiempo:

- La relación usuario-equipo en el gráfico: solo se presentan los usuarios que inician sesión en el equipo dentro de este ámbito de tiempo.
- Las alertas que se presentan al examinar los equipos y los usuarios: solo se presentan las alertas que se producen dentro del ámbito de tiempo.
- La pestaña Entidades sigue la misma lógica que el gráfico.

Los siguientes elementos se van a presentar independientemente del ámbito de tiempo seleccionado:

- Cuando se presenta una alerta, todas las entidades que contiene, como usuarios y equipos, siempre se presentan.
- Si un incidente contiene una entidad, se presenta.

> [!NOTE]
> Las pestañas **Búsqueda** y **Exploración** solo muestran los registros dentro de este ámbito de tiempo.

## <a name="how-to-perform-an-investigation"></a>¿Cómo se realiza una investigación?

Una investigación puede partir de un incidente de seguridad o de una alerta; la opción que elija variará según sus necesidades. Los pasos siguientes se usan para iniciar una investigación a partir de una alerta:

1.  Abra el panel **Security Center**.
2.  Haga clic en **Alertas de seguridad** y seleccione el incidente que desea investigar.
3.  En la página del incidente, haga clic en el botón **Iniciar investigación** y aparecerá el panel **Investigación**.

    ![Alerta](./media/security-center-investigation/security-center-investigation-fig8.png)

4. En este panel puede seleccionar la entidad en el mapa y la información de interés sobre esta entidad aparece en el lado derecho de la pantalla.

    ![Panel Investigación](./media/security-center-investigation/security-center-investigation-fig9.png)

A partir de aquí, puede explorar las entidades que estuvieron implicadas en este incidente y explorar más detalles sobre cada una de ellas.

## <a name="see-also"></a>Consulte también
En este documento, ha aprendido a utilizar la característica de investigación de Security Center. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Administración y respuesta a las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Aprenda a administrar las alertas y responder a incidentes de seguridad en Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.
* [Comprensión de las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Obtenga información acerca de los distintos tipos de alertas de seguridad.