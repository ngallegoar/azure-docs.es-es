---
title: Relaciones de delimitadores y búsqueda de caminos
description: Obtenga información sobre el modelo conceptual que sustenta las relaciones de anclaje. Aprenda a conectar anclajes dentro de un espacio y a usar Nearby API para satisfacer un escenario de búsqueda de caminos.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 13c85dff40e266287d893d9e45b32a66a33f027e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83006014"
---
# <a name="anchor-relationships-and-way-finding-in-azure-spatial-anchors"></a>Relaciones de anclaje y búsqueda de caminos en Azure Spatial Anchors

Mediante el uso de relaciones de anclaje, puede crear anclajes conectados en un espacio y, a continuación, formular preguntas como estas:

* ¿Hay anclajes cerca?
* ¿Cuánto de lejos están?

## <a name="examples"></a>Ejemplos

Puede usar anclajes conectados en estos casos:

* Un trabajador debe completar una tarea que implica visitar varias ubicaciones en una fábrica industrial. La fábrica tiene anclajes espaciales en cada ubicación. Una aplicación móvil o un dispositivo HoloLens ayudan a guiar al trabajador de una ubicación a la siguiente. La aplicación primero pregunta por los anclajes espaciales cercanos y, a continuación, guía al trabajador a la siguiente ubicación. La aplicación muestra visualmente la dirección general y la distancia a la siguiente ubicación.

* Un museo crea anclajes espaciales en las exposiciones públicas. Juntos, estos anclajes forman un paseo de una hora por las exposiciones públicas esenciales del museo. En una exposición pública, los visitantes pueden abrir la aplicación de realidad mixta del museo en su dispositivo móvil. A continuación, pueden apuntar con la cámara del teléfono alrededor del espacio para ver la dirección general y la distancia a las otras exposiciones públicas del paseo. A medida que un usuario camina hacia una exposición pública, la aplicación actualiza la dirección general y la distancia para ayudar a guiar al usuario.

## <a name="set-up-way-finding"></a>Configuración de la búsqueda de caminos

Una aplicación que usa la dirección de la línea de visión y la distancia entre los anclajes para proporcionar orientación utiliza la *búsqueda de caminos*. La búsqueda de caminos es diferente de la navegación giro a giro. En la navegación giro a giro se guía al usuario alrededor de paredes, a través de puertas y entre plantas. Con la búsqueda de caminos, el usuario obtiene sugerencias sobre la dirección general del destino. No obstante, la inferencia y los conocimientos del espacio también ayudan al usuario a navegar por la estructura hacia el destino.

Para crear una experiencia de búsqueda de caminos, primero debe preparar un espacio para la experiencia y desarrollar una aplicación con la que los usuarios interactuarán. Estos son los pasos conceptuales:

1. **Planear el espacio**: decida qué ubicaciones dentro del espacio formarán parte de la experiencia de búsqueda de caminos. En nuestros escenarios, el supervisor de la fábrica o el coordinador del paseo por el museo podrían decidir qué ubicaciones se deben incluir en la experiencia de búsqueda de caminos.
2. **Conectar los anclajes**: visite las ubicaciones elegidas para crear anclajes espaciales. Puede hacerlo en un modo de administrador de la aplicación de usuario final o en otra aplicación completamente distinta. Va a conectar o relacionar cada anclaje a los demás. El servicio mantiene estas relaciones.
3. **Iniciar la experiencia del usuario final**: los usuarios ejecutan la aplicación para localizar un anclaje, el cual puede estar en cualquiera de las ubicaciones elegidas. El diseño general debe determinar las ubicaciones en las que los usuarios pueden entrar a la experiencia.
4. **Encontrar anclajes cercanos**: después de que el usuario encuentra un anclaje, la aplicación puede solicitar anclajes cercanos. Este procedimiento devuelve una posición entre el dispositivo y estos anclajes.
5. **Guiar al usuario**: la aplicación puede usar la posición con respecto a cada uno de estos anclajes para proporcionar orientación acerca de la dirección general y la distancia del usuario. Por ejemplo, la entrada de la cámara en la aplicación podría mostrar un icono y una flecha para representar cada posible destino, como se muestra en la siguiente imagen.
6. **Refinar la guía**: a medida que el usuario camina, la aplicación puede calcular periódicamente una posición nueva entre el dispositivo y el anclaje de destino. La aplicación continúa refinando las sugerencias de orientación que ayudan al usuario a llegar a su destino.

    ![Ejemplo de cómo puede mostrar una aplicación la guía de la búsqueda de caminos](./media/meeting-spot.png)

## <a name="connect-anchors"></a>Conexión de los anclajes

Para crear una experiencia de búsqueda de caminos, primero debe colocar los anclajes en las ubicaciones elegidas. En esta sección, supondremos que el administrador de la aplicación ya ha finalizado este trabajo.

### <a name="connect-anchors-in-a-single-session"></a>Conexión de los anclajes en una única sesión

Para conectar los anclajes:

1. Camine a la primera ubicación y cree el anclaje A mediante el uso de CloudSpatialAnchorSession.
2. Camine a la segunda ubicación. La plataforma subyacente de MR/AR realiza un seguimiento del movimiento.
3. Cree el anclaje B mediante el uso del mismo elemento CloudSpatialAnchorSession. Los anclajes A y B ahora están conectados. El servicio Spatial Anchors mantiene esta relación.
4. Siga el procedimiento para los anclajes restantes.

### <a name="connect-anchors-in-multiple-sessions"></a>Conexión de los anclajes en varias sesiones

Puede conectar anclajes espaciales en varias sesiones. Con este método, puede crear y conectar algunos anclajes en un momento del tiempo y más adelante crear y conectar más anclajes.

Para conectar anclajes en varias sesiones:

1. La aplicación crea algunos anclajes en un elemento CloudSpatialAnchorSession.
2. En otro momento, la aplicación busca uno de estos anclajes (por ejemplo, el anclaje A) mediante el uso de un nuevo elemento CloudSpatialAnchorSession.
3. Camine a una nueva ubicación. La plataforma subyacente de realidad mixta o realidad aumentada realiza un seguimiento del movimiento.
4. Cree el anclaje C mediante el uso del mismo elemento CloudSpatialAnchorSession. Los anclajes A, B y C ahora están conectados. El servicio Spatial Anchors mantiene esta relación.

Puede seguir este procedimiento para más anclajes y más sesiones a lo largo del tiempo.

### <a name="verify-anchor-connections"></a>Comprobación de las conexiones de los anclajes

La aplicación puede comprobar que dos anclajes están conectados mediante la emisión de una consulta de anclajes cercanos, lo que se hace estableciendo el `NearAnchorCriteria` en un `CloudSpatialAnchorWatcher`. Cuando los resultados de la consulta contienen el anclaje de destino, se comprueba la conexión del anclaje. Si los anclajes no están conectados, la aplicación puede intentar conectarlos de nuevo.

Estos son algunos de los motivos por los que los anclajes podrían no conectarse:

* La plataforma subyacente de realidad mixta o realidad aumentada ha perdido el seguimiento durante el proceso de conexión de los anclajes.
* Debido a un error de red durante la comunicación con el servicio Spatial Anchors, no se pudo guardar la conexión del anclaje.

### <a name="find-sample-code"></a>Encontrar código de ejemplo

Para buscar el código de ejemplo que muestra cómo conectar los anclajes y cómo hacer consultas de proximidad, consulte [Aplicaciones de ejemplo de Spatial Anchors](https://github.com/Azure/azure-spatial-anchors-samples).
