---
title: Guía de rendimiento para Azure SignalR Service
description: Información general sobre el rendimiento y la prueba comparativa de Azure SignalR Service. Métricas clave que se deben tener en cuenta al planear la capacidad.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 68cad32be177fa20794399157fca89e87c2f8f59
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "74157670"
---
# <a name="performance-guide-for-azure-signalr-service"></a>Guía de rendimiento para Azure SignalR Service

Una de las principales ventajas de usar Azure SignalR Service es la facilidad de escalado de las aplicaciones de SignalR. En un escenario a gran escala, el rendimiento es un factor importante. 

En esta guía, presentaremos los factores que afectan al rendimiento de las aplicaciones de SignalR. Vamos a describir un rendimiento típico en distintos escenarios de casos de uso. Al final, presentaremos el entorno y las herramientas que puede usar para generar un informe de rendimiento.

## <a name="term-definitions"></a>Definiciones de términos

*Entrante*: el mensaje entrante a Azure SignalR Service.

*Salida*: el mensaje saliente de Azure SignalR Service.

*Ancho de banda*: el tamaño total de todos los mensajes en 1 segundo.

*Modo predeterminado*: el modo de funcionamiento predeterminado en el momento de creación de una instancia de Azure SignalR Service. Azure SignalR Service espera que el servidor de aplicaciones establezca una conexión con él antes de aceptar las conexiones de cliente.

*Modo sin servidor*: modo en que Azure SignalR Service acepta únicamente conexiones de cliente. No se permite ninguna conexión de servidor.

## <a name="overview"></a>Información general

Azure SignalR Service define siete niveles estándar para distintas capacidades de rendimiento. En esta guía se responden las preguntas siguientes:

-   ¿Cuál es el rendimiento típico de Azure SignalR Service para cada nivel?

-   ¿Azure SignalR Service cumple mis requisitos de rendimiento de mensajes (por ejemplo, envío de 100 000 mensajes por segundo)?

-   Para mi escenario concreto, ¿qué nivel es adecuado para mí? O bien, ¿cómo puedo seleccionar el nivel adecuado?

-   ¿Qué tipo de servidor de aplicaciones (tamaño de VM) es adecuado para mí? ¿Cuántos debo implementar?

Para responder estas preguntas, en esta guía primero se proporciona una explicación detallada de los factores que afectan al rendimiento. Luego, se muestra el número máximo de mensajes entrantes y salientes de cada nivel para casos de uso típicos: **eco**, **difusión**, **enviar al grupo** y **enviar a conexión** (chats punto a punto).

En esta guía no se pueden tratar todos los escenarios (ni casos de uso, tamaños de mensaje, patrones de envío de mensajes, etc. diferentes). Sin embargo, se proporcionan algunos métodos para ayudarle a hacer lo siguiente:

- Evaluar los requisitos aproximados de los mensajes entrantes o salientes.
- Encontrar los niveles adecuados mediante la comprobación de la tabla de rendimiento.

## <a name="performance-insight"></a>Conclusión sobre el rendimiento

En esta sección se describen las metodologías de evaluación del rendimiento y luego se enumeran todos los factores que afectan al rendimiento. Al final, se proporcionan métodos para ayudarle a evaluar los requisitos de rendimiento.

### <a name="methodology"></a>Metodología

El *rendimiento* y la *latencia* son dos aspectos típicos de la comprobación del rendimiento. Para Azure SignalR Service, cada nivel de SKU tiene su propia directiva de limitación de rendimiento. La directiva define *el rendimiento máximo permitido (ancho de banda entrante y saliente)* como el rendimiento máximo obtenido cuando el 99 % de los mensajes tiene una latencia menor que 1 segundo.

La latencia es el intervalo de tiempo entre que la conexión envía el mensaje hasta recibir el mensaje de respuesta de Azure SignalR Service. Miremos a **eco** como ejemplo. Cada conexión de cliente agrega una marca de tiempo en el mensaje. El centro del servidor de aplicaciones envía el mensaje original de nuevo al cliente. Por lo tanto, el retraso de propagación la calcula fácilmente cada conexión de cliente. La marca de tiempo se adjunta para todos los mensajes de **difusión**, **enviar al grupo** y **enviar a conexión**.

Para simular miles de conexiones de cliente simultáneas, se crean varias VM en una red privada virtual en Azure. Todas estas VM se conectan a la misma instancia de Azure SignalR Service.

En el modo predeterminado de Azure SignalR Service, las VM del servidor de aplicaciones se implementan en la misma red privada virtual como VM de cliente. Todas las Cm de cliente y VM de servidor de aplicaciones se implementan en la misma red de la misma región para evitar una latencia entre regiones.

### <a name="performance-factors"></a>Factores de rendimiento

En teoría, la capacidad de Azure SignalR Service se ve limitada por los recursos de cálculo: CPU, memoria y red. Por ejemplo, un mayor número de conexiones a Azure SignalR Service hace que el servicio use más memoria. Para mayor tráfico de mensajes (por ejemplo, cada mensaje es mayor que 2048 bytes), para procesar el tráfico Azure SignalR Service debe dedicar más ciclos de CPU. Mientras tanto, el ancho de banda de red de Azure también impone un límite para el tráfico máximo.

El tipo de transporte es otro factor que afecta al rendimiento. Los tres tipos son [WebSocket](https://en.wikipedia.org/wiki/WebSocket), [evento enviado del servidor](https://en.wikipedia.org/wiki/Server-sent_events) y [sondeo largo](https://en.wikipedia.org/wiki/Push_technology). 

WebSocket es un protocolo de comunicación bidireccional de dúplex completo sobre una única conexión TCP. El evento enviado del servidor es un protocolo unidireccional para insertar mensajes desde el servidor al cliente. El sondeo largo requiere que los clientes sondeen periódicamente la información del servidor a través de una solicitud HTTP. Para la misma API en las mismas condiciones, WebSocket tiene el mejor rendimiento, eventos enviados del servidor es más lento y el sondeo largo es el más lento de los tres. Para Azure SignalR Service, se recomienda usar WebSocket de forma predeterminada.

El costo de enrutamiento de mensajes también limita el rendimiento. Azure SignalR Service desempeña un papel como enrutador de mensajes, que enruta el mensaje desde un conjunto de clientes o servidores a otros clientes o servidores. Un escenario o API diferente requiere una directiva de enrutamiento diferente. 

Para **eco**, el cliente envía un mensaje a sí mismo y es él mismo el destino del enrutamiento. Este patrón tiene el menor costo de enrutamiento. Sin embargo, para **difusión**, **enviar al grupo** y **enviar a conexión**, Azure SignalR Service debe buscar las conexiones de destino a través de la estructura de datos internos distribuidos. Este procesamiento adicional usa más recursos de la CPU, memoria y ancho de banda de red. Como resultado, el rendimiento es más lento.

En el modo predeterminado, el servidor de aplicaciones también podría convertirse en un cuello de botella para determinados escenarios. El SDK de Azure SignalR tiene que invocar el centro y, al mismo tiempo, mantener una conexión dinámica con cada cliente mediante señales de latido.

En el modo sin servidor, el cliente envía un mensaje mediante HTTP Post, que es menos eficaz que WebSocket.

Otro factor es el protocolo: JSON y [MessagePack](https://msgpack.org/index.html). MessagePack es más pequeño y se entrega con más rapidez que JSON. Sin embargo, es posible que MessagePack no mejore el rendimiento. El rendimiento de Azure SignalR Service no es sensible a los protocolos porque no descodifica la carga del mensaje durante el reenvío de mensajes desde los clientes a los servidores o viceversa.

En resumen, los siguientes factores afectan a la capacidad de entrada y salida:

-   Nivel de SKU (CPU y memoria)

-   Número de conexiones

-   Tamaño del mensaje

-   Velocidad de envío de mensajes

-   Tipo de transporte (WebSocket, evento enviado de servidor o sondeo largo)

-   Escenario de caso de uso (costo de enrutamiento)

-   Conexiones de servidor de aplicaciones y servicio (en modo de servidor)


### <a name="finding-a-proper-sku"></a>Encontrar una SKU adecuada

¿Cómo se puede evaluar la capacidad de entrada y salida o buscar el nivel adecuado para un caso de uso específico?

Suponga que el servidor de aplicaciones es lo suficientemente eficaz y no es el cuello de botella del rendimiento. Luego, compruebe el ancho de banda entrante y saliente máximo para cada nivel.

#### <a name="quick-evaluation"></a>Evaluación rápida

Simplifiquemos la evaluación primero al suponer algunos valores de configuración predeterminados: 

- El tipo de transporte es WebSocket.
- El tamaño del mensaje es de 2048 bytes.
- Se envía un mensaje cada segundo.
- Azure SignalR Service está en el modo predeterminado.

Cada nivel tiene su propio ancho de banda entrante y saliente máximo. No se garantiza una experiencia de usuario agilizada cuando la conexión de entrada o salida supera el límite.

**Eco** ofrece el ancho de banda entrante máximo porque tiene el menor costo de enrutamiento. **Difusión** define el ancho de banda de mensaje saliente máximo.

*No* se deben superan los valores resaltados en las dos tablas siguientes.

|       Eco                        | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones                       | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| **Ancho de banda entrante** | **2 MBps**    | **4 MBps**    | **10 MBps**   | **20 MBps**    | **40 MBps**    | **100 MBps**   | **200 MBps**    |
| Ancho de banda saliente | 2 MBps   | 4 MBps   | 10 MBps  | 20 MBps   | 40 MBps   | 100 MBps  | 200 MBps   |


|     Difusión             | Unidad1 | Unidad2 | Unidad5  | Unidad10 | Unidad20 | Unidad50  | Unidad100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1,000 | 2\.000 | 5\.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Ancho de banda entrante  | 4 Kbps   | 4 Kbps   | 4 Kbps    | 4 Kbps    | 4 Kbps    | 4 Kbps     | 4 Kbps    |
| **Ancho de banda saliente** | **4 MBps**    | **8 MBps**    | **20 MBps**    | **40 MBps**    | **80 MBps**    | **200 MBps**    | **400 MBps**   |

*Ancho de banda entrante* y *Ancho de banda saliente* son el tamaño total de mensajes por segundo.  Estas son las fórmulas para ellos:
```
  inboundBandwidth = inboundConnections * messageSize / sendInterval
  outboundBandwidth = outboundConnections * messageSize / sendInterval
```

- *inboundConnections*: el número de conexiones que envían el mensaje.

- *outboundConnections*: el número de conexiones que reciben el mensaje.

- *messageSize*: el tamaño de un único mensaje (valor medio). Un mensaje pequeño de menos de 1024 bytes tiene un impacto sobre el rendimiento que es similar a un mensaje de 1024 bytes.

- *sendInterval*: el tiempo de envío de un mensaje. Normalmente es 1 segundo por mensaje, lo que implica enviar un mensaje cada segundo. Un intervalo más pequeño significa enviar más mensajes en un período de tiempo. Por ejemplo, 0,5 segundos por cada mensaje significa enviar dos mensajes cada segundo.

- *Conexiones*: el umbral máximo confirmado de Azure SignalR Service para cada nivel. Si el número de conexiones aumenta aún más, se verá afectado por la limitación de la conexión.

#### <a name="evaluation-for-complex-use-cases"></a>Evaluación para casos de uso complejos

##### <a name="bigger-message-size-or-different-sending-rate"></a>Mayor tamaño de mensaje o velocidad de envío diferente

El caso de uso real es más complicado. Se podría enviar un mensaje de más de 2048 bytes o quizás la velocidad de envío de mensajes no sea un mensaje por segundo. Tomemos como ejemplo la difusión de Unidad100 para ver cómo evaluar su rendimiento.

En la tabla siguiente se muestra un caso de uso real de **difusión**. Sin embargo, el tamaño de mensaje, número de conexiones y velocidad de envío de mensajes son diferentes de lo que se supone que en la sección anterior. La cuestión es cómo podemos deducir alguno de esos elementos (tamaño de mensaje, número de conexiones o velocidad de envío de mensaje) si conocemos solo dos de ellos.

| Difusión  | Tamaño del mensaje | Mensajes entrantes por segundo | Conexiones | Intervalos de envío |
|---|---------------------|--------------------------|-------------|-------------------------|
| 1 | 20 KB                | 1                        | 100 000     | 5 segundos                      |
| 2 | 256 KB               | 1                        | 8,000       | 5 segundos                      |

La siguiente fórmula es fácil de deducir basándose en la fórmula anterior:

```
outboundConnections = outboundBandwidth * sendInterval / messageSize
```

Para Unidad100, el ancho de banda saliente máximo es 400 MB de la tabla anterior. Para un tamaño de mensaje de 20 KB, el número máximo de conexiones de salida debe ser 400 MB \* 5 / 20 KB = 100 000, que coincide con el valor real.

##### <a name="mixed-use-cases"></a>Casos de uso mixtos

En el caso de uso real, se suele mezclar los cuatro casos de uso básicos: **eco**, **difusión**, **enviar al grupo** y **enviar a conexión**. La metodología que se usa para evaluar la capacidad es la siguiente:

1. Dividir los casos de uso mixtos en cuatro casos de uso básicos.
1. Calcular el ancho de banda máximo de mensajes entrantes y salientes con las fórmulas anteriores por separado.
1. Sumar los cálculos de ancho de banda para obtener el total ancho de banda entrante y saliente máximo. 

Luego, elegir el nivel adecuado de las tablas de ancho de banda entrante y saliente máximo.

> [!NOTE]
> Para enviar un mensaje a cientos o miles de grupos pequeños o para miles de clientes que envían un mensaje entre sí, el costo de enrutamiento será un elemento dominante. Debe tener en cuenta este impacto.

Para el caso de uso de enviar un mensaje a los clientes, asegúrese de que el servidor de aplicaciones *no* sea el cuello de botella. En la siguiente sección "Caso práctico" se proporcionan directrices sobre cuántos servidores de aplicaciones se necesitan y cuántas conexiones de servidor se deben configurar.

## <a name="case-study"></a>Caso práctico

En las siguientes secciones se describen cuatro casos de uso típicos de transporte de WebSocket: **eco**, **difusión**, **enviar al grupo** y **enviar a conexión**. Para cada escenario, en la sección se muestra la capacidad actual de entrada y salida para Azure SignalR Service. También se explican los principales factores que afectan al rendimiento.

En el modo predeterminado, el servidor de aplicaciones crea cinco conexiones de servidor con Azure SignalR Service. El servidor de aplicaciones usa el SDK de Azure SignalR Service de forma predeterminada. En los siguientes resultados de pruebas de rendimiento, las conexiones de servidor se aumentan a 15 (o más para la difusión y el envío de un mensaje a un grupo de gran tamaño).

Casos de uso diferentes tienen requisitos diferentes para los servidores de aplicaciones. **Difusión** necesita un número pequeño de servidores de aplicaciones. **Eco** o **enviar a conexión** necesita varios servidores de aplicación.

En todos los casos de uso, el tamaño predeterminado de mensaje es 2048 bytes y el intervalo de envío de mensajes es 1 segundo.

### <a name="default-mode"></a>Modo predeterminado

En el modo predeterminado, participan clientes, servidores de aplicaciones web y Azure SignalR Service. Cada cliente significa una sola conexión.

#### <a name="echo"></a>Eco

En primer lugar, una aplicación web se conecta a Azure SignalR Service. En segundo lugar, muchos clientes se conectan a la aplicación web, que redirige los clientes a Azure SignalR Service con el token de acceso y el punto de conexión. Luego, los clientes establecen conexiones WebSocket con Azure SignalR Service.

Cuando todos los clientes establezcan conexiones, comienzan a enviar un mensaje que contiene una marca de tiempo al centro concreto cada segundo. El centro envía de vuelta un mensaje de eco a su cliente original. Cada cliente calcula la latencia cuando recibe el mensaje de eco de vuelta.

En el siguiente diagrama, 5 a 8 (tráfico resaltado en rojo) se encuentran en un bucle. El bucle se ejecuta durante un tiempo predeterminado (5 minutos) y obtiene la estadística de la latencia de todos los mensajes.

![Tráfico para el caso de uso de eco](./media/signalr-concept-performance/echo.png)

El comportamiento de **eco** determina que el ancho de banda entrante máximo es igual que el ancho de banda saliente máximo. Consulte la tabla siguiente para más detalles:

|       Eco                        | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|-----------------------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones                       | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Mensajes entrantes y salientes por segundo | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Ancho de banda entrante y saliente | 2 MBps   | 4 MBps   | 10 MBps  | 20 MBps   | 40 MBps   | 100 MBps  | 200 MBps   |

En este caso de uso, cada cliente invoca el centro definido en el servidor de aplicaciones. El centro simplemente llama al método definido en el cliente original. Este centro es el más ligero para **eco**.

```
        public void Echo(IDictionary<string, object> data)
        {
            Clients.Client(Context.ConnectionId).SendAsync("RecordLatency", data);
        }
```

Incluso para este centro sencillo, la presión del tráfico en el servidor de aplicaciones se destaca a medida que aumenta la carga de mensajes entrantes **eco**. Esta presión de tráfico requiere muchos servidores de aplicaciones para niveles de SKU de gran tamaño. En la tabla siguiente se muestra el número de servidores de aplicaciones para cada nivel.


|    Eco          | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicaciones | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> El número de conexiones de cliente, tamaño de mensaje, velocidad de envío de mensajes, nivel de SKU y CPU o memoria del servidor de aplicaciones afectan al rendimiento global de **eco**.

#### <a name="broadcast"></a>Difusión

Para **difusión**, cuando la aplicación web recibe el mensaje, lo difunde a todos los clientes. Cuantos más clientes haya para difundir, más tráfico de mensajes habrá a todos los clientes. Consulte el diagrama siguiente:

![Tráfico para el caso de uso de difusión](./media/signalr-concept-performance/broadcast.png)

Un número pequeño de clientes difunden. El ancho de banda de mensajes entrante es pequeño, pero el ancho de banda saliente es enorme. El ancho de banda de mensajes saliente aumenta a medida que aumenta la velocidad de conexión o difusión de cliente.

En la tabla siguiente se proporciona un resumen de las conexiones de cliente, número de mensajes entrantes y salientes, y ancho de banda máximos.

|     Difusión             | Unidad1 | Unidad2 | Unidad5  | Unidad10 | Unidad20 | Unidad50  | Unidad100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1,000 | 2\.000 | 5\.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Mensajes entrantes por segundo  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Mensajes salientes por segundo | 2\.000 | 4\.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda entrante  | 4 Kbps   | 4 Kbps   | 4 Kbps    | 4 Kbps    | 4 Kbps    | 4 Kbps     | 4 Kbps     |
| Ancho de banda saliente | 4 MBps   | 8 MBps   | 20 MBps   | 40 MBps   | 80 MBps   | 200 MBps   | 400 MBps   |

Los clientes de difusión que publican mensajes son no más que cuatro. Necesitan menos servidores de aplicaciones en comparación con **eco** porque la cantidad de mensajes entrantes es pequeña. Dos servidores de aplicaciones son suficientes en cuanto al contrato de nivel de servicio y el rendimiento. Sin embargo, debería incrementar las conexiones de servidor predeterminadas para evitar provocar un desequilibrio, especialmente para Unidad50 y Unidad100.

|   Difusión      | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicaciones | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Aumente las conexiones de servidor predeterminadas de 5 a 40 en cada servidor de aplicaciones para evitar posibles conexiones de servidor desequilibradas a Azure SignalR Service.
>
> El número de conexiones de cliente, tamaño de mensaje, velocidad de envío de mensajes y nivel de SKU afectan al rendimiento global para **difusión**.

#### <a name="send-to-group"></a>Enviar al grupo

El caso de uso de **enviar al grupo** tiene un patrón de tráfico similar a **difusión**. La diferencia es que después de que los clientes establezcan conexiones WebSocket a Azure SignalR Service, deben unirse a grupos antes de poder enviar un mensaje a un grupo específico. En el diagrama siguiente se muestra este flujo de tráfico.

![Tráfico para el caso de uso de enviar a grupo](./media/signalr-concept-performance/sendtogroup.png)

Los miembros de grupo y el número de grupos son dos factores que afectan al rendimiento. Para simplificar el análisis, se definen dos tipos de grupos:

- **Grupo pequeño**: cada grupo tiene 10 conexiones. El número de grupos es igual a (número máximo de conexiones) / 10. Por ejemplo, para Unidad1, si hay 1000 números de conexión, tenemos 1000 / 10 = 100 grupos.

- **Grupo grande**: el número de grupos es siempre 10. El número de miembros del grupo es igual a (número máximo de conexiones) / 10. Por ejemplo, para Unidad1, si hay 1000 números de conexión, cada grupo tiene 1000 / 10 = 100 miembros.

**Enviar al grupo** agrega un costo de enrutamiento a Azure SignalR Service porque debe buscar las conexiones de destino a través de una estructura de datos distribuidos. A medida que aumentan las conexiones de envío, aumenta el coste.

##### <a name="small-group"></a>Grupo pequeño

El coste de enrutamiento es importante para enviar mensajes a muchos grupos pequeños. Actualmente, la implementación de Azure SignalR Service alcanza el límite de costes de enrutamiento en Unidad50. La adición de más CPU y memoria no ayuda, por lo que, por diseño, Unidad100 no puede mejorar más. Si necesita más ancho de banda entrante, póngase en contacto con el soporte técnico.

|   Enviar a un grupo pequeño     | Unidad1 | Unidad2 | Unidad5  | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|---------------------------|-------|-------|--------|--------|--------|--------|---------|
| Conexiones               | 1,000 | 2\.000 | 5\.000  | 10 000 | 20.000 | 50.000 | 100 000
| Número de miembros del grupo        | 10    | 10    | 10     | 10     | 10     | 10     | 10 
| Número de grupos               | 100   | 200   | 500    | 1,000  | 2\.000  | 5\.000  | 10 000 
| Mensajes entrantes por segundo  | 200   | 400   | 1,000  | 2500  | 4\.000  | 7000  | 7000   |
| Ancho de banda entrante  | 400 Kbps  | 800 Kbps  | 2 MBps     | 5 MBps     | 8 MBps     | 14 Mbps    | 14 Mbps     |
| Mensajes salientes por segundo | 2\.000 | 4\.000 | 10 000 | 25 000 | 40.000 | 70 000 | 70 000  |
| Ancho de banda saliente | 4 MBps    | 8 MBps    | 20 MBps    | 50 Mbps     | 80 MBps    | 140 MBps   | 140 MBps    |

Muchas de conexiones de cliente llaman al centro, por lo que el número de servidores aplicaciones también es fundamental para el rendimiento. En la tabla siguiente se enumeran los números de servidores de aplicaciones sugeridos.

|  Enviar a un grupo pequeño   | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicaciones | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> El número de conexiones de cliente, tamaño de mensaje, velocidad de envío de mensajes, coste de enrutamiento, nivel de SKU y CPU o memoria del servidor de aplicaciones afectan al rendimiento global de **enviar a grupo pequeño**.

##### <a name="big-group"></a>Grupo grande

Para **enviar a grupo grande**, el ancho de banda saliente se convierte en el cuello de botella antes de alcanzar el límite de coste de enrutamiento. En la tabla siguiente se muestra el ancho de banda saliente máximo, que es prácticamente el mismo que para **difusión**.

|    Enviar a grupo grande      | Unidad1 | Unidad2 | Unidad5  | Unidad10 | Unidad20 | Unidad50  | Unidad100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1,000 | 2\.000 | 5\.000  | 10 000 | 20.000 | 50.000  | 100 000
| Número de miembros del grupo        | 100   | 200   | 500    | 1,000  | 2\.000  | 5\.000   | 10 000 
| Número de grupos               | 10    | 10    | 10     | 10     | 10     | 10      | 10
| Mensajes entrantes por segundo  | 20    | 20    | 20     | 20     | 20     | 20      | 20      |
| Ancho de banda entrante  | 80 Kbps   | 40 KBps   | 40 KBps    | 20 Kbps    | 40 KBps    | 40 KBps     | 40 KBps     |
| Mensajes salientes por segundo | 2\.000 | 4\.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda saliente | 8 MBps    | 8 MBps    | 20 MBps    | 40 MBps    | 80 MBps    | 200 MBps    | 400 MBps    |

El número de conexiones de envío 40 como máximo. La carga en el servidor de aplicaciones es pequeña, por lo que el número de aplicaciones web sugerido es pequeño.

|  Enviar a grupo grande  | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicaciones | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

> [!NOTE]
> Aumente las conexiones de servidor predeterminadas de 5 a 40 en cada servidor de aplicaciones para evitar posibles conexiones de servidor desequilibradas a Azure SignalR Service.
> 
> El número de conexiones de cliente, tamaño de mensaje, velocidad de envío de mensajes, coste de enrutamiento y nivel de SKU afectan al rendimiento global de **enviar a grupo grande**.

#### <a name="send-to-connection"></a>Enviar a conexión

En el caso de uso de **enviar a conexión**, cuando los clientes establecen las conexiones a Azure SignalR Service, cada cliente llama a un centro especial para obtener su propio identificador de conexión. El banco de pruebas de rendimiento recopila todos los identificadores de conexión, los pone en orden aleatorio y los reasigna a todos los clientes como un destino de envío. Los clientes siguen enviando el mensaje a la conexión de destino hasta que finalice la prueba de rendimiento.

![Tráfico para el caso de uso de enviar al cliente](./media/signalr-concept-performance/sendtoclient.png)

El coste de enrutamiento para **enviar a conexión** es similar al coste de **enviar a grupo pequeño**.

A medida que aumenta el número de conexiones, el coste de enrutamiento limita el rendimiento general. Unidad50 ha alcanzado el límite. Como resultado, Unidad 100 no se puede mejorar más.

En la tabla siguiente se proporciona un resumen estadístico después de varias etapas de ejecución del banco de pruebas **enviar a conexión**.

|   Enviar a conexión   | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50          | Unidad100         |
|------------------------------------|-------|-------|-------|--------|--------|-----------------|-----------------|
| Conexiones                        | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000          | 100 000         |
| Mensajes entrantes y salientes por segundo | 1,000 | 2\.000 | 5\.000 | 8,000  | 9000  | 20.000 | 20.000 |
| Ancho de banda entrante y saliente | 2 MBps    | 4 MBps    | 10 MBps   | 16 MBps    | 18 Mbps    | 40 MBps       | 40 MBps       |

En este caso de uso se requiere una carga elevada en el servidor de aplicaciones. Consulta en la tabla siguiente el número de servidores de aplicaciones sugerido.

|  Enviar a conexión  | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicaciones | 2     | 2     | 2     | 3      | 3      | 10     | 20      |

> [!NOTE]
> El número de conexiones de cliente, tamaño de mensaje, velocidad de envío de mensajes, coste de enrutamiento, nivel de SKU y CPU o memoria del servidor de aplicaciones afectan al rendimiento global de **enviar a conexión**.

#### <a name="aspnet-signalr-echo-broadcast-and-send-to-small-group"></a>Eco, difusión y enviar a grupo pequeño de ASP.NET SignalR

Azure SignalR Service proporciona la misma capacidad de rendimiento para ASP.NET SignalR. 

En la prueba de rendimiento se usa Azure Web Apps desde el [plan de servicio estándar S3](https://azure.microsoft.com/pricing/details/app-service/windows/) para ASP.NET SignalR.

En la tabla siguiente se proporciona el número de aplicaciones web sugerido para **eco** de ASP.NET SignalR.

|   Eco           | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicaciones | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

En la tabla siguiente se proporciona el número de aplicaciones web sugerido para **difusión** de ASP.NET SignalR.

|  Difusión       | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicaciones | 2     | 2     | 2     | 2      | 2      | 2      | 2       |

En la tabla siguiente se proporciona el número de aplicaciones web sugerido para **enviar a grupo pequeño** de ASP.NET SignalR.

|  Enviar a un grupo pequeño     | Unidad1 | Unidad2 | Unidad5 | Unidad10 | Unidad20 | Unidad50 | Unidad100 |
|------------------|-------|-------|-------|--------|--------|--------|---------|
| Conexiones      | 1,000 | 2\.000 | 5\.000 | 10 000 | 20.000 | 50.000 | 100 000 |
| Número de servidores de aplicaciones | 2     | 2     | 4     | 4      | 8      | 32      | 40       |

### <a name="serverless-mode"></a>Modo sin servidor

Los clientes y Azure SignalR Service participan en el modo sin servidor. Cada cliente significa una sola conexión. El cliente envía mensajes a través de la API de REST a otro cliente o mensajes de difusión a todos.

Enviar mensajes de alta densidad a través de la API de REST es menos eficaz que usar WebSocket. Requiere la creación de una nueva conexión HTTP cada vez y eso implica un coste adicional en modo sin servidor.

#### <a name="broadcast-through-rest-api"></a>Difusión a través de la API de REST
Todos los clientes establecen conexiones WebSocket con Azure SignalR Service. Luego, algunos clientes comienzan a difundir a través de la API de REST. El envío de mensajes (entrantes) se realiza a través de HTTP Post, que no es eficaz en comparación con WebSocket.

|   Difusión a través de la API de REST     | Unidad1 | Unidad2 | Unidad5  | Unidad10 | Unidad20 | Unidad50  | Unidad100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1,000 | 2\.000 | 5\.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Mensajes entrantes por segundo  | 2     | 2     | 2      | 2      | 2      | 2       | 2       |
| Mensajes salientes por segundo | 2\.000 | 4\.000 | 10 000 | 20.000 | 40.000 | 100 000 | 200 000 |
| Ancho de banda entrante  | 4 Kbps    | 4 Kbps    | 4 Kbps     | 4 Kbps     | 4 Kbps     | 4 Kbps      | 4 Kbps      |
| Ancho de banda saliente | 4 MBps    | 8 MBps    | 20 MBps    | 40 MBps    | 80 MBps    | 200 MBps    | 400 MBps    |

#### <a name="send-to-user-through-rest-api"></a>Enviar al usuario a través de la API de REST
El banco de pruebas asigna nombres de usuario a todos los clientes antes de estos empiecen a conectarse a Azure SignalR Service. Después de que los clientes establezcan conexiones de WebSocket con Azure SignalR Service, comienzan a enviar mensajes a otros usuarios a través de HTTP Post.

|   Enviar al usuario a través de la API de REST | Unidad1 | Unidad2 | Unidad5  | Unidad10 | Unidad20 | Unidad50  | Unidad100 |
|---------------------------|-------|-------|--------|--------|--------|---------|---------|
| Conexiones               | 1,000 | 2\.000 | 5\.000  | 10 000 | 20.000 | 50.000  | 100 000 |
| Mensajes entrantes por segundo  | 300   | 600   | 900    | 1300  | 2\.000  | 10 000  | 18 000  |
| Mensajes salientes por segundo | 300   | 600   | 900    | 1300  | 2\.000  | 10 000  | 18 000 |
| Ancho de banda entrante  | 600 KBps  | 1,2 MBps  | 1,8 MBps   | 2,6 MBps   | 4 MBps     | 10 MBps     | 36 MBps    |
| Ancho de banda saliente | 600 KBps  | 1,2 MBps  | 1,8 MBps   | 2,6 MBps   | 4 MBps     | 10 MBps     | 36 MBps    |

## <a name="performance-test-environments"></a>Entornos de prueba de rendimiento

Para todos los casos de uso mencionados anteriormente, las pruebas de rendimiento se llevaron a cabo en un entorno de Azure. Como máximo, usamos 50 máquinas virtuales cliente y 20 máquinas virtuales de servidor de aplicaciones. Estos son algunos detalles:

- Tamaño de VM cliente: DS2V2 estándar (2 vCPU, 7 G de memoria)

- Tamaño de VM de servidor de aplicaciones: F4sV2 estándar (4 vCPU, 8 G de memoria)

- Conexiones de servidor de Azure SDK de SignalR: 15

## <a name="performance-tools"></a>Herramientas de rendimiento

Puede encontrar herramientas de rendimiento para Azure SignalR Service en [GitHub](https://github.com/Azure/azure-signalr-bench/).

## <a name="next-steps"></a>Pasos siguientes

En este artículo, se obtuvo introducción al rendimiento de Azure SignalR Service en escenarios de caso de uso típicos.

Para obtener detalles sobre los aspectos internos del servicio y el escalado, consulte las siguientes guías:

* [Aspectos internos de Azure SignalR Service](signalr-concept-internals.md)
* [Escalado de Azure SignalR Service](signalr-howto-scale-multi-instances.md)
