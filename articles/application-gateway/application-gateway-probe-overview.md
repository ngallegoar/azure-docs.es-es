---
title: Introducción al seguimiento de estado para Azure Application Gateway
description: Azure Application Gateway supervisa el estado de todos los recursos de su grupo de back-end y elimina automáticamente del grupo aquellos que se considera que están en mal estado.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2020
ms.author: victorh
ms.openlocfilehash: c5a53167c6a4ca6c886b858a1608eaa173185bd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335859"
---
# <a name="application-gateway-health-monitoring-overview"></a>Información general sobre la supervisión de estado de la puerta de enlace de aplicaciones

La puerta de enlace de aplicaciones de Azure supervisa de forma predeterminada el estado de todos los recursos de su grupo de back-end y elimina automáticamente del grupo los recursos que se considera que están en mal estado. Además, continúa supervisando las instancias en mal estado y las agrega de nuevo al grupo de back-end en buen estado, una vez que están disponibles y responden a los sondeos de estado. Application Gateway envía los sondeos de mantenimiento con el mismo puerto que se define en la configuración de HTTP de back-end. Esta configuración garantiza que el sondeo prueba el mismo puerto que utilizarían los clientes para conectarse al back-end. 

La dirección IP de origen que usa Application Gateway para los sondeos de estado depende del grupo de back-end:
 
- Si el grupo de back-end es un punto de conexión público, la dirección de origen es la dirección IP pública de front-end de Application Gateway.
- Si el grupo de back-end es un punto de conexión privado, la dirección de origen proviene del espacio de direcciones IP privadas de la subred de Application Gateway.


![ejemplo de sondeo de Application Gateway][1]

Aparte del uso de la supervisión del sondeo de estado, también puede personalizar el sondeo de estado para adaptarlo a las necesidades de su aplicación. En este artículo trataremos ambos sondeos de estado: el personalizado y el predeterminado.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-health-probe"></a>Sondeo de estado predeterminado

Una puerta de enlace de aplicaciones configura automáticamente un sondeo de estado predeterminado cuando no hay ningún sondeo personalizado configurado. El comportamiento de supervisión consiste en realizar una solicitud HTTP a las direcciones IP configuradas para el grupo de back-end. En el caso de los sondeos predeterminados si las opciones de HTTP del back-end se configuran para HTTPS, el sondeo usa HTTPS también para comprobar el mantenimiento de los back-ends.

Por ejemplo: Configure la puerta de enlace de aplicaciones para usar los servidores back-end A, B y C para recibir el tráfico de red HTTP en el puerto 80. La supervisión de estado predeterminada comprueba los tres servidores cada 30 segundos para ver que la respuesta de HTTP es correcta. Una respuesta HTTP correcta tiene un [código de estado](https://msdn.microsoft.com/library/aa287675.aspx) entre 200 y 399.

Si la comprobación de sondeo predeterminado da error en el servidor A, la puerta de enlace de aplicaciones lo elimina de su grupo de back-end y el tráfico de red deja de fluir a este servidor. El sondeo predeterminado sigue comprobando el servidor A cada 30 segundos. Cuando el servidor A responde correctamente a una solicitud de un sondeo de estado predeterminado, se considera que está en buen estado y se agrega de nuevo al grupo de back-end; el tráfico comienza a fluir de nuevo a él.

### <a name="probe-matching"></a>Sondeo de búsqueda de coincidencia

De forma predeterminada, una respuesta HTTP (S) con el código de estado entre 200 y 399 se considera correcta. Los sondeos de estado personalizados admiten además dos criterios de coincidencia. Los criterios de coincidencia pueden usarse para modificar opcionalmente la interpretación predeterminada de lo que constituye una respuesta correcta.

Estos son los criterios de coincidencia: 

- **Coincidencia de código de estado de respuesta HTTP**: criterio de coincidencia de sondeo para aceptar el código de respuesta, o los intervalos de códigos de respuesta, HTTP especificados por el usuario. Se admiten códigos de estado de respuesta individuales, o un intervalo de códigos de estado de respuesta, separados por coma.
- **Coincidencia de cuerpo de respuesta HTTP**: criterio de coincidencia de sondeo que examina el cuerpo de la respuesta HTTP y busca la coincidencia con una cadena especificada por el usuario. La coincidencia solo busca la existencia de la cadena especificada por el usuario en el cuerpo de la respuesta, no es una coincidencia de expresión regular completa.

Los criterios de coincidencia se pueden especificar mediante el cmdlet `New-AzApplicationGatewayProbeHealthResponseMatch`.

Por ejemplo:

```azurepowershell
$match = New-AzApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399
$match = New-AzApplicationGatewayProbeHealthResponseMatch -Body "Healthy"
```
Una vez que se especifican los criterios de coincidencia, se pueden asociar a la configuración de sondeo con un parámetro `-Match` en PowerShell.

### <a name="default-health-probe-settings"></a>Configuración de sondeo de estado predeterminado

| Propiedad de sondeo | Value | Descripción |
| --- | --- | --- |
| Dirección URL de sondeo |http://127.0.0.1:\<port\>/ |Ruta de acceso URL |
| Intervalo |30 |Cantidad de tiempo en segundos que se debe esperar antes de que se envíe el siguiente sondeo de estado.|
| Tiempo de espera |30 |Cantidad de tiempo en segundos que la puerta de enlace de la aplicación espera una respuesta de sondeo antes de marcar dicho sondeo como incorrecto. Si un sondeo devuelve un estado correcto, el back-end correspondiente se marca inmediatamente como correcto.|
| Umbral incorrecto |3 |Controla cuántos sondeos se van a enviar si se produce un error en el sondeo de estado normal. Estos sondeos de estado adicionales se envían en sucesión rápida para determinar el estado del back-end rápidamente y no esperar al intervalo de sondeo. Este comportamiento es solo en el caso de la SKU v1. En el caso de la SKU v2, los sondeos de estado esperan el intervalo. El servidor back-end se marca como inactivo después de que el número de errores de sondeo consecutivos alcanza el umbral incorrecto. |

> [!NOTE]
> El puerto es el mismo que la configuración de HTTP del back-end.

El sondeo predeterminado solo examina http:\///127.0.0.1:\<puerto\> para determinar el estado de mantenimiento. Si necesita configurar el sondeo de estado para ir a una dirección URL personalizada o modificar alguna otra configuración, debe usar sondeos personalizados.

### <a name="probe-intervals"></a>Intervalos de sondeo

Todas las instancias de Application Gateway sondean el back-end de manera independiente unas de otras. La misma configuración de sondeo se aplica a cada instancia de Application Gateway. Por ejemplo, si la configuración de sondeo es enviar sondeos de estado cada 30 segundos y Application Gateway tiene dos instancias, entonces ambas instancias envían el sondeo de estado cada 30 segundos.

También si hay varios agentes de escucha, de manera que cada uno de ellos sondea el back-end de manera independiente unos de otros. Por ejemplo, si existen dos agentes de escucha que apuntan al mismo grupo de back-ends en dos puertos diferentes (definidos por dos configuraciones http de back-end), entonces cada agente de escucha sondea el mismo back-end de forma independiente. En este caso, hay dos sondeos procedentes de cada instancia de Application Gateway para los dos agentes de escucha. Si hay dos instancias de la puerta de Application Gateway en este escenario, la máquina virtual de back-end vería cuatro sondeos por el intervalo de sondeo configurado.

## <a name="custom-health-probe"></a>Sondeo de estado personalizado

Los sondeos personalizados permiten un control más específico sobre la supervisión de estado. Cuando se usan sondeos personalizados, puede configurar el intervalo de sondeo, la dirección URL y la ruta de acceso a la comprobación y cuántas respuestas erróneas se aceptan antes de marcar la instancia del grupo de back-end como en mal estado.

### <a name="custom-health-probe-settings"></a>Configuración de sondeo de estado personalizado

La siguiente tabla proporciona definiciones de las propiedades de un sondeo de mantenimiento personalizado.

| Propiedad de sondeo | Descripción |
| --- | --- |
| Nombre |Nombre del sondeo. Este nombre se usa para hacer referencia al sondeo en la configuración de HTTP de back-end. |
| Protocolo |Protocolo usado para enviar el sondeo. El sondeo utiliza el protocolo definido en la configuración de HTTP del back-end. |
| Host |Nombre de host para enviar el sondeo. Solo se puede aplicar cuando se ha configurado un entorno multisitio en Application Gateway; de lo contrario hay que usar '127.0.0.1'. Este valor es distinto del nombre de host de máquina virtual. |
| Path |Ruta de acceso relativa del sondeo. La ruta de acceso válida se inicia desde '/'. |
| Intervalo |Intervalo de sondeo en segundos. Este valor es el intervalo de tiempo entre dos sondeos consecutivos. |
| Tiempo de espera |Tiempo de espera del sondeo en segundos. Si no se recibe una respuesta válida dentro del período de espera, el sondeo se marca como erróneo.  |
| Umbral incorrecto |Número de reintentos de sondeo. El servidor back-end se marca como inactivo después de que el número de errores de sondeo consecutivos alcanza el umbral incorrecto. |

> [!IMPORTANT]
> Si la instancia de Puerta de enlace de aplicaciones está configurada para un único sitio, de forma predeterminada, el nombre de host debe especificarse como 127.0.0.1, salvo que se configure de otra manera en la sonda personalizada.
> A modo de referencia se envía un sondeo personalizado a \<protocolo\>://\<host\>:\<puerto\>\<ruta de acceso\>. El puerto usado será el definido en la configuración de HTTP del back-end.

## <a name="nsg-considerations"></a>Consideraciones sobre NSG

Debe permitir el tráfico entrante de Internet en los puertos TCP 65503-65534 para la SKU de Application Gateway v1, y los puertos TCP 65200-65535 para la SKU de V2 con la subred de destino como **cualquiera** y origen como la etiqueta de servicio de **GatewayManager**. Este intervalo de puertos es necesario para la comunicación de la infraestructura de Azure.

Además, no se puede bloquear la conectividad saliente de Internet y se debe permitir el tráfico entrante desde la etiqueta **AzureLoadBalancer**.

Para más información, consulte [Introducción a la configuración de Application Gateway](configuration-overview.md#network-security-groups-on-the-application-gateway-subnet).

## <a name="next-steps"></a>Pasos siguientes
Tras conocer todo lo referente a la supervisión del mantenimiento de Application Gateway, puede configurar un [sondeo de mantenimiento personalizado](application-gateway-create-probe-portal.md) en Azure Portal o un [sondeo de mantenimiento personalizado](application-gateway-create-probe-ps.md) mediante PowerShell y el modelo de implementación con Azure Resource Manager.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
