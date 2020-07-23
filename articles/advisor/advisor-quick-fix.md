---
title: Corrección rápida para recomendaciones de Advisor
description: Realización de correcciones masivas con la corrección rápida en Advisor
ms.topic: article
ms.date: 03/13/2020
ms.author: sagupt
ms.openlocfilehash: ebd993733c1aca9cd4f7d92f0a75e22d68a87ffe
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86518069"
---
# <a name="quick-fix-remediation-for-advisor"></a>Corrección rápida para Advisor
**Corrección rápida** es una forma más rápida y sencilla de corregir las recomendaciones en varios recursos. Proporciona funcionalidades para realizar correcciones masivas de recursos y ayuda a optimizar las suscripciones más rápido gracias a la corrección a escala para los recursos.
La característica solo está disponible para determinadas recomendaciones a través de Azure Portal.


## <a name="steps-to-use-quick-fix"></a>Pasos para usar Corrección rápida

1. En la lista de recomendaciones que tienen la etiqueta **Corrección rápida**, haga clic en la recomendación.

   ![Corrección rápida de Advisor](./media/quick-fix-1.png)
   
   *Los precios de la imagen son solo para los fines de este ejemplo*

2. En la página Detalles de la recomendación, verá una lista de los recursos para los que tiene esta recomendación. Seleccione todos los recursos que quiera corregir para la recomendación.

   ![Corrección rápida de Advisor](./media/quick-fix-2.png)
   
   *Los precios de la imagen son solo para los fines de este ejemplo*

3. Una vez que haya seleccionado los recursos, haga clic en el botón **Corrección rápida** para ejecutar las soluciones de forma masiva.

   > [!NOTE]
   > Algunos de los recursos enumerados podrían estar deshabilitados porque no tiene los permisos adecuados para modificarlos.
   
   > [!NOTE]
   > Si además de las ventajas mencionadas en Advisor hay otras complicaciones, se le comunicará en la pantalla para ayudarle a tomar decisiones de corrección bien informadas.
   
4. Recibirá una notificación cuando se haya completado la corrección. Verá un error si algunos recursos no se corrigieron y si hay recursos en el modo seleccionado de la vista de lista de recursos.  


## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las recomendaciones de Advisor, consulte:
* [Introducción a Azure Advisor](advisor-overview.md)
* [Introducción a Advisor](advisor-get-started.md)
* [Recomendaciones sobre el costo de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-performance-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-security-recommendations.md)
* [Recomendaciones de excelencia operativa de Advisor](advisor-operational-excellence-recommendations.md)
* [API REST de Advisor](/rest/api/advisor/)
