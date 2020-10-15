---
title: 'Inicio rápido: Fivetran y almacenamiento de datos'
description: Introducción a Fivetran y un almacenamiento de datos de Azure Synapse Analytics.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 10/12/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 96e679c0b284cc649dbde3fba1b640f4e09df05e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85201657"
---
# <a name="quickstart-fivetran-with-data-warehouse"></a>Inicio rápido: Fivetran con almacenamiento de datos 

En este inicio rápido se describe cómo configurar un nuevo usuario de Fivetran para que funcione con un almacenamiento de datos de Azure Synapse Analytics aprovisionado con un grupo de SQL. En este artículo se supone que ya tiene un almacenamiento de datos.

## <a name="set-up-a-connection"></a>Configuración de una conexión

1. Busque el nombre completo del servidor y la base de datos que se usarán para conectarse al almacenamiento de datos.
    
    Si necesita ayuda para encontrar esta información, consulte [Conexión al almacenamiento de datos](../sql/connect-overview.md).

2. En el Asistente para instalación, elija si desea conectarse a la base de datos directamente o mediante un túnel SSH.

   Si decide conectarse directamente a la base de datos, deberá crear una regla de firewall para permitir el acceso. Este es el método más sencillo y seguro.

   Si elige conectarse mediante un túnel SSH, Fivetran se conecta a un servidor independiente de la red. El servidor proporciona un túnel SSH a la base de datos. Debe usar este método si la base de datos está en una subred inaccesible dentro de una red virtual.

3. Agregue la dirección IP **52.0.2.4** al firewall de nivel de servidor para permitir conexiones entrantes a su instancia de almacenamiento de datos desde Fivetran.

   Para más información, consulte [Creación de una regla de firewall de nivel de servidor](create-data-warehouse-portal.md#create-a-server-level-firewall-rule).

## <a name="set-up-user-credentials"></a>Configuración de las credenciales de usuario

1. Conéctese a su almacenamiento de datos mediante SQL Server Management Studio (SSMS) o la herramienta que prefiera. Inicie sesión como usuario administrador del servidor. A continuación, ejecute los siguientes comandos SQL para crear un usuario de Fivetran:

    - En la base de datos maestra: 
    
      ```sql
      CREATE LOGIN fivetran WITH PASSWORD = '<password>'; 
      ```

    - En la base de datos de almacenamiento de datos:

      ```sql
      CREATE USER fivetran_user_without_login without login;
      CREATE USER fivetran FOR LOGIN fivetran;
      GRANT IMPERSONATE on USER::fivetran_user_without_login to fivetran;
      ```

2. Conceda al usuario de Fivetran los siguientes permisos para el almacenamiento de datos:

    ```sql
    GRANT CONTROL to fivetran;
    ```

    Se requiere el permiso de CONTROL para crear credenciales con ámbito de base de datos que se usan cuando un usuario carga archivos desde Azure Blob Storage mediante PolyBase.

3. Agregue una clase de recurso adecuado al usuario de Fivetran. La clase de recurso que use dependerá de la memoria necesaria para crear un índice de almacén de columnas. Por ejemplo, las integraciones con productos como Marketo y Salesforce requieren una clase de recurso superior debido a la gran cantidad de columnas y el mayor volumen de datos que usan los productos. Una clase de recurso superior requiere más memoria para crear índices de almacén de columnas.

    Se recomienda usar clases de recursos estáticos. Puede comenzar con la clase de recurso `staticrc20`. La clase de recurso `staticrc20` asigna 200 MB para cada usuario, independientemente del nivel de rendimiento que se use. Si se produce un error de indexación de almacén de columnas en el nivel de clase de recurso inicial, aumente la clase de recurso.

    ```sql
    EXEC sp_addrolemember '<resource_class_name>', 'fivetran';
    ```

    Para más información, lea sobre los [límites de memoria y simultaneidad](memory-concurrency-limits.md) y las [clases de recursos](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md#ways-to-allocate-more-memory).


## <a name="connect-from-fivetran"></a>Conexión desde Fivetran

Para conectarse a su almacenamiento de datos desde su cuenta de Fivetran, escriba las credenciales que usa para acceder al almacenamiento de datos: 

* Host (el nombre del servidor)
* Puerto
* Base de datos
* Usuario (el nombre de usuario debe ser **fivetran\@_server_name_**, donde *server_name* es parte del URI de host de Azure: **_server\_name_.database.windows.net**).
* Password.
