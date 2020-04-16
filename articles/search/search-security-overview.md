---
title: Seguridad y privacidad de los datos
titleSuffix: Azure Cognitive Search
description: Azure Cognitive Search es compatible con SOC 2, HIPAA y otras certificaciones. Conexión y cifrado de datos, autenticación y acceso a identidades mediante identificadores de seguridad de usuarios y grupos en expresiones de filtro.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: fe7d076fab6a70736843fc644cd56bef44a55df2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415122"
---
# <a name="security-and-data-privacy-in-azure-cognitive-search"></a>Seguridad y privacidad de datos en Azure Cognitive Search

Azure Cognitive Search lleva integrados completos controles de acceso y características de seguridad para garantizar que el contenido privado se mantenga privado. En este artículo se enumeran las características de seguridad y el cumplimiento de normas integrados en Azure Cognitive Search.

La arquitectura de seguridad de Azure Cognitive Search abarca la seguridad física, las transmisiones cifradas, el almacenamiento cifrado y el cumplimiento de normas en toda la plataforma. Azure Cognitive Search solo acepta solicitudes autenticadas en sus operaciones. De modo opcional, puede agregar controles de acceso por usuario en el contenido mediante filtros de seguridad. En este artículo se trata la seguridad en cada capa, pero se centra principalmente en cómo se protegen los datos y las operaciones en Azure Cognitive Search.

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>Cumplimiento de estándares: ISO 27001, SOC 2, HIPAA

Azure Cognitive Search está certificado por los siguientes estándares, tal como se [anunció en junio de 2018](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/):

+ [ISO 27001:2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [Cumplimiento de SOC 2 tipo 2](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) Para ver el informe completo, vaya a [Azure - and Azure Government SOC 2 Type II Report](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports) (Informe de SOC 2 de tipo II de Azure y Azure Government). 
+ [Ley de Portabilidad y Contabilidad de los Seguros de Salud (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP (21 CFR Part 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11) (GxP [21 CFR Parte 11])
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS Level 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard) (PCI DSS de nivel 1)

El cumplimiento de estándares se aplica a las características disponibles con carácter general. Las características en versión preliminar se certifican cuando pasan a disponibilidad general y no se deben usar en soluciones que tengan requisitos de estándares estrictos. La certificación de cumplimiento está documentada en [Overview of Microsoft Azure compliance](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) (Información general del cumplimiento de Microsoft Azure) y el [Centro de confianza](https://www.microsoft.com/en-us/trustcenter). 

## <a name="encrypted-transmission-and-storage"></a>Transmisión y almacenamiento cifrados

El cifrado se extiende a lo largo de la canalización de indexación entera: desde las conexiones, pasando por la transmisión y hasta los datos indexados almacenados en Azure Cognitive Search.

| Nivel de seguridad | Descripción |
|----------------|-------------|
| Cifrado en tránsito <br>(HTTPS/SSL/TLS) | Azure Cognitive Search escucha en el puerto HTTPS 443. Las conexiones a los servicios de Azure están cifradas en toda la plataforma. <br/><br/>Todas las interacciones de Azure Cognitive Search entre el cliente y el servicio son compatibles con los protocolos SSL o TLS 1.2.  Asegúrese de usar TLSv1.2 para las conexiones SSL con el servicio.|
| Cifrado en reposo <br>Claves administradas por Microsoft | El cifrado se internaliza totalmente en el proceso de indexación, sin impacto cuantificable a la hora de indexar el tiempo que tarda en completarse ni el tamaño de indexación. Se produce automáticamente en todas las indexaciones, incluidas las actualizaciones incrementales a un índice que no esté totalmente cifrado (creado antes de enero de 2018).<br><br>Internamente, el cifrado se basa en el [cifrado del servicio de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), que usa [cifrado AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits.<br><br> El cifrado es interno para Azure Cognitive Search, con certificados y claves de cifrado que Microsoft administra internamente, y se aplica universalmente. No se puede activar ni desactivar el cifrado, administrar ni sustituir sus claves, ni ver la configuración de cifrado en el portal o mediante programación.<br><br>El cifrado en reposo se anunció el 24 de enero de 2018 y se aplica a todos los niveles de servicio, incluido el nivel Gratis, en todas las regiones. Para el cifrado completo, los índices creados antes de esa fecha deben quitarse y volver a generarse para que se produzca el cifrado. En caso contrario, solo se cifrarán los datos nuevos a partir del 24 de enero.|
| Cifrado en reposo <br>Claves administradas por el cliente | El cifrado con claves administradas por el cliente ya está disponible con carácter general para los servicios de búsqueda creados en enero de 2019 o con posterioridad. No se admite en servicios Gratis (compartidos).<br><br>Los mapas de sinónimos y los índices de Azure Cognitive Search ya se pueden cifrar en reposo con claves administradas por el cliente en Azure Key Vault. Para más información, consulte [Administración de claves de cifrado en Azure Cognitive Search](search-security-manage-encryption-keys.md).<br><br>Esta característica no reemplaza el cifrado en reposo de forma predeterminada, sino que se aplica además de este.<br><br>Si habilita esta característica, aumentará el tamaño del índice y reducirá el rendimiento de las consultas. Según las observaciones hechas hasta la fecha, puede esperar un aumento del 30 al 60% en los tiempos de consultas, aunque el rendimiento real variará según la definición del índice y los tipos de consultas. Debido a este impacto en el rendimiento, se recomienda habilitar esta característica solo en los índices que realmente la necesitan.

## <a name="azure-wide-user-access-controls"></a>Controles de acceso de usuario en todo Azure

Azure cuenta con varios mecanismos de seguridad y, por tanto, están disponibles automáticamente para los recursos de Azure Cognitive Search que se creen.

+ [Bloqueos en el nivel de suscripción y recurso para evitar la eliminación](../azure-resource-manager/management/lock-resources.md)
+ [Control de acceso basado en roles (RBAC) para controlar el acceso a la información y las operaciones administrativas](../role-based-access-control/overview.md)

Todos los servicios de Azure admiten controles de acceso basado en roles (RBAC) para establecer niveles de acceso de forma coherente en todos los servicios. Por ejemplo, la visualización de información confidencial, como la clave de administración, está restringida a los roles Colaborador y Propietario. Sin embargo, la visualización del estado del servicio está disponible para los miembros de cualquier rol. RBAC proporciona los roles de Propietario, Colaborador y Lector. De forma predeterminada, todos los administradores de servicios son miembros del rol de propietario.

<a name="service-access-and-authentication"></a>

## <a name="endpoint-access"></a>Acceso al punto de conexión

### <a name="public-access"></a>Acceso público

Azure Cognitive Search hereda las medidas de seguridad de la plataforma Azure y proporciona su propia autenticación basada en claves. Una clave de API es una cadena que se compone de letras y números generados aleatoriamente. El tipo de clave (administrador o consulta) determina el nivel de acceso. El envío de una clave válida se considera una prueba de que la solicitud se origina desde una entidad de confianza. 

Hay dos niveles de acceso al servicio de búsqueda, habilitados por dos tipos de claves:

* Acceso de administración (válido para cualquier operación de lectura y escritura en el servicio)
* Acceso de consulta (válido para operaciones de solo lectura, como las consultas en la colección de documentos de un índice)

Las *claves de administración* se crean cuando se aprovisiona el servicio. Hay dos claves de administración, designadas como *principal* y *secundaria*, pero en realidad son intercambiables. Todos los servicios tienen dos claves de administración, con el fin de que se pueda dejar de usar una de ellas sin perder el acceso al servicio. También puede [regenerar la clave de administrador](search-security-api-keys.md#regenerate-admin-keys) periódicamente para seguir los procedimientos recomendados de seguridad de Azure, pero no se puede agregar al número total de claves de administrador. Hay un máximo de dos claves de administrador por servicio de búsqueda.

Las *claves de consulta* se crean a medida que son necesarias y están diseñadas para las aplicaciones cliente que emiten consultas. Puede crear hasta 50 claves de consulta. En el código de la aplicación, especifique la dirección URL de búsqueda y una clave de API de consulta para permitir el acceso de solo lectura a la colección de documentos de un índice específico. Juntos, el punto de conexión, una clave de API para el acceso de solo lectura y un índice de destino definen el nivel de acceso y ámbito de la conexión desde la aplicación cliente.

Se requiere autenticación en cada solicitud, y cada solicitud se compone de una clave obligatoria, una operación y un objeto. Cuando se encadenan, los dos niveles de permisos (completo y de solo lectura) y el contexto (por ejemplo, una operación de consulta en un índice) son suficientes para proporcionar seguridad en la gama completa de las operaciones del servicio. Para más información acerca de las claves, vea [Create and manage api-keys](search-security-api-keys.md) (Creación y administración de claves de API).

### <a name="restricted-access"></a>Acceso restringido

Si tiene un servicio público y quiere restringir el uso de este, puede usar la regla de restricción de IP en la versión de la API REST de administración: 2020-03-13, [IpRule](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service#IpRule). IpRule le permite restringir el acceso a su servicio mediante la identificación (de forma individual o en un intervalo) de las direcciones IP a las que quiera otorgar acceso a su servicio de búsqueda. 

### <a name="private-access"></a>Acceso privado

Los [puntos de conexión privados](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) para Azure Cognitive Search permiten a un cliente de una red virtual obtener acceso de forma segura a los datos de un índice de búsqueda a través de un [vínculo privado](https://docs.microsoft.com/azure/private-link/private-link-overview). El punto de conexión privado usa una dirección IP del espacio de direcciones de la red virtual para el servicio de búsqueda. El tráfico de red entre el cliente y el servicio de búsqueda atraviesa la red virtual y un vínculo privado de la red troncal de Microsoft, lo que elimina la exposición a la red pública de Internet.

[Azure Virtual Network (VNet)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) permite establecer una comunicación segura entre recursos, con su red local y con Internet. 

## <a name="index-access"></a>Acceso a los índices

En Azure Cognitive Search, un índice individual no es un objeto protegible. En su lugar, el acceso a un índice se determina en la capa de servicio (acceso de lectura o escritura), junto con el contexto de una operación.

Para el acceso del usuario final, puede estructurar las solicitudes de consulta para conectarse con una clave de consulta, lo que hace que cualquier solicitud sea de solo lectura e incluir el índice específico utilizado por la aplicación. En una solicitud de consulta, no existe el concepto de combinación de índices ni de acceso simultáneo a varios índices para que todas las solicitudes tengan un único índice de destino por definición. Por lo tanto, la construcción de la solicitud de consulta misma (una clave y un índice único de destino) define el límite de seguridad.

Los accesos de administrador y de desarrollador a los índices no se diferencian: ambos necesitan tener acceso de escritura para crear, eliminar y actualizar objetos administrados por el servicio. Cualquier persona con una clave de administración del servicio puede leer, modificar o eliminar cualquier índice en el mismo servicio. Para la protección contra la eliminación accidental o malintencionada de índices, su control de código fuente interno para los recursos de código es la solución para revertir una eliminación o modificación de índices no deseada. Azure Cognitive Search incluye conmutación por error dentro del clúster para garantizar la disponibilidad, pero no almacena ni ejecuta el código propietario utilizado para crear o cargar los índices.

Para soluciones multiinquilino que requieren límites de seguridad en el nivel de índice, estas soluciones suelen incluir un nivel intermedio que los clientes utilizan para controlar el aislamiento de índices. Para más información sobre los casos de uso de varios inquilinos, consulte [Modelos de diseño de aplicaciones SaaS para varios inquilinos y Azure Cognitive Search](search-modeling-multitenant-saas-applications.md).

## <a name="authentication"></a>Authentication

### <a name="admin-access"></a>Acceso de administrador

El [acceso basado en roles (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) determina si tiene acceso a los controles del servicio y su contenido. Si es propietario o colaborador en un servicio de Azure Cognitive Search, puede usar el módulo **Az.Search** de PowerShell o el portal para crear, actualizar o eliminar objetos del servicio. También puede usar la [API REST de administración de Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

### <a name="user-access"></a>Acceso de usuarios

De forma predeterminada, el acceso de usuario a un índice viene determinado por la clave de acceso en la solicitud de consulta. La mayoría de los desarrolladores crean y asignan [*claves de consulta*](search-security-api-keys.md) para las solicitudes de búsqueda del lado cliente. Una clave de consulta concede acceso de lectura a todo el contenido del índice.

Si necesita tener un control por usuario pormenorizado del contenido, puede crear filtros de seguridad en las consultas, que devuelven documentos asociados con una identidad de seguridad determinada. En lugar de funciones predefinidas y asignaciones de roles, el control de acceso basado en identidades se implementa como un *filtro* que recorta los resultados de búsqueda de documentos y contenido en función de las identidades. La tabla siguiente describe dos enfoques para recortar el contenido no autorizado de los resultados de la búsqueda.

| Enfoque | Descripción |
|----------|-------------|
|[Recorte de seguridad basado en filtros de identidad](search-security-trimming-for-azure-search.md)  | Documenta el flujo de trabajo básico para implementar el control de acceso de identidades de usuario. Trata la incorporación de identificadores de seguridad a un índice y, a continuación, se explica el filtrado por ese campo para no incluir el contenido prohibido en los resultados. |
|[Recorte de seguridad basado en identidades de Azure Active Directory](search-security-trimming-for-azure-search-with-aad.md)  | En este artículo se amplía el artículo anterior y se proporcionan los pasos necesarios para recuperar identidades de Azure Active Directory (AAD), uno de los [servicios gratuitos](https://azure.microsoft.com/free/) de la plataforma de nube de Azure. |

## <a name="table-permissioned-operations"></a>Tabla: Operaciones con permisos

En la tabla siguiente se resumen las operaciones permitidas en Azure Cognitive Search y la clave que desbloquea el acceso a una operación determinada.

| Operación | Permisos |
|-----------|-------------------------|
| Crear un servicio | Titular de la suscripción de Azure|
| Escalar un servicio | Clave de administración, RBAC propietario o colaborador en el recurso  |
| Eliminar un servicio | Clave de administración, RBAC propietario o colaborador en el recurso |
| Crear, modificar y eliminar objetos en el servicio: <br>Índices y partes de componentes (incluidas las definiciones del analizador, los perfiles de puntuación y las opciones de CORS), indexadores, orígenes de datos, sinónimos, proveedores de sugerencias. | Clave de administración, RBAC propietario o colaborador en el recurso  |
| Consultar un índice | Clave de administrador o de consulta (RBAC no es aplicable) |
| Consultar la información del sistema, como devolver estadísticas, recuentos y listas de objetos. | Clave de administración, RBAC en el recurso (propietario, colaborador y lector) |
| Administrar claves de administración | Clave de administración, RBAC propietario o colaborador en el recurso |
| Administrar claves de consulta |  Clave de administración, RBAC propietario o colaborador en el recurso  |

## <a name="physical-security"></a>Seguridad física

Los centros de datos de Microsoft proporcionan una seguridad física líder en la industria y son compatibles con una amplia gama de normas y reglas. Para más información, vaya a la página [Centros de datos globales](https://www.microsoft.com/cloud-platform/global-datacenters) o vea un vídeo corto sobre la seguridad de los centros de datos.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>Consulte también

+ [Introducción a .NET (se muestra el uso de una clave de administración para crear un índice)](search-create-index-dotnet.md)
+ [Introducción a REST (se muestra el uso de una clave de administración para crear un índice)](search-create-index-rest-api.md)
+ [Control de acceso basado en identidades mediante filtros de Azure Cognitive Search](search-security-trimming-for-azure-search.md)
+ [Control de acceso basado en identidades de Active Directory mediante filtros de Azure Cognitive Search](search-security-trimming-for-azure-search-with-aad.md)
+ [Filtros de Azure Cognitive Search](search-filters.md)