---
title: 'Inicio rápido: Creación de una memoria caché con el nivel Enterprise'
description: En este inicio rápido, aprenderá a crear una instancia de Azure Cache for Redis con el nivel Enterprise.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.custom: mvc
ms.topic: quickstart
ms.date: 10/28/2020
ms.openlocfilehash: bd5e05f38d34199d9012c52ca3fdad33af231aad
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93127992"
---
# <a name="quickstart-create-an-enterprise-tier-cache-preview"></a>Inicio rápido: Creación de una memoria caché con el nivel Enterprise (versión preliminar)

Los niveles Enterprise de Azure Cache for Redis proporcionan una instancia de [Redis Enterprise](https://redislabs.com/redis-enterprise/) totalmente integrada y administrada en Azure. Actualmente están disponibles en forma de versión preliminar. Hay dos nuevos niveles en esta versión preliminar:
* Enterprise, que usa memoria volátil (DRAM) en una máquina virtual para almacenar datos.
* Enterprise Flash, que usa memoria volátil y no volátil (NVMe o SSD) para almacenar los datos.

## <a name="prerequisites"></a>Prerrequisitos

Necesitará una suscripción a Azure antes de empezar. En caso de no tener ninguna, [cree una cuenta gratuita](https://azure.microsoft.com/free/) primero.

## <a name="create-a-cache"></a>Creación de una caché
1. Para crear una caché, inicie sesión en Azure Portal mediante el vínculo de la invitación a la versión preliminar y seleccione **Crear un recurso**.

1. En la página **Nuevo** , seleccione **Base de datos** y, a continuación, seleccione **Azure Cache for Redis**.
   
   :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Selección de Azure Cache for Redis":::
   
1. En la página **Nueva instancia de Redis Cache** , configure las opciones de la nueva caché.
   
   | Configuración      | Valor sugerido  | Descripción |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Suscripción** | Desplácese hacia abajo y seleccione su suscripción. | La suscripción en la que se creará esta nueva instancia de Azure Cache for Redis. | 
   | **Grupos de recursos** | Desplácese hacia abajo y seleccione un grupo de recursos o **Crear nuevo** y escriba un nombre nuevo para el grupo de recursos. | Nombre del grupo de recursos en el que se van a crear la caché y otros recursos. Al colocar todos los recursos de la aplicación en un grupo de recursos, puede administrarlos o eliminarlos fácilmente. | 
   | **Nombre DNS** | Escriba un nombre único global. | El nombre de la memoria caché debe ser una cadena de entre 1 y 63 caracteres, y solo puede contener números, letras o guiones. El nombre debe comenzar y terminar por un número o una letra y no puede contener guiones consecutivos. El *nombre de host* de la instancia de caché será *\<DNS name>.<Azure region>.redisenterprise.cache.azure.net*. | 
   | **Ubicación** | Desplácese hacia abajo y seleccione una ubicación. | Los niveles Enterprise están disponibles en regiones limitadas de Azure durante la versión preliminar. |
   | **Tipo de caché** | Despliegue el menú y seleccione el nivel *Enterprise* o *Enterprise Flash* y un tamaño. |  El nivel determina el tamaño, rendimiento y características disponibles para la memoria caché. |
   
   :::image type="content" source="media/cache-create/enterprise-tier-basics.png" alt-text="Aspectos básicos del nivel Enterprise":::

   > [!NOTE] 
   > Asegúrese de activar la casilla situada bajo "Términos" antes de continuar.
   >

1. Seleccione **Siguiente: Redes** y omítalo.

   > [!NOTE] 
   > Se está implementando la opción de Private Link y es posible que no esté disponible de forma inmediata en su región.
   >

1. Seleccione **Siguiente: Avanzado** y establezca **Directiva de agrupación en clústeres** en **Enterprise**.
   
   Puede conservar la configuración predeterminada o cambiarla según corresponda. Al activar **Allow access only via TLS** (Permitir el acceso solo mediante TLS), debe usar TLS para acceder a la nueva memoria caché desde la aplicación.

   :::image type="content" source="media/cache-create/enterprise-tier-advanced.png" alt-text="Aspectos avanzados del nivel Enterprise":::

   > [!NOTE] 
   > Los módulos de Redis no se admiten aún en el nivel Enterprise Flash. Si tiene previsto usar un módulo de Redis, asegúrese de elegir una caché de nivel Enterprise.
   >
   
1. Seleccione **Siguiente: Etiquetas** y omítalo.

1. Seleccione **Siguiente: Review + create** (Revisar y crear).

   :::image type="content" source="media/cache-create/enterprise-tier-summary.png" alt-text="Resumen del nivel Enterprise":::

1. Revise la configuración y haga clic en **Crear**.
   
   La caché tarda un tiempo en crearse. Puede supervisar el progreso en la página **Información general** de Azure Cache for Redis. Cuando **Estado** se muestra como **En ejecución** , la memoria caché está lista para su uso.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, aprenderá a crear una instancia de Azure Cache for Redis con el nivel Enterprise.

> [!div class="nextstepaction"]
> [Creación de una aplicación web ASP.NET que usa Azure Redis Cache.](./cache-web-app-howto.md)

