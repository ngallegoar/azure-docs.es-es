---
title: Conexiones híbridas
description: Aprenda a crear y usar conexiones híbridas en Azure App Service para acceder a recursos de redes distintas.
author: ccompy
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18, fasttrack-edit
ms.openlocfilehash: ec842530f3cae26b869a649617f279d204b98fcc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80047768"
---
# <a name="azure-app-service-hybrid-connections"></a>Hybrid Connections de Azure App Service

Conexiones híbridas es tanto un servicio de Azure como una característica de Azure App Service. Como servicio tiene un uso y unas funcionalidades que van más allá de aquellas que se usan en App Service. Para más información sobre Conexiones híbridas y su uso fuera de App Service, consulte [Protocolo de Conexiones híbridas de Azure Relay][HCService].

En App Service, Conexiones híbridas se puede usar para acceder a recursos de aplicaciones en otras redes. Proporciona acceso desde la aplicación a un punto de conexión de la aplicación. No permiten una posibilidad alternativa de acceder a su aplicación. Dado que se utiliza en App Service, cada conexión híbrida se correlaciona con una combinación única de host y puerto TCP. Esto significa que el punto de conexión híbrida puede estar en cualquier sistema operativo y en cualquier aplicación, siempre y cuando se acceda a un puerto de escucha TCP. La característica Conexiones híbridas no sabe lo que es el protocolo de aplicaciones ni sabe a qué se accede. Simplemente ofrece acceso a la red.  


## <a name="how-it-works"></a>Funcionamiento ##
La característica Conexiones híbridas consta de dos llamadas salientes a Azure Service Bus Relay. Existe una conexión desde una biblioteca en el host donde la aplicación se ejecuta en App Service. También hay una conexión desde el Administrador de conexiones híbridas (HCM) a Service Bus Relay. HCM es un servicio de retransmisión que se implementa en la red que hospeda el recurso al que intenta acceder. 

Mediante las dos conexiones unidas, la aplicación tiene un túnel TCP a una combinación fija de host:puerto al otro lado del HCM. La conexión usa TLS 1.2 para la seguridad y las claves de firma de acceso compartido (SAS) para la autenticación y la autorización.    

![Diagrama del flujo de alto nivel de conexión híbrida][1]

Cuando la aplicación realiza una solicitud DNS que coincide con un punto de conexión híbrida configurado, el tráfico TCP saliente se redirige por la conexión híbrida,  

> [!NOTE]
> lo que significa que debe intentar usar siempre un nombre DNS para la conexión híbrida. Cierto software cliente no lleva a cabo una búsqueda de DNS si el punto de conexión utiliza una dirección IP. 
>

### <a name="app-service-hybrid-connection-benefits"></a>Beneficios de usar las conexiones híbridas de App Service ###

El uso de la funcionalidad de conexiones híbridas reporta varios beneficios, entre los que se cuentan los siguientes:

- Las aplicaciones pueden acceder de forma segura a los servicios y sistemas locales.
- La característica no requiere ningún punto de conexión al que se pueda acceder a través de Internet.
- Se configura de forma rápida y sencilla. 
- Cada conexión híbrida coincide con una combinación única de host:puerto, lo que resulta útil para la seguridad.
- Normalmente no requiere vulnerabilidades de seguridad en el firewall, porque todas las conexiones salen a través de puertos web estándar.
- Dado que la característica está en el nivel de la red, da igual el lenguaje que use la aplicación y la tecnología que emplee el punto de conexión.
- Se puede utilizar para proporcionar acceso en varias redes desde una única aplicación. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Cosas que no se pueden hacer con las conexiones híbridas ###

Las cosas que no se pueden hacer con las conexiones híbridas incluyen:

- Montaje de una unidad.
- Uso de UDP.
- Acceso a servicios basados en TCP que usen puertos dinámicos, como el modo pasivo FTP o el modo pasivo extendido.
- Compatibilidad con LDAP, porque puede requerir UDP.
- Compatibilidad con Active Directory, porque un trabajo de App Service no se puede unir a un dominio.

### <a name="prerequisites"></a>Prerequisites ###
 - Se requiere Windows App Service. Solo está disponible en Windows.  

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Incorporación y creación de Conexiones híbridas en la aplicación ##

Para crear una conexión híbrida, vaya a [Azure Portal][portal] y seleccione su aplicación. Seleccione **Redes** > **Configure los puntos de conexión de la conexión híbrida**. Aquí se pueden ver las conexiones híbridas que están configuradas para su aplicación.  

![Captura de pantalla de la lista de conexiones híbridas][2]

Para agregar una conexión híbrida nueva, seleccione **[+] Agregar conexión híbrida**.  Verá una lista de las conexiones híbridas que ya haya creado. Para agregar una o varias de ellas en la aplicación, seleccione las que desee y, luego, seleccione **Agregar conexión híbrida seleccionada**.  

![Captura de pantalla del portal de conexiones híbridas][3]

Si quiere crear una conexión híbrida, seleccione **Crear conexión híbrida nueva**. Especifique: 

- Nombre de la conexión híbrida.
- El nombre de host del punto de conexión.
- El puerto del punto de conexión.
- El espacio de nombres de Service Bus que desea usar.

![Captura de pantalla del cuadro de diálogo Crear conexión híbrida nueva][4]

Cada conexión híbrida está asociada a un espacio de nombres de Service Bus y cada uno de estos espacios se encuentra en una región de Azure. Es importante intentar usar un espacio de nombres de Service Bus que se encuentre en la misma región que la aplicación, para evitar una latencia de red excesiva.

Si desea quitar una conexión híbrida de una aplicación, haga clic con el botón derecho en ella y seleccione **Desconectar**.  

Cuando se agrega una conexión híbrida a la aplicación, puede seleccionarla para ver su información detallada. 

![Captura de pantalla de los detalles de las conexiones híbridas][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Creación de una conexión híbrida en el portal de Azure Relay ###

Además de la experiencia del portal dentro de la aplicación, puede crear conexiones híbridas en el portal de Azure Relay. Para que App Service use una conexión híbrida, debe:

* Requerir autorización del cliente.
* Tener un elemento de metadatos llamado punto de conexión que contenga una combinación de host:puerto como valor.

## <a name="hybrid-connections-and-app-service-plans"></a>Conexiones híbridas y planes de App Service ##

Las conexiones híbridas de App Service solo están disponibles en las SKU de precios de nivel Básico, Estándar, Premium y Aislado. No hay límites asociados al plan de precios.  

| Plan de precios | Número de conexiones híbridas que se pueden usar en el plan |
|----|----|
| Básica | 5 |
| Estándar | 25 |
| Premium | 200 |
| Aislado | 200 |

La interfaz de usuario del plan de App Service muestra cuántas conexiones híbridas se usan y qué aplicaciones las usan.  

![Captura de pantalla de las propiedades del plan de App Service][6]

Seleccione la conexión híbrida para ver detalles. Puede consultar toda la información que vio en la vista de la aplicación. También puede ver cuántas otras aplicaciones en el mismo plan usan esa conexión híbrida.

Hay un límite en el número de puntos de conexión de las conexiones híbridas que se pueden usar en un plan de App Service. Sin embargo, cada conexión híbrida se puede usar en cualquier cantidad de aplicaciones de ese plan. Por ejemplo, una conexión híbrida única que se usa en cinco aplicaciones distintas en un plan de App Service cuenta como una conexión híbrida.

### <a name="pricing"></a>Precios ###

Además de un requisito de SKU del plan de App Service, hay un costo adicional por el uso de las conexiones híbridas. Hay un cargo por cada agente de escucha que una conexión híbrida utilice. El agente de escucha es el Administrador de conexiones híbridas. Si tiene cinco conexiones híbridas compatibles con dos Administradores de conexiones híbridas, esto sería un total de 10 agentes de escucha. Para más información, consulte [Precios de Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybrid Connection Manager ##

La característica Conexiones híbridas requiere un agente de retransmisión en la red que hospeda el punto de conexión de la conexión híbrida. Dicho agente de retransmisión se denomina Hybrid Connection Manager (HCM). Para descargar HCM, desde la aplicación en [Azure Portal][portal], seleccione **Redes** > **Configure los puntos de conexión de la conexión híbrida**.  

Esta herramienta se ejecuta en Windows Server 2012 y versiones posteriores. El Administrador de conexiones híbridas se ejecuta como un servicio y se conecta para la salida a Azure Relay en el puerto 443.  

Después de instalar HCM, puede ejecutar HybridConnectionManagerUi.exe para usar la interfaz de usuario de la herramienta. Este archivo se encuentra en el directorio de instalación del Administrador de conexiones híbridas. En Windows 10, también puede simplemente buscar la *interfaz de usuario del Administrador de conexiones híbridas* en el cuadro de búsqueda.  

![Captura de pantalla de Administrador de conexiones híbridas][7]

Cuando inicia la interfaz de usuario de HCM, lo primero que se ve es una tabla donde se muestran todas las conexiones híbridas configuradas con esta instancia de HCM. Si desea hacer algún cambio, primero debe autenticarse con Azure. 

Para agregar una o varias conexiones híbridas a HCM:

1. Inicie la interfaz de usuario de HCM.
2. Seleccione **Configure another Hybrid Connection** (Configurar otra conexión híbrida).
![Captura de pantalla de configuración de conexiones híbridas nuevas][8]

1. Inicie sesión con su cuenta de Azure para obtener las conexiones híbridas disponibles con sus suscripciones. El HCM no usará su cuenta de Azure aparte de para esto. 
1. Elija una suscripción.
1. Seleccione las conexiones híbridas que desea que retransmita el HCM.
![Captura de pantalla de las conexiones híbridas][9]

1. Seleccione **Guardar**.

Ahora puede ver las conexiones híbridas que agregó. También puede seleccionar la conexión híbrida configurada para ver detalles.

![Captura de pantalla de los detalles de la conexión híbrida][10]

Para que HCM pueda admitir las conexiones híbridas con las que se configuró, necesita:

- Acceso de TCP a Azure a través del puerto 443.
- Acceso de TCP al punto de conexión de la conexión híbrida.
- La capacidad de realizar búsquedas de DNS en el host del punto de conexión y el espacio de nombres de Service Bus.

> [!NOTE]
> Azure Relay depende de Web Sockets para la conectividad. Esta funcionalidad solo está disponible en Windows Server 2012 o versiones posteriores. Por este motivo, HCM no se admite en versiones anteriores de Windows Server 2012.
>

### <a name="redundancy"></a>Redundancia ###

Cada HCM puede admitir varias conexiones híbridas. Además, varios HCM pueden admitir cualquier conexión híbrida dada. El comportamiento predeterminado es enrutar el tráfico a través de los HCM configurados en cualquier punto de conexión dado. Si desea alta disponibilidad en las conexiones híbridas de la red, ejecute varias instancias de HCM en máquinas independientes. El algoritmo de distribución de carga usado por el servicio Relay para distribuir el tráfico a los Administradores de conexiones híbridas es de asignación aleatoria. 

### <a name="manually-add-a-hybrid-connection"></a>Incorporación manual de una conexión híbrida ###

Para permitir que alguien de fuera de su suscripción hospede una instancia de HCM para una conexión híbrida determinada, comparta con dicho usuario la cadena de conexión de puerta de enlace de la conexión híbrida. Puede ver la cadena de conexión de la puerta de enlace en las propiedades de la conexión híbrida en [Azure Portal][portal]. Para usar dicha cadena, seleccione **Enter Manually** (Especificar manualmente) en el HCM y pegue la cadena de conexión de puerta de enlace.

![Incorporación manual de una conexión híbrida][11]

### <a name="upgrade"></a>Actualizar ###

El Administrador de conexiones híbridas se actualiza periódicamente para solucionar problemas o proporcionar mejoras. Cuando se publique una actualización, se mostrará un elemento emergente en la interfaz de usuario del Administrador de conexiones híbridas. Al aplicar la actualización se aplicarán los cambios y se reiniciará el Administrador de conexiones híbridas. 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>Incorporación de una conexión híbrida a su aplicación mediante programación ##

Las API que se indican a continuación pueden utilizarse directamente para administrar las conexiones híbridas conectadas a las aplicaciones. 

    /subscriptions/[subscription name]/resourceGroups/[resource group name]/providers/Microsoft.Web/sites/[app name]/hybridConnectionNamespaces/[relay namespace name]/relays/[hybrid connection name]?api-version=2016-08-01

El objeto JSON asociado con una conexión híbrida tiene el siguiente aspecto:

    {
      "name": "[hybrid connection name]",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "[location]",
      "properties": {
        "serviceBusNamespace": "[namespace name]",
        "relayName": "[hybrid connection name]",
        "relayArmUri": "/subscriptions/[subscription id]/resourceGroups/[resource group name]/providers/Microsoft.Relay/namespaces/[namespace name]/hybridconnections/[hybrid connection name]",
        "hostName": "[endpoint host name]",
        "port": [port],
        "sendKeyName": "defaultSender",
        "sendKeyValue": "[send key]"
      }
    }

Una forma de usar esta información es con ARMClient, que se puede obtener del proyecto de GitHub [ARMClient][armclient]. Este es un ejemplo acerca de cómo vincular una conexión híbrida existente con una aplicación. Cree un archivo JSON siguiendo el esquema anterior como:

    {
      "name": "relay-demo-hc",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "North Central US",
      "properties": {
        "serviceBusNamespace": "demo-relay",
        "relayName": "relay-demo-hc",
        "relayArmUri": "/subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myrelay-rg/providers/Microsoft.Relay/namespaces/demo-relay/hybridconnections/relay-demo-hc",
        "hostName": "my-wkstn.home",
        "port": 1433,
        "sendKeyName": "defaultSender",
        "sendKeyValue": "Th9is3is8a82lot93of3774stu887ff122235="
      }
    }

Para usar esta API, necesita la clave de envío y el identificador de recurso de retransmisión. Si ha guardado la información con el nombre de archivo hctest.json, emita este comando para adjuntar la conexión híbrida a la aplicación: 

    armclient login
    armclient put /subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myapp-rg/providers/Microsoft.Web/sites/myhcdemoapp/hybridConnectionNamespaces/demo-relay/relays/relay-demo-hc?api-version=2016-08-01 @hctest.json

## <a name="secure-your-hybrid-connections"></a>Proteja su Conexiones híbridas ##

Cualquier usuario que tenga permisos suficientes en la retransmisión de Azure Service Bus subyacente puede agregar una conexión híbrida existente a otros App Service Web Apps. Esto significa que, si debe evitar que otros usuarios puedan reutilizar la misma conexión híbrida (por ejemplo, cuando el recurso de destino es un servicio que no tiene ninguna medida de seguridad adicional para evitar el acceso no autorizado), debe bloquear el acceso a Azure Service Bus Relay.

Cualquier persona con `Reader` acceso a la Retransmisión podrá _ver_ la conexión híbrida al intentar agregarla a su aplicación web en Azure portal, pero no podrá _agregarla_ ya que carece de los permisos para recuperar la cadena de conexión que se usa para establecer la conexión de retransmisión. Para agregar correctamente la conexión híbrida, deben tener el permiso `listKeys` (`Microsoft.Relay/namespaces/hybridConnections/authorizationRules/listKeys/action`). El rol `Contributor` o cualquier otro rol que incluya este permiso en la Retransmisión permitirá a los usuarios usar la conexión híbrida y agregarla a su propia Web Apps.

## <a name="troubleshooting"></a>Solución de problemas ##

El estado "Conectado" significa que hay al menos una instancia de HCM configurada con esa conexión híbrida y es capaz de conectarse a Azure. Si en el estado de la conexión híbrida no indica **Conectado**, la conexión híbrida no está configurada en ningún HCM que tenga acceso a Azure.

La razón principal por la que los clientes no se pueden conectar a su punto de conexión es porque este se especificó mediante una dirección IP en lugar de un nombre DNS. Si la aplicación no puede acceder al punto de conexión deseado y ha usado una dirección IP, utilice un nombre DNS que sea válido en el host en el que se ejecuta HCM. Compruebe también que el nombre DNS se resuelve adecuadamente en el host donde se ejecuta el HCM. Confirme que existe conectividad desde el host donde se ejecuta el HCM al punto de conexión de la conexión híbrida.  

En App Service, es posible invocar la herramienta de la línea de comandos **tcpping** desde la consola de herramientas avanzadas (Kudu). Esta herramienta puede indicarle si tienen acceso a un punto de conexión TCP, pero no indica si tiene acceso a un punto de conexión híbrida. Cuando usa la herramienta de la consola en un punto de conexión de la conexión híbrida, simplemente confirma que usa una combinación de host:puerto.  

Si tiene un cliente de línea de comandos para el punto de conexión, puede probar la conectividad desde la consola de la aplicación. Por ejemplo, puede probar el acceso a los puntos de conexión de servidor web mediante cURL.

## <a name="biztalk-hybrid-connections"></a>Conexiones híbridas de BizTalk ##

La forma temprana de esta característica se llamó Conexiones híbridas de BizTalk. Esta funcionalidad llegó al final d su ciclo de vida el 31 de mayo de 2018 y haya dejado de funcionar. Las conexiones híbridas de BizTalk se han quitado de todas las aplicaciones y no se puede obtener acceso a ellas mediante el portal ni la API. Si aún tiene estas conexiones anteriores configuradas en el Administrador de conexiones híbridas, verá que tienen un estado Interrumpido y que en la parte inferior aparece una nota de final del ciclo de vida.

![Conexiones híbridas de BizTalk en el Administrador de conexiones híbridas][12]


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png
[11]: ./media/app-service-hybrid-connections/hybridconn-manual.png
[12]: ./media/app-service-hybrid-connections/hybridconn-bt.png

<!--Links-->
[HCService]: https://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: https://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/
