---
title: Conexión a SQL Server o Azure SQL Database
description: Automatización de tareas en bases de datos SQL locales o en la nube mediante Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, jonfan, logicappspm
ms.topic: conceptual
ms.date: 05/12/2020
tags: connectors
ms.openlocfilehash: f63553ced8484b3ce328fb9537d5831ae1e27fe8
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84191486"
---
# <a name="automate-workflows-for-sql-server-or-azure-sql-database-by-using-azure-logic-apps"></a>Automatización de flujos de trabajo en SQL Server o Azure SQL Database mediante Azure Logic Apps

En este artículo se muestra cómo puede acceder a los datos de la base de datos SQL desde una aplicación lógica con el conector de SQL Server. De este modo, puede automatizar las tareas, los procesos o los flujos de trabajo que administran los datos y los recursos de SQL mediante la creación de aplicaciones lógicas. El conector de SQL Server funciona en [SQL Server](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation), así como [Azure SQL Database](../azure-sql/database/sql-database-paas-overview.md) y la [instancia administrada de Azure SQL](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md).

Puede crear aplicaciones lógicas que se ejecutan cuando las desencadenan eventos en la base de datos SQL o en otros sistemas,como Dynamics CRM Online. Las aplicaciones lógicas también pueden obtener, insertar o eliminar datos y, además, pueden ejecutar consultas SQL o procedimientos almacenados. Por ejemplo, puede compilar una aplicación lógica que comprueba automáticamente si hay registros nuevos en Dynamics CRM Online, agrega elementos a la base de datos SQL para cualquier registro nuevo y envía alertas de correo electrónicos sobre los elementos agregados.

Si no está familiarizado con las aplicaciones lógicas, consulte [¿Qué es Azure Logic Apps?](../logic-apps/logic-apps-overview.md) e [Inicio rápido: Creación de la primera aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md). Para información técnica específica del conector, así como sobre las limitaciones y los problemas conocidos, consulte la [página de referencia sobre el conector de SQL Server](https://docs.microsoft.com/connectors/sql/).

## <a name="prerequisites"></a>Requisitos previos

* Suscripción a Azure. Si aún no tiene una, [regístrese para obtener una cuenta de Azure gratuita](https://azure.microsoft.com/free/).

* Una [base de datos SQL Server](https://docs.microsoft.com/sql/relational-databases/databases/create-a-database) o una [base de datos de Azure SQL](../azure-sql/database/single-database-create-quickstart.md)

  Las tablas deben tener datos para que la aplicación lógica pueda devolver los resultados cuando se llame a las operaciones. Si crea una base de datos de Azure SQL, puede usar las bases de datos de ejemplo incluidas.

* El nombre del servidor SQL, el nombre de la base de datos, el nombre de usuario y la contraseña. Necesita estas credenciales para poder autorizar que la lógica acceda al servidor SQL.

  * Para SQL Server, puede encontrar estos detalles en la cadena de conexión:

    `Server={your-server-address};Database={your-database-name};User Id={your-user-name};Password={your-password};`

  * Para Azure SQL Database, puede encontrar estos detalles en la cadena de conexión o en Azure Portal, en las propiedades de SQL Database:

    `Server=tcp:{your-server-name}.database.windows.net,1433;Initial Catalog={your-database-name};Persist Security Info=False;User ID={your-user-name};Password={your-password};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;`

* La [puerta de enlace de datos local](../logic-apps/logic-apps-gateway-install.md) instalada en un equipo local y un [recurso de puerta de enlace de datos de Azure creado en Azure Portal](../logic-apps/logic-apps-gateway-connection.md) para estos escenarios:

  * Sus aplicaciones lógicas no se ejecutan en un [entorno del servicio de integración (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

  * Sus aplicaciones lógicas *se* ejecutan en un entorno del servicio de integración, pero debe usar la autenticación de Windows para su conexión de SQL Server. En este escenario, use la versión no ISE del conector de SQL Server junto con la puerta de enlace de datos, ya que la versión de ISE no admite la autenticación de Windows.

* La aplicación lógica en la que necesita acceso a la base de datos SQL. Para iniciar la aplicación lógica con un desencadenador de SQL, necesita una [aplicación lógica en blanco](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-sql-trigger"></a>

## <a name="add-a-sql-trigger"></a>Adición de un desencadenador de SQL

En Azure Logic Apps, cada aplicación lógica debe comenzar con un [desencadenador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que se activa cuando sucede un evento específico o cuando se cumple una condición determinada. Cada vez que el desencadenador se activa, el motor de Logic Apps crea una instancia de aplicación lógica y empieza a ejecutar el flujo de trabajo de la aplicación lógica.

1. En Azure Portal o Visual Studio, cree una aplicación lógica en blanco, con lo que se abre el diseñador de Logic Apps. En este ejemplo se usa Azure Portal.

1. En el diseñador, escriba "sql server" como filtro en el cuadro de búsqueda. En la lista de desencadenadores, seleccione el desencadenador de SQL que desee.

   En este ejemplo se usa el desencadenador **Cuando se crea un elemento**.

   ![Selección del desencadenador "Cuando se crea un elemento"](./media/connectors-create-api-sqlazure/select-sql-server-trigger.png)

1. Si se le pide que cree una conexión, [cree ahora la conexión de SQL](#create-connection). Si la conexión existe, seleccione un **nombre de tabla**.

   ![Selección de la tabla que quiera](./media/connectors-create-api-sqlazure/azure-sql-database-table.png)

1. Establezca las propiedades **Interval** (Intervalo) y **Frequency** (Frecuencia), que especifican la frecuencia con que la aplicación lógica comprueba la tabla.

   Este desencadenador solo devuelve una fila de la tabla seleccionada, nada más. Para realizar otras tareas, agregue otras acciones que realicen las tareas que quiera. Por ejemplo, para ver los datos de esta fila, puede agregar otras acciones que creen un archivo que incluya los campos de la fila devuelta y, luego, que luego envíen alertas por correo electrónico. Para información sobre otras acciones disponibles para este conector, consulte la [página de referencia del conector](https://docs.microsoft.com/connectors/sql/).

1. Cuando esté listo, seleccione **Guardar** en la barra de herramientas del diseñador.

   Este paso habilita y publica de manera automática la aplicación lógica en vivo en Azure.

<a name="add-sql-action"></a>

## <a name="add-a-sql-action"></a>Incorporación de una acción de SQL

En Azure Logic Apps, una [acción](../logic-apps/logic-apps-overview.md#logic-app-concepts) es un paso del flujo de trabajo que sigue a un desencadenador u otra acción. En este ejemplo, la aplicación lógica empieza con el [desencadenador de periodicidad](../connectors/connectors-native-recurrence.md) y llama a una acción que obtiene una fila de una base de datos SQL.

1. En Azure Portal o Visual Studio, abra la aplicación lógica el Diseñador de Logic Apps. En este ejemplo se usa Azure Portal.

1. En el desencadenador o la acción donde quiera agregar la acción de SQL, seleccione **Nuevo paso**.

   ![Adición de un nuevo paso a la aplicación lógica](./media/connectors-create-api-sqlazure/select-new-step-logic-app.png)

   Para agregar una acción entre los pasos existentes, mueva el mouse sobre la flecha de conexión. Seleccione el signo más ( **+** ) que aparece y, luego, seleccione **Agregar una acción**.

1. En **Elegir una acción**, en el cuadro de búsqueda, escriba "sql-server" como filtro. En la lista de acciones, seleccione la acción SQL que quiera.

   En este ejemplo se usa la acción **Obtener fila**, que obtiene un único registro.

   ![Búsqueda y selección de la acción "Obtener fila" de SQL](./media/connectors-create-api-sqlazure/find-select-sql-get-row-action.png)

   Esta acción solo devuelve una fila de la tabla seleccionada, nada más. Para ver los datos de esta fila, puede agregar otras acciones que creen un archivo que incluya los campos de la fila devuelta y almacenar ese archivo en una cuenta de almacenamiento en la nube. Para información sobre otras acciones disponibles para este conector, consulte la [página de referencia del conector](https://docs.microsoft.com/connectors/sql/).

1. Si se le pide que cree una conexión, [cree ahora la conexión de SQL](#create-connection). Si la conexión existe, seleccione un **nombre de tabla** y escriba el **identificador de fila** del registro que desee.

   ![Escribir el nombre de la tabla y el identificador de la fila](./media/connectors-create-api-sqlazure/specify-table-row-id-property-value.png)

1. Cuando esté listo, seleccione **Guardar** en la barra de herramientas del diseñador.

   Este paso habilita y publica de manera automática la aplicación lógica en vivo en Azure.

<a name="create-connection"></a>

## <a name="connect-to-your-database"></a>Conectarse a la base de datos

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to SQL Server or Azure SQL Database](../../includes/connectors-create-api-sqlazure.md)]

## <a name="handle-bulk-data"></a>Controlar datos masivos

En ocasiones, tendrá que trabajar con conjuntos de resultados tan grandes que el conector no devuelva todos los resultados al mismo tiempo, o necesitará un mayor control sobre el tamaño y la estructura de los conjuntos de resultados. A continuación se indican algunas maneras de controlar conjuntos de resultados tan grandes:

* Para ayudarle a administrar los resultados como conjuntos más pequeños, active la *paginación*. Para obtener más información, consulte el tema sobre la [obtención de datos masivos, registros y elementos mediante la paginación](../logic-apps/logic-apps-exceed-default-page-size-with-pagination.md).

* Cree un procedimiento almacenado que organice los resultados como desee.

  Al obtener o insertar varias filas, la aplicación lógica puede iterar a través de estas filas mediante un [*bucle Until*](../logic-apps/logic-apps-control-flow-loops.md#until-loop) dentro de estos [límites](../logic-apps/logic-apps-limits-and-config.md). No obstante, si su aplicación lógica tiene que trabajar con conjuntos de registros tan grandes, por ejemplo, con miles o millones de filas, y quiere minimizar los costos de las llamadas a la base de datos.

  Para organizar los resultados de la manera deseada, puede crear un [*procedimiento almacenado*](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) que se ejecute en la instancia de SQL y use la instrucción **SELECT - ORDER BY**. Esta solución le permite tener mayor control sobre el tamaño y la estructura de los resultados. La aplicación lógica llama al procedimiento almacenado mediante la acción **Ejecutar procedimiento almacenado** del conector de SQL Server.

  Para más información sobre las soluciones, consulte estos artículos:

  * [SQL Pagination for bulk data transfer with Logic Apps](https://social.technet.microsoft.com/wiki/contents/articles/40060.sql-pagination-for-bulk-data-transfer-with-logic-apps.aspx) (Paginación de SQL para la transferencia de datos masiva con Logic Apps)

  * [Cláusula SELECT - ORDER BY](https://docs.microsoft.com/sql/t-sql/queries/select-order-by-clause-transact-sql)

### <a name="handle-dynamic-bulk-data"></a>Manipulación de datos dinámicos masivos

A veces, cuando realiza una llamada a un procedimiento almacenado en el conector de SQL Server, la salida devuelta es dinámica. En este ejemplo, siga estos pasos:

1. Abra el **diseñador de Logic Apps**.
1. Realice una serie de pruebas de la aplicación lógica para ver el formato de salida. Copie el resultado del ejemplo.
1. En el diseñador, en la acción para llamar al procedimiento almacenado, seleccione **Nuevo paso**.
1. En la opción para **elegir una acción**, busque y seleccione la acción [**Analizar JSON**](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action).
1. En la acción **Análisis del archivo JSON**, seleccione **Usar una carga de ejemplo para generar el esquema**.
1. En la ventana para **escribir o pegar una carga de JSON de ejemplo**, proporcione una salida de ejemplo y luego seleccione **Listo**.
1. Si recibe un error que dice que Logic Apps no puede generar un esquema, compruebe que la sintaxis de la salida de ejemplo tiene el formato correcto. Si todavía no puede generar el esquema, escríbalo manualmente en el cuadro de **esquemas**.
1. En la barra de herramientas del diseñador, seleccione **Save** (Guardar).
1. Para obtener acceso a las propiedades de contenido de JSON, use los tokens de datos que aparecen en la lista de contenido dinámico en la acción para [**analizar JSON**](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action).

## <a name="connector-specific-details"></a>Detalles específicos del conector

Para información técnica sobre los desencadenadores, las acciones y los límites de este conector, consulte la [página de referencia del conector](https://docs.microsoft.com/connectors/sql/).

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre otros [conectores para Azure Logic Apps](../connectors/apis-list.md).
