---
ms.openlocfilehash: 4df49b4e68769a907423c4edef9f8820df4d20b7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83006468"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Búsqueda de un anclaje espacial de la nube

Una de las principales razones para usar Azure Spatial Anchors es poder localizar un anclaje espacial en la nube previamente guardado. Hay varias formas de buscar un anclaje espacial en la nube. Una estrategia no se puede usar en varios monitores a la vez.
- Busque delimitadores por identificador.
- Busque los delimitadores conectados a un delimitador ubicado previamente. [Aquí](/azure/spatial-anchors/concepts/anchor-relationships-way-finding/) puede obtener información sobre las relaciones de los delimitadores.
- Busque un delimitador mediante la [relocalización general](/azure/spatial-anchors/concepts/coarse-reloc/).

> [!NOTE]
> Cada vez que se encuentra un delimitador, Azure Spatial Anchors intenta usar los datos de entorno recopilados para aumentar la información visual del delimitador. Si tiene problemas para encontrar un delimitador, puede ser útil crear uno y, a continuación, colocarlo varias veces en diferentes ángulos y condiciones de iluminación.

Si va a buscar anclajes espaciales en la nube por identificador, querrá almacenar el identificador de anclaje espacial de la nube en el servicio back-end de la aplicación y hacer que todos los dispositivos que puedan autenticarse correctamente en él puedan acceder a él. For ver un ejemplo de esto, consulte [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Crear una instancia de un objeto `AnchorLocateCriteria`, establezca los identificadores que busca e invoque el método `CreateWatcher` en la sesión mediante su `AnchorLocateCriteria`.