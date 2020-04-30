---
title: 'Alta disponibilidad y recuperación ante desastres: Azure Batch'
description: Aprenda a diseñar la aplicación de Batch para una interrupción regional. Las cargas de trabajo deben conmutar por error a otra región o dividirse entre dos regiones o más.
ms.topic: article
ms.date: 01/29/2019
ms.openlocfilehash: da46753906e27a94e3c76fcaf9c4a26861bba6c8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82117444"
---
# <a name="design-your-application-for-high-availability"></a>Diseño de la aplicación para una alta disponibilidad

Azure Batch es un servicio regional. Batch está disponible en todas las regiones de Azure, pero cuando se crea una cuenta de Batch, se debe asociar con una región. A continuación, todas las operaciones para la cuenta de Batch se aplican a esa región. Por ejemplo, se crean los grupos y las máquinas virtuales (VM) asociadas en la misma región que la cuenta de Batch.

Al diseñar una aplicación que usa Batch, debe considerar la posibilidad de que Batch no esté disponible en una región. Es posible que encuentre una situación poco frecuente en la que haya un problema con la región como un todo, con el servicio completo de Batch en la región o un problema con su cuenta de Batch específica.

Si la aplicación o solución que usa Batch siempre debe estar disponible, entonces se debe diseñar para conmutarse por error a otra región o para que la carga de trabajo siempre se divida en dos o más regiones. Ambos enfoques requieren al menos dos cuentas de Batch y que cada cuenta se encuentre en una región distinta.

## <a name="multiple-batch-accounts-in-multiple-regions"></a>Varias cuentas de Batch en varias regiones

Usar varias cuentas de Batch en regiones distintas permite que su aplicación siga ejecutándose si una cuenta de Batch en otra región deja de estar disponible. Esta práctica es particularmente importante si su aplicación debe tener una alta disponibilidad.

En algunos casos, se puede diseñar una aplicación para que siempre use dos o más regiones. Por ejemplo, cuando necesite una cantidad considerable de capacidad, puede que necesite usar varias regiones para controlar una aplicación a gran escala o satisfacer las necesidades de crecimiento futuro.

## <a name="design-considerations-for-providing-failover"></a>Consideraciones de diseño para proporcionar la conmutación por error

Un punto clave a tener en cuenta al proporcionar la capacidad de conmutación por error a una región alternativa, es que se deben incluir todos los componentes de la solución. No basta con tener simplemente una segunda cuenta de Batch. Por ejemplo, en la mayoría de las aplicaciones de Batch, se requiere de una cuenta de almacenamiento de Azure y dicha cuenta debe pertenecer a la misma región que la cuenta de Batch para que el rendimiento sea aceptable.

Tenga en cuenta lo siguiente a la hora de diseñar una solución con conmutación por error:

- Cree previamente todas las cuentas necesarias en cada región, como la cuenta de Batch y la de almacenamiento. A menudo, crear las cuentas no genera ningún costo, a menos que haya datos almacenados o la cuenta esté en uso.
- Asegúrese de establecer las cuotas en las cuentas de antemano, de forma que pueda asignar el número necesario de núcleos mediante la cuenta de Batch.
- Use scripts o plantillas para automatizar la implementación de la aplicación en una región.
- Mantenga actualizados los archivos binarios de aplicación y los datos de referencia en todas las regiones. Mantenerlos actualizados garantizará que la región se pueda conectar rápidamente sin necesidad de esperar a que los archivos se carguen y se implementen. Por ejemplo, si una aplicación personalizada que se va a instalar en los nodos de grupo se almacena mediante paquetes de aplicación de Batch y se hace referencia a ella mediante los mismos, cuando se genere una nueva versión de dicha aplicación, la configuración de grupo deberá cargarla a cada cuenta de Batch y hacer referencia a ella (o la nueva versión tendrá que establecerse como versión predeterminada).
- En la aplicación que realice llamadas a Batch, al almacenamiento y a cualquier otro servicio, cambie los clientes o la carga con facilidad a una región distinta.
- Un procedimiento recomendado para garantizar que se realice correctamente una conmutación por error consiste en cambiar con frecuencia a una región alternativa como parte de la operación normal. Por ejemplo, si tiene dos implementaciones en regiones distintas, cambie a la región alternativa cada mes.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información acerca de cómo crear cuentas de Batch con [Azure Portal](batch-account-create-portal.md), la [CLI de Azure](cli-samples.md), [Powershell](batch-powershell-cmdlets-get-started.md) o [Batch Management API](batch-management-dotnet.md).
- Las cuotas predeterminadas están asociadas con las cuentas de Batch; en [este artículo](batch-quota-limit.md) se explican a detalle los valores predeterminados de cuotas y se describe el proceso para aumentarlas.
