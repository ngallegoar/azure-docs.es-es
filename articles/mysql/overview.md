---
title: 'Introducción: Azure Database for MySQL'
description: Obtenga más información acerca de Azure Database for MySQL, que es un servicio de base de datos relacional de Microsoft Cloud basado en MySQL Community Edition.
author: ajlam
ms.service: mysql
ms.author: andrela
ms.custom: mvc
ms.topic: overview
ms.date: 3/18/2020
ms.openlocfilehash: 8f49811ad0d40c70933d32227cfb17a5144b857a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80067816"
---
# <a name="what-is-azure-database-for-mysql"></a>¿Qué es Azure Database for MySQL?

Azure Database for MySQL es un servicio de base de datos relacional de Microsoft Cloud basado en el motor de base de datos de [MySQL Community Edition](https://www.mysql.com/products/community/) (disponible bajo la licencia GPLv2), versiones 5.6, 5.7 y 8.0. Azure Database for MySQL ofrece lo siguiente:

- Alta disponibilidad integrada sin coste adicional.
- Rendimiento predecible, con precios de pago por uso inclusivos.
- Escalado inmediato según las necesidades.
- Protección de información confidencial en reposo y en movimiento.
- Copias de seguridad automáticas y restauración a un momento dado durante un máximo de 35 días.
- Seguridad y cumplimiento de nivel empresarial.

Estas funcionalidades no requieren casi ninguna tarea de administración y todas se proporcionan sin ningún costo adicional. Le permiten centrarse en el desarrollo rápido de aplicaciones y en reducir el plazo de acceso al mercado, en lugar de tener que dedicar tiempo y recursos a la administración tanto de máquinas virtuales como de infraestructura. Además, puede seguir desarrollando su aplicación con las herramientas y la plataforma de código abierto de su elección, y entregar con la velocidad y eficacia que exige su negocio sin tener que aprender nuevas habilidades.

![Diagrama conceptual de Azure Database for MySQL](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

En este artículo se presentan los principales conceptos y características de Azure Database for MySQL relacionados con el rendimiento, la escalabilidad y la manejabilidad, con vínculos para explorar los detalles. Consulte estas guías de inicio rápido para comenzar:

- [Create an Azure Database for MySQL server using Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md) (Creación de un servidor de Azure Database for MySQL mediante Azure Portal)
- [Create an Azure Database for MySQL server using Azure CLI](quickstart-create-mysql-server-database-using-azure-cli.md) (Creación de un servidor de Azure Database for MySQL mediante la CLI de Azure)

Para ver ejemplos de la CLI de Azure, consulte:

- [Ejemplos de la CLI de Azure para Azure Database for MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Ajustar el rendimiento y la escala en cuestión de segundos
El servicio Azure Database for MySQL ofrece varios niveles de servicio: Básico, Uso general y Optimizado para memoria. Cada nivel ofrece diferentes niveles de rendimiento y funcionalidades para admitir cargas de trabajo de bases de datos, de ligeras a pesadas. Puede compilar su primera aplicación en una base de datos pequeña por poco dinero al mes y, después, ajustar la escala para satisfacer las necesidades de la solución. La escalabilidad dinámica permite a la base de datos responder de manera transparente a los cambiantes requisitos de recursos. Solo paga por los recursos que necesite y cuando los necesite. Para obtener más información, consulte los  [planes de tarifa](concepts-service-tiers.md).

## <a name="monitoring-and-alerting"></a>Supervisión y alertas
¿Cómo se decide cuándo adaptar la velocidad? Utilice las características de supervisión y alertas de rendimiento integradas, combinadas con la clasificación del rendimiento basado en los núcleos virtuales. Con estas herramientas, puede evaluar rápidamente el impacto de ajustar la escala de los núcleos virtuales en función de sus necesidades de rendimiento actuales o futuras. Vea las [Alertas](howto-alert-on-metric.md) para obtener más información.

## <a name="keep-your-app-and-business-running"></a>Mantenimiento de la aplicación y el negocio en funcionamiento
El Acuerdo de Nivel de Servicio (SLA) de Azure, con una disponibilidad del 99,99 % líder del sector y con la tecnología de una red global de centros de datos administrados por Microsoft, ayuda a mantener las aplicaciones en funcionamiento de forma ininterrumpida. Con cada servidor de Azure Database for MySQL, se saca provecho de las ventajas de la seguridad integrada, la tolerancia a errores y la protección de datos, algo que de lo contrario tendría que adquirir o diseñar, compilar y administrar. Con Azure Database for MySQL puede usar la restauración a un momento dado para recuperar un servidor a un estado anterior, con un plazo máximo de 35 días.

## <a name="secure-your-data"></a>Protección de los datos
La tradicional seguridad de los datos de los servicios de bases de datos de Azure se mantiene en Azure Database for MySQL con características que limitan el acceso, protegen los datos en reposo y en movimiento, y le ayudan a supervisar la actividad. Para obtener más información sobre la seguridad de plataforma de Azure, vaya al [Centro de confianza de Azure](https://www.microsoft.com/trustcenter/security). Para más información acerca de las características de seguridad de Azure Database for MySQL, consulte la [información general sobre seguridad](concepts-security.md).

## <a name="contacts"></a>Contactos
Para cualquier pregunta o sugerencia que pueda tener con respecto al uso de Azure Database for MySQL, envíe un correo electrónico al equipo de Azure Database for MySQL ([@Ask Azure DB for MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com)). Esta dirección de correo electrónico no es un alias de soporte técnico.

Además, tenga en cuenta los siguientes puntos de contacto según corresponda:

- Para ponerse en contacto con el servicio de soporte técnico de Azure, [presente una incidencia de soporte técnico en Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Para corregir un problema con la cuenta, envíe una [solicitud de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) en Azure Portal.
- Para proporcionar comentarios o solicitar nuevas características, cree una entrada mediante [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha leído una introducción a Azure Database for MySQL y ha contestado a la pregunta "¿Qué es Azure Database for MySQL?", ya está listo para:

- Consulte la página de precios para ver comparaciones de costos y calculadoras. [Precios](https://azure.microsoft.com/pricing/details/mysql/)
- Comience por crear su primer servidor. [Create an Azure Database for MySQL server using Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md) (Creación de un servidor de Azure Database for MySQL mediante Azure Portal)
- Cree su primera aplicación con su lenguaje preferido: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
