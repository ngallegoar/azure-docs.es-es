---
title: Copias de seguridad en línea y restauración de datos a petición en Azure Cosmos DB
description: Este artículo describe cómo funcionan en Azure Cosmos DB las copias de seguridad automáticas en línea y la restauración de datos a petición.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 8ed9e23b178b8eeefbd3c3a690491124e6901180
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85112929"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Copias de seguridad en línea y restauración de datos a petición en Azure Cosmos DB

Azure Cosmos DB crea automáticamente copias de seguridad de los datos a intervalos regulares. Las copias de seguridad automáticas se crean sin afectar al rendimiento o a la disponibilidad de las operaciones de las bases de datos. Todas las copias de seguridad se almacenan por separado en un servicio de almacenamiento y se replican globalmente para lograr resistencia frente a desastres regionales. Las copias de seguridad automáticas son útiles en escenarios en los se elimina accidentalmente o se actualiza su cuenta, base de datos o contenedor de Azure Cosmos y más tarde se requiere la recuperación de datos.

## <a name="automatic-and-online-backups"></a>Copias de seguridad automáticas y en línea

Con Azure Cosmos DB no solo los datos, sino también las copias de seguridad de los mismos, son altamente redundantes y resistentes ante los desastres regionales. Los pasos siguientes muestran cómo Azure Cosmos DB realiza la copia de seguridad de datos:

* Azure Cosmos DB toma automáticamente una copia de seguridad de la base de datos cada 4 horas y en cualquier momento, solo se almacenan las últimas 2. Pero si se elimina el contenedor o la base de datos, Azure Cosmos DB conserva las instantáneas existentes de un contenedor o base de datos dado durante 30 días.

* Azure Cosmos DB almacena estas copias de seguridad en Azure Blob Storage, mientras que los datos reales residen localmente dentro de Azure Cosmos DB.

*  Para garantizar la baja latencia, la instantánea de la copia de seguridad se guarda en Azure Blob Storage en la misma región que la región de escritura actual (o una de las regiones de escritura, si tiene una configuración de arquitectura multimaestro) de la cuenta de Azure Cosmos Database. Para lograr resistencia frente a desastres regionales, cada instantánea de la copia de seguridad de los datos en Azure Blob Storage se vuelve a replicar en otra región mediante almacenamiento con redundancia geográfica (GRS). La región a la que se replica la copia de seguridad se basa en la región de origen y el par regional asociado con la región de origen. Para más información, consulte el artículo [lista de parejas regionales de Azure para redundancia geográfica](../best-practices-availability-paired-regions.md). No se puede acceder directamente a esta copia de seguridad. Azure Cosmos DB utilizará esta copia de seguridad solo si se inicia una restauración de copia de seguridad.

* Las copias de seguridad se crean sin afectar el rendimiento ni la disponibilidad de la aplicación. Azure Cosmos DB realiza la copia de seguridad de datos en segundo plano y no consume rendimiento aprovisionado (RU) aprovisionadas, tampoco afecta al rendimiento ni a la disponibilidad de su base de datos.

* Si accidentalmente ha eliminado o dañado los datos, debe ponerse en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/) en un plazo de ocho horas para que el equipo de Azure Cosmos DB pueda ayudarle a restaurar los datos a partir de las copias de seguridad.

La imagen siguiente muestra cómo se crea una copia de seguridad de un contenedor de Azure Cosmos con las tres particiones físicas principales en el Oeste de EE. UU. en una cuenta de Azure Blob Storage remota en el Oeste de EE. UU. y, luego, se replica al Este de EE. UU.:

:::image type="content" source="./media/online-backup-and-restore/automatic-backup.png" alt-text="Copias de seguridad completas y periódicas de todas las entidades de Cosmos DB en Azure Storage de GRS" border="false":::

## <a name="options-to-manage-your-own-backups"></a>Opciones para gestionar sus propias copias de seguridad

Con las cuentas de la API de SQL de Azure Cosmos DB, también puede mantener sus propias copias de seguridad mediante uno de los métodos siguientes:

* Con [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) para mover datos periódicamente a un almacenamiento de su elección.

* Con la [fuente de cambios](change-feed.md) de Azure Cosmos DB para leer datos periódicamente con el fin de crear tanto una copia de seguridad completa como de los cambios incrementales y almacenarlo todo en su propio almacenamiento.

## <a name="backup-retention-period"></a>Período de retención de copia de seguridad

Azure Cosmos DB captura instantáneas de los datos cada cuatro horas. En un momento dado del tiempo, solo las dos últimas instantáneas se conservan. Pero si se elimina el contenedor o la base de datos, Azure Cosmos DB conserva las instantáneas existentes de un contenedor o base de datos dado durante 30 días.

## <a name="restoring-data-from-online-backups"></a>Restauración de datos a partir de copias de seguridad en línea

La eliminación accidental o modificación de datos puede ocurrir en uno de los escenarios siguientes:  

* Se elimina toda la cuenta de Azure Cosmos

* Se eliminan una o más bases de datos de Azure Cosmos

* Se eliminan uno o más contenedores de Azure Cosmos

* Se eliminan o modifican elementos de Cosmos Azure (por ejemplo, los documentos) dentro de un contenedor. Este caso concreto normalmente se conoce como "datos dañados".

* Se elimina o se daña una base de datos de oferta compartida o los contenedores dentro de una base de datos de oferta compartida

Azure Cosmos DB puede restaurar los datos en todos los escenarios anteriores. El proceso de restauración siempre crea una nueva cuenta de Azure Cosmos para almacenar los datos restaurados. El nombre de la nueva cuenta, si no se especifica, tendrá el formato `<Azure_Cosmos_account_original_name>-restored1`. Si se realizan varios intentos de restauración, el último dígito se va incrementando. No se pueden restaurar los datos en una cuenta de Azure Cosmos creada previamente.

Cuando se elimina una cuenta de Azure Cosmos, podemos restaurar los datos en una cuenta con el mismo nombre, siempre que el nombre de cuenta no esté en uso. En tales casos, se recomienda no volver a crear la cuenta después de la eliminación, porque no solo impide que los datos restaurados usen el mismo nombre, sino que además dificulta la detección de la cuenta correcta que se quiere restaurar. 

Cuando se elimina una base de datos de Azure Cosmos, se puede restaurar la base de datos completa o un subconjunto de los contenedores dentro de la misma. También se pueden seleccionar contenedores en varias bases de datos y restaurarlos, todos los datos restaurados se colocan en una nueva cuenta de Azure Cosmos.

Cuando uno o varios elementos dentro de un contenedor se eliminan o cambian accidentalmente (el caso de los "datos dañados"), tendrá que especificar el momento al que se restaurará. En este caso el tiempo es esencial. Puesto que el contenedor está activo, las copias de seguridad se siguen ejecutando, por lo que si espera más allá del período de retención (el valor predeterminado es de ocho horas) se sobrescribirán las copias de seguridad. En el caso de eliminaciones, ya no se almacenan los datos, no es necesario porque el ciclo de copia de seguridad no los sobrescribe. Las copias de seguridad para las bases de datos o contenedores eliminados se guardan durante 30 días.

Si se aprovisiona el rendimiento en el nivel de base de datos (es decir, donde un conjunto de contenedores comparte el rendimiento aprovisionado), el proceso de copia de seguridad y restauración se producen a nivel de base de datos completa y no a nivel de contenedores individuales. En tales casos, no existe la opción de seleccionar un subconjunto de contenedores para restaurar.

## <a name="migrating-data-to-the-original-account"></a>Migración de datos a la cuenta original

El objetivo principal de la restauración de datos es proporcionar una manera de recuperar los datos que haya eliminado o modificado accidentalmente. Por lo tanto, se recomienda que primero inspeccione el contenido de los datos recuperados para garantizar que contiene lo que esperaba. Después realice la migración de los datos de vuelta a la cuenta principal. Aunque se puede usar la cuenta restaurada como cuenta activa, no es una opción recomendada si tiene cargas de trabajo de producción.  

Las siguientes son las diferentes formas para migrar datos a la cuenta de Azure Cosmos original:

* Con la [herramienta de migración de datos de Cosmos DB](import-data.md)
* Con [Azure Data Factory]( ../data-factory/connector-azure-cosmos-db.md)
* Con la [fuente de cambios](change-feed.md) en Azure Cosmos DB 
* Con un código personalizado

Elimine las cuentas restauradas tan pronto como termine la migración para evitar que se les apliquen gastos actuales.

## <a name="next-steps"></a>Pasos siguientes

A continuación puede aprender cómo restaurar los datos de una cuenta de Azure Cosmos o cómo migrar datos a una cuenta de Azure Cosmos

* Para hacer una solicitud de restauración, póngase en contacto con el servicio de soporte técnico de Azure y [presente una incidencia de soporte técnico en Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
* [Restauración de los datos desde una cuenta de Azure Cosmos](how-to-backup-and-restore.md)
* [Uso de la fuente de cambios en Cosmos DB](change-feed.md) para mover datos a Azure Cosmos DB.
* [Uso de Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) para mover datos a Azure Cosmos DB.

