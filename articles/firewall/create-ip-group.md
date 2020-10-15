---
title: Creación de grupos de IP en Azure Firewall
description: Los grupos de IP permiten agrupar y administrar direcciones IP en las reglas de Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/23/2020
ms.author: victorh
ms.openlocfilehash: c3ae62bf5b4f0b4796cac2e7079c8a09116d4895
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85602540"
---
# <a name="create-ip-groups"></a>Creación de grupos de IP

Los grupos de IP permiten agrupar y administrar direcciones IP en las reglas de Azure Firewall. Pueden tener una sola dirección IP, varias direcciones IP o uno o varios intervalos de direcciones IP.

## <a name="create-an-ip-group"></a>Creación de un grupo de IP

1. En la página principal de Azure Portal, seleccione **Crear un recurso**.
2. Escriba **IP Groups** en el cuadro de búsqueda y, luego, seleccione **IP Groups**.
3. Seleccione **Crear**.
4. Seleccione su suscripción.
5. seleccione un grupo de recursos o cree uno.
6. Escriba un nombre único para el grupo de IP y, luego, seleccione una región.

6. Seleccione **Siguiente: Direcciones IP**.
7. Escriba una dirección IP, varias direcciones IP o intervalos de direcciones IP.

   Hay dos maneras de escribir direcciones IP:
   - Puede escribirlas manualmente.
   - Puede importarlas desde un archivo.

   Para importarlas desde un archivo, seleccione **Import from a file** (Importar de un archivo). Puede arrastrar el archivo al cuadro o seleccionar **Examinar archivos**. Si es necesario, puede revisar y editar las direcciones IP cargadas.

   Al escribir una dirección IP, el portal la valida para comprobar que no haya problemas de duplicados, superposición y formato.

5. Cuando termine, seleccione **Revisar y crear**.
6. Seleccione **Crear**.


## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre los grupos de IP](ip-groups.md)