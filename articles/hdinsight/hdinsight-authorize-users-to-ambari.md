---
title: Autorización de usuarios para vistas de Ambari en Azure HDInsight
description: Procedimiento para administrar los permisos de usuarios y grupos de Ambari en los clústeres de HDInsight con ESP habilitado.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/27/2019
ms.openlocfilehash: 3bb58334e96bf5378fb78b70125f9c7994a7c2fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435642"
---
# <a name="authorize-users-for-apache-ambari-views"></a>Autorización de usuarios para Apache Ambari Views

Los [clústeres de HDInsight habilitados con Enterprise Security Package (ESP)](./domain-joined/hdinsight-security-overview.md) proporcionan funcionalidades empresariales, incluida la autenticación basada en Azure Active Directory. Puede [sincronizar los usuarios nuevos](hdinsight-sync-aad-users-to-cluster.md) agregados a los grupos de Azure AD a los que se ha proporcionado acceso al clúster y permitir que esos usuarios específicos realicen determinadas acciones. El trabajo con usuarios, grupos y permisos en [Apache Ambari](https://ambari.apache.org/) es compatible tanto con clústeres de HDInsight ESP como con clústeres de HDInsight estándar.

Los usuarios de Active Directory pueden iniciar sesión en los nodos del clúster con sus credenciales de dominio. También pueden usar sus credenciales de dominio para autenticar las interacciones del clúster con otros puntos de conexión aprobados como [Hue](https://gethue.com/), Ambari Views, ODBC, JDBC, PowerShell y las API REST.

> [!WARNING]  
> No cambie la contraseña del guardián Ambari (hdinsightwatchdog) en el clúster de HDInsight basado en Linux. El cambio de la contraseña impide usar acciones de script o realizar operaciones de escalado con el clúster.

Si no lo ha hecho aún, siga [estas instrucciones](./domain-joined/apache-domain-joined-configure.md) para aprovisionar un nuevo clúster ESP.

## <a name="access-the-ambari-management-page"></a>Acceso a la página de administración de Ambari

Para llegar a la **página de administración de Ambari** en la [interfaz de usuario web de Ambari](hdinsight-hadoop-manage-ambari.md), vaya a `https://CLUSTERNAME.azurehdinsight.net`. Escriba el nombre de usuario y la contraseña del administrador del clúster que definió cuando creó el clúster. A continuación, en el panel de Ambari, seleccione **Manage Ambari** (Administrar Ambari) en el menú **admin** (Administrador):

![Administración del panel de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/manage-apache-ambari.png)

## <a name="add-users"></a>Agregar usuarios

### <a name="add-users-through-the-portal"></a>Adición de usuarios a través del portal

1. En la página de administración, seleccione **Usuarios**.

    ![Usuarios de la página de administración de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/apache-ambari-management-page-users.png)

1. Seleccione **+ Crear usuario local**.

1. Proporcione un **nombre de usuario** y una **contraseña**. Seleccione **Guardar**.

### <a name="add-users-through-powershell"></a>Adición de usuarios a través de PowerShell

Edite las variables siguientes, reemplazando `CLUSTERNAME`, `NEWUSER` y `PASSWORD` por los valores adecuados.

```powershell
# Set-ExecutionPolicy Unrestricted

# Begin user input; update values
$clusterName="CLUSTERNAME"
$user="NEWUSER"
$userpass='PASSWORD'
# End user input

$adminCredentials = Get-Credential -UserName "admin" -Message "Enter admin password"

$clusterName = $clusterName.ToLower()
$createUserUrl="https://$($clusterName).azurehdinsight.net/api/v1/users"

$createUserBody=@{
    "Users/user_name" = "$user"
    "Users/password" = "$userpass"
    "Users/active" = "$true"
    "Users/admin" = "$false"
} | ConvertTo-Json

# Create user
$statusCode =
Invoke-WebRequest `
    -Uri $createUserUrl `
    -Credential $adminCredentials `
    -Method POST `
    -Headers @{"X-Requested-By" = "ambari"} `
    -Body $createUserBody | Select-Object -Expand StatusCode

if ($statusCode -eq 201) {
    Write-Output "User is created: $user"
}
else
{
    Write-Output 'User is not created'
    Exit
}

$grantPrivilegeUrl="https://$($clusterName).azurehdinsight.net/api/v1/clusters/$($clusterName)/privileges"

$grantPrivilegeBody=@{
    "PrivilegeInfo" = @{
        "permission_name" = "CLUSTER.USER"
        "principal_name" = "$user"
        "principal_type" = "USER"
    }
} | ConvertTo-Json

# Grant privileges
$statusCode =
Invoke-WebRequest `
    -Uri $grantPrivilegeUrl `
    -Credential $adminCredentials `
    -Method POST `
    -Headers @{"X-Requested-By" = "ambari"} `
    -Body $grantPrivilegeBody | Select-Object -Expand StatusCode

if ($statusCode -eq 201) {
    Write-Output 'Privilege is granted'
}
else
{
    Write-Output 'Privilege is not granted'
    Exit
}

Write-Host "Pausing for 100 seconds"
Start-Sleep -s 100

$userCredentials = "$($user):$($userpass)"
$encodedUserCredentials = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($userCredentials))
$zookeeperUrlHeaders = @{ Authorization = "Basic $encodedUserCredentials" }
$getZookeeperurl="https://$($clusterName).azurehdinsight.net/api/v1/clusters/$($clusterName)/services/ZOOKEEPER/components/ZOOKEEPER_SERVER"

# Perform query with new user
$zookeeperHosts =
Invoke-WebRequest `
    -Uri $getZookeeperurl `
    -Method Get `
    -Headers $zookeeperUrlHeaders

Write-Output $zookeeperHosts
```

### <a name="add-users-through-curl"></a>Adición de usuarios a través de cURL

Edite las variables siguientes, reemplazando `CLUSTERNAME`, `ADMINPASSWORD`, `NEWUSER` y `USERPASSWORD` por los valores adecuados. El script está diseñado para ejecutarse con Bash. Se necesitan ligeras modificaciones para un símbolo del sistema de Windows.

```bash
export clusterName="CLUSTERNAME"
export adminPassword='ADMINPASSWORD'
export user="NEWUSER"
export userPassword='USERPASSWORD'

# create user
curl -k -u admin:$adminPassword -H "X-Requested-By: ambari" -X POST \
-d "{\"Users/user_name\":\"$user\",\"Users/password\":\"$userPassword\",\"Users/active\":\"true\",\"Users/admin\":\"false\"}" \
https://$clusterName.azurehdinsight.net/api/v1/users

echo "user created: $user"

# grant permissions
curl -k -u admin:$adminPassword -H "X-Requested-By: ambari" -X POST \
-d '[{"PrivilegeInfo":{"permission_name":"CLUSTER.USER","principal_name":"'$user'","principal_type":"USER"}}]' \
https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/privileges

echo "Privilege is granted"

echo "Pausing for 100 seconds"
sleep 10s

# perform query using new user account
curl -k -u $user:$userPassword -H "X-Requested-By: ambari" \
-X GET "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER"
```

## <a name="grant-permissions-to-apache-hive-views"></a>Concesión de permisos para vistas de Apache Hive

Ambari incluye instancias de vista para [Apache Hive](https://hive.apache.org/) y [Apache TEZ](https://tez.apache.org/), entre otros. Para conceder acceso a una o varias instancias de vistas de Hive, vaya a la **página de administración de Ambari**.

1. En la página de administración, seleccione el vínculo **Views** (Vistas) que está debajo del título del menú **Views** (Vistas) de la izquierda.

    ![Vínculos a vistas de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/apache-ambari-views-link.png)

2. En la página de vistas, expanda la fila **HIVE**. Hay una vista de Hive personalizada que se crea cuando se agrega el servicio Hive al clúster. También puede crear más instancias de la vista de Hive según sea necesario. Seleccione una vista de Hive:

    ![Vistas de HDInsight: vista de Apache Hive](./media/hdinsight-authorize-users-to-ambari/views-apache-hive-view.png)

3. Desplácese hacia la parte inferior de la página de la vista. En la sección *Permissions* (Permisos), tiene dos opciones para conceder permisos a los usuarios del dominio para la vista:

**Grant permission to these users** (Conceder permiso a estos usuarios) ![Grant permission to these users](./media/hdinsight-authorize-users-to-ambari/hdi-add-user-to-view.png) (Conceder permiso a estos usuarios)

**Grant permission to these groups** (Conceder permiso a estos grupos) ![Grant permission to these groups](./media/hdinsight-authorize-users-to-ambari/add-group-to-view-permission.png) (Conceder permiso a estos grupos)

1. Para agregar un usuario, seleccione el botón **Add User** (Agregar usuario).

   * Empiece a escribir el nombre de usuario y se mostrará una lista desplegable de nombres definidos anteriormente.

     ![Autocompletar de usuario de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/ambari-user-autocomplete.png)

   * Seleccione el nombre de usuario o termine de escribirlo. Para agregar este nombre de usuario como un usuario nuevo, seleccione el botón **New** (Nuevo).

   * Para guardar los cambios, seleccione la **casilla azul**.

     ![Concesión de permisos de usuario de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/user-entered-permissions.png)

1. Para agregar un grupo, seleccione el botón **Add Group** (Agregar grupo).

   * Comience a escribir el nombre del grupo. El proceso para seleccionar un nombre de grupo existente o para agregar un grupo nuevo es el mismo que el que se aplica para agregar usuarios.
   * Para guardar los cambios, seleccione la **casilla azul**.

     ![Concesión de permisos de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/ambari-group-entered.png)

Agregar los usuarios directamente a una vista es útil cuando desea asignar permisos a un usuario para que use esa vista, pero no desea que sea miembro de un grupo que tiene permisos adicionales. Para reducir la cantidad de trabajo administrativo, puede ser más fácil asignar permisos a grupos.

## <a name="grant-permissions-to-apache-tez-views"></a>Concesión de permisos para vistas de Apache TEZ

Las instancias de vista de [Apache TEZ](https://tez.apache.org/) permiten a los usuarios supervisar y depurar todos los trabajos de Tez, enviados por consultas de [Apache Hive](https://hive.apache.org/) y scripts de [Apache Pig](https://pig.apache.org/). Hay una instancia de vista de Tez predeterminada que se crea cuando se aprovisiona el clúster.

Para asignar usuarios y grupos a una instancia de vista de Tez, expanda la fila **TEZ** en la página de vistas, como se describió anteriormente.

![Vistas de HDInsight: vista de Apache Tez](./media/hdinsight-authorize-users-to-ambari/views-apache-tez-view.png)

Para agregar usuarios o grupos, repita los pasos 3 a 5 de la sección anterior.

## <a name="assign-users-to-roles"></a>Asignación de usuarios a roles

Hay cinco roles de seguridad para usuarios y grupos, que se enumeran como permisos de acceso en orden decreciente:

* Administrador de clústeres
* Operador de clústeres
* Administrador de servicios
* Operador de servicio
* Usuario del clúster

Para administrar roles, vaya a la **página de administración de Ambari** y después seleccione el vínculo **Roles** en el grupo de menús *Clusters* (Clústeres) de la izquierda.

![Vínculos del menú de roles de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/cluster-roles-menu-link.png)

Para ver la lista de permisos concedidos a cada rol, haga clic en el signo de interrogación azul junto al encabezado de tabla **Roles** de la página Roles.

![Permisos de los vínculos del menú de roles de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/roles-menu-permissions.png "Permisos de los vínculos del menú de roles de Apache Ambari")

En esta página, hay dos vistas diferentes que puede usar para administrar roles para los usuarios y grupos: Block (Bloque) y List (Lista).

### <a name="block-view"></a>Vista de bloque

La vista Block (Bloque) muestra cada rol en su propia fila y ofrece las opciones **Assign roles to these users** (Asignar roles a estos usuarios) y **Assign roles to these groups** (Asignar roles a estos grupos) descritas anteriormente.

![Vista de bloques de roles de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/ambari-roles-block-view.png)

### <a name="list-view"></a>Vista de lista

La vista List (Lista) proporciona funcionalidades de edición rápidas en dos categorías: Users (Usuarios) y Groups (Grupos).

* La categoría Users (Usuarios) de la vista List (Lista) muestra una lista de todos los usuarios, que le permite seleccionar un rol para cada usuario en la lista desplegable.

    ![Vista de lista de roles de Apache Ambari: usuarios](./media/hdinsight-authorize-users-to-ambari/roles-list-view-users.png)

* La categoría Groups (Grupos) de la vista List (Lista) muestra todos los grupos y los roles asignados a cada grupo. En el ejemplo, la lista de grupos se sincroniza desde los grupos de Azure AD especificados en la propiedad **Access user group** (Grupo de usuarios con acceso) de la configuración del dominio del clúster. Consulte [Creación de un clúster de HDInsight con ESP habilitado](./domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp).

    ![Vista de lista de roles de Apache Ambari: grupos](./media/hdinsight-authorize-users-to-ambari/roles-list-view-groups.png)

    En la imagen anterior, se asigna al grupo "hiveusers" el rol *Cluster User* (Usuario del clúster). Se trata de un rol de solo lectura que permite a los usuarios de ese grupo ver, pero no cambiar, las configuraciones del servicio y las métricas del clúster.

## <a name="log-in-to-ambari-as-a-view-only-user"></a>Inicio de sesión en Ambari como un usuario de solo lectura

Se han asignado a los usuarios del dominio de Azure AD "hiveuser1" permisos para las vistas de Hive y Tez. Cuando se inicia la interfaz de usuario web de Ambari y se escriben las credenciales del dominio de este usuario (nombre de usuario de Azure AD en formato de correo electrónico y contraseña), se redirige al usuario a la página de vistas de Ambari. Desde aquí, el usuario puede seleccionar cualquier vista accesible. El usuario no puede visitar cualquier otra parte del sitio, incluidas las páginas de panel, servicios, hosts, alertas o administrador.

![Usuario de Apache Ambari solo con vistas](./media/hdinsight-authorize-users-to-ambari/ambari-user-views-only.png)

## <a name="log-in-to-ambari-as-a-cluster-user"></a>Inicio de sesión en Ambari como un usuario del clúster

Se ha asignado al usuario del dominio de Azure AD "hiveuser2" el rol *Cluster User* (Usuario del clúster). Este rol puede acceder al panel y a todos los elementos de menú. Un usuario del clúster tiene menos opciones permitidas que un administrador. Por ejemplo, hiveuser2 puede ver las configuraciones de cada uno de los servicios, pero no puede modificarlas.

![Pantalla del panel de Apache Ambari](./media/hdinsight-authorize-users-to-ambari/user-cluster-user-role.png)

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de directivas de Apache Hive en HDInsight con ESP](./domain-joined/apache-domain-joined-run-hive.md)
* [Administración de clústeres de HDInsight ESP](./domain-joined/apache-domain-joined-manage.md)
* [Uso de la vista de Apache Hive con Apache Hadoop en HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md)
* [Sincronización de usuarios de Azure AD con el clúster](hdinsight-sync-aad-users-to-cluster.md)
* [Administración de clústeres de HDInsight mediante la API REST de Apache Ambari](./hdinsight-hadoop-manage-ambari-rest-api.md)
