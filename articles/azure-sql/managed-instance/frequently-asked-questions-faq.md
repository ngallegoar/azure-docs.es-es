---
title: Preguntas más frecuentes
titleSuffix: Azure SQL Managed Instance
description: Preguntas frecuentes acerca de Instancia administrada de Azure SQL (P+F)
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: d2e4b07c97e09fce5cdaa034e2fe67a18ef0d7f1
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2020
ms.locfileid: "86171166"
---
# <a name="azure-sql-managed-instance-frequently-asked-questions-faq"></a>Preguntas frecuentes acerca de Instancia administrada de Azure SQL (P+F)
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Este artículo contiene las preguntas más comunes sobre [Instancia administrada de Azure SQL](sql-managed-instance-paas-overview.md).

## <a name="supported-features"></a>Características admitidas

**¿Dónde puedo encontrar una lista de las características que se admiten en Instancia administrada de SQL?**

Para obtener una lista de las características admitidas en Instancia administrada de SQL, consulte [Características de Instancia administrada de Azure SQL](../database/features-comparison.md).

Para conocer las diferencias de sintaxis y comportamiento entre Instancia administrada de Azure SQL y SQL Server, consulte [Diferencias de T-SQL de SQL Server](transact-sql-tsql-differences-sql-server.md).


## <a name="technical-specification-resource-limits-and-other-limitations"></a>Especificación técnica, límites de recursos y otras limitaciones
 
**¿Dónde puedo encontrar las características técnicas y los límites de recursos de Instancia administrada de SQL?**

Para conocer las características disponibles para la generación de hardware, consulte las [diferencias técnicas en las generaciones de hardware](resource-limits.md#hardware-generation-characteristics).
Para ver los niveles de servicio disponibles y sus características, consulte las [diferencias técnicas entre los niveles de servicio](resource-limits.md#service-tier-characteristics).

**¿A qué nivel de servicio puedo optar?**

Cualquier cliente puede optar a cualquier nivel de servicio. Sin embargo, si quiere cambiar las licencias existentes por tarifas con descuento en Azure SQL Managed Instance mediante [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/), tenga en cuenta que los clientes de SQL Server Enterprise Edition con Software Assurance son aptos para los niveles de rendimiento [De uso general](../database/service-tier-general-purpose.md) o [Crítico para la empresa](../database/service-tier-business-critical.md) y los clientes de SQL Server Standard Edition con Software Assurance solo son válidos para el nivel de rendimiento De uso general. Para obtener más información, consulte los [derechos específicos de AHB](../azure-hybrid-benefit.md?tabs=azure-powershell#what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server).

**¿Qué tipos de suscripción se admiten para SQL Managed Instance?**

Para ver la lista de tipos de suscripción admitidos, consulte [Tipos de suscripción admitidos](resource-limits.md#supported-subscription-types). 

**¿Qué regiones de Azure se admiten?**

Las instancias administradas se pueden crear en la mayoría de las regiones de Azure. Consulte las [regiones admitidas para SQL Managed Instance](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Si necesita crear una instancia administrada en una región que no se admite actualmente, [envíe una solicitud de soporte técnico a través de Azure Portal](../database/quota-increase-request.md).

**¿Hay alguna limitación de cuota para las implementaciones de SQL Managed Instance?**

Instancia administrada tiene dos límites predeterminados: límite en el número de subredes que puede usar y límite en el número de núcleos virtuales que puede aprovisionar. Los límites varían entre tipos de suscripción y regiones. Para ver la lista de limitaciones de recursos regionales por tipo de suscripción, consulte la tabla de [Limitaciones de recursos regionales](resource-limits.md#regional-resource-limitations). Estos son límites moderados que se pueden aumentar a petición. Si necesita aprovisionar más instancias administradas en sus regiones actuales, puede enviar una solicitud de soporte técnico para aumentar la cuota a través de Azure Portal. Para más información, consulte [Solicitud de aumentos de cuota para Azure SQL Database](../database/quota-increase-request.md).

**¿Puedo aumentar el número del límite de bases de datos (100) en mi instancia administrada a petición?**

No, y actualmente no hay ningún plan confirmado para aumentar el número de bases de datos en SQL Managed Instance. 

**¿Dónde puedo migrar si tengo más de 8 TB de datos?**
Puede considerar la posibilidad de migrar a otros tipos de Azure que se ajusten a su carga de trabajo: [Hiperescala de Azure SQL Database](../database/service-tier-hyperscale.md) o [SQL Server en Azure Virtual Machines](../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md).

**¿Dónde puedo migrar si tengo requisitos de hardware específicos, como una RAM mayor a una proporción de núcleos virtuales o más CPU?**
Puede considerar la po0sibilidad de migrar a las opciones optimizadas de memoria o CPU de [SQL Server en Azure Virtual Machines](../virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview.md) o [Azure SQL Database](../database/sql-database-paas-overview.md).

## <a name="known-issues--bugs"></a>Errores y problemas conocidos

**¿Dónde puedo encontrar soluciones a errores y problemas conocidos?**

Para ver los errores y los problemas conocidos, consulte [Problemas conocidos](../database/doc-changes-updates-release-notes.md#known-issues).

## <a name="new-features"></a>Nuevas características

**¿Dónde puedo encontrar las características más recientes y las características en versión preliminar pública?**

Para obtener características nuevas y en vista previa, consulte las [Notas de la versión](../database/doc-changes-updates-release-notes.md?tabs=managed-instance).

## <a name="create-update-delete-or-move-sql-managed-instance"></a>Creación, actualización, eliminación o traslado de SQL Managed Instance

**¿Cómo puedo aprovisionar SQL Managed Instance?**

Puede aprovisionar una instancia desde [Azure Portal](instance-create-quickstart.md), [PowerShell](scripts/create-configure-managed-instance-powershell.md), la [CLI de Azure](https://techcommunity.microsoft.com/t5/azure-sql-database/create-azure-sql-managed-instance-using-azure-cli/ba-p/386281) y las [plantillas de ARM](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/creating-azure-sql-managed-instance-using-arm-templates).

**¿Puedo aprovisionar instancias administradas en una suscripción existente?**

Sí, puede aprovisionar una instancia administrada en una suscripción existente si esa suscripción pertenece a los [tipos de suscripción admitidos](resource-limits.md#supported-subscription-types).

**¿Por qué no se pudo aprovisionar una instancia administrada en la subred cuyo nombre empieza por un dígito?**

Se trata de una limitación actual del componente subyacente que comprueba el nombre de la subred con respecto a la expresión regular ^[a-zA-Z_][^\\\/\:\*\?\"\<\>\|\`\'\^]*(?<![\.\s])$. Actualmente se admiten todos los nombres que pasan la expresión regular y son nombres de subred válidos.

**¿Cómo puedo escalar mi instancia administrada?**

Puede escalar la instancia administrada desde [Azure Portal](../database/service-tiers-vcore.md?tabs=azure-portal#selecting-a-hardware-generation), [PowerShell](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/change-size-azure-sql-managed-instance-using-powershell), la [CLI de Azure](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-update) o las [plantillas de ARM](https://docs.microsoft.com/archive/blogs/sqlserverstorageengine/updating-azure-sql-managed-instance-properties-using-arm-templates).

**¿Puedo trasladar mi instancia administrada de una región a otra?**

Sí, puede hacerlo. Para obtener instrucciones, consulte [Traslado de recursos entre regiones](../database/move-resources-across-regions.md).

**¿Cómo puedo eliminar mi instancia administrada?**

Puede eliminar instancias administradas a través de Azure portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlinstance?view=azps-4.3.0), la [CLI de Azure](https://docs.microsoft.com/cli/azure/sql/mi?view=azure-cli-latest#az-sql-mi-delete) o las [API de REST de Resource Manager](https://docs.microsoft.com/rest/api/sql/managedinstances/delete).

**¿Cuánto tiempo se tarda en crear o actualizar una instancia, o en restaurar una base de datos?**

El tiempo esperado para crear una instancia administrada o cambiar los niveles de servicio (núcleos virtuales, almacenamiento) depende de varios factores. Consulte [Operaciones de administración](sql-managed-instance-paas-overview.md#management-operations).
 
## <a name="naming-conventions"></a>Convenciones de nomenclatura

**¿Puede una instancia administrada tener el mismo nombre que una instancia de SQL Server local?**

No se admite el cambio de nombre de la instancia administrada.

**¿Puedo cambiar el prefijo de zona DNS?**

Sí, se puede cambiar la zona DNS predeterminada *.database.windows.net* de la instancia administrada. 

Para usar otra zona DNS en lugar de la predeterminada, por ejemplo, *.contoso.com*, haga lo siguiente: 
- Use CliConfig para definir un alias. La herramienta no es más que un contenedor de la configuración del registro, por lo que también se puede hacer mediante una directiva de grupo o un script.
- Use *CNAME* con la opción *TrustServerCertificate = true*.

## <a name="move-a-database-from-sql-managed-instance"></a>Traslado de una base de datos de SQL Managed Instance 

**¿Cómo puedo devolver una base de datos de SQL Managed Instance a SQL Server o Azure SQL Database?**

Puede [exportar una base de datos a BACPAC](../database/database-export.md) y, a continuación, [importar el archivo BACPAC](../database/database-import.md). Este enfoque es aconsejable si la base de datos tiene menos de 100 GB.

La replicación transaccional se puede utilizar si todas las tablas de la base de datos tienen claves principales.

Las copias de seguridad nativas `COPY_ONLY` realizadas de una instancia administrada de SQL no se pueden restaurar en SQL Server, porque la instancia administrada de SQL tiene una versión superior de la base de datos en comparación con SQL Server.

## <a name="migrate-an-instance-database"></a>Migración de una bases de datos de instancia

**¿Cómo puedo migrar mi base de datos de instancia a Azure SQL Database?**

Una opción es [exportar la base de datos a un archivo BACPAC](../database/database-export.md) y, a continuación, [importar dicho archivo](../database/database-import.md). 

Este enfoque es aconsejable si la base de datos tiene menos de 100 GB. La replicación transaccional se puede utilizar si todas las tablas de la base de datos tienen claves principales.

## <a name="switch-hardware-generation"></a>Cambio de generación de hardware 

**¿Puedo cambiar la generación de hardware de mi instancia administrada de SQL entre Gen 4 y Gen 5 en línea?**

La conmutación en línea automatizada entre dos generaciones de hardware es posible si ambas están disponibles en la misma región en la que se ha aprovisionado SQL Managed Instance. En este caso, puede consultar la [página Introducción al modelo de núcleos virtuales](../database/service-tiers-vcore.md), que explica cómo pasar de una generación de hardware a otra.

Se trata de una operación de larga duración, ya que la nueva instancia administrada se aprovisionará en segundo plano y las bases de datos se transferirán automáticamente entre la instancia antigua y la nueva, con una conmutación por error rápida al final del proceso. 

**¿Qué ocurre si ambas generaciones de hardware no se admiten en la misma región?**

Si las dos generaciones de hardware no se admiten en la misma región, es posible cambiar una de ellas, pero se debe hacer manualmente. Esto requiere que se aprovisione una nueva instancia en la región en que esté disponible la generación de hardware que se quiera llevar a cabo, y que se realicen una copia de seguridad y una restauración de datos manualmente entre las instancias antigua y nueva.

**¿Qué ocurre si no hay suficientes direcciones IP para realizar la operación de actualización?**

En caso de que no haya suficientes direcciones IP en la subred en la que se ha aprovisionado la instancia administrada, tendrá que crear una nueva subred y una nueva instancia administrada dentro de esta. También se recomienda crear la nueva subred con más direcciones IP asignadas para que las operaciones de actualización futuras eviten situaciones similares. (Para saber el tamaño de subred adecuado, consulte [Cómo determinar el tamaño de una subred de red virtual](vnet-subnet-determine-size.md)). Una vez aprovisionada la nueva instancia, puede realizar manualmente una copia de seguridad de los datos y restaurarlos entre la instancia antigua y la nueva, o bien realizar una [restauración a un momento dado](point-in-time-restore.md?tabs=azure-powershell) entre las instancias. 


## <a name="tune-performance"></a>Ajustar rendimiento

**¿Cómo se optimiza el rendimiento de SQL Managed Instance?**

SQL Managed Instance en el nivel De uso general utiliza el almacenamiento remoto, por lo que el tamaño de los archivos de registro y datos es importante para el rendimiento. Para más información, consulte [Influencia del tamaño del archivo de registro en el rendimiento de Instancia administrada de SQL de uso general](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e).

Si la carga de trabajo consta de muchas transacciones pequeñas, considere la posibilidad de cambiar el tipo de conexión de proxy a modo de redirección.

## <a name="maximum-storage-size"></a>Tamaño máximo de almacenamiento

**¿Cuál es el tamaño máximo de almacenamiento de Instancia administrada de SQL?**

El tamaño de almacenamiento de Instancia administrada de SQL depende del nivel de servicio seleccionado (De uso general o Crítico para la empresa). Para conocer las limitaciones de almacenamiento de estos niveles de servicio, consulte [Características de los niveles de servicio](../database/service-tiers-general-purpose-business-critical.md).

  
## <a name="networking-requirements"></a>Requisitos de red 

**¿Cuáles son las restricciones de NSG entrantes o salientes actuales en la subred de instancia administrada?**

Las reglas NSG y UDR necesarias se documentan [aquí](connectivity-architecture-overview.md#mandatory-inbound-security-rules-with-service-aided-subnet-configuration), y el servicio las establece automáticamente.
Tenga en cuenta que estas reglas son solo las únicas que necesitamos para mantener el servicio. Para conectarse a la instancia administrada y usar diferentes características, deberá establecer reglas adicionales específicas de las características que necesita mantener.

**¿Cómo se pueden establecer reglas de grupos de seguridad de red de entrada en los puertos de administración?**

SQL Managed Instance es responsable de establecer reglas en los puertos de administración. Esto se logra a través de la funcionalidad denominada [configuración de subred asistida por servicio](connectivity-architecture-overview.md#service-aided-subnet-configuration).
El objetivo es garantizar un flujo ininterrumpido del tráfico de administración con el fin de cumplir un contrato de nivel de servicio.

**¿Puedo obtener los intervalos de IP de origen que se usan para el tráfico de administración de entrada?**

Sí. Puede analizar el tráfico que llega a través del grupo de seguridad de red mediante la [configuración de los registros de flujo de Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#analyze-traffic-to-or-from-a-network-security-group).

**¿Puedo establecer NSG para controlar el acceso al punto de conexión de datos (puerto 1433)?**

Sí. Después de aprovisionar una instancia administrada, puede establecer un NSG que controle el acceso de entrada al puerto 1433. Es aconsejable limitar su intervalo de IP tanto como sea posible.

**¿Puedo establecer la NVA o el firewall local para filtrar el tráfico de administración saliente en función de los FQDN?**

No. No se admite por distintos motivos:
-   El tráfico de enrutamiento que representa la respuesta a la solicitud de administración entrante sería asimétrico y podría no funcionar.
-   El enrutamiento del tráfico que va al almacenamiento se vería afectado por las restricciones de rendimiento y la latencia, lo que nos impediría proporcionar la calidad y la disponibilidad esperadas del servicio.
-   En función de la experiencia, estas configuraciones son propensas a errores y no se admiten.

**¿Puedo establecer la NVA o el firewall para el tráfico que no es de administración saliente?**

Sí. La manera más sencilla de lograrlo es agregar la regla 0/0 a una UDR asociada a la subred de instancia administrada para enrutar el tráfico a través de la NVA.
 
**¿Cuántas direcciones IP necesito para un instancia administrada?**

La subred debe tener un número de [direcciones IP](connectivity-architecture-overview.md#network-requirements) disponibles suficiente. Para determinar el tamaño de la subred de red virtual para SQL Managed Instance, consulte [Determinación del tamaño e intervalo de subred necesarios para Instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-determine-size-vnet-subnet). 

**¿Qué ocurre si no hay suficientes direcciones IP para realizar la operación de la instancia administrada?**

En caso de que no haya suficientes [direcciones IP](connectivity-architecture-overview.md#network-requirements) en la subred en la que se ha aprovisionado la instancia administrada, tendrá que crear una nueva subred y una nueva instancia administrada dentro de esta. También se recomienda crear la nueva subred con más direcciones IP asignadas para que las operaciones de actualización futuras eviten situaciones similares. Una vez aprovisionada la nueva instancia, puede realizar manualmente una copia de seguridad de los datos y restaurarlos entre la instancia antigua y la nueva, o bien realizar una [restauración a un momento dado](point-in-time-restore.md?tabs=azure-powershell) entre las instancias.

**¿Necesito una subred vacía para crear una instancia administrada?**

No. Puede usar una subred vacía o una subred que ya contenga instancias administradas. 

**¿Puedo cambiar el intervalo de direcciones de la subred?**

No si esta contiene instancias administradas. Se trata de una limitación de la infraestructura de redes de Azure. Solo se permite [agregar espacio de direcciones adicional a una subred vacía](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet#change-subnet-settings). 

**¿Puedo transferir mi instancia administrada a otra subred?**

No. Se trata de una limitación de diseño de instancia administrada actual. No obstante, puede aprovisionar una nueva instancia en otra subred y realizar manualmente una copia de seguridad de los datos y restaurarlos entre la instancia antigua y la nueva, o bien realizar una [restauración a un momento dado](point-in-time-restore.md?tabs=azure-powershell) entre las instancias.

**¿Necesito una red virtual vacía para crear una instancia administrada?**

No es necesario. Puede [crear una red virtual para Azure SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-vnet-subnet) o [configurar una red virtual existente para Azure SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-configure-vnet-subnet).

**¿Puedo colocar un instancia administrada con otros servicios en una subred?**

No. Actualmente no se admite la colocación de una instancia administrada en una subred que ya contiene otros tipos de recursos.


## <a name="mitigate-data-exfiltration-risks"></a>Mitigación de los riesgos de filtración de datos  

**¿Cómo se pueden mitigar los riesgos de filtración de datos?**

Para mitigar los riesgos de filtración de datos, es aconsejable que los clientes apliquen un conjunto de valores y controles de seguridad:

- Activar el [cifrado de datos transparente (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) en todas las bases de datos.
- Desactivar Common Language Runtime (CLR). Se recomienda hacerlo también en el entorno local.
- Usar solo la autenticación de Azure Active Directory (Azure AD).
- Acceder a la instancia con una cuenta de DBA con pocos privilegios.
- Configurar el acceso del jumpbox de JIT para la cuenta de sysadmin.
- Activar la [auditoría de SQL](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) e integrarla en los mecanismos de alerta.
- Activar la [detección de amenazas](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) desde el conjunto de pruebas de [Advanced Data Security (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).


## <a name="cost-saving-use-cases"></a>Casos de uso que permiten reducir costos

**¿Dónde puedo encontrar casos de uso y el ahorro de costos resultante con Instancia administrada de SQL?**

Casos prácticos de Instancia administrada de SQL:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)

Existe también un estudio de Forrester en el que se explican en detalle los beneficios, costos y riesgos asociados a la implementación de Azure SQL Managed Instance: [The Total Economic Impact de la instancia administrada de Microsoft Azure SQL Database](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance).


## <a name="dns"></a>DNS

**¿Puedo configurar un DNS personalizado para SQL Managed Instance?**

Sí. Consulte [Configuración de un DNS personalizado para Azure SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).

**¿Puedo actualizar el DNS?**

Actualmente, no proporcionamos ninguna característica para actualizar la configuración del servidor DNS para la instancia administrada de SQL.

La configuración de DNS se actualiza:

- Cuando expira la concesión de DHCP.
- Al actualizar la plataforma.

Como alternativa, cambie SQL Managed Instance a la versión de 4 núcleos virtuales y vuelva a actualizarla posteriormente. Esto tiene el efecto secundario de actualizar la configuración de DNS.


## <a name="ip-address"></a>Dirección IP

**¿Puedo realizar una conexión a SQL Managed Instance mediante una dirección IP?**

No se admite la conexión a SQL Managed Instance mediante una dirección IP. El nombre de host de SQL Managed Instance se asigna a un equilibrador de carga delante del clúster virtual de SQL Managed Instance. Dado que un clúster virtual podría hospedar varias instancias administradas, las conexiones no se pueden enrutar a la instancia administrada adecuada sin especificar explícitamente el nombre.

Para más información sobre la arquitectura del clúster virtual de Instancia administrada de SQL, consulte [Arquitectura de conectividad del clúster virtual](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture).

**¿SQL Managed Instance puede tener una dirección IP estática?**

En situaciones poco frecuentes pero necesarias, es posible que tengamos que realizar una migración en línea de SQL Managed Instance a un nuevo clúster virtual. Si es necesaria, esta migración se debe a los cambios realizados en nuestra pila de tecnología destinados a mejorar la seguridad y confiabilidad del servicio. La migración a un nuevo clúster virtual provoca el cambio de dirección IP que está asignada al nombre de host de Instancia administrada de SQL. El servicio de Instancia administrada de SQL no solicita compatibilidad con direcciones IP estáticas y se reserva el derecho a cambiar la dirección IP sin previo aviso como parte de los ciclos de mantenimiento regular.

Por este motivo se desaconseja confiar en la inmutabilidad de la dirección IP, ya que podría provocar un tiempo de inactividad innecesario.

## <a name="change-time-zone"></a>Cambio de la zona horaria

**¿Puedo cambiar la zona horaria de una instancia administrada existente?**

La configuración de la zona horaria se puede establecer la primera vez que se aprovisiona una instancia administrada. No se admite el cambio de zona horaria de una instancia administrada existente. Para más información, consulte las [limitaciones de zona horaria](timezones-overview.md#limitations).

Las soluciones alternativas incluyen la creación de una nueva instancia administrada con la zona horaria adecuada, y la posterior realización de una copia de seguridad y una restauración de forma manual, o lo que recomendamos, realizar una [restauración a un momento dado entre instancias](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/).


## <a name="security-and-database-encryption"></a>Seguridad y cifrado de la base de datos

**¿El rol de servidor sysadmin está disponible para SQL Managed Instance?**

Sí, los clientes pueden crear inicios de sesión que sean miembros del rol sysadmin.  Los clientes que asumen el privilegio sysadmin también asumen la responsabilidad de operar la instancia, lo que puede afectar negativamente al compromiso del contrato de nivel de servicio. Para agregar el inicio de sesión al rol de servidor sysadmin, consulte [Autenticación de Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-aad-security-tutorial#azure-ad-authentication).

**¿El cifrado de datos es transparente para SQL Managed Instance?**

Si, el cifrado de datos transparente es compatible con SQL Managed Instance. Para obtener más información, consulte [Cifrado de datos transparente para SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal).

**¿Puedo aprovechar el modelo "Bring Your Own Key" para TDE?**

Sí, el escenario de Azure Key Vault para BYOK está disponible para Azure SQL Managed Instance. Para obtener más información, consulte [Cifrado de datos transparente con una clave administrada por el cliente](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?view=sql-server-ver15&tabs=azure-portal#customer-managed-transparent-data-encryption---bring-your-own-key).

**¿Puedo migrar una base de datos de SQL Server cifrada?**

Sí, puede hacerlo. Para migrar una base de datos de SQL Server cifrada, debe exportar e importar los certificados existentes en Instancia administrada y, después, realizar una copia de seguridad completa de la base de datos y restaurarla en Instancia administrada. 

También puede usar [Azure Database Migration Service](https://azure.microsoft.com/services/database-migration/) para migrar las bases de datos cifradas de TDE.

**¿Cómo puedo configurar la rotación del protector de TDE para SQL Managed Instance?**

Puede girar el protector de TDE de Instancia administrada mediante Azure Cloud Shell. Para obtener instrucciones, consulte [Cifrado de datos transparente en SQL Managed Instance con su propia clave desde Azure Key Vault](scripts/transparent-data-encryption-byok-powershell.md).

**¿Puedo restaurar mi base de datos cifrada en Instancia administrada de SQL?**

Sí, no es preciso descifrar la base de datos para poder restaurarla en Instancia administrada de SQL. Tiene que especificar la clave o el certificado utilizados como un protector de clave de cifrado en el sistema de origen a SQL Managed Instance para poder leer los datos desde el archivo de copia de seguridad cifrado. Existen dos formas posibles de hacerlo:

- *Cargar el protector de certificado en SQL Managed Instance*. Solo se puede hacer mediante PowerShell. El [script de ejemplo](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) describe todo el proceso.
- *Cargar el protector de clave asimétrica en Azure Key Vault y apuntar SQL Managed Instance hacia este*. Este enfoque es similar al caso de uso del cifrado de datos transparente de Bring Your Own Key (BYOK), que también usa la integración de Key Vault para almacenar la clave de cifrado. Si no quiere usar la clave como un protector de clave de cifrado y solo quiere que esté disponible para Instancia administrada de SQL y así poder restaurar las bases de datos cifradas, siga las instrucciones para [configurar BYOK TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption), y no seleccione la casilla **Hacer que la clave seleccionada sea el protector de TDE predeterminado**.

Una vez que el protector de cifrado está disponible para Instancia administrada de SQL, puede continuar con el procedimiento de restauración de base de datos estándar.

## <a name="purchasing-models-and-benefits"></a>Modelos de compra y ventajas

**¿Qué modelos de compra están disponibles para SQL Managed Instance?**

SQL Managed Instance ofrece el [modelo de compra basado en núcleo virtual](sql-managed-instance-paas-overview.md#vcore-based-purchasing-model).

**¿Qué ventajas de costos están disponibles para SQL Managed Instance?**

Puede ahorrar costos con las ventajas de SQL Azure de las siguientes maneras:
-   Maximice las inversiones existentes en licencias locales y ahorre hasta un 55 % con [Ventaja híbrida de Azure](https://docs.microsoft.com/azure/azure-sql/azure-hybrid-benefit?tabs=azure-powershell). 
-   Confirme una reserva de recursos de proceso y ahorre hasta un 33 % con [Ventaja de instancia reservada](https://docs.microsoft.com/azure/sql-database/sql-database-reserved-capacity). Combínela con Ventaja híbrida de Azure para ahorrar hasta un 82 %. 
-   Ahorre hasta un 55 % en comparación con la lista de [Ventaja de precios para desarrollo/pruebas de Azure](https://azure.microsoft.com/pricing/dev-test/), que ofrece tarifas con descuento para las cargas de trabajo de desarrollo y pruebas en curso.

**¿Quién puede optar a la Ventaja de instancia reservada?**

Para poder beneficiarse de la Ventaja de instancia reservada, el tipo de suscripción debe ser un Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P) o un contrato individual con precios de pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P). Para más información sobre las reservas, consulte [Ventaja de instancia reservada](https://docs.microsoft.com/azure/sql-database/sql-database-reserved-capacity). 

**¿Se pueden cancelar, intercambiar o reembolsar reservas?**

Puede cancelar, intercambiar o reembolsar reservas con ciertas limitaciones. Para más información, consulte [Autoservicio de intercambios y reembolsos de reservas de Azure](https://docs.microsoft.com/azure/cost-management-billing/reservations/exchange-and-refund-azure-reservations).

## <a name="billing-for-managed-instance-and-backup-storage"></a>Facturación de Instancia administrada y almacenamiento de copia de seguridad

**¿Cuáles son las opciones de precios de SQL Managed Instance?**

Para explorar las opciones de precios de Instancia administrada, consulte la [página de precios](https://azure.microsoft.com/pricing/details/azure-sql/sql-managed-instance/single/).

**¿Cómo puedo realizar un seguimiento del costo de facturación de mi instancia administrada?**

Puede hacerlo con la [solución Azure Cost Management](https://docs.microsoft.com/azure/cost-management-billing/). Navegue a **Suscripciones** en [Azure Portal](https://portal.azure.com) y seleccione **Análisis de costos**. 

Use la opción **Costos acumulados** y luego filtre por **Tipo de recurso**, como `microsoft.sql/managedinstances`.

**¿Cuánto cuestan las copias de seguridad automatizadas?**

Obtiene la misma cantidad de espacio de almacenamiento de copia de seguridad gratis que el espacio de almacenamiento de datos reservado adquirido, independientemente del período de retención de copia de seguridad establecido. Si el consumo del almacenamiento de copia de seguridad se encuentra dentro del espacio de almacenamiento de copia de seguridad asignado, las copias de seguridad automatizadas en la instancia administrada no tendrán ningún costo adicional para usted, por lo que serán gratuitas. Si se supera el uso del almacenamiento de copia de seguridad por encima del espacio disponible, se aplicará un costo de aproximadamente 0,20 USD a 0,24 USD por GB/mes en las regiones de EE. UU. Consulte la página de precios para obtener más información sobre su región. Para obtener más información, consulte la [explicación sobre el consumo del almacenamiento de copia de seguridad](https://techcommunity.microsoft.com/t5/azure-sql-database/backup-storage-consumption-on-managed-instance-explained/ba-p/1390923).

**¿Cómo puedo supervisar el costo de facturación del consumo de almacenamiento de copia de seguridad?**

Puede supervisar el costo del almacenamiento de copia de seguridad a través de Azure Portal. Para obtener instrucciones, consulte [Supervisión de los costos de las copias de seguridad automatizadas](https://docs.microsoft.com/azure/azure-sql/database/automated-backups-overview?tabs=managed-instance#monitor-costs). 

**¿Cómo puedo optimizar mis costos de almacenamiento de copia de seguridad en la instancia administrada?**

Para optimizar los costos de almacenamiento de copia de seguridad, consulte el artículo sobre la [optimización de la copia de seguridad en SQL Managed Instance](https://techcommunity.microsoft.com/t5/azure-sql-database/fine-tuning-backup-storage-costs-on-managed-instance/ba-p/1390935).

## <a name="password-policy"></a>Directiva de contraseñas 

**¿Qué directivas de contraseña se aplican a los inicios de sesión SQL de Instancia administrada de SQL?**

La Directiva de contraseña de Instancia administrada de SQL para inicios de sesión de SQL hereda las directivas de la plataforma Azure que se aplican a las máquinas virtuales que forman el clúster virtual que contiene la instancia administrada. En este momento no es posible cambiar ninguno de estos valores, ya que estos se definen en Azure y los hereda la instancia administrada.

 > [!IMPORTANT]
 > La plataforma Azure puede cambiar los requisitos de directiva sin notificar a los servicios que dependen de esas directivas.

**¿Cuáles son las directivas actuales de la plataforma Azure?**

Cada inicio de sesión tiene que establecer su contraseña al iniciar sesión y cambiarla una vez que alcance la antigüedad máxima.

| **Directiva** | **Configuración de seguridad** |
| --- | --- |
| Antigüedad máxima de contraseña | 42 días |
| Antigüedad mínima de contraseña | 1 día |
| Longitud mínima de contraseña | 10 caracteres |
| La contraseña tiene que cumplir los requisitos de complejidad | habilitado |

**¿Es posible deshabilitar la complejidad de la contraseña y la expiración en Instancia administrada de SQL en el nivel de inicio de sesión?**

Sí, los campos CHECK_POLICY y CHECK_EXPIRATION se pueden controlar en el nivel de inicio de sesión. Puede comprobar la configuración actual ejecutando el siguiente comando de T-SQL:

```sql
SELECT *
FROM sys.sql_logins
```

Después, puede modificar valores de inicio de sesión especificados ejecutando:

```sql
ALTER LOGIN <login_name> WITH CHECK_POLICY = OFF;
ALTER LOGIN <login_name> WITH CHECK_EXPIRATION = OFF;
```

(reemplace "Test" por el nombre de inicio de sesión deseado y ajuste los valores de directiva y expiración)

## <a name="azure-feedback-and-support"></a>Comentarios y soporte técnico de Azure

**¿Dónde puedo dejar mis ideas de mejora de SQL Managed Instance?**

Puede votar por una nueva característica de Instancia administrada o someter una nueva idea de mejora a votación en el [Foro de comentarios de SQL Managed Instance](https://feedback.azure.com/forums/915676-sql-managed-instance). De este modo, puede contribuir al desarrollo del producto y ayudarnos a priorizar nuestras posibles mejoras.

**¿Cómo puedo crear una solicitud de soporte técnico de Azure?**

Para obtener información acerca de cómo crear una solicitud de soporte técnico de Azure, consulte [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).

