---
title: Preguntas frecuentes sobre instancia administrada
description: Preguntas frecuentes acerca de Instancia administrada de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: 99fbda6f6d5e8fc88f9f4f34c6e194412a120057
ms.sourcegitcommit: acc558d79d665c8d6a5f9e1689211da623ded90a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2020
ms.locfileid: "82598518"
---
# <a name="sql-database-managed-instance-frequently-asked-questions-faq"></a>Preguntas frecuentes acerca de Instancia administrada de Azure SQL Database

En este artículo encontrará muchas de las preguntas más frecuentes acerca de [Instancia administrada de SQL Database](sql-database-managed-instance.md).

## <a name="supported-features"></a>Características admitidas

**¿Dónde puedo encontrar una lista de las características que se admiten en una instancia administrada?**

Para obtener una lista de las características admitidas en una instancia administrada, consulte [Azure SQL Database frente a SQL Server](sql-database-features.md).

Para conocer las diferencias de sintaxis y comportamiento entre Instancia administrada de Azure SQL Database y SQL Server local, consulte [Diferencias entre T-SQL y SQL Server](sql-database-managed-instance-transact-sql-information.md).


## <a name="tech-spec--resource-limits"></a>Especificaciones técnicas y límites de recursos
 
**¿Dónde puedo encontrar las características técnicas y los límites de recursos de una instancia administrada?**

Para conocer las características disponibles para la generación de hardware, consulte las [diferencias técnicas en las generaciones de hardware](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).
Para ver los niveles de servicio disponibles y sus características, consulte las [diferencias técnicas entre los niveles de servicio](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="known-issues--bugs"></a>Errores y problemas conocidos

**¿Dónde puedo encontrar soluciones a errores y problemas conocidos?**

Para ver los errores y los problemas conocidos, consulte [Problemas conocidos](sql-database-release-notes.md#known-issues).

## <a name="new-features"></a>Nuevas características

**¿Dónde puedo encontrar las características más recientes y las características en versión preliminar pública?**

Para obtener características nuevas y en vista previa, consulte las [notas de la versión](sql-database-release-notes.md?tabs=managed-instance).

## <a name="deployment-times"></a>Tiempos de implementación 

**¿Cuánto tiempo se tarda en crear o actualizar una instancia, o en restaurar una base de datos?**

El tiempo esperado para crear una nueva instancia administrada o cambiar el nivel de servicio (núcleos virtuales, almacenamiento) depende de varios factores. Eche un vistazo a las [operaciones de administración](/azure/sql-database/sql-database-managed-instance#managed-instance-management-operations). 

## <a name="naming-convention"></a>Convención de nomenclatura

**¿Puede una instancia administrada tener el mismo nombre que una instancia de SQL Server local?**

No se admite cambiar el nombre de la instancia administrada.

Se puede cambiar la zona DNS predeterminada *.database.windows.net* de la instancia administrada. 

Para usar otra zona DNS en lugar de la predeterminada, por ejemplo, *.contoso.com*, haga lo siguiente: 
- Use CliConfig para definir un alias. La herramienta no es más que un contenedor de la configuración del registro, por lo que también se puede hacer mediante una directiva de grupo o un script.
- Use *CNAME* con la opción *TrustServerCertificate = true*.

## <a name="move-db-from-mi"></a>Traslado de bases de datos desde una instancia administrada 

**¿Cómo puedo trasladar una base de datos de una instancia administrada a SQL Server o Azure SQL Database?**

Puede [exportar la base de datos a BACPAC](sql-database-export.md) y, a continuación, [importar el archivo BACPAC]( sql-database-import.md). Este enfoque es aconsejable si la base de datos tiene menos de 100 GB.

La replicación transaccional se puede utilizar si todas las tablas de la base de datos tienen claves principales.

Las copias de seguridad nativas `COPY_ONLY` realizadas de una instancia administrada no se pueden restaurar en SQL Server, porque la instancia administrada tiene una versión superior de la base de datos en comparación con SQL Server.

## <a name="migrate-instance-db"></a>Migración de bases de datos de instancia

**¿Cómo puedo migrar mi base de datos de instancia a una instancia individual de Azure SQL Database?**

Una opción es [exportar la base de datos a un archivo BACPAC](sql-database-export.md) y, a continuación, [importar dicho archivo](sql-database-import.md). 

Este enfoque es aconsejable si la base de datos tiene menos de 100 GB. La replicación transaccional se puede utilizar si todas las tablas de la base de datos tienen claves principales.

## <a name="switch-hardware-generation"></a>Cambio de generación de hardware 

**¿Puedo cambiar la generación de hardware de mi instancia administrada entre Gen 4 y Gen 5 en línea?**

La conmutación en línea automatizada entre dos generaciones de hardware es posible si ambas están disponibles en la misma región en la que está aprovisionada la instancia administrada. En este caso, puede comprobar la [página Introducción al modelo de núcleos virtuales](sql-database-service-tiers-vcore.md), que explica cómo pasar de una generación de hardware a otra.

Se trata de una operación de larga duración, ya que la nueva instancia administrada se aprovisionará en segundo plano y en las bases de datos transferidas automáticamente entre la instancia antigua y la nueva, con una conmutación por error rápida al final del proceso. 

**¿Qué ocurre si ambas generaciones de hardware no se admiten en la misma región?**

Si las dos generaciones de hardware no se admiten en la misma región, es posible cambiar una de ellas, pero se debe hacer manualmente. Esto requiere que se aprovisione una nueva instancia en la región en que esté disponible la generación de hardware que se quiera llevar a cabo, y que se realice una copia de seguridad manual y una restauración de los datos entre las instancias antigua y nueva.

**¿Qué ocurre si no hay suficientes direcciones IP para realizar la operación de actualización?**

En caso de que no haya suficientes direcciones IP en la subred en la que se aprovisiona la instancia administrada, tendrá que crear una nueva subred y una nueva instancia administrada dentro de ella. También se recomienda crear una nueva subred con más direcciones IP asignadas para que las operaciones de actualización futuras eviten una situación similar (para conocer el tamaño de subred adecuado, consulte [cómo determinar el tamaño de la subred de la red virtual](sql-database-managed-instance-determine-size-vnet-subnet.md). Una vez aprovisionada la nueva instancia, puede realizar manualmente una copia de seguridad de los datos y restaurarlos entre la instancia antigua y la nueva, o bien realizar una [restauración a un momento dado](sql-database-managed-instance-point-in-time-restore.md?tabs=azure-powershell). 


## <a name="tune-performance"></a>Ajustar rendimiento

**¿Cómo se optimiza el rendimiento de una instancia administrada?**

Una instancia administrada de uso general usa el almacenamiento remoto debido a que el tamaño de los archivos de registro y datos es importante para el rendimiento. Para obtener más información, consulte [Impact of log file size on General Purpose Managed Instance performance](https://medium.com/azure-sqldb-managed-instance/impact-of-log-file-size-on-general-purpose-managed-instance-performance-21ad170c823e) (Impacto del tamaño del archivo de registro en el rendimiento de la Instancia administrada de uso general).

Si la carga de trabajo consta de muchas transacciones pequeñas, considere la posibilidad de cambiar el tipo de conexión de proxy a modo de redirección.

## <a name="maximum-storage-size"></a>Tamaño máximo de almacenamiento

**¿Cuál es el tamaño máximo de almacenamiento de una instancia administrada?**

El tamaño de almacenamiento de una instancia administrada depende del nivel de servicio seleccionado (De uso general o Crítico para la empresa). Para conocer las limitaciones de almacenamiento de estos niveles de servicio, consulte [Características de los niveles de servicio](sql-database-service-tiers-general-purpose-business-critical.md).

## <a name="back-up-storage-cost"></a>Costo de almacenamiento de copia de seguridad 

**¿Se deduce el almacenamiento de copia de seguridad del almacenamiento de mi instancia administrada?**

No, el almacenamiento de copia de seguridad no se deduce del espacio de almacenamiento de su instancia administrada. El almacenamiento de copia de seguridad es independiente del espacio de almacenamiento de la instancia y su tamaño no está limitado. El almacenamiento de copia de seguridad está limitado por el período de tiempo durante el que se conserva la copia de seguridad de las bases de datos de la instancia; se puede configurar entre 7 y 35 días. Para más información, consulte [Copias de seguridad automatizadas](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).

## <a name="track-billing"></a>Seguimiento de la facturación

**¿Hay alguna manera de realizar un seguimiento del costo de facturación de mi instancia administrada?**

Puede hacerlo con la [solución Azure Cost Management](/azure/cost-management/). Navegue a **Suscripciones** en [Azure Portal](https://portal.azure.com) y seleccione **Análisis de costos**. 

Use la opción **Costos acumulados** y luego filtre por **Tipo de recurso**, como `microsoft.sql/managedinstances`. 
  
## <a name="inbound-nsg-rules"></a>Reglas de NSG de entrada

**¿Cómo se pueden establecer reglas de grupos de seguridad de red de entrada en los puertos de administración?**

El plano de control de la instancia administrada mantiene las reglas de NSG que protegen los puertos de administración.

Esto es para lo que se usan los puertos de administración:

La infraestructura de Service Fabric usa los puertos 9000 y 9003. El rol principal de Service Fabric consiste en mantener siempre el clúster virtual en un estado correcto y en mantener el estado del objetivo en cuanto al número de réplicas de componente.

Node Agent utiliza los puertos 1438, 1440 y 1452. Node Agent es una aplicación que se ejecuta dentro del clúster y que el plano de control usa para ejecutar comandos de administración.

Además de las reglas de NSG, el firewall integrado protege la instancia en el nivel de red. En el nivel de aplicación, la comunicación está protegida con los certificados.
  
Para más información acerca del firewall integrado y acerca de cómo verificarlo, consulte [Firewall integrado en Instancia administrada de Azure SQL Database](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).


## <a name="mitigate-data-exfiltration-risks"></a>Mitigación de los riesgos de filtración de datos  

**¿Cómo se pueden mitigar los riesgos de filtración de datos?**

Para mitigar los riesgos de filtración de datos, es aconsejable que los clientes apliquen un conjunto de valores y controles de seguridad:

- Activar el [cifrado de datos transparente (TDE)](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql) en todas las bases de datos.
- Desactivar Common Language Runtime (CLR). Se recomienda hacerlo también en el entorno local.
- Usar solo la autenticación de Azure Active Directory (AAD).
- Acceder a la instancia con una cuenta de DBA con pocos privilegios.
- Configurar el acceso del jumpbox de JiT a la cuenta sysadmin.
- Activar la [auditoría de SQL](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) e integrarla en los mecanismos de alerta.
- Activar la [detección de amenazas](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection) del conjunto de pruebas de [Advanced Data Security (ADS)](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).


## <a name="cost-saving-use-cases"></a>Casos de uso con ahorro de costos

**¿Dónde puedo encontrar casos de uso y el ahorro de costos resultante con una instancia administrada?**

Casos prácticos de instancia administrada:

- [Komatsu](https://customers.microsoft.com/story/komatsu-australia-manufacturing-azure)
- [KMD](https://customers.microsoft.com/en-ca/story/kmd-professional-services-azure-sql-database)
- [PowerDETAILS](https://customers.microsoft.com/story/powerdetails-partner-professional-services-azure-sql-database-managed-instance)
- [Allscripts](https://customers.microsoft.com/story/allscripts-partner-professional-services-azure)   
Existe un estudio de Forrester en el que se explican a la perfección los beneficios, costos y riesgos asociados a la implementación de Instancia administrada de Azure SQL Database: [Total Economic Impact of MI](https://azure.microsoft.com/resources/forrester-tei-sql-database-managed-instance) (Impacto económico total de Instancia administrada de Azure SQL Database).


## <a name="dns-refresh"></a>Actualización de DNS 

**¿Puedo actualizar el DNS?**

Actualmente, no proporcionamos ninguna característica para actualizar la configuración del servidor DNS para la instancia administrada.

La configuración de DNS se actualiza:

- Cuando expira la concesión de DHCP.
- Al actualizar la plataforma.

Como alternativa, cambie la instancia administrada a la versión 4 del núcleo virtual y vuelva a actualizarla posteriormente. Esto tiene el efecto secundario de actualizar la configuración de DNS.


## <a name="ip-address"></a>Dirección IP

**¿Se puede realizar una conexión a una instancia administrada mediante la dirección IP?**

No se admite la conexión a una instancia administrada mediante la dirección IP. El nombre de host de la instancia administrada se asigna al equilibrador de carga delante del clúster virtual de instancia administrada. Puesto que un clúster virtual podría hospedar varias instancias administradas, es posible que la conexión no se pueda enrutar a la instancia administrada adecuada sin especificar su nombre.

Para obtener más información sobre la arquitectura del clúster virtual de instancia administrada, vea [Arquitectura de conectividad del clúster virtual](sql-database-managed-instance-connectivity-architecture.md#virtual-cluster-connectivity-architecture).

**¿Puede una instancia administrada tener una dirección IP estática?**

En situaciones poco frecuentes pero necesarias, es posible que tengamos que realizar una migración en línea de una instancia administrada a un nuevo clúster virtual. Si es necesaria, esta migración se debe a los cambios realizados en nuestra pila de tecnología destinados a mejorar la seguridad y confiabilidad del servicio. La migración a un nuevo clúster virtual provoca el cambio de dirección IP que está asignada al nombre de host de la instancia administrada. El servicio de instancia administrada no solicita compatibilidad con direcciones IP estáticas y se reserva el derecho a cambiar la dirección IP sin previo aviso como parte de los ciclos de mantenimiento regular.

Por este motivo se desaconseja confiar en la inmutabilidad de la dirección IP, ya que podría provocar un tiempo de inactividad innecesario.

## <a name="change-time-zone"></a>Cambio de la zona horaria

**¿Puedo cambiar la zona horaria de una instancia administrada existente?**

La configuración de la zona horaria se puede establecer la primera vez que se aprovisiona una instancia administrada. No se admite el cambio de zona horaria de la instancia administrada existente. Para más información, consulte [limitaciones de la zona horaria](sql-database-managed-instance-timezone.md#limitations).

Las soluciones alternativas incluyen la creación de una nueva instancia administrada con la zona horaria adecuada y la posterior realización de una copia de seguridad y una restauración de forma manual, o lo que recomendamos, realizar una [restauración entre instancias en un momento específico](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/07/cross-instance-point-in-time-restore-in-azure-sql-database-managed-instance/).


## <a name="resolve-performance-issues"></a>Resolución de problemas de rendimiento

**¿Cómo resuelvo los problemas de rendimiento de mi instancia administrada?**

Para obtener una comparación del rendimiento entre una instancia administrada y SQL Server, un buen punto de partida es leer los [procedimientos recomendados para la comparación de rendimiento entre la Instancia administrada de Azure SQL y SQL Server](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/The-best-practices-for-performance-comparison-between-Azure-SQL/ba-p/683210).

Las cargas de datos suelen ser más lentas en una instancia administrada que en SQL Server debido al modelo de recuperación completa obligatorio y a los [límites](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics) en el rendimiento de la escritura en el registro de transacciones. En ocasiones, esto se puede solucionar mediante la carga de datos transitorios en tempdb, en lugar de en la base de datos de usuario, o bien mediante el uso de un almacén de columnas agrupado o tablas optimizadas para memoria.


## <a name="restore-encrypted-backup"></a>Restauración de copia de seguridad cifrada

**¿Puedo restaurar mi base de datos cifrada en una instancia administrada?**

Sí, no es preciso descifrar la base de datos para poder restaurarla en una instancia administrada. Debe especificar la clave o el certificado utilizados como un protector de clave de cifrado en el sistema de origen a la instancia administrada para poder leer los datos desde el archivo de copia de seguridad cifrado. Existen dos formas posibles de hacerlo:

- *Cargar un protector de certificado en la instancia administrada*. Solo se puede hacer mediante PowerShell. El [script de ejemplo](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-migrate-tde-certificate) describe todo el proceso.
- *Cargar el protector de clave asimétrica a Azure Key Vault (AKV) y apuntar la instancia administrada hacia él*. Este enfoque es similar al caso de uso del cifrado de datos transparente de Bring Your Own Key (BYOK), que también usa la integración de Azure Key Vault para almacenar la clave de cifrado. Si no quiere usar la clave como un protector de clave de cifrado y solo quiere que esté disponible para la instancia administrada y así poder restaurar las bases de datos cifradas, siga las instrucciones para [configurar BYOK TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#manage-transparent-data-encryption), y no seleccione la casilla *Hacer que la clave seleccionada sea el protector de TDE predeterminado*.

Una vez que el protector de cifrado está disponible para una instancia administrada, puede continuar con el procedimiento de restauración de base de datos estándar.

## <a name="migrate-from-single-db"></a>Migración desde una única base de datos 

**¿Cómo puedo migrar un grupo individual o un grupo de elástico de Azure SQL Database a instancia administrada?**

Una instancia administrada ofrece los mismos niveles de rendimiento por proceso y por tamaño de almacenamiento que otras opciones de implementación de Azure SQL Database. Si desea consolidar los datos en una sola instancia o simplemente necesita una característica admitida exclusivamente en una instancia administrada, puede migrar los datos mediante la funcionalidad de exportación e importación (BACPAC).
