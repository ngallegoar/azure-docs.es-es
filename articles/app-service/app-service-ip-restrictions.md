---
title: Restricciones de acceso de Azure App Service
description: Aprenda a proteger la aplicación en Azure App Service configurando restricciones de acceso.
author: ccompy
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: e1549dda367105db34272eab8a90c1760dd5bb5c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96010187"
---
# <a name="set-up-azure-app-service-access-restrictions"></a>Configuración de las restricciones de acceso de Azure App Service

Al configurar las restricciones de acceso, puede definir una lista ordenada por prioridad de elementos permitidos o denegados con la que se controla el acceso de red a la aplicación. La lista puede incluir direcciones IP o subredes de Azure Virtual Network. Si hay una o varias entradas, existe un valor *denegar todo* implícito al final de la lista.

La capacidad de restricción de acceso funciona con todas las cargas de trabajo hospedadas en Azure App Service. Las cargas de trabajo pueden incluir aplicaciones web, aplicaciones de API, aplicaciones de Linux, aplicaciones de contenedor de Linux y funciones.

Cuando se realiza una solicitud a la aplicación, la dirección de origen se evalúa con respecto a una serie de reglas de dirección IP de la lista de restricción de acceso. Si la dirección de origen está en una subred configurada con puntos de conexión de servicio a Microsoft.Web, la subred de origen se compara con las reglas de red virtual de la lista de restricción de acceso. Si la dirección no tiene permitido el acceso según las reglas de la lista, el servicio responde con un código de estado [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403).

La funcionalidad de restricción de acceso se implementa en los roles front-end de App Service, que son de nivel superior de los hosts de trabajo donde el código se ejecuta. Por lo tanto, las restricciones de acceso son listas de control de acceso de red de facto.

La capacidad de restringir el acceso a la aplicación web desde una instancia de Azure Virtual Network (red virtual) se obtiene mediante [puntos de conexión de servicio][serviceendpoints]. Con los puntos de conexión de servicio, puede restringir el acceso de un servicio multiinquilino de las subredes seleccionadas. No funciona para restringir el tráfico a aplicaciones hospedadas en un entorno App Service Environment. Si está en un entorno App Service Environment, puede controlar el acceso a la aplicación con reglas de direcciones IP.

> [!NOTE]
> Los puntos de conexión de servicio se deben habilitar tanto en el lado de la red como en el servicio de Azure con el que se están habilitando. Para obtener una lista de los servicios de Azure que admiten puntos de conexión de servicio, consulte [Puntos de conexión de servicio de red virtual](../virtual-network/virtual-network-service-endpoints-overview.md).
>

![Diagrama del flujo de restricciones de acceso.](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="add-or-edit-access-restriction-rules-in-the-portal"></a>Incorporación y edición de reglas de restricciones de acceso en el portal

Para agregar una regla de restricción de acceso a la aplicación, haga lo siguiente:

1. Inicie sesión en Azure Portal.

1. En el panel izquierdo, seleccione **Redes**.

1. En el panel **Redes**, bajo **Restricciones de acceso**, seleccione **Configurar restricciones de acceso**.

   ![Captura de pantalla del panel de opciones de redes de App Service en Azure Portal.](media/app-service-ip-restrictions/access-restrictions.png)  

1. En la página **Restricciones de acceso**, revise la lista de reglas de restricción de acceso definidas para la aplicación.

   ![Captura de pantalla de la página Restricciones de acceso en Azure Portal que muestra la lista de reglas de restricción de acceso definidas para la aplicación seleccionada.](media/app-service-ip-restrictions/access-restrictions-browse.png)

   La lista muestra todas las restricciones actuales referentes a la aplicación. Si tiene una restricción de red virtual en la aplicación, la tabla muestra si los puntos de conexión del servicio están habilitados para Microsoft.Web. Si no hay restricciones definidas en la aplicación, se puede acceder a la aplicación desde cualquier lugar.  

### <a name="add-an-access-restriction-rule"></a>Adición de una regla de restricción de acceso

Para agregar una regla de restricción de acceso a la aplicación, en el panel **Restricciones de acceso**, seleccione **Agregar regla**. Una vez que agregue una regla, entrará en vigor de inmediato. 

Las reglas se aplican en orden de prioridad, empezando por el número más bajo en la columna **Prioridad**. Una *denegación de todo* implícita entra en vigor una vez que agregue incluso una sola regla.

En el panel **Agregar restricción de IP**, cuando cree una regla, haga lo siguiente:

1. En **Acción**, seleccione **Permitir** o **Denegar**.  

   ![Captura de pantalla del panel "Agregar restricción de IP".](media/app-service-ip-restrictions/access-restrictions-ip-add.png)
   
1. Opcionalmente, escriba un nombre y una descripción para la regla.  
1. En la lista desplegable **Tipo**, seleccione el tipo de regla.  
1. En el cuadro **Prioridad**, escriba un valor de prioridad.  
1. En las listas desplegables **Suscripción**, **Red virtual** y **Subred**, seleccione a qué desea restringir el acceso.  

### <a name="set-an-ip-address-based-rule"></a>Establecimiento de una regla basada en direcciones IP

Siga el procedimiento descrito en la sección anterior, pero con la siguiente variación:
* En el paso 3, en la lista desplegable **Tipo**, seleccione **IPv4** o **IPv6**. 

Especifique la dirección IP en la notación de enrutamiento de interdominios sin clases (CIDR) para las direcciones IPv4 e iPv6. Para especificar una dirección, puede usar un formato como *1.2.3.4/32*, donde los cuatro primeros octetos representan la dirección IP y */32* es la máscara. La notación CIDR IPv4 para todas las direcciones es 0.0.0.0/0. Para más información acerca de la notación CIDR, consulte [Enrutamiento de interdominios sin clases](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). 

## <a name="use-service-endpoints"></a>Uso de puntos de conexión de servicio

Con los puntos de conexión de servicio, puede restringir el acceso a las subredes de Azure Virtual Network. Para restringir el acceso a una subred específica, cree una regla de restricción con un tipo **Virtual Network**. Después, puede seleccionar la suscripción, la red virtual y la subred a las que desea permitir o denegar el acceso. 

Si los puntos de conexión de servicio ya no están habilitados con Microsoft.Web para la subred seleccionada, se habilitarán automáticamente a menos que active la casilla **Omitir los puntos de conexión de servicio de Microsoft.Web que faltan**. La situación en la que convendría habilitar puntos de conexión de servicio en la aplicación pero no en la subred depende fundamentalmente de si se tienen los permisos para habilitarlos en la subred. 

Si necesita que otra persona habilite los puntos de conexión de servicio en la subred, active la casilla **Omitir los puntos de conexión de servicio de Microsoft.Web que faltan**. La aplicación se configurará para los puntos de conexión de servicio en previsión de que se habiliten posteriormente en la subred. 

![Captura de pantalla del panel "Agregar restricción de IP" con el tipo Virtual Network seleccionado.](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

No puede usar puntos de conexión de servicio para restringir el acceso a las aplicaciones que se ejecutan en un entorno App Service Environment. Si la aplicación está en un entorno App Service Environment, puede controlar el acceso a ella con reglas de acceso de IP. 

Con puntos de conexión de servicio, la aplicación se puede configurar con puertas de enlace de aplicación u otros dispositivos de firewall de aplicaciones web (WAF). También se pueden configurar aplicaciones de varios niveles con back-ends seguros. Para más información, lea [Características de redes de App Service](networking-features.md) e [Integración de Application Gateway con puntos de conexión de servicio](networking/app-gateway-with-service-endpoints.md).

> [!NOTE]
> - Actualmente no se admiten puntos de conexión de servicio para las aplicaciones web que usan IP virtual (VIP) con Capa de sockets seguros (SSL) de IP.
> - Hay un límite de 512 filas de restricciones de IP o punto de conexión de servicio. Si necesita más de 512 filas de restricciones, se recomienda que considere la instalación de un producto de seguridad independiente, como Azure Front Door, Azure Application Gateway o WAF.
>

## <a name="manage-access-restriction-rules"></a>Administración de reglas de restricción de acceso

Puede editar o eliminar una regla de restricción de acceso existente.

### <a name="edit-a-rule"></a>Edición de una regla

1. Para empezar a editar una regla de restricción de acceso existente, en la página **Restricciones de acceso**, haga doble clic en la regla que desea editar.

1. En el panel **Editar la restricción de IP**, realice los cambios y, a continuación, seleccione **Actualizar regla**. Las modificaciones son efectivas inmediatamente, incluidos los cambios en el orden de prioridad.

   ![Captura de pantalla del panel "Editar la restricción de IP" en Azure Portal que muestra los campos para una regla de restricción de acceso existente.](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

   > [!NOTE]
   > Al editar una regla, no puede cambiar entre una regla de dirección IP y una regla de red virtual. 

   ![Captura de pantalla del panel "Editar la restricción de IP" en Azure Portal que muestra la configuración de una regla de red virtual.](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

### <a name="delete-a-rule"></a>Eliminar una regla

Para eliminar una regla, en la página **Restricciones de acceso**, seleccione los puntos suspensivos ( **...** ) junto a la regla que desea eliminar y luego seleccione **Quitar**.

![Captura de pantalla de la página "Restricciones de acceso", que muestra los puntos suspensivos "Quitar" junto a la regla de restricción de acceso que se va a eliminar.](media/app-service-ip-restrictions/access-restrictions-delete.png)

## <a name="block-a-single-ip-address"></a>Bloqueo de una dirección IP única

Al agregar la primera regla de restricción de IP, el servicio agrega una regla *Denegar todo* explícita con una prioridad de 2147483647. En la práctica, la regla explícita *Denegar todo* es la última regla que se ejecuta, y bloquea el acceso a cualquier dirección IP que no esté expresamente permitida mediante una regla *Permitir*.

En una situación en la que quiera bloquear expresamente una única dirección IP o un bloque de direcciones IP, pero permitir el acceso a todo lo demás, agregue una regla *Permitir todo* explícita.

![Captura de pantalla de la página "Restricciones de acceso" en Azure Portal, que muestra una única dirección IP bloqueada.](media/app-service-ip-restrictions/block-single-address.png)

## <a name="restrict-access-to-an-scm-site"></a>Restricción del acceso a un sitio de SCM 

Aparte de controlar el acceso a la aplicación, también puede restringir el acceso al sitio de SCM que utiliza la aplicación. El sitio de SCM es el punto de conexión de Web Deploy y la consola de Kudu. Puede asignar restricciones de acceso al sitio de SCM desde la aplicación de manera independiente o usar el mismo conjunto de restricciones para la aplicación y el sitio de SCM. Al activar la casilla **Mismas restricciones que \<app name>** , todo está en blanco. Si desactiva la casilla, se vuelve a aplicar la configuración del sitio de SCM. 

![Captura de pantalla de la página "Restricciones de acceso" en Azure Portal que muestra que no hay restricciones de acceso establecidas para la aplicación o el sitio de SCM.](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="manage-access-restriction-rules-programatically"></a>Administración de reglas de restricción de acceso mediante programación

Puede agregar restricciones de acceso mediante programación realizando una de las siguientes acciones: 

* Use la [CLI de Azure](/cli/azure/webapp/config/access-restriction?view=azure-cli-latest&preserve-view=true). Por ejemplo:
   
  ```azurecli-interactive
  az webapp config access-restriction add --resource-group ResourceGroup --name AppName \
  --rule-name 'IP example rule' --action Allow --ip-address 122.133.144.0/24 --priority 100
  ```

* Use [Azure PowerShell](/powershell/module/Az.Websites/Add-AzWebAppAccessRestrictionRule?view=azps-3.1.0&preserve-view=true). Por ejemplo:


  ```azurepowershell-interactive
  Add-AzWebAppAccessRestrictionRule -ResourceGroupName "ResourceGroup" -WebAppName "AppName"
      -Name "Ip example rule" -Priority 100 -Action Allow -IpAddress 122.133.144.0/24
  ```

También puede establecer los valores manualmente mediante una de las siguientes acciones:

* Use una operación PUT de la [API REST de Azure](/rest/api/azure/) en la configuración de la aplicación en Azure Resource Manager. La ubicación de esta información en Azure Resource Manager es la siguiente:

  management.azure.com/subscriptions/**Id. de suscripción**/resourceGroups/**resource groups**/providers/Microsoft.Web/sites/**nombre de aplicación web**/config/web?api-version=2018-02-01

* Use una plantilla de Resource Manager. Por ejemplo, puede usar resources.azure.com y editar el bloque ipSecurityRestrictions para agregar el código JSON requerido.

  La sintaxis JSON para el ejemplo anterior es:

  ```json
  {
    "properties": {
      "ipSecurityRestrictions": [
        {
          "ipAddress": "122.133.144.0/24",
          "action": "Allow",
          "priority": 100,
          "name": "IP example rule"
        }
      ]
    }
  }
  ```

## <a name="set-up-azure-functions-access-restrictions"></a>Configuración de las restricciones de acceso de Azure Functions

También hay restricciones de acceso disponibles para las aplicaciones de funciones con la misma funcionalidad que los planes de App Service. Al habilitar las restricciones de acceso, también deshabilita el editor de código de Azure Portal en las direcciones IP no permitidas.

## <a name="next-steps"></a>Pasos siguientes
[Restricciones de acceso para Azure Functions](../azure-functions/functions-networking-options.md#inbound-access-restrictions)  
[Integración de Application Gateway con puntos de conexión de servicio](networking/app-gateway-with-service-endpoints.md)

<!--Links-->
[serviceendpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
