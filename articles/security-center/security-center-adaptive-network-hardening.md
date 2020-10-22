---
title: Protección de red adaptable en Azure Security Center | Microsoft Docs
description: Obtenga información sobre cómo usar patrones de tráfico reales para proteger las reglas de los grupos de seguridad de red (NSG) y mejorar aún más la posición de seguridad.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 09d62d23-ab32-41f0-a5cf-8d80578181dd
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/11/2020
ms.author: memildin
ms.openlocfilehash: e2b17e15c5548b4c9b93a62a7d4dfe62ff44404c
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92341760"
---
# <a name="adaptive-network-hardening-in-azure-security-center"></a>Protección de red adaptable en Azure Security Center
Obtenga información sobre cómo configurar la protección de red adaptable en Security Center.

## <a name="availability"></a>Disponibilidad
|Aspecto|Detalles|
|----|:----|
|Estado de la versión:|Disponible con carácter general|
|Precios:|Requiere [Azure Defender para servidores](defender-for-servers-introduction.md).|
|Roles y permisos necesarios:|Permisos de escritura en los NSG de la máquina|
|Nubes:|![Sí](./media/icons/yes-icon.png) Nubes comerciales<br>![No](./media/icons/no-icon.png) Nacionales o soberanas (US Gov, China Gov, otros gobiernos)|
|||

## <a name="what-is-adaptive-network-hardening"></a>¿Qué es la protección de red adaptable?
La aplicación de [grupos de seguridad de red (NSG)](../virtual-network/network-security-groups-overview.md) para filtrar el tráfico hacia y desde los recursos mejora la postura de seguridad de red. Sin embargo, aún puede haber algunos casos en los que el tráfico real que fluye a través del NSG es un subconjunto de las reglas de NSG definidas. En estos casos, puede mejorar la postura de seguridad al proteger aún más las reglas de NSG, según los patrones de tráfico real.

La protección de red adaptable proporciona recomendaciones para proteger mejor las reglas de NSG. Usa un algoritmo de aprendizaje automático que tiene en cuenta el tráfico real, la configuración de confianza conocida, la inteligencia de amenazas y otros indicadores de riesgo, y luego proporciona recomendaciones para permitir el tráfico solo desde tuplas IP y puerto específicas.

Por ejemplo, supongamos que la regla de NSG existente es permitir el tráfico desde 140.20.30.10/24 en el puerto 22. En función del análisis del tráfico, la protección de red adaptable podría recomendar la limitación del intervalo para permitir el tráfico desde 140.23.30.10/29 y denegar todo el tráfico restante en ese puerto.

>[!Note]
> Las recomendaciones de la protección de red adaptable solo se admiten en los siguientes puertos (para UDP y TCP): 13, 17, 19, 22, 23, 53, 69, 81, 111, 119, 123, 135, 137, 138, 139, 161, 162, 389, 445, 512, 514, 593, 636, 873, 1433, 1434, 1900, 2049, 2301, 2323, 2381, 3268, 3306, 3389, 4333, 5353, 5432, 5555, 5800, 5900, 5900, 5985, 5986, 6379, 6379, 7000, 7001, 7199, 8081, 8089, 8545, 9042, 9160, 9300, 11211, 16379, 26379, 27017 y 37215.


## <a name="view-and-manage-hardening-alerts-and-rules"></a>Visualización y administración de alertas y reglas de protección

1. En el menú de Security Center, abra el panel de **Azure Defender** y seleccione el icono de protección de red adaptable (1) o el elemento del panel de conclusiones relacionado con la protección de red adaptable (2). 

    :::image type="content" source="./media/security-center-adaptive-network-hardening/traffic-hardening.png" alt-text="Acceso a las herramientas de protección de red adaptable" lightbox="./media/security-center-adaptive-network-hardening/traffic-hardening.png":::

    > [!TIP]
    > En el panel de conclusiones se muestra el porcentaje de las máquinas virtuales que se defienden actualmente con la protección de red adaptable. 

1. La página de detalles de la recomendación **Las recomendaciones de protección de redes adaptables se deben aplicar en las máquinas virtuales orientadas a Internet** se abre con las máquinas virtuales de red agrupadas en tres pestañas:
   * **Recursos con estado incorrecto** : VM que tienen actualmente recomendaciones y alertas que se desencadenaron mediante la ejecución del algoritmo de protección de red adaptable. 
   * **Recursos con estado correcto**: VM sin alertas ni recomendaciones.
   * **Recursos sin analizar** : VM en las que no se puede ejecutar el algoritmo de protección de red adaptable debido a uno de los motivos siguientes:
      * **Las VM son VM clásicas**: solo se admiten VM de Azure Resource Manager.
      * **No hay suficientes datos disponibles**: para generar recomendaciones de protección de tráfico precisas, Security Center requiere al menos 30 días de datos de tráfico.
      * **La VM no está protegida por Azure Defender**: Solo las máquinas virtuales protegidas con [Azure Defender para servidores](defender-for-servers-introduction.md) son válidas para esta característica.

    :::image type="content" source="./media/security-center-adaptive-network-hardening/recommendation-details-page.png" alt-text="Acceso a las herramientas de protección de red adaptable":::

1. En la pestaña **Recursos con estado incorrecto**, seleccione una VM para ver sus alertas y las reglas de protección recomendadas que se deben aplicar.

    - En la pestaña **Reglas** se enumeran las reglas que recomiendan la protección de red adaptable que agregue.
    - En la pestaña **Alertas** se enumeran las alertas que se generaron debido al tráfico, que fluye al recurso y que no está dentro del intervalo de IP permitidas en las reglas recomendadas.

1. Si lo desea, edite las reglas:

    - [Modificación de una regla](#modify-rule)
    - [Eliminar una regla](#delete-rule) 
    - [Adición de una regla](#add-rule)

3. Seleccione las reglas que desea aplicar en el NSG y haga clic en **Aplicar**.

    > [!TIP]
    > Si los intervalos de direcciones IP de origen permitidos se muestran como "Ninguno", significa que la regla recomendada es una regla de *denegación*; de lo contrario, es una regla de *permiso*.

    :::image type="content" source="./media/security-center-adaptive-network-hardening/hardening-alerts.png" alt-text="Acceso a las herramientas de protección de red adaptable":::

      > [!NOTE]
      > Las reglas aplicadas se agregan a los NSG que protegen la VM. (Una VM podría estar protegida por un NSG asociado a su NIC, la subred en la que reside la VM o ambos).

### <a name="modify-a-rule"></a>Modificación de una regla <a name ="modify-rule"> </a>

Es posible modificar los parámetros de una regla que se haya recomendado. Por ejemplo, quizás querrá cambiar los intervalos de IP recomendados.

Instrucciones importantes a la hora de modificar una regla de protección de red adaptable:

* Solo puede modificar los parámetros de las reglas de tipo "permitir". 
* No puede cambiar reglas de tipo "permitir" para convertirlas en reglas de tipo "denegar". 

  > [!NOTE]
  > La creación y modificación de reglas de tipo "denegar" se realiza directamente en el NSG. Para más información, consulte [Creación, modificación o eliminación de un grupo de seguridad de red ](../virtual-network/manage-network-security-group.md).

* Una regla de tipo **Denegar todo el tráfico** es el único tipo de regla "denegar" que figuraría aquí, y no se puede modificar. Sin embargo, puede eliminar la regla (consulte [Eliminación de una regla](#delete-rule)).
  > [!NOTE]
  > Una regla tipo **Denegar todo el tráfico** se recomienda cuando, como resultado de la ejecución del algoritmo, Security Center no identifica el tráfico que se debe permitir, según la configuración de NSG existente. Por lo tanto, la regla recomendada es denegar todo el tráfico al puerto especificado. El nombre de este tipo de regla se muestra como "*Generada por el sistema*". Después de aplicar esta regla, su nombre real en el NSG será una cadena compuesta por el protocolo, la dirección del tráfico, "DENY" y un número aleatorio.

*Para modificar una regla de protección de red adaptable:*

1. Para modificar algunos de los parámetros de una regla, en la pestaña **Reglas**, haga clic en los puntos suspensivos (…) al final de la fila de la regla y seleccione **Editar**.

   ![Edición de reglas](./media/security-center-adaptive-network-hardening/edit-hard-rule.png)

1. En la ventana **Editar regla**, actualice los detalles que desea cambiar y haga clic en **Guardar**.

   > [!NOTE]
   > Después de hacer clic **Guardar**, habrá cambiado la regla correctamente. *Sin embargo, no la ha aplicado al NSG.* Para ello, debe seleccionar la regla de la lista y seleccionar en **Exigir** (tal como se explica en el paso siguiente).

   ![Seleccionar Guardar](./media/security-center-adaptive-network-hardening/edit-hard-rule3.png)

3. Para aplicar la regla actualizada, en la lista, seleccione la regla actualizada y haga clic en **Aplicar**.

    ![Aplicar regla](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)

### <a name="add-a-new-rule"></a>Adición de una nueva regla <a name ="add-rule"> </a>

Puede agregar una regla de tipo "permitir" no recomendada por Security Center.

> [!NOTE]
> Aquí solo se pueden agregar reglas de tipo "permitir". Si quiere agregar reglas de tipo "denegar", puede hacerlo directamente en el NSG. Para más información, consulte [Creación, modificación o eliminación de un grupo de seguridad de red ](../virtual-network/manage-network-security-group.md).

*Para agregar una regla de protección de red adaptable:*

1. Haga clic en **Agregar regla** (situado en la esquina superior izquierda).

   ![Agregar regla](./media/security-center-adaptive-network-hardening/add-hard-rule.png)

1. En la ventana **Nueva regla**, especifique los detalles y haga clic en **Agregar**.

   > [!NOTE]
   > Después de hacer clic en **Agregar**, habrá agregado correctamente la regla y esta se muestra con las demás reglas recomendadas. Sin embargo, no la ha aplicado al NSG. Para ello, debe seleccionar la regla de la lista y hacer clic en **Aplicar** (tal como se explica en el paso siguiente).

3. Para aplicar la nueva regla, en la lista, seleccione la nueva regla y haga clic en **aplicar**.

    ![Aplicar regla](./media/security-center-adaptive-network-hardening/enforce-hard-rule.png)


### <a name="delete-a-rule"></a>Eliminación de una regla <a name ="delete-rule"> </a>

Cuando sea necesario, puede eliminar una regla recomendada de la sesión actual. Por ejemplo, puede determinar que aplicar una regla sugerida podría bloquear tráfico legítimo.

*Para eliminar una regla de protección de red adaptable de la sesión actual:*

1. En la pestaña **Reglas**, haga clic en los puntos suspensivos (…) al final de la fila de la regla y seleccione **Eliminar**.  

    ![Eliminar una regla](./media/security-center-adaptive-network-hardening/delete-hard-rule.png)