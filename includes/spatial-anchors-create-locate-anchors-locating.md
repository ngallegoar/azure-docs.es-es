---
ms.openlocfilehash: b5fec8bbc0db78454b080a411702014bd96f7db9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "76887690"
---
## <a name="locate-a-cloud-spatial-anchor"></a>Búsqueda de un anclaje espacial de la nube

Una de las principales razones para usar Azure Spatial Anchors es poder localizar un anclaje espacial en la nube previamente guardado. Hay varias formas de buscar un anclaje espacial en la nube. Una estrategia no se puede usar en varios monitores a la vez.
- Busque delimitadores por identificador.
- Busque los delimitadores conectados a un delimitador ubicado previamente. [Aquí](/azure/spatial-anchors/concepts/anchor-relationships-way-finding/) puede obtener información sobre las relaciones de los delimitadores.
- Busque un delimitador mediante la [relocalización general](/azure/spatial-anchors/concepts/coarse-reloc/).

Si va a buscar anclajes espaciales en la nube por identificador, querrá almacenar el identificador de anclaje espacial de la nube en el servicio back-end de la aplicación y hacer que todos los dispositivos que puedan autenticarse correctamente en él puedan acceder a él. For ver un ejemplo de esto, consulte [Tutorial: Uso compartido de Spatial Anchors entre dispositivos](/azure/spatial-anchors/tutorials/tutorial-share-anchors-across-devices/).

Crear una instancia de un objeto `AnchorLocateCriteria`, establezca los identificadores que busca e invoque el método `CreateWatcher` en la sesión mediante su `AnchorLocateCriteria`.