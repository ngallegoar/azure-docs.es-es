---
title: Controles de aplicación adaptables en Azure Security Center
description: Este documento le ayuda a usar el control de aplicación adaptable en Azure Security Center para incluir en la lista de permitidos las aplicaciones que se ejecutan en máquinas virtuales de Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/23/2019
ms.author: memildin
ms.openlocfilehash: 1dc94c5ec08cc27fb1819ccc16fd766c62aad796
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604679"
---
# <a name="adaptive-application-controls"></a>Controles de aplicación adaptables
Obtenga información acerca de cómo configurar el control de aplicación en Azure Security Center con este tutorial.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>¿Qué son los controles de aplicación adaptables en Security Center?
El control de aplicaciones adaptables es una solución de un extremo a otro inteligente y automatizada de Azure Security Center que ayuda a controlar qué aplicaciones se pueden ejecutar en las máquinas virtuales de Azure y que no son de Azure (Windows y Linux). Entre otras ventajas, esto ayuda a proteger las máquinas virtuales frente al malware. Security Center usa el aprendizaje automático para analizar las aplicaciones que se ejecutan en sus máquinas virtuales y crea una lista de permitidas a partir de esta inteligencia. Esta funcionalidad simplifica enormemente el proceso de configuración y mantenimiento de directivas de listas de aplicaciones permitidas, lo que le permite:

- Bloquear o alertar sobre intentos de ejecución de aplicaciones malintencionadas, incluidas aquellas que podrían ser omitidas por las soluciones antimalware.
- Cumplir con la directiva de seguridad de la organización que dicta el uso de software con licencia únicamente.
- Evitar el uso de software no deseado en el entorno.
- Evitar la ejecución de aplicaciones anteriores y no compatibles.
- Impedir herramientas de software específicas no permitidas en la organización.
- Permiten que TI controle el acceso a información confidencial a través del uso de aplicaciones.

> [!NOTE]
> Para las máquinas virtuales que no son de Azure y las de Linux, se admiten controles de aplicación adaptables solo en modo auditoría.

## <a name="how-to-enable-adaptive-application-controls"></a>¿Cómo habilitar los controles de aplicación adaptables?

Los controles de aplicación adaptables ayudan a definir un conjunto de aplicaciones que se pueden ejecutar en los grupos configurados de las máquinas virtuales. Esta característica está disponible tanto para máquinas virtuales y servidores de Azure como Windows (todas las versiones, clásica o Azure Resource Manager) y Linux que no son de Azure. Siga estos pasos para configurar las listas de aplicaciones permitidas:

1. Abra el panel **Security Center**.

1. En el panel de la izquierda, seleccione **Controles de aplicaciones adaptables** que se encuentra en **Protección en la nube avanzada**.

    [![Defensa](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png#lightbox)

Se muestra la página **Controles de aplicaciones adaptables**.

![controls](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

La sección **Grupos de máquinas virtuales** contiene tres pestañas:

* **Configured** (Configurado): lista de grupos que contienen las máquinas virtuales que se configuraron con el control de aplicaciones.
* **Recommended** (Recomendado): lista de grupos para los que se recomienda el control de aplicación. Security Center utiliza el aprendizaje automático para identificar las máquinas virtuales que son buenas candidatas para el control de aplicación en función de si las máquinas virtuales ejecutan de forma coherente las mismas aplicaciones.
* **No recommendation** (Sin recomendación): lista de grupos que contienen máquinas virtuales sin ninguna recomendación de control de aplicación. Por ejemplo, máquinas virtuales en las que las aplicaciones cambian constantemente y no han alcanzado un estado estable.

> [!NOTE]
> Security Center utiliza un algoritmo de agrupación en clústeres propietario para crear grupos de máquinas virtuales, lo que garantiza que máquinas virtuales similares obtienen la directiva de control de aplicaciones recomendada óptima.
>
>

### <a name="configure-a-new-application-control-policy"></a>Configuración de una nueva directiva de control de aplicación

1. Seleccione la pestaña **Recommended** (Recomendado) para obtener una lista de los grupos con recomendaciones de control de aplicación:

   ![Recomendado](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

   La lista incluye:

   - **Nombre de grupo**: nombre de la suscripción y el grupo
   - **Máquinas virtuales y equipos**: número de máquinas virtuales del grupo
   - **Estado**: estado de las recomendaciones
   - **Gravedad**: nivel de gravedad de las recomendaciones

2. Pulse en un grupo para abrir la opción **Crear reglas de control de aplicaciones**.

   [![Reglas de control de aplicación](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png#lightbox)

3. En **Seleccionar máquinas virtuales**, revise la lista de máquinas virtuales recomendadas y desactive cualquiera a la que no desee aplicar la directiva de inclusión en la listas de aplicaciones permitidas. A continuación, se ven dos listas:

   - **Recommended applications** (Aplicaciones recomendadas): una lista de las aplicaciones frecuentes en las máquinas virtuales dentro de este grupo y cuya ejecución se recomienda permitir.
   - **More applications** (Más aplicaciones): una lista de las aplicaciones que son menos frecuentes en las máquinas virtuales dentro de este grupo o que se conocen como Infringibles (encontrará más información al respecto a continuación) y se recomienda para su revisión.

4. Revise las aplicaciones en cada una de las listas y desactive las que no desee aplicar. Cada lista incluye:

   - **NOMBRE**: la información del certificado o de la ruta de acceso completa de una aplicación
   - **TIPOS DE ARCHIVO**: el tipo de archivo de la aplicación. Puede ser EXE, Script, MSI o cualquier permutación de estos tipos.
   - **INFRINGIBLE**: un icono de advertencia indica si un atacante podría usar una determinada aplicación para evitar una lista de aplicaciones permitidas. Se recomienda revisar estas aplicaciones antes de su aprobación.
   - **USUARIOS**: los usuarios a los que se recomienda admitir para ejecutar una aplicación

5. Cuando haya terminado de realizar las selecciones, elija **Crear**. <br>
   Después de seleccionar Crear, Azure Security Center crea automáticamente las reglas adecuadas en la solución integrada de listas de aplicaciones permitidas disponible en los servidores de Windows (AppLocker).

> [!NOTE]
> - Security Center necesita al menos dos semanas de datos para crear una base de referencia y rellenar las recomendaciones únicas para cada grupo de máquinas virtuales. Los clientes nuevos del nivel estándar de Security Center experimentarán un comportamiento en el que, al principio, sus grupos de máquinas virtuales aparecen en la pestaña *Ninguna recomendación*.
> - Controles de aplicación adaptables de Security Center no admite máquinas virtuales para las que ya esté habilitada una directiva de AppLocker mediante un GPO o una directiva de seguridad local.
> -  Como procedimiento recomendado de seguridad, Security Center siempre intenta crear una regla de publicador para las aplicaciones permitidas y solo si una aplicación no tiene información del publicador (también conocido como no firmada), se creará una regla de ruta de acceso para la ruta de acceso completa de la aplicación específica.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Edición y supervisión de un grupo configurado con control de aplicación

1. Para editar y supervisar un grupo configurado con una directiva de listas de aplicaciones permitidas, vuelva a la página **Controles de aplicaciones adaptables** y seleccione **CONFIGURADO** en **Grupos de máquinas virtuales**:

   ![Grupos](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

   La lista incluye:

   - **Nombre de grupo**: el nombre de la suscripción y el grupo
   - **Máquinas virtuales y equipos**: el número de máquinas virtuales del grupo
   - **Modo**: El modo Auditoría registrará los intentos de ejecución de aplicaciones que no están en la listas de aplicaciones permitidas; el modo Forzar no permitirá que se ejecuten aplicaciones que no estén en la lista de permitidas.
   - **Alertas**: cualquier infracción actual

2. Haga clic en un grupo para realizar cambios en la página **Editar directiva de control de aplicaciones**.

   ![Protección](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. En **Modo de protección**, puede seleccionar entre las siguientes opciones:

   - **Auditoría**: en este modo, la solución de control de aplicación no exige el cumplimiento de las reglas y solo audita la actividad en las máquinas virtuales protegidas. Esto se recomienda para escenarios donde desea primero observar el comportamiento general antes de bloquear la ejecución de una aplicación en la máquina virtual de destino.
   - **Forzar**: en este modo, la solución de control de aplicación exige el cumplimiento de las reglas y se asegura de bloquear las aplicaciones cuya ejecución no está permitida.

   > [!NOTE]
   > -  El modo de aplicación **Forzar** está deshabilitado hasta nuevo aviso.
   > - Como se mencionó anteriormente, una nueva directiva de control de aplicación siempre se configura en modo *Auditoría* de forma predeterminada. 
   >

4. En **Extensión de directiva**, agregue cualquier ruta de acceso de aplicación que quiera permitir. Después de agregar estas rutas de acceso, Security Center actualiza la directiva de lista de aplicaciones permitidas en las máquinas virtuales dentro del grupo seleccionado de máquinas virtuales y crea las reglas adecuadas para estas aplicaciones, además de las reglas que ya están en vigor.

5. Revise las infracciones actuales que aparecen en la sección **Alertas recientes**. Haga clic en cada línea para ser redirigido a la página **Alertas** de Azure Security Center, y vea todas las alertas que detectó Azure Security Center en las máquinas virtuales asociadas.
   - **Alertas**: cualquier infracción que se registró.
   - **Núm. de máquinas virtuales**: número de máquinas virtuales con este tipo de alerta.

6. En **Reglas de inclusión en lista aprobada de publicadores**, **Reglas de inclusión en listas de rutas de acceso permitidas** y **Reglas hash relativas a las listas de permitidos** puede ver qué reglas de la lista de aplicaciones permitidas están actualmente configuradas en las máquinas virtuales dentro de un grupo, según el tipo de regla de recopilación. Para cada regla se puede ver lo siguiente:

   - **Regla**: los parámetros específicos según los cuales AppLocker examina una aplicación para determinar si una aplicación puede ejecutarse.
   - **Tipo de archivo**: los tipos de archivo cubiertos por una regla específica. Puede ser uno de los siguientes: EXE, Script, MSI o cualquier permutación de estos tipos.
   - **Usuarios**: nombre o número de usuarios que tienen permiso para ejecutar una aplicación que está cubierta por una regla de inclusión en listas de aplicaciones permitidas.

   ![Reglas de inclusión en listas de permitidos](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

7. Haga clic en los puntos suspensivos al final de cada línea si quiere eliminar la regla concreta o editar los usuarios permitidos.

8. Después de realizar cambios en una directiva **Controles de aplicación adaptables**, haga clic en **Guardar**.

### <a name="not-recommended-list"></a>Lista Ninguna recomendación

Security Center solo recomienda las directivas de listas de aplicaciones permitidas para las máquinas virtuales que ejecutan un conjunto estable de aplicaciones. No se crean recomendaciones si las aplicaciones en las máquinas virtuales asociadas cambian continuamente.

![Recomendación](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

La lista contiene:
- **Nombre de grupo**: el nombre de la suscripción y el grupo
- **Máquinas virtuales y equipos**: el número de máquinas virtuales del grupo

Azure Security Center también le permite definir una directiva de creación de listas de aplicaciones permitidas en grupos no recomendados de máquinas virtuales. Siga los mismos principios que se han descrito anteriormente, para configurar una directiva de creación de listas de aplicaciones permitidas también en esos grupos.

## <a name="move-a-vm-from-one-group-to-another"></a>Mover una máquina virtual de un grupo a otro

 Cuando se mueve una máquina virtual de un grupo a otro, la directiva de control de aplicaciones que se le aplica cambia a la configuración del grupo al que se ha movido. También se puede mover una máquina virtual de un grupo configurado a otro grupo no configurado, lo que da lugar a que se quite cualquier directiva de control de aplicaciones que se hubiera aplicado antes a una máquina virtual.

 1. En la página **Controles de aplicaciones adaptables**, en la pestaña **CONFIGURACIÓN REALIZADA**, haga clic en el grupo al que pertenece actualmente la máquina virtual que va a mover.
1. Haga clic en **Máquinas virtuales y equipos configurados**.
1. Haga clic en los tres puntos en la línea de la máquina virtual que se va a mover y haga clic en **Mover**. Se abre la ventana **Operación para mover un equipo a otro grupo** .

    ![Protección](./media/security-center-adaptive-application/adaptive-application-move-group.png)

 1. Seleccione el grupo al que quiere mover la máquina virtual y haga clic en **Mover equipo** y luego en **Guardar**.

    ![Protección](./media/security-center-adaptive-application/adaptive-application-move-group2.png)

 > [!NOTE]
> Asegúrese de hacer clic en **Guardar**, después de hacer clic en **Mover equipo**. Si no hace clic en **Guardar**, el equipo no se moverá.

## <a name="next-steps"></a>Pasos siguientes
En este documento ha aprendido a usar el control de aplicación adaptable en Azure Security Center para incluir en la lista de permitidos las aplicaciones que se ejecutan en máquinas virtuales de Azure y no Azure. Para obtener más información sobre Azure Security Center, consulte los siguientes recursos:

* [Administración y respuesta a las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Aprenda a administrar las alertas y responder a incidentes de seguridad en Security Center.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md). Aprenda a supervisar el estado de los recursos de Azure.
* [Comprensión de las alertas de seguridad en Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Obtenga información acerca de los distintos tipos de alertas de seguridad.
* [Guía de solución de problemas de Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Obtenga información acerca de cómo solucionar problemas comunes en Security Center.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/). Encuentre artículos de blog sobre el cumplimiento y la seguridad de Azure.
