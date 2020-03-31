---
title: 'Inicio rápido: Creación de un perfil de Front Door para que las aplicaciones tengan alta disponibilidad'
description: Este artículo de la guía de inicio rápido describe cómo crear una instancia de Front Door para una aplicación web global de alta disponibilidad y alto rendimiento.
services: front-door
documentationcenter: ''
author: sharad4u
editor: ''
ms.assetid: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2018
ms.author: sharadag
ms.openlocfilehash: a98a933113322509f6fda8678350e9415d0b4058
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "79471428"
---
# <a name="quickstart-create-a-front-door-for-a-highly-available-global-web-application"></a>Inicio rápido: Cree una instancia de Front Door para una aplicación web global de alta disponibilidad.

En esta guía de inicio rápido se describe cómo crear un perfil de Front Door que ofrece alta disponibilidad y alto rendimiento para una aplicación web global. 

El escenario descrito en esta guía de inicio rápido incluye dos instancias de una aplicación web que se ejecutan en regiones diferentes de Azure. Se crea una configuración de Front Door basada en back-ends de la misma [ponderación y la misma prioridad](front-door-routing-methods.md) que ayuda a dirigir el tráfico del usuario al conjunto más cercano de back-ends del sitio que ejecuta la aplicación. Front Door supervisa continuamente la aplicación web y proporciona conmutación por error automática al siguiente back-end disponible cuando el sitio más cercano no está disponible.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure 
Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="prerequisites"></a>Prerrequisitos
Esta guía de inicio rápido requiere que haya implementado dos instancias de una aplicación web que se ejecuten en distintas regiones de Azure (*Este de EE. UU.* y *Oeste de Europa*). Las dos instancias de la aplicación web se ejecutan en modo Activo/activo, es decir, cualquiera de ellas puede asumir el tráfico en cualquier momento, a diferencia de una configuración Activo/en espera, donde uno actúa como una conmutación por error.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Web** > **Aplicación web** > **Crear** .
2. En **Aplicación web**, escriba o seleccione la información siguiente y especifique la configuración predeterminada cuando no se especifica ninguna:

     | Configuración         | Value     |
     | ---              | ---  |
     | Nombre           | Escriba un nombre único para la aplicación web  |
     | Resource group          | Seleccione **Nuevo** y, después, escriba *myResourceGroupFD1* |
     | Plan de App Service/Ubicación         | Seleccione **Nuevo**.  En el plan de App Service, escriba *myAppServicePlanEastUS* y, a continuación, seleccione **Aceptar**. 
     |      Location  |   Este de EE. UU.        |
    |||

3. Seleccione **Crear**.
4. Cuando la aplicación web se implementa correctamente, se crea un sitio web predeterminado.
5. Repita los pasos 1 a 3 para crear un segundo sitio web en otra región de Azure con la configuración siguiente:

     | Configuración         | Value     |
     | ---              | ---  |
     | Nombre           | Escriba un nombre único para la aplicación web  |
     | Resource group          | Seleccione **Nuevo** y, después, escriba *myResourceGroupFD2* |
     | Plan de App Service/Ubicación         | Seleccione **Nuevo**.  En el plan de App Service, escriba *myAppServicePlanWestEurope* y, a continuación, seleccione **Aceptar**. 
     |      Location  |   Oeste de Europa      |
    |||


## <a name="create-a-front-door-for-your-application"></a>Creación de una instancia de Front Door para una aplicación
### <a name="a-add-a-frontend-host-for-front-door"></a>A. Adición de un host de front-end a Front Door
Cree una configuración de Front Door que dirija el tráfico del usuario en función de la latencia más baja entre los dos back-ends.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Front Door** > **Crear**.
2. En **Crear una instancia de Front Door**, empiece por agregar la información básica y especifique la suscripción en la que desea que se configure Front Door. Al igual que en cualquier otro recurso de Azure también tiene que especificar un grupo de recursos y una región del grupo de recursos si va a crear uno. Por último, debe especificar un nombre para su instancia de Front Door.
3. Una vez que se rellena la información básica, el primer paso necesario es definir el **host de front-end** para la configuración. El resultado debe ser un nombre de dominio válido como `myappfrontend.azurefd.net`. Este nombre de host debe ser globalmente único, pero Front Door se encargará de dicha validación. 

### <a name="b-add-application-backend-and-backend-pools"></a>B. Adición del back-end y de los grupos de back-end de una aplicación

A continuación, debe configurar los back-ends de su aplicación en un grupo de back-end para que Front Door sepa dónde reside la aplicación. 

1. Haga clic en el icono "+" para agregar un grupo de back-end y, después, especifique el **nombre** del grupo de back-end, como por ejemplo `myBackendPool`.
2. A continuación, haga clic en Agregar back-ends para agregar los sitios web que creó anteriormente.
3. En **Tipo de host de destino**, seleccione "App Service", seleccione la suscripción en la que creó el sitio web y, a continuación, elija el primer sitio web en **Nombre de host de destino**, es decir,  *myAppServicePlanEastUS.azurewebsites.net*.
4. Por ahora deje los restantes campos tal cual y haga clic en **Agregar'** .
5. Repita los pasos 2 a 4 para agregar el otro sitio web, es decir, *myAppServicePlanWestEurope.azurewebsites.net*
6. Opcionalmente, puede elegir actualizar la configuración de Sondeos de estado Equilibrio de carga para el grupo de back-end, pero también deberían funcionar los valores predeterminados. Haga clic en **Agregar**.


### <a name="c-add-a-routing-rule"></a>C. Agregar una regla de enrutamiento
Por último, haga clic en el icono "+" en Reglas de enrutamiento para configurar una regla de enrutamiento. Esto es necesario para asignar su host de front-end al grupo de back-end, que básicamente configura que si una solicitud llega a `myappfrontend.azurefd.net`, hay que reenviarla al grupo de back-end `myBackendPool`. Haga clic en **Agregar** para agregar la regla de enrutamiento de Front Door. Ya debe poder crear la instancia de Front Door y, por lo tanto, haga clic en **Review and Create** (Revisar y crear).

>[!WARNING]
> **Debe** asegurarse de que todos los hosts de front-end de Front Door tienen una regla de enrutamiento con una ruta de acceso predeterminada ('/\*') asociada. Es decir, entre todas las reglas de enrutamientos debe haber al menos una para cada uno de los hosts de front-end que se definen en la ruta de acceso predeterminada ('/\*'). Si no la hay, puede darse el caso de que el tráfico del usuario final no se enrute correctamente.

## <a name="view-front-door-in-action"></a>Ver Front Door en acción
Una vez que cree una instancia de Front Door, la configuración tardará unos minutos en implementarse globalmente en todas partes. Cuando haya terminado, acceda al host de front-end que ha creado, es decir, vaya a un explorador web y a la dirección URL `myappfrontend.azurefd.net`. La solicitud se enrutará automáticamente al back-end más cercano de los back-ends especificados en el grupo de back-end. 

### <a name="view-front-door-handle-application-failover"></a>Ver la conmutación por error de la aplicación del manipulador de Front Door
Si desea instantánea probar la conmutación por error global instantánea de Front Door, puede ir a uno de sitios web que ha creado y detenerlo. Según la configuración de Sondeo de estado definida para el grupo de back-end, conmutaremos el tráfico en caso de error a la implementación del otro sitio web. También puede probar el comportamiento deshabilitando el back-end en la configuración del grupo de back-end de Front Door. 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no los necesite, elimine el grupo de recursos, las aplicaciones web y todos los recursos relacionados.

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido, ha creado una instancia de Front Door que le permite dirigir el tráfico del usuario a las aplicaciones web que requieren una alta disponibilidad y el máximo rendimiento. Para más información acerca del enrutamiento de tráfico, lea los [métodos de enrutamiento](front-door-routing-methods.md) que utiliza Front Door.