---
title: Retención de copia de seguridad a largo plazo
description: Obtenga información sobre la forma en que Azure SQL Database admite el almacenamiento de copias de seguridad de bases de datos completas durante un máximo de 10 años a través de la directiva de retención a largo plazo.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 05/18/2019
ms.openlocfilehash: 6fc5fab9ae61c8c8ade9260b32078ffa430b077f
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83771407"
---
# <a name="azure-sql-database-long-term-retention"></a>Retención a largo plazo de Azure SQL Database

Muchas aplicaciones tienen finalidades normativas, de conformidad u otras de carácter empresarial que requieren que se conserven copias de seguridad de las bases de datos más allá de entre los 7 y 35 días proporcionados por las [copias de seguridad automáticas](sql-database-automated-backups.md) de Azure SQL Database. Con la característica Retención a largo plazo, puede almacenar copias de seguridad completas de SQL Database en Azure Blob Storage con almacenamiento con redundancia geográfica con acceso de lectura durante un máximo de 10 años. Así, podrá restaurar cualquier copia de seguridad como si fuera una base de datos nueva. Para más información sobre la redundancia de Azure Storage, consulte [Redundancia de Azure Storage](../storage/common/storage-redundancy.md). 

La retención a largo plazo se puede habilitar para bases de datos únicas y agrupadas, y se encuentra en versión preliminar pública limitada para las instancias administradas de Azure SQL Database. 

> [!NOTE]
> Puede usar los trabajos del Agente SQL para programar [copias de seguridad de base de datos de solo copia](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) como alternativa a la retención a largo plazo después de 35 días.


## <a name="how-sql-database-long-term-retention-works"></a>Funcionamiento de la retención a largo plazo de SQL Database

La retención a largo plazo (LTR) aprovecha las copias de seguridad de base de datos completas que se [crean automáticamente](sql-database-automated-backups.md) para hacer posible la restauración a un momento dado (PITR). Si se configura una directiva LTR, estas copias de seguridad se copian en diferentes blobs para almacenarlas a largo plazo. La copia es un trabajo en segundo plano que no afecta al rendimiento de la carga de trabajo de la base de datos. La directiva LTR de cada base de datos SQL también puede especificar con qué frecuencia se crean las copias de seguridad LTR.

Para habilitar LTR, puede definir la directiva mediante una combinación de cuatro parámetros: retención de copia de seguridad semanal (W), retención de copia de seguridad mensual (M), retención de copia de seguridad anual (Y) y semana del año (WeekOfYear). Si especifica W, se copiará una copia de seguridad cada semana en el almacenamiento a largo plazo. Si especifica M, la primera copia de seguridad de cada mes se copiará en el almacenamiento a largo plazo. Si especifica Y, se copiará una copia de seguridad durante la semana especificada en WeekOfYear en el almacenamiento a largo plazo. Si el valor de WeekOfYear especificado pertenece al pasado al configurar la directiva, la primera copia de seguridad de LTR se creará el año siguiente. Cada copia de seguridad se conservará en el almacenamiento a largo plazo según los parámetros de la directiva configurados al crear la copia de seguridad de LTR.

> [!NOTE]
> Cualquier cambio en la directiva LTR se aplica solo a las copias de seguridad futuras. Por ejemplo, si se modifican los parámetros de retención de copia de seguridad semanal (W), retención de copia de seguridad mensual (M) o retención de copia de seguridad anual (Y), la nueva configuración de retención solo se aplicará a las copias de seguridad nuevas. La retención de las copias de seguridad existentes no se modificará. Si su intención es eliminar las copias de seguridad de LTR anteriores antes de que expire su período de retención, tendrá que [eliminar manualmente las copias de seguridad](https://docs.microsoft.com/azure/sql-database/sql-database-long-term-backup-retention-configure#delete-ltr-backups).
> 

Ejemplos de la directiva LTR:

-  W=0, M=0, Y=5, WeekOfYear=3

   La tercera copia de seguridad completa de cada año se conservará durante cinco años.
   
- W=0, M=3, Y=0

   La primera copia de seguridad completa de cada mes se conservará durante tres meses.

- W=12, M=0, Y=0

   Cada copia de seguridad completa semanal se conservará durante 12 semanas.

- W=6, M=12, Y=10, WeekOfYear=16

   Cada copia de seguridad completa semanal se conservará durante seis semanas. Excepto la primera copia de seguridad completa de cada mes, que se conservará durante 12 meses. Excepto la copia de seguridad completa realizada en la 16ª semana del año, que se conservará durante 10 años. 

En la tabla siguiente se muestra la cadencia y caducidad de las copias de seguridad a largo plazo para la siguiente directiva:

W=12 semanas (84 días), M=12 meses (365 días), Y=10 años (3650 días), WeekOfYear=15 (semana posterior al 15 de abril)

   ![Ejemplo de LTR](./media/sql-database-long-term-retention/ltr-example.png)



Si modifica la directiva anterior y establece W=0 (sin copias de seguridad semanales), la cadencia de las copias de seguridad cambiará tal y como se muestra en la tabla anterior en función de las fechas destacadas. La cantidad de almacenamiento necesaria para mantener estas copias de seguridad se reduciría según corresponda. 

> [!IMPORTANT]
> Azure SQL Database controla los intervalos en que se hacen las copias de seguridad de LTR individuales. No se puede crear una copia de seguridad de LTR manualmente ni controlar los intervalos de creación de copias de seguridad. Después de configurar una directiva LTR, pueden transcurrir hasta siete días para que la primera copia de seguridad de LTR aparezca en la lista de copias de seguridad disponibles.  
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Retención de copia de seguridad a largo plazo y replicación geográfica

Si usa grupos de conmutación por error o de replicación geográfica activa como su solución de continuidad del negocio, debe prepararse para posibles conmutaciones por error y configurar la misma directiva LTR en la base de datos geográfica secundaria. El costo de almacenamiento de LTR no aumentará, ya que las copias de seguridad no se generan desde las bases de datos secundarias. Solo cuando la base de datos secundaria se convierte en principal se crearán las copias de seguridad. Esto garantiza una generación de las copias de seguridad LTR ininterrumpida cuando se desencadene la conmutación por error y la base de datos principal se mueva a la región secundaria. 

> [!NOTE]
> Cuando la base de datos principal original se recupere de una interrupción que provoque la conmutación por error, se convertirá en una nueva base de datos secundaria. Por lo tanto, no se reanudará la creación de copia de seguridad y la directiva LTR existente no surtirá efecto hasta que vuelva a ser la base de datos principal de nuevo. 

## <a name="managed-instance-support"></a>Compatibilidad con Instancia administrada

El uso de la retención de copias de seguridad a largo plazo con instancias administradas de Azure SQL Database tiene las siguientes limitaciones:

- **Versión preliminar pública limitada**: esta versión preliminar solo está disponible para las suscripciones EA y CSP, y está sujeta a una disponibilidad limitada.  
- [**Solo PowerShell**](sql-database-managed-instance-long-term-backup-retention-configure.md): actualmente no hay compatibilidad con Azure Portal. La retención a largo plazo sebe habilitarse mediante PowerShell. 

Para solicitar la inscripción, cree una [incidencia de Soporte técnico de Azure](https://azure.microsoft.com/support/create-ticket/). Como tipo de incidencia, seleccione Problema técnico, como servicio elija Instancia administrada de SQL Database y, como tipo de problema, seleccione **Backup, restauración y continuidad empresarial/Retención de copias de seguridad a largo plazo**. En la solicitud, indique que le gustaría inscribirse en la versión preliminar pública limitada de LTR de la instancia administrada.

## <a name="configure-long-term-backup-retention"></a>Configuración de la retención de copia de seguridad a largo plazo

Para más información sobre cómo configurar la retención a largo plazo mediante Azure Portal o mediante PowerShell, vea [Administración de la retención de copias de seguridad a largo plazo de Azure SQL Database](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-database-from-ltr-backup"></a>Restaurar la base de datos a partir de una copia de seguridad de LTR

Para restaurar una base de datos desde el almacenamiento de LTR, puede seleccionar una copia de seguridad específica en función de su marca de tiempo. La base de datos se puede restaurar en cualquier servidor existente en la misma suscripción que la base de datos original. Para más información sobre cómo restaurar la base de datos a partir de una copia de seguridad de LTR mediante Azure Portal o PowerShell, vea [Administración de la retención de copias de seguridad a largo plazo de Azure SQL Database](sql-database-long-term-backup-retention-configure.md). En la solicitud, indique que le gustaría inscribirse en la versión preliminar pública limitada de LTR de la instancia administrada.

## <a name="next-steps"></a>Pasos siguientes

Dado que las copias de seguridad de bases de datos protegen los datos de eliminaciones o daños accidentales, son una parte esencial de cualquier estrategia de recuperación ante desastres y de continuidad empresarial. Para descubrir otras soluciones de continuidad empresarial de SQL Database, consulte el artículo de [información general sobre la continuidad empresarial](sql-database-business-continuity.md).
