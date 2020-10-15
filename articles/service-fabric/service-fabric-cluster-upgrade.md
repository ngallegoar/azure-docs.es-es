---
title: Actualización de un clúster de Azure Service Fabric
description: 'Aprenda a actualizar la versión o la configuración de un clúster de Azure Service Fabric: establecimiento del modo de actualización del clúster, la actualización de certificados, la adición de puertos de la aplicación, la creación de revisiones del sistema operativo y lo que puede esperar cuando se realizan las actualizaciones.'
ms.topic: conceptual
ms.date: 11/12/2018
ms.custom: sfrev
ms.openlocfilehash: d92ac90e0e41d534231bafbe991a05764dbee07d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82789562"
---
# <a name="upgrading-and-updating-an-azure-service-fabric-cluster"></a>Actualización de un clúster de Azure Service Fabric

Para cualquier sistema moderno, diseñar la capacidad de actualización es clave para lograr el éxito a largo plazo de su producto. Un clúster de Azure Service Fabric es un recurso de su propiedad que está parcialmente administrado por Microsoft. En este artículo se describe lo que se administra automáticamente y lo que puede configurar usted mismo.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Control de la versión de Service Fabric que se ejecuta en el clúster

Asegúrese de que el clúster siempre ejecute una [versión admitida de Service Fabric](service-fabric-versions.md). Cada vez que Microsoft anuncia el lanzamiento de una nueva versión de Service Fabric, se marca la versión anterior para que finalice el soporte técnico después de un mínimo de 60 días a partir de esa fecha. Las nuevas versiones se anuncian en el [blog del equipo de Service Fabric](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric).

14 días antes de la expiración de la versión que se está ejecutando en el clúster, se genera un evento de mantenimiento que coloca el clúster en un estado de mantenimiento de advertencia. El clúster permanece en un estado de advertencia hasta que actualiza a una versión compatible de Service Fabric.

Puede configurar el clúster para recibir las actualizaciones automáticas cuando Microsoft las publica o puede seleccionar la versión compatible de Service Fabric que desea que tenga el clúster.  Para obtener más información, lea [Actualización de la versión de Service Fabric de un clúster](service-fabric-cluster-upgrade-version-azure.md).

## <a name="fabric-upgrade-behavior-during-automatic-upgrades"></a>Comportamiento de actualización de Fabric durante las actualizaciones automáticas

Microsoft mantiene el código de tejido y la configuración que se ejecuta en un clúster de Azure. Realizamos actualizaciones supervisadas automáticas del software según se necesitan. Estas actualizaciones podrían ser de código, de configuración o ambas. Para asegurarse de que la aplicación no sufre ningún impacto, o solo un impacto mínimo, durante el proceso, las actualizaciones se realizan en las fases siguientes:

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Fase 1: La actualización se realiza con todas las directivas de mantenimiento de clústeres

Durante esta fase, las actualizaciones se realizan en un dominio de actualización cada vez, y las aplicaciones que se ejecutaban en el clúster continúan ejecutándose sin tiempo de inactividad. Las directivas de mantenimiento de clústeres (en cuanto al estado del nodo y el estado de la aplicación) se aplican durante la actualización.

Si las directivas del clúster no se cumplen, la actualización se revierte y se envía un correo electrónico al propietario de la suscripción. El correo electrónico contiene la siguiente información:

* Notificación de que tuvimos que revertir una actualización de clúster.
* Acciones correctoras sugeridas, si hay alguna.
* Número de días (*n*) hasta que ejecutemos la fase 2.

Vamos a intentar ejecutar la misma actualización unas cuantas veces más por si se produjeron errores en las actualizaciones por razones de infraestructura. Después de los *n* días desde la fecha en que se envió el correo electrónico, continuamos con la fase 2.

Si se cumplen las directivas de mantenimiento del clúster, la actualización se considera correcta y se marca como completada. Esto puede ocurrir durante la ejecución inicial de la actualización o cualquiera de las ejecuciones posteriores de la actualización en esta fase. No hay ningún correo electrónico de confirmación de una ejecución correcta. Esto es para evitar el envío de demasiados mensajes de correo electrónico; recibir un correo electrónico debe considerarse una excepción a la normalidad. Esperamos que la mayoría de las actualizaciones de clúster funcionen sin afectar a la disponibilidad de las aplicaciones.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Fase 2: La actualización se realiza solo con las directivas de mantenimiento predeterminadas

En esta fase, las directivas de mantenimiento se establecen de forma que el número de aplicaciones que tenían un estado correcto al principio de la actualización siga siendo el mismo durante el proceso de actualización. Al igual que en la fase 1, en la fase 2 las actualizaciones se realizan en un dominio de actualización cada vez, y las aplicaciones que se ejecutaban en el clúster continúan ejecutándose sin tiempo de inactividad. Las directivas de mantenimiento del clúster (una combinación del estado del nodo y el estado de todas las aplicaciones que se ejecutan en el clúster) se cumplen mientras dura la actualización.

Si no se cumplen las directivas de mantenimiento del clúster, la actualización se revierte. Después, se envía un correo electrónico al propietario de la suscripción. El correo electrónico contiene la siguiente información:

* Notificación de que tuvimos que revertir una actualización de clúster.
* Acciones correctoras sugeridas, si hay alguna.
* El número de días (n) hasta que ejecutemos la fase 3.

Vamos a intentar ejecutar la misma actualización unas cuantas veces más por si se produjeron errores en las actualizaciones por razones de infraestructura. Se envía un recordatorio por correo electrónico un par de días antes de llegar a n días. Después de los n días desde la fecha en que se envió el correo electrónico, continuamos con la fase 3. Es necesario tomarse en serio los correos electrónicos que le enviamos en la fase 2 y realizar acciones de corrección.

Si se cumplen las directivas de mantenimiento del clúster, la actualización se considera correcta y se marca como completada. Esto puede ocurrir durante la ejecución inicial de la actualización o cualquiera de las ejecuciones posteriores de la actualización en esta fase. No hay ningún correo electrónico de confirmación de una ejecución correcta.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Fase 3: La actualización se realiza con directivas de mantenimiento agresivas

En esta fase, estas directivas de mantenimiento están orientadas a la finalización de la actualización en lugar de al mantenimiento de las aplicaciones. Pocas actualizaciones del clúster terminarán en esta fase. Si el clúster llega a esta fase, es probable que la aplicación no tenga el mantenimiento adecuado o pierda disponibilidad.

Igual que las otras dos fases, las actualizaciones de la fase 3 se realizan en un dominio de actualización cada vez.

Si no se cumplen las directivas de mantenimiento del clúster, la actualización se revierte. Vamos a intentar ejecutar la misma actualización unas cuantas veces más por si se produjeron errores en las actualizaciones por razones de infraestructura. Después, el clúster se ancla y ya no recibirán soporte técnico ni actualizaciones.

Se enviará un correo electrónico con esta información al propietario de la suscripción, junto con las acciones correctoras. No esperamos que los clústeres entren en un estado en el que la fase 3 produzca errores.

Si se cumplen las directivas de mantenimiento del clúster, la actualización se considera correcta y se marca como completada. Esto puede ocurrir durante la ejecución inicial de la actualización o cualquiera de las ejecuciones posteriores de la actualización en esta fase. No hay ningún correo electrónico de confirmación de una ejecución correcta.

## <a name="manage-certificates"></a>Administración de certificados

Service Fabric usa los [certificados de servidor X.509](service-fabric-cluster-security.md) que especifica al crear un clúster para proteger las comunicaciones entre los nodos del clúster y autenticar a los clientes. Puede agregar, actualizar o eliminar certificados para el clúster y el cliente en [Azure Portal](https://portal.azure.com) o mediante la CLI de Azure o PowerShell.  Para obtener más información, lea [Agregar o quitar certificados](service-fabric-cluster-security-update-certs-azure.md)

## <a name="open-application-ports"></a>Puertos de aplicación abiertos

Puede cambiar los puertos de aplicación cambiando las propiedades del recurso del equilibrador de carga asociado al tipo de nodo. Puede usar Azure Portal, PowerShell o el CLI de Azure. Para obtener más información, lea [Open application ports for a cluster](create-load-balancer-rule.md) (Abrir los puertos de aplicación para un clúster).

## <a name="define-node-properties"></a>Definición de las propiedades del nodo

A veces, es posible que quiera asegurarse de que ciertas cargas de trabajo solo se ejecuten en determinados tipos de nodos en el clúster. Por ejemplo, algunas cargas de trabajo pueden requerir GPU o SSD, al contrario que otras. Para cada uno de los tipos de nodo de un clúster, puede agregar propiedades de nodo personalizados a los nodos del clúster. Las restricciones de posición son las instrucciones adjuntas a los servicios individuales que se seleccionan para una o más propiedades de nodo. Las restricciones de posición definen dónde deben ejecutarse los servicios.

Para obtener más información acerca de las restricciones de selección de ubicación, de las propiedades del nodo y de cómo definirlas, lea [Restricciones de ubicación y propiedades de nodo](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="add-capacity-metrics"></a>Adición de métricas de capacidad

Para cada uno de los tipos de nodo, puede agregar las métricas de capacidad personalizadas que desee usar en las aplicaciones para la carga de informes. Para obtener más información sobre el uso de las métricas de capacidad para notificar la carga, consulte los documentos del Administrador de recursos de clúster de Service Fabric que se describen en [Descripción de un clúster de Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md) y sobre [métricas y cargas](service-fabric-cluster-resource-manager-metrics.md).

## <a name="set-health-policies-for-automatic-upgrades"></a>Establecimiento de directivas de mantenimiento para actualizaciones automáticas

Puede especificar directivas de mantenimiento personalizado para la actualización de Service Fabric. Si ha configurado el clúster para que las actualizaciones de Service Fabric se realicen automáticamente, estas directivas se aplicarán en la fase 1 de las actualizaciones automáticas de Service Fabric.
Si ha configurado el clúster para que las actualizaciones de Service Fabric se realicen de forma manual, estas directivas se aplicarán cada vez que seleccione una nueva versión, lo cual hará que el sistema ponga en marcha la actualización de Service Fabric en el clúster. Si no reemplaza las directivas, se usarán los valores predeterminados.

Puede especificar las directivas de mantenimiento personalizado o revisar la configuración actual en la hoja "actualización de Service Fabric", seleccionando la configuración avanzada de la actualización. Revise la siguiente imagen sobre cómo hacerlo.

![Administración de las directivas de mantenimiento personalizado][HealthPolices]

## <a name="customize-fabric-settings-for-your-cluster"></a>Personalización de la configuración de Service Fabric para el clúster

Pueden personalizarse muchas opciones de configuración diferentes en un clúster, como el nivel de confiabilidad de las propiedades del nodo y del clúster. Para más información, lea [Service Fabric cluster fabric settings](service-fabric-cluster-fabric-settings.md) (Configuración de un clúster de Service Fabric).

## <a name="patch-the-os-in-the-cluster-nodes"></a>Revisión del sistema operativo en los nodos del clúster

La aplicación de orquestación de revisiones (POA) es una aplicación de Service Fabric que automatiza la aplicación de revisiones de sistema operativo en un clúster de Service Fabric sin tiempo de inactividad. La [aplicación de orquestación de revisiones de Windows](service-fabric-patch-orchestration-application.md) se puede implementar en el clúster para instalar revisiones de una manera orquestada, manteniendo los servicios disponibles todo el tiempo.

## <a name="next-steps"></a>Pasos siguientes

* Aprender a personalizar la [configuración de Service Fabric para el clúster](service-fabric-cluster-fabric-settings.md)
* Aprenda cómo [escalar o reducir horizontalmente el clúster](service-fabric-cluster-scale-in-out.md)
* Obtenga información sobre [actualizaciones de aplicaciones](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
