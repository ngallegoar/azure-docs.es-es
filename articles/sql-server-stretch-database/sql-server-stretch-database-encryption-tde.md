---
title: Habilitación del cifrado de datos transparente para Stretch Database
description: Habilitación del cifrado de datos transparente (TDE) para SQL Server Stretch Database en Azure
services: sql-server-stretch-database
documentationcenter: ''
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/14/2016
author: blazem-msft
ms.author: blazem
ms.reviewer: jroth
manager: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 465338bb793ad7b4fc406e9d1c073b9dc0dc06c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034010"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Habilitación del cifrado de datos transparente (TDE) para Stretch Database en Azure
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

El cifrado de datos transparente (TDE) facilita la protección contra la amenaza de actividades malintencionadas, ya que la base de datos se cifra y descifra en tiempo real, se realizan copias de seguridad asociadas y archivos de registro de transacciones en reposo sin requerir cambios en la aplicación.

TDE cifra el almacenamiento de una base de datos completa mediante el uso de una clave simétrica denominada clave de cifrado de base de datos. La clave de cifrado de base de datos está protegida por un certificado de servidor integrado. El certificado de servidor integrado es único para cada servidor de Azure. Microsoft gira automáticamente estos certificados al menos cada 90 días. Para obtener una descripción general del TDE, vea [Cifrado de datos transparente (TDE)].

## <a name="enabling-encryption"></a>Habilitar el cifrado
Para habilitar TDE para una base de datos de Azure que almacena los datos migrados desde una base de datos SQL Server habilitada para Stretch, haga lo siguiente:

1. Abra la base de datos en el [Portal de Azure](https://portal.azure.com)
2. En la hoja de la base de datos, haga clic en el botón **Configuración**
3. Seleccione la opción **Cifrado de datos transparente** . ![][1]
4. Seleccione la configuración **Activado** y, luego, **Guardar**
   ![][2]

## <a name="disabling-encryption"></a>Deshabilitar el cifrado
Para deshabilitar TDE para una base de datos de Azure que almacena los datos migrados desde una base de datos SQL Server habilitada para Stretch, haga lo siguiente:

1. Abra la base de datos en el [Portal de Azure](https://portal.azure.com)
2. En la hoja de la base de datos, haga clic en el botón **Configuración**
3. Seleccione la opción **Cifrado de datos transparente** .
4. Seleccione la configuración **Desactivado** y, luego, **Guardar**

<!--Anchors-->
[Cifrado de datos transparente (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
