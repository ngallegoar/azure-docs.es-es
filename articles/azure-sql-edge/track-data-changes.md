---
title: Seguimiento de cambios de datos en Azure SQL Edge (versión preliminar)
description: Información sobre el seguimiento de cambios y la captura de datos modificados en Azure SQL Edge (versión preliminar)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 3e3a6c3e171383d1812f63b945735d1b82f70b9c
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235098"
---
# <a name="tracking-data-changes-in-azure-sql-edge-preview"></a>Seguimiento de cambios de datos en Azure SQL Edge (versión preliminar)

Azure SQL Edge proporciona dos características de SQL Server que realizan el seguimiento de los cambios en los datos de una base de datos: [seguimiento de cambios](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#Tracking) y [captura de datos modificados](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#Capture). Estas características permiten a las aplicaciones determinar los cambios de DML (operaciones de inserción, actualización y eliminación) que se realizaron en las tablas de usuario de una base de datos. La captura de datos modificados y el seguimiento de cambios pueden habilitarse en la misma base de datos; no se requiere ninguna consideración especial.

La capacidad para consultar los datos que han cambiado en una base de datos es un requisito importante para conseguir que algunas aplicaciones sean eficaces. Normalmente, para determinar los datos modificados, los desarrolladores de aplicaciones deben implementar un método de seguimiento personalizado en sus aplicaciones utilizando una combinación de desencadenadores, columnas de marca de tiempo y tablas adicionales. La creación de estas aplicaciones implica normalmente mucho trabajo de implementación, provoca actualizaciones del esquema y, a menudo, supone una gran sobrecarga en el rendimiento. En el caso de la solución de IoT, donde es necesario trasladar datos periódicamente desde el borde a una nube o un centro de datos, el seguimiento de cambios puede ser muy útil. Esto permitiría su uso para consultar de forma rápida y eficaz solo los cambios diferenciales desde la última sincronización y cargar esos cambios en el destino del centro de datos o la nube. Para obtener más información sobre las ventajas del uso de Change Tracking y la captura de datos modificados, consulte [Ventajas de utilizar la captura de datos modificados y el seguimiento de cambios](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#benefits-of-using-change-data-capture-or-change-tracking). 

Para comprender las diferencias de características entre el seguimiento de cambios y la captura de datos modificados, consulte [Diferencias de características entre la captura de datos modificados y el seguimiento de cambios](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#feature-differences-between-change-data-capture-and-change-tracking).

## <a name="change-data-capture"></a>Captura de datos modificados

Para comprender los detalles de cómo funciona la captura de datos modificados, consulte [Acerca de la captura de datos modificados](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server).

Para saber cómo habilitar o deshabilitar la captura de datos modificados, consulte [Habilitar y deshabilitar la captura de datos modificados](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server).

Para administrar y supervisar la captura de datos modificados, consulte [Administrar y supervisar la captura de datos modificados](https://docs.microsoft.com/sql/relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server).

Para saber cómo consultar y trabajar con los datos modificados, consulte [Trabajar con datos modificados](https://docs.microsoft.com/sql/relational-databases/track-changes/work-with-change-data-sql-server).

## <a name="change-tracking"></a>Seguimiento de cambios

Para comprender los detalles de cómo funciona el seguimiento de cambios, consulte [Acerca del seguimiento de cambios](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server).

Para saber cómo habilitar o deshabilitar el seguimiento de cambios, consulte [Habilitar y deshabilitar el seguimiento de cambios](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server).

Para administrar y supervisar el seguimiento de cambios, consulte [Administrar el seguimiento de cambios](https://docs.microsoft.com/sql/relational-databases/track-changes/manage-change-tracking-sql-server).

Para saber cómo consultar y trabajar con los datos modificados, consulte [Trabajar con datos modificados](https://docs.microsoft.com/sql/relational-databases/track-changes/work-with-change-tracking-sql-server).

## <a name="temporal-tables"></a>Tablas temporales

Además de admitir las características de seguimiento de cambios y captura de datos modificados de SQL Server, Azure SQL Edge también admite la característica de tablas temporales de SQL Server. Las tablas temporales (también conocidas como tablas temporales con versión del sistema) son una característica de base de datos que ofrece soporte integrado, lo que permite proporcionar información sobre los datos almacenados en la tabla en cualquier momento en el tiempo, en vez de únicamente los datos que son correctos en el momento actual determinado.

Una tabla temporal con versión del sistema es un tipo de tabla de usuario pensada para conservar un historial completo de los cambios de datos y para facilitar los análisis en un momento específico. Este tipo de tabla temporal se conoce como tabla temporal con versión del sistema, porque el período de validez de cada fila se administra por medio del sistema (es decir, del motor de base de datos).

Cada tabla temporal tiene dos columnas definidas explícitamente, cada una con un tipo de datos datetime2. Estas columnas se conocen como columnas de periodo. Estas columnas de periodo son de uso exclusivo por parte del sistema para registrar el período de validez de cada fila cada vez que una fila se modifica.

Aparte de estas columnas, una tabla temporal contiene también una referencia a otra tabla con un esquema reflejado. El sistema usa esta tabla para almacenar automáticamente la versión anterior de una fila de la tabla temporal cada vez que dicha fila se actualiza o elimina. Esta tabla adicional se conoce como tabla de historial, mientras que la tabla principal que almacena las versiones de fila actuales (reales) se conoce como tabla actual o, simplemente, como tabla temporal. Durante la creación de una tabla temporal, los usuarios pueden especificar una tabla de historial existente (debe admitir esquemas) o dejar que el sistema cree una tabla de historial predeterminada.

Para obtener más información sobre las tablas temporales, consulte [Tablas temporales](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables).

## <a name="see-also"></a>Consulte también

- [Streaming de datos en Azure SQL Edge (versión preliminar)](stream-data.md)
- [Aprendizaje automático e IA con ONNX en Azure SQL Edge (versión preliminar)](onnx-overview.md)
- [Configuración de la replicación en Azure SQL Edge (versión preliminar)](configure-replication.md)
- [Copia de seguridad y restauración de bases de datos en Azure SQL Edge (versión preliminar)](backup-restore.md)



