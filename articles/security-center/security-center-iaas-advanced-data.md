---
title: Advanced Data Security para IaaS en Azure Security Center | Microsoft Docs
description: " Obtenga información sobre cómo habilitar Advanced Data Security para IaaS en Azure Security Center. "
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2019
ms.author: memildin
ms.openlocfilehash: a2970ea3f5ad360deaedd7efc82154cd3bc50337
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79236780"
---
# <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Advanced Data Security para servidores SQL Server en Azure Virtual Machines (versión preliminar)
Advanced Data Security para servidores SQL Server en Azure Virtual Machines es un paquete unificado de funcionalidades avanzadas de seguridad de SQL. En esta característica en vista previa se incluye una funcionalidad para buscar y mitigar posibles vulnerabilidades de la base de datos, así como detectar actividades anómalas que puedan indicar una amenaza para dicha base de datos. 

Esta oferta de seguridad para los servidores SQL Server de Azure VM se basa en la misma tecnología básica que se usa en el [paquete de seguridad de Advanced Data Security de Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).


## <a name="overview"></a>Información general

Advanced Data Security proporciona un conjunto de funcionalidades avanzadas de seguridad de SQL, entre las que se incluyen la evaluación de vulnerabilidades y Advanced Threat Protection.

* [Evaluación de vulnerabilidades](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) es un servicio fácil de configurar que puede detectar, realizar un seguimiento y ayudarle a corregir posibles puntos vulnerables de una base de datos. Permite ver el estado de la seguridad e incluye los pasos para resolver problemas de seguridad y mejorar las defensas de su base de datos.
* [Advanced Threat Protection](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) detecta actividades anómalas que indican intentos inusuales y potencialmente perjudiciales de acceder a su servidor SQL Server o de aprovechar sus vulnerabilidades. Supervisa de forma constante la base de datos en busca de actividad sospechosa y proporciona alertas de seguridad sobre patrones de acceso a la base de datos anómalos para que pueda tomar medidas. Las alertas proporcionan detalles de la actividad sospechosa y recomiendan acciones para investigar y mitigar la amenaza.

## <a name="get-started-with-advanced-data-security-for-sql-on-azure-vms"></a>Introducción a Advanced Data Security para SQL en Azure VM

Los pasos siguientes le ayudarán a empezar a usar la versión preliminar pública de Advanced Data Security para SQL en Azure VM.

### <a name="set-up-advanced-data-security-for-sql-on-azure-vms"></a>Configuración de Advanced Data Security para SQL en Azure VM

Habilite Advanced Data Security para instancias de SQL Server en Virtual Machines en el nivel de suscripción o de área de trabajo:
 
1. En la barra lateral de Security Center, abra la página **Precios y configuración**.

1. Seleccione la suscripción o el área de trabajo para la que desea habilitar Advanced Data Security para SQL en Azure VM.

1. Active la opción de **SQL servers on VM (Preview)** (Servidores SQL en VM [versión preliminar]) para habilitarla. 

    (Haga clic en la captura de pantalla para expandirla).

    [![Recomendaciones y alertas de Security Center, tal como se ven en Windows Admin Center](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png)](media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-large.png#lightbox)

    Advanced Data Security para instancias de SQL Server se habilitará en todas las instancias de SQL Server conectadas al área de trabajo seleccionada o al área de trabajo predeterminada de la suscripción seleccionada.

    >[!NOTE]
    > La solución estará totalmente activa después del primer reinicio de SQL Server. 

Para crear una nueva área de trabajo, siga las instrucciones de [Crear un área de trabajo de Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

Para conectar el host de SQL Server a un área de trabajo, siga las instrucciones de [Conexión de equipos Windows a Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows).


## <a name="set-up-email-notification-for-security-alerts"></a>Configuración de notificaciones de alertas de seguridad por correo electrónico 

Puede establecer una lista de destinatarios que reciban una notificación por correo electrónico cuando se generen alertas de Security Center. El correo electrónico contiene un vínculo directo a la alerta de Azure Security Center con todos los detalles pertinentes. 

1. Vaya a **Security Center** > **Pricing & settings** (Precios y configuración) y haga clic en la suscripción correspondiente.

    ![Configuración de la suscripción](./media/security-center-advanced-iaas-data/subscription-settings.png)

1. En el menú Configuración, haga clic en **Notificaciones por correo electrónico**. 
1. En el cuadro de texto **Dirección de correo electrónico**, escriba las direcciones de correo electrónico a las que se quiere recibir notificaciones. Si quiere, puede especificar más de una dirección de correo electrónico. Para ello, deberá separarlas con una coma (,).  Por ejemplo, admin1@mycompany.com,admin2@mycompany.com,admin3@mycompany.com.

    ![Configuración de correo electrónico](./media/security-center-advanced-iaas-data/email-settings.png)

1. En la configuración **Notificación por correo electrónico**, establezca las siguientes opciones:
  
    * **Enviar notificaciones sobre alertas de gravedad alta por correo electrónico**: en lugar de enviar mensajes de correo electrónico para todas las alertas, se envían solo para las alertas de gravedad alta.
    * **Also send email notifications to subscription owners** (Enviar también notificaciones por correo electrónico a los propietarios de las suscripciones):  también se envían notificaciones a los propietarios de las suscripciones.

1. En la parte superior de la pantalla **Notificaciones por correo electrónico**, haga clic en **Guardar**.

  > [!NOTE]
  > Asegúrese de hacer clic en **Guardar** antes de cerrar la ventana, ya que, de lo contrario, no se guardará la nueva configuración para **Notificación por correo electrónico**.

## <a name="explore-vulnerability-assessment-reports"></a>Exploración de los informes de evaluación de vulnerabilidades

El panel de evaluación de vulnerabilidades proporciona información general de los resultados de evaluación de todas sus bases de datos. Puede ver la distribución de las bases de datos según la versión de SQL Server, junto con un resumen de las bases de datos que pasaron o no la evaluación y un resumen general de las comprobaciones con errores de acuerdo con la distribución de riesgos.

Puede ver los resultados de la evaluación de vulnerabilidades directamente desde Security Center.

1. En la barra lateral de Security Center, en PROTECCIÓN DE SEGURIDAD DE RECURSOS, seleccione **Datos y almacenamiento**.

1. Seleccione la recomendación **Las vulnerabilidades de las bases de datos SQL en máquinas virtuales deben corregirse (versión preliminar)** . Para más información, consulte [Recomendaciones de Security Center](security-center-recommendations.md). 

    [![Recomendación **Las vulnerabilidades de las bases de datos SQL en máquinas virtuales deben corregirse (versión preliminar)**](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png)](media/security-center-advanced-iaas-data/data-and-storage-sqldb-vulns-on-vm.png#lightbox)

    Aparece la vista detallada de esta recomendación.

    [![Vista detallada de la recomendación **Las vulnerabilidades de las bases de datos SQL en máquinas virtuales deben corregirse (versión preliminar)**](media/security-center-advanced-iaas-data/all-servers-view.png)](media/security-center-advanced-iaas-data/all-servers-view.png#lightbox)

1. Para obtener más detalles:

    * Para obtener una información general de los recursos examinados (bases de datos) y la lista de comprobaciones de seguridad que se probaron, haga clic en el servidor que le interese.
    [![Vulnerabilidades agrupadas por SQL Server](media/security-center-advanced-iaas-data/single-server-view.png)](media/security-center-advanced-iaas-data/single-server-view.png#lightbox)

    * Para obtener información general sobre las vulnerabilidades agrupadas por una base de datos SQL específica, haga clic en la base de datos que le interese.
    [![Vulnerabilidades agrupadas por SQL Server](media/security-center-advanced-iaas-data/single-database-view.png)](media/security-center-advanced-iaas-data/single-database-view.png#lightbox)

    En cada vista, las comprobaciones de seguridad se ordenan por **gravedad**. Haga clic en una comprobación de seguridad específica para ver un panel de detalles con una **descripción**, cómo **corregir** el problema y otra información relacionada como, por ejemplo, el **impacto** o el **banco de pruebas**.

## <a name="advanced-threat-protection-for-sql-servers-on-azure-vms-alerts"></a>Alertas de Advanced Threat Protection para instancias de SQL Server en Azure VM
Las alertas se generan a partir de la realización de intentos inusuales y potencialmente dañinos para acceder a los servidores SQL Server o aprovechar sus vulnerabilidades. Estos eventos pueden desencadenar las siguientes alertas:

### <a name="anomalous-access-pattern-alerts-preview"></a>Alertas de patrón de acceso anómalo (versión preliminar)

* **Acceso desde una ubicación inusual:** esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a SQL Server, donde alguien ha iniciado sesión en el servidor SQL Server desde una ubicación geográfica inusual. Causas posibles:
    * Un atacante o un exempleado malintencionado ha accedido a su servidor SQL Server.
    * Un usuario legítimo ha accedido a su servidor SQL Server desde una ubicación nueva.
* **Acceso desde una aplicación potencialmente dañina**: esta alerta se desencadena cuando una aplicación potencialmente dañina se utiliza para tener acceso a la base de datos. Causas posibles:
    * Un atacante intenta vulnerar su SQL mediante herramientas comunes de ataque.
    * Se están realizando pruebas de penetración legítimas.
* **Acceso desde una entidad de seguridad desconocida**: esta alerta se desencadena cuando se produce un cambio en el patrón de acceso a SQL Server, donde alguien ha iniciado sesión en el servidor SQL Server mediante una entidad de seguridad inusual (usuario de SQL). Causas posibles:
    * Un atacante o un exempleado malintencionado ha accedido a su servidor SQL Server. 
    * Un usuario legítimo ha accedido a su servidor SQL Server desde una entidad de seguridad nueva.
* **Credenciales de SQL por fuerza bruta**: esta alerta se desencadena cuando hay un número anormalmente elevado de inicios de sesión infructuosos con distintas credenciales. Causas posibles:
    * Un atacante intenta vulnerar su instancia de SQL mediante fuerza bruta.
    * Se están realizando pruebas de penetración legítimas.

### <a name="potential-sql-injection-attacks-supported-in-sql-server-2019"></a>Posibles ataques por inyección de código SQL (admitidos en SQL Server 2019)

* **Vulnerabilidad a la inyección de código SQL**: esta alerta se desencadena cuando una aplicación genera una instrucción SQL errónea en la base de datos. Esta alerta puede indicar una posible vulnerabilidad a los ataques de inyección de código SQL. Causas posibles:
    * Existe un defecto en el código de la aplicación que crea la instrucción SQL errónea
    * El código de la aplicación o los procedimientos almacenados no corrigen los datos que proporciona el usuario al construir la instrucción SQL errónea, lo que se puede aprovechar para ataques por inyección de código SQL
* **Potencial inyección de código SQL**: esta alerta se desencadena cuando se produce una vulnerabilidad de seguridad activa contra una vulnerabilidad de la aplicación identificada ante inyección de código SQL. Esto significa que el atacante intentan inyectar instrucciones SQL malintencionadas mediante el código de la aplicación vulnerable o procedimientos almacenados.


### <a name="unsafe-command-supported-in-sql-server-2019"></a>Comando inseguro (compatible con SQL Server 2019)

* **Acción potencialmente insegura**: Esta alerta se desencadena cuando se ejecuta un comando con privilegios elevados y potencialmente no seguro. Causas posibles:
    * El comando que se recomienda deshabilitar para mejorar la seguridad está habilitado.
    * Un atacante está intentando aprovechar el acceso a SQL o escalar los privilegios.   


## <a name="explore-and-investigate-security-alerts"></a>Exploración e investigación de las alertas de seguridad

Las alertas de seguridad de datos están disponibles en la página de alertas de Security Center, en la pestaña seguridad del recurso o a través del vínculo directo de los correos electrónicos de alerta.

1. Para ver las alertas:

    * En Security Center haga clic en **Alertas de seguridad** de la barra lateral y seleccione una alerta.
    * En el ámbito de recursos, abra la página de recursos pertinente y, en la barra lateral, haga clic en **Seguridad**. 

1. Las alertas están diseñadas para ser independientes, con pasos de corrección detallados e información de investigación en cada una. Puede investigar más con otras funcionalidades de Azure Security Center y Azure Sentinel para obtener una visión más general:

    * Habilite la característica de auditoría de SQL Server para realizar más investigaciones. Si es usuario de Azure Sentinel, puede cargar los registros de auditoría de SQL desde los eventos del registro de seguridad de Windows a Sentinel y disfrutar de una experiencia de investigación enriquecida.
    * Para mejorar la posición de seguridad, siga las recomendaciones de Security Center para la máquina host indicadas en cada alerta. Esto reducirá los riesgos de futuros ataques. 


## <a name="next-steps"></a>Pasos siguientes

Para obtener material relacionado, vea el siguiente artículo:

- [Recomendaciones de corrección](security-center-remediate-recommendations.md)