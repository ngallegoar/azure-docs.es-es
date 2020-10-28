---
title: Más información sobre las opciones de soporte técnico de Azure Service Fabric
description: Versiones de clúster de Azure Service Fabric compatibles y vínculos para presentar incidencias de soporte técnico
author: pkcsf
ms.topic: troubleshooting
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: 977cd79de629670cef526f072340f8897fa6446e
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92316495"
---
# <a name="azure-service-fabric-support-options"></a>Opciones de soporte técnico de Azure Service Fabric

Hemos creado una serie de opciones de solicitud de soporte técnico para satisfacer las necesidades de administración de los clústeres de Service Fabric y las cargas de trabajo de la aplicación. En función de la urgencia del soporte necesario y la gravedad del problema, puede elegir la opción que sea adecuada para usted.

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Notifique problemas en el entorno de producción o bien solicite soporte técnico de pago para Azure

Para notificar los problemas relacionados con el clúster de Service Fabric que se ejecuta en Azure, abra una incidencia de soporte técnico [en Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) o en el [portal de soporte técnico de Microsoft](https://support.microsoft.com/oas/default.aspx?prid=16146).

Más información sobre:
 
- [Soporte técnico de Microsoft para Azure](https://azure.microsoft.com/support/plans/?b=16.44).
- [Soporte técnico Premier de Microsoft](https://support.microsoft.com/en-us/premier).

> [!Note]
> Los clústeres que se ejecutan en un nivel de confiabilidad Bronze o clúster de un solo nodo le permitirán ejecutar solo cargas de trabajo de prueba. Si experimenta problemas con un clúster que se ejecuta en una confiabilidad de nivel Bronze o clúster de un solo nodo, el equipo de soporte técnico de Microsoft le ayudará a mitigar el problema, pero no llevará a cabo un análisis de causa principal. Para obtener más información, consulte las [características de confiabilidad del clúster](./service-fabric-cluster-capacity.md#reliability-characteristics-of-the-cluster).
>
> Para obtener más detalles sobre lo que es necesario para un clúster listo para producción, consulte la [lista de comprobación sobre la preparación para la producción](./service-fabric-production-readiness-checklist.md).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Notifique problemas en el entorno de producción o bien solicite soporte técnico de pago para clústeres independientes de Service Fabric

Si desea informar sobre problemas relacionados con los clústeres de Service Fabric que se ejecutan de forma local o en otras nubes, puede abrir una incidencia de soporte técnico en el [Portal de soporte técnico de Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

Más información sobre:

- [Soporte técnico profesional para Microsoft local](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Soporte técnico Premier de Microsoft](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Información de problemas con Azure Service Fabric

Configuramos un repositorio de GitHub para informar problemas con Service Fabric.  También supervisamos activamente los foros siguientes.

### <a name="github-repo"></a>Repositorio de GitHub 

Informe de los problemas de Azure Service Fabric en el [GitHub de Service Fabric](https://github.com/microsoft/service-fabric/issues). Este repositorio está diseñado para informar y rastrear problemas con Azure Service Fabric y para generar solicitudes de funciones pequeñas. **No use este medio para informar problemas con el sitio activo** .

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow y foros de MSDN

La [etiqueta Service Fabric en StackOverflow][stackoverflow] y el [foro de Service Fabric en MSDN][msdn-forum] son ideales para hacer preguntas generales sobre cómo funciona la plataforma y por qué podría usarla para realizar ciertas tareas.

### <a name="azure-feedback-forum"></a>Foro de comentarios de Azure

El [foro de comentarios de Azure para Service Fabric][uservoice-forum] es el mejor lugar para enviar ideas significativas sobre las características del producto. Revisamos las solicitudes más populares y las factorizamos para nuestra planeación de medio a largo plazo. Se recomienda buscar apoyo para sus sugerencias dentro de la comunidad.

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Versiones de vista previa de Service Fabric: no admitidas para su uso en producción

En ocasiones, creamos versiones preliminares especiales que contienen cambios significativos en las características para los que nos gustaría sondear los comentarios iniciales. Solo debe usar versiones preliminares en entornos de prueba aislados que no atiendan las cargas de trabajo de producción. El clúster de producción debe estar ejecutando siempre una versión de Service Fabric compatible y estable. No ofrecemos ninguna opción de soporte técnico de pago para estas versiones preliminares.

Una versión preliminar siempre comienza con un número de versión principal y secundaria de 255. Por ejemplo, si ve una versión 255.255.5703.949 de Service Fabric, esa versión es preliminar y solo está prevista para su uso en clústeres de prueba. Estas versiones preliminares también se anuncian en el [blog del equipo de Service Fabric](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric) y se detallan en las características incluidas. Use una de las opciones enumeradas en [Notificación de problemas Azure Service Fabric](#report-azure-service-fabric-issues) para formular preguntas o proporcionar comentarios.

## <a name="next-steps"></a>Pasos siguientes

[Versiones admitidas de Service Fabric](service-fabric-versions.md)

<!--references-->
[Microsoft Q&A question page]: /answers/topics/azure-service-fabric.html
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: ./index.yml
[sample-repos]: /samples/browse/?products=azure
[msdn-forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?category=windowsazureplatform