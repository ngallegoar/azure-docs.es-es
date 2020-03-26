---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-network
author: asudbring
ms.service: virtual-network
ms.topic: include
ms.date: 03/01/2020
ms.author: allensu
ms.custom: include file
ms.openlocfilehash: 81db46b52c9b4fe800f2fbfeadad966995d66e12
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78244969"
---
## <a name="create-the-virtual-network"></a>Crear la red virtual

En esta sección, creará una red virtual y una subred.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso > Redes > Red virtual** o busque **Red virtual** en el cuadro de búsqueda.

2. En **Crear red virtual**, escriba o seleccione esta información en la pestaña **Conceptos básicos**:

    | **Configuración**          | **Valor**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Detalles del proyecto**  |                                                                 |
    | Subscription     | Selección de su suscripción a Azure                                  |
    | Grupo de recursos   | Seleccione **Crear nuevo**, escriba **\<resource-group-name>** , seleccione Aceptar o un **\<resource-group-name>** existente en función de los parámetros. |
    | **Detalles de instancia** |                                                                 |
    | Nombre             | Especifique **\<virtual-network-name>** .                                    |
    | Region           | Seleccione **\<region-name>** . |

3. Seleccione la pestaña **Direcciones IP** o el botón **Siguiente: Direcciones IP** situado en la parte inferior de la página.

4. En la pestaña **Direcciones IP**, especifique esta información:

    | Configuración            | Value                      |
    |--------------------|----------------------------|
    | Espacio de direcciones IPv4 | Indique **\<IPv4-address-space>** . |

5. En **Nombre de subred**, seleccione la palabra **predeterminada**.

6. En **Editar subred**, especifique esta información:

    | Configuración            | Value                      |
    |--------------------|----------------------------|
    | Nombre de subred | Indique **\<subnet-name>** |
    | Intervalo de direcciones de subred | Especifique **\<subnet-address-range>**

7. Seleccione **Guardar**.

8. Seleccione la pestaña **Revisar y crear** o el botón **Revisar y crear**.

9. Seleccione **Crear**.