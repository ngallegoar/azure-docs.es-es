---
title: 'Configuración de una alta disponibilidad en Azure Database for PostgreSQL: Hiperescala (Citus)'
description: Cómo habilitar o deshabilitar la alta disponibilidad
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: a8d4b5949b34d16191e9ec10a1dd39faff3660dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74977678"
---
# <a name="configure-hyperscale-citus-high-availability"></a>Configurar la alta disponibilidad de Hiperescala (Citus)

Azure Database for PostgreSQL - Hiperescala (Citus) proporciona alta disponibilidad (HA) para evitar tiempo de inactividad en la base de datos. Con la alta disponibilidad habilitada, todos los nodos de un grupo de servidores obtendrán un estado de espera. Si el nodo original pasa a ser incorrecto, se promoverá su modo de espera para reemplazarlo.

> [!IMPORTANT]
> Dado que la alta disponibilidad duplica el número de servidores del grupo, también duplicará el coste.

La habilitación de la alta disponibilidad es posible durante la creación de grupos de servidores, o después en la pestaña **Configurarar** para su grupo de servidores en el Azure Portal. La interfaz de usuario es similar en cualquier caso. Arrastre el control deslizante de **Alta disponibilidad** a SÍ:

![control de alta disponibilidad](./media/howto-hyperscale-high-availability/01-ha-slider.png)

Haga clic en el botón **Guardar** para aplicar la selección. La habilitación de la alta disponibilidad puede llevar algún tiempo, ya que el grupo de servidores aprovisiona los datos en espera y los transmite por secuencias.

En la pestaña **Introducción** del grupo de servidores se enumerarán todos los nodos y sus esperas, junto con una columna **Alta disponibilidad** que indica si la alta disponibilidad está habilitada correctamente para cada nodo.

![Columna de alta disponibilidad en la información general del grupo de servidores](./media/howto-hyperscale-high-availability/02-ha-column.png)

### <a name="next-steps"></a>Pasos siguientes

Para más información sobre la [alta disponibilidad](concepts-hyperscale-high-availability.md).
