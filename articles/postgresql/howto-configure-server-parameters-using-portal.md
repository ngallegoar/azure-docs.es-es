---
title: 'Configuración de parámetros de servidor de Azure Database for PostgreSQL: servidor único mediante Azure Portal'
description: En este artículo se explica cómo configurar los parámetros de Postgres en Azure Database for PostgreSQL mediante Azure Portal.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 02/28/2018
ms.openlocfilehash: e1b40e3116d56e87a2f397350ef2ba5510e04c0c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707700"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>Configuración de parámetros de servidor en Azure Database for PostgreSQL con un único servidor en Azure Portal 
Puede enumerar, mostrar y actualizar los parámetros de configuración de un servidor de Azure Database for PostgreSQL a través de Azure Portal.

## <a name="prerequisites"></a>Prerequisites
Para seguir esta guía, necesitará:
- [Un servidor de Azure Database for PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Ver y editar parámetros
1. Abra [Azure Portal](https://portal.azure.com).

2. Seleccione su servidor de Azure Database for PostgreSQL.

3. En la sección **CONFIGURACIÓN**, seleccione **Parámetros del servidor**. La página muestra una lista de parámetros, sus valores y descripciones.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png" alt-text="Página de información general de parámetros":::

4. Seleccione el botón **desplegable** para ver los posibles valores para los parámetros de tipo enumerado, como client_min_messages.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png" alt-text="Página de información general de parámetros":::

5. Seleccione o mantenga el puntero sobre el botón **i** (información) para ver el rango de valores posibles para los parámetros numéricos, como cpu_index_tuple_cost.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/4-information-button.png" alt-text="Página de información general de parámetros":::

6. Si es necesario, utilice el **cuadro de búsqueda** para reducir a un parámetro específico. Se busca en el nombre y la descripción de los parámetros.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/5-search.png" alt-text="Página de información general de parámetros":::

7. Cambie los valores de los parámetros que le gustaría ajustar. Todos los cambios que realiza en una sesión se resaltan en color púrpura. Una vez que haya cambiado los valores, puede seleccionar **Guardar**. O bien puede **descartar** los cambios.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png" alt-text="Página de información general de parámetros":::

8. Si ha guardado los nuevos valores para los parámetros, siempre puede revertir todos los elementos a los valores predeterminados; para ello, seleccione **Restablecer todos los valores predeterminados**.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png" alt-text="Página de información general de parámetros":::

## <a name="next-steps"></a>Pasos siguientes
Más información sobre lo siguiente:
- [Información general de los parámetros del servidor en Azure Database for PostgreSQL](concepts-servers.md)
- [Configuración de parámetros con la CLI de Azure](howto-configure-server-parameters-using-cli.md)
