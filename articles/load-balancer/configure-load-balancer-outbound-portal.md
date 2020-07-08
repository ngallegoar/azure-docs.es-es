---
title: Configuración del equilibrio de carga y las reglas de salida con Azure Portal
titleSuffix: Azure Load Balancer
description: En este artículo se muestra cómo configurar el equilibrio de carga y las reglas de salida en Standard Load Balancer mediante Azure Portal.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: 2dff916bf005b307f27264ad7a17864fbba50872
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85367400"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-the-azure-portal"></a>Configuración del equilibrio de carga y las reglas de salida en Standard Load Balancer mediante Azure Portal

En este artículo se muestra cómo configurar las reglas de salida en Standard Load Balancer mediante Azure Portal.  

El recurso del equilibrador de carga contiene dos front-ends y las reglas asociadas. Tiene un front-end para el tráfico entrante y otro para el saliente.  

Cada front-end hace referencia a una dirección IP pública. En este escenario, la dirección IP pública para el tráfico entrante difiere de la del tráfico saliente.   La regla de equilibrio de carga solo proporciona equilibrio de carga de entrada. La regla de salida controla la traducción de direcciones de red (NAT) de salida de la máquina virtual.  

En este escenario se usan dos grupos de servidores back-end: uno para el tráfico entrante y otro para el saliente. Estos grupos muestran la funcionalidad y proporcionan flexibilidad para el escenario.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-load-balancer"></a>Creación de un equilibrador de carga

En esta sección, va a crear un equilibrador de carga que ayuda a equilibrar la carga de las máquinas virtuales. Puede crear un equilibrador de carga público o interno. Cuando se crea un equilibrador de carga público, se crea una nueva dirección IP pública que se configura como front-end para el equilibrador de carga. El front-end se denominará **LoadBalancerFrontEnd** de forma predeterminada.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Load Balancer**.
2. En la pestaña **Conceptos básicos** de la página **Crear equilibrador de carga**, escriba o seleccione la siguiente información:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Suscripción               | Seleccione su suscripción.    |    
    | Resource group         | Seleccione **Crear nuevo** y escriba **MyResourceGroupSLB** en el cuadro de texto.|
    | Nombre                   | **myLoadBalancer**                                   |
    | Region         | Seleccione **Oeste de Europa**.                                        |
    | Tipo          | Seleccione **Público**.                                        |
    | SKU           | Seleccione **Estándar**. |
    | Dirección IP pública | Seleccione **Crear nuevo**. Si tiene una dirección IP pública que le gustaría usar, seleccione **Utilizar existente**.  La dirección IP pública existente debe ser una SKU **estándar**.  Las direcciones IP públicas básicas no son compatibles con el equilibrador de carga de la SKU **estándar**.  |
    | Nombre de la dirección IP pública              | Escriba **myPublicIP** en el cuadro de texto.|
    | Zona de disponibilidad | Seleccione **Con redundancia de zona** para crear un equilibrador de carga resistente. Para crear una instancia de Load Balancer de zonas, seleccione una zona específica de 1, 2 o 3. |

3. En el resto de la configuración, acepte los valores predeterminados.
4. Seleccionar **Revisar y crear**.

    > [!IMPORTANT]
    > En el resto de esta guía de inicio rápido se supone que se elige la SKU **estándar** durante el proceso de selección de la SKU anterior.

5. En la pestaña **Revisar + crear**, seleccione **Crear**.   

    ![Crear un equilibrador de carga estándar](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Creación de recursos del equilibrador de carga

En esta sección se configura el equilibrador de carga para un grupo de direcciones de back-end y un sondeo de mantenimiento, y se especifica una regla del equilibrador de carga.

### <a name="create-a-backend-pool"></a>Creación de un grupo de back-end

Un grupo de direcciones de back-end contiene las direcciones IP de las NIC virtuales en el grupo de back-end. Cree el grupo de direcciones de back-end **myBackendPool** para incluir máquinas virtuales para el tráfico de Internet de equilibrio de carga.

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.
2. En **Configuración**, seleccione **Grupos de back-end** y, a continuación, seleccione **Agregar**.
3. En la página **Agregar un grupo back-end**, en nombre, escriba **myBackEndPool**, como el nombre del grupo de back-end y, a continuación, seleccione **Aceptar**.

### <a name="create-a-health-probe"></a>Creación de un sondeo de estado

Un sondeo de estado se usa para supervisar el estado de la aplicación. El sondeo de estado agrega o quita las máquinas virtuales del equilibrador de carga a partir de su respuesta a las comprobaciones de estado. Cree un sondeo de estado, **myHealthProbe**, para supervisar el estado de las máquinas virtuales.

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.
2. En **Configuración**, seleccione **Sondeos de estado** y, a continuación, seleccione **Agregar**.
    
    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba **myHealthProbe**. |
    | Protocolo | Seleccione **HTTP**. |
    | Port | Escriba **80**.|
    | Intervalo | Escriba **15** como número de **Intervalo**, en segundos, entre los intentos de sondeo. |
    | Umbral incorrecto | Seleccione **2** como número de **Umbral incorrecto** o errores de sondeo consecutivos que deben producirse para que una máquina virtual se considere que no funciona de manera correcta.|
    | | |
4. Seleccione **Aceptar**.

### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga
Las reglas de equilibrador de carga se utilizan para definir cómo se distribuye el tráfico a las máquinas virtuales. 

Defina lo siguiente:
 - La configuración de IP del front-end para el tráfico entrante.
 - El grupo de IP de back-end para recibir el tráfico.
 - Los puertos de origen y de destino requeridos. 

En la sección siguiente, creará lo siguiente:
 - La regla del equilibrador de carga **myHTTPRule** para escuchar en el puerto 80.
 - El front-end **LoadBalancerFrontEnd**.
 - E grupo de direcciones de back-end **myBackEndPool** también con el puerto 80. 

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.
2. En **Configuración**, seleccione **Reglas de equilibrio de carga** y, a continuación, seleccione **Agregar**.
3. Use estos valores para configurar la regla de equilibrio de carga:
    
    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba **myHTTPRule**. |
    | Protocolo | seleccione **TCP**. |
    | Port | Escriba **80**.|
    | Puerto back-end | Escriba **80**. |
    | Grupo back-end | Seleccione **MyBackendPool**.|
    | Sondeo de mantenimiento | Seleccione **myHealthProbe**. |
    | Creación de reglas de salida implícitas | así que seleccione **No**. Vamos a crear reglas de salida en una sección posterior con una dirección IP pública dedicada. |
4. Deje el resto de valores predeterminados y después seleccione **Aceptar**.

## <a name="create-outbound-rule-configuration"></a>Creación de la configuración de regla de salida
Las reglas de salida del equilibrador de carga configuran SNAT saliente para las máquinas virtuales del grupo de back-end. 

### <a name="create-an-outbound-public-ip-address-and-frontend"></a>Creación de un front-end y una dirección IP pública de salida

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.

2. En **Configuración**, seleccione **Configuración de direcciones IP de front-end** y seleccione **Agregar**.

3. Use estos valores para configurar la configuración de IP de front-end para la salida:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba **LoadBalancerFrontEndOutbound**. |
    | Versión de la dirección IP | Seleccione **IPv4**. |
    | Tipo de IP | Seleccione **Dirección IP**.|
    | Dirección IP pública | Seleccione **Crear nuevo**. En **Agregar una dirección IP pública**, escriba **myPublicIPOutbound**.  Seleccione **Aceptar**. |

4. Seleccione **Agregar**.

### <a name="create-an-outbound-backend-pool"></a>Creación de un grupo de back-end de salida

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.

2. En **Configuración**, seleccione **Grupos de back-end** y, a continuación, seleccione **Agregar**.

3. En la página **Agregar un grupo back-end**, en nombre, escriba **myBackendPoolOutbound**, como el nombre del grupo de back-end y, a continuación, seleccione **Aceptar**.

### <a name="create-outbound-rule"></a>Creación de una regla de salida

1. Seleccione **Todos los servicios** en el menú de la izquierda, **Todos los recursos** y, después, en la lista de recursos, **myLoadBalancer**.

2. En **Configuración**, seleccione **Reglas de salida** y, a continuación, seleccione **Agregar**.

3. Use estos valores para configurar las reglas de salida:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba **myOutboundRule**. |
    | Dirección IP del front-end | Seleccione **LoadBalancerFrontEndOutbound**. |
    | Tiempo de espera de inactividad (minutos) | Mueva el control deslizante a **15 minutos.|
    | Restablecimiento de TCP | Seleccione **Habilitado**.|
    | Grupo back-end | Seleccione **myBackendPoolOutbound**. |
    | Asignación de puertos: > Asignación de puertos | Seleccione **Elegir manualmente el número de puertos de salida**. |
    | Puertos de salida: > Elegir por | Seleccione **Puertos por instancia**. |
    | Puertos de salida -> Puertos por instancia | Escriba **10 000**. |

4. Seleccione **Agregar**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no los necesite, elimine el grupo de recursos, el equilibrador de carga y todos los recursos relacionados. Seleccione el grupo de recursos **myResourceGroupSLB** que contiene el equilibrador de carga y, a continuación, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este artículo:
 - Ha creado un equilibrador de carga estándar.
 - Se han configurado las reglas de tráfico de equilibrador de carga entrante y saliente.
 - Configuró un sondeo de estado para las máquinas virtuales del grupo de back-end. 

Para más información, continúe con los [tutoriales de Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md).
