---
title: 'Inicio rápido: Creación de un servidor (Azure Portal): Azure Database for PostgreSQL (servidor único)'
description: En esta guía de inicio rápido va a crear y administrar un servidor de Azure Database for PostgreSQL desde Azure Portal.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.custom: mvc
ms.topic: quickstart
ms.date: 10/18/2020
ms.openlocfilehash: 000ab3e3911c65554622a48d34abda79d60411df
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96492393"
---
# <a name="quickstart-create-an-azure-database-for-postgresql-server-by-using-the-azure-portal"></a>Inicio rápido: Creación de un servidor de Azure Database for PostgreSQL desde Azure Portal

Azure Database for PostgreSQL es un servicio administrado que usa para ejecutar, administrar y escalar bases de datos de PostgreSQL de alta disponibilidad en la nube. En este inicio rápido se muestra cómo crear un servidor único de Azure Database for PostgreSQL mediante Azure Portal.

## <a name="prerequisites"></a>Requisitos previos
Se necesita una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) antes de empezar.

## <a name="create-an-azure-database-for-postgresql-server"></a>Creación de un servidor de Azure Database for PostgreSQL
Vaya a [Azure Portal](https://portal.azure.com/) para crear una base de datos para el servidor único de Azure Database for PostgreSQL. Busque y seleccione los *servidores de Azure Database for PostgreSQL*.

>[!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-database-portal/search-postgres.png" alt-text="Busque Azure Database for PostgreSQL.":::

1. Seleccione **Agregar**.

2. En la página Creación de una instancia de Azure Database for PostgreSQL, seleccione **Un solo servidor**.

    >[!div class="mx-imgBorder"]
    > :::image type="content" source="./media/quickstart-create-database-portal/select-single-server.png" alt-text="Seleccionar un solo servidor":::

3. Complete el formulario **Conceptos básicos** con la siguiente información.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="./media/quickstart-create-database-portal/create-basics.png" alt-text="Captura de pantalla que muestra la pestaña Conceptos básicos para crear un servidor único.":::

   |Configuración|Valor sugerido|Descripción|
   |:---|:---|:---|
   |Suscripción|El nombre de la suscripción|Seleccione la suscripción de Azure que desee.|
   |Resource group|*myresourcegroup*| Un nuevo grupo de recursos o uno ya existente de la suscripción.|
   |Nombre de servidor |*mydemoserver*|Un nombre único que identifique al servidor de Azure Database for PostgreSQL. El nombre de dominio *postgres.database.azure.com* se anexa al nombre del servidor que especifique. El servidor solo puede contener letras minúsculas, números y el carácter de guion (-). Debe contener entre 3 y 63 caracteres.|
   |Origen de datos | Ninguno | Seleccione **None** para crear un servidor desde cero. Seleccione **Copia de seguridad** solo si se restaura a partir de una copia de seguridad geográfica de un servidor existente.|
   |Nombre de usuario administrador |*myadmin*| Escriba su nombre de usuario de administrador del servidor. No puede empezar por **pg_** y no se permiten estos valores: **azure_superuser**, **azure_pg_admin**, **admin**, **administrator**, **root**, **guest** ni **public**.|
   |Contraseña |la contraseña| Una contraseña nueva para el usuario administrador del servidor. Debe contener entre 8 y 128 caracteres de tres de las siguientes categorías: letras del alfabeto inglés mayúsculas y minúsculas, números (0 a 9) y caracteres no alfanuméricos (por ejemplo, !, $, # o %).|
   |Location|Su ubicación deseada| Seleccione una ubicación en la lista desplegable.|
   |Versión|La versión principal más reciente| La versión principal más reciente de PostgreSQL, a menos que tenga requisitos específicos.|
   |Proceso y almacenamiento | *Usar los valores predeterminados*| El plan de tarifa predeterminado es **De uso general** con **4 núcleos virtuales** y **100 GB** de almacenamiento. La retención de copias de seguridad se establece en **7 días** con opción de copia de seguridad con **redundancia geográfica**.<br/>Obtenga información sobre los [precios](https://azure.microsoft.com/pricing/details/postgresql/server/) y actualice los valores predeterminados si es necesario.|


   > [!NOTE]
   > Considere la posibilidad de usar el plan de tarifa Básico si menos proceso y E/S resultan adecuados para su carga de trabajo. Tenga en cuenta que los servidores que se creen en el plan de tarifa Básico no se podrán escalar más adelante a De uso general u Optimizada para memoria.

5. Seleccione **Review + create** (Revisar y crear) para revisar las selecciones. Seleccione **Crear** para realizar el aprovisionamiento del servidor. Esta operación puede tardar unos minutos.
    > [!NOTE]
    > Se ha creado una base de datos vacía llamada **postgres**. También encontrará la base de datos **azure_maintenance**, que se usa para separar los procesos del servicio administrado de las acciones del usuario. No se puede acceder a la base de datos **azure_maintenance**.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-database-portal/deployment-success.png" alt-text="Implementación correcta.":::

[¿Tiene problemas? Háganoslo saber.](https://aka.ms/postgres-doc-feedback)

## <a name="configure-a-firewall-rule"></a>Configuración de una regla de firewall
De forma predeterminada, no se puede acceder públicamente al servidor que se crea. Es preciso conceder permisos a su dirección IP. Vaya al recurso de servidor en Azure Portal y seleccione **Seguridad de la conexión** en el menú izquierdo del recurso de servidor. Si no está seguro de cómo encontrar el recurso, consulte la sección [Apertura de recursos](../azure-resource-manager/management/manage-resources-portal.md#open-resources).

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/quickstart-create-database-portal/add-current-ip-firewall.png" alt-text="Captura de pantalla que muestra las reglas de firewall para la seguridad de la conexión.":::

Ahora, seleccione **Agregar dirección IP del cliente actual** y, después, **Guardar**. Puede agregar más direcciones IP o especificar un intervalo IP para conectarse al servidor desde dichas direcciones IP. Para más información, consulte el artículo [Reglas de firewall de Azure Database for PostgreSQL](./concepts-firewall-rules.md).

> [!NOTE]
> Para evitar problemas de conectividad, compruebe si la red permite el tráfico de salida a través el puerto 5432, ya que Azure Database for PostgreSQL usa ese puerto.

[¿Tiene problemas? Háganoslo saber.](https://aka.ms/postgres-doc-feedback)

## <a name="connect-to-the-server-with-psql"></a>Conexión al servidor con psql

Puede usar [psql](http://postgresguide.com/utilities/psql.html) o [pgAdmin](https://www.pgadmin.org/docs/pgadmin4/latest/connecting.html), que son clientes de PostgreSQL muy conocidos. En este inicio rápido, se conectará mediante psql a [Azure Cloud Shell](../cloud-shell/overview.md) dentro de Azure Portal.

1. Anote el nombre del servidor, el nombre de inicio de sesión del administrador del servidor, la contraseña y el identificador de la suscripción del servidor recién creado de la sección **Información general** del servidor.
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/quickstart-create-database-portal/overview-new.png" alt-text="Obtención de información sobre la conexión.":::


2. Abra Azure Cloud Shell en el portal, para lo que debe seleccionar el icono de la parte superior izquierda.

   > [!NOTE]
   > Si es la primera vez que abre Cloud Shell, verá un símbolo del sistema para crear un grupo de recursos y una cuenta de almacenamiento. Esto es un paso único y se adjuntará automáticamente en todas las sesiones.

   > [!div class="mx-imgBorder"]
   > :::image type="content" source="media/quickstart-create-database-portal/use-in-cloud-shell.png" alt-text="Captura de pantalla que muestra información del servidor y el icono para abrir Azure Cloud Shell.":::

3. Ejecute el siguiente comando en el terminal de Azure Cloud Shell. Reemplace los valores por el nombre del servidor real y el nombre de inicio de sesión del usuario administrador. Use la base de datos vacía **postgres** con el usuario administrador en este formato: `<admin-username>@<servername>`.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```

   Este es el aspecto final en el terminal de Cloud Shell:

   ```bash
    Requesting a Cloud Shell.Succeeded.
    Connecting terminal...

    Welcome to Azure Cloud Shell

    Type "az" to use Azure CLI
    Type "help" to learn about Cloud Shell

    user@Azure:~$psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
    Password for user myadmin@mydemoserver.postgres.database.azure.com:
    psql (12.2 (Ubuntu 12.2-2.pgdg16.04+1), server 11.6)
    SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
    Type "help" for help.

    postgres=>
    ```
4. En el mismo terminal de Azure Cloud Shell, cree una base de datos llamada **guest**.

   ```bash
   postgres=> CREATE DATABASE guest;
   ```

5. Cambie las conexiones a la base de datos **guest** recién creada.

   ```bash
   \c guest
   ```
6. Escriba `\q` y seleccione la tecla Entrar para cerrar psql.

[¿Tiene problemas? Háganoslo saber.](https://aka.ms/postgres-doc-feedback)

## <a name="clean-up-resources"></a>Limpieza de recursos
Ha creado correctamente un servidor de Azure Database for PostgreSQL en un grupo de recursos. Si no cree que vaya a necesitar estos recursos en el futuro, puede eliminarlos mediante la eliminación del grupo de recursos o del servidor de PostgreSQL.

Para eliminar el grupo de recursos:

1. En Azure Portal, busque y seleccione **Grupos de recursos**.
2. En la lista de los grupos de recursos, elija el nombre del grupo de recursos.
3. En la página **Información general** del grupo de recursos, seleccione **Eliminar grupo de recursos**.
4. En el cuadro de diálogo de confirmación, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

Para eliminar el servidor, seleccione el botón **Eliminar** de la página **Información general** del servidor:

> [!div class="mx-imgBorder"]
> :::image type="content" source="media/quickstart-create-database-portal/12-delete.png" alt-text="Captura de pantalla que muestra el botón para probar eliminar un servidor.":::

[¿Tiene problemas? Háganoslo saber.](https://aka.ms/postgres-doc-feedback)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Migración de una base de datos mediante exportación e importación](./howto-migrate-using-export-and-import.md) <br/>

> [!div class="nextstepaction"]
> [Diseño de una base de datos](./tutorial-design-database-using-azure-portal.md#create-tables-in-the-database)

[¿No encuentra lo que busca? Háganoslo saber.](https://aka.ms/postgres-doc-feedback)