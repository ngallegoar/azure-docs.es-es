---
title: Sincronización de LDAP en Ranger y Apache Ambari en Azure HDInsight
description: Aborde la sincronización de LDAP en Ranger y Ambari y proporcione directrices generales.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 99bd1ac156b12a5be7b8c5c17eb5b568b7070a25
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "77465669"
---
# <a name="ldap-sync-in-ranger-and-apache-ambari-in-azure-hdinsight"></a>Sincronización de LDAP en Ranger y Apache Ambari en Azure HDInsight

Los clústeres de Enterprise Security Package (ESP) de HDInsight usan Ranger para la autorización. Apache Ambari y Ranger sincronizan usuarios y grupos de forma independiente y funcionan de un modo ligeramente diferente. El objetivo de este artículo es abordar la sincronización de LDAP en Ranger y Ambari.

## <a name="general-guidelines"></a>Directrices generales

* Implemente siempre los clústeres con grupos.
* En lugar de cambiar los filtros de grupo en Ambari y Ranger, intente administrar todos ellos en Azure AD y use grupos anidados para incorporar los usuarios requeridos.
* Una vez que un usuario se sincroniza, no se quita, incluso si no forma parte de los grupos.
* Si necesita cambiar los filtros de LDAP directamente, use la interfaz de usuario en primer lugar, ya que contiene algunas validaciones.

## <a name="users-are-synced-separately"></a>Los usuarios se sincronizan por separado

Ambari y Ranger no comparten la base de datos de usuario porque tienen dos fines diferentes. Si un usuario necesita usar la interfaz de usuario de Ambari, el usuario debe sincronizarse con Ambari. Si el usuario no está sincronizado con Ambari, la API o la interfaz de usuario de Ambari lo rechazará, pero otras partes del sistema funcionarán (es Ranger o Resource Manager quien las protege, no Ambari). Si quiere incluir el usuario en una directiva de Ranger, sincronice el usuario con Ranger.

Cuando se implementa un clúster seguro, los miembros del grupo se sincronizan de forma transitiva (todos los subgrupos y sus miembros) en Ambari y Ranger. 

## <a name="ambari-user-sync-and-configuration"></a>Sincronización y configuración de usuarios de Ambari

Desde los nodos principales, un trabajo Cron, `/opt/startup_scripts/start_ambari_ldap_sync.py`, se ejecuta cada hora para programar la sincronización de usuarios. El trabajo Cron llama a las API REST de Ambari para realizar la sincronización. El script envía una lista de usuarios y grupos para sincronizar (ya que es posible que los usuarios no pertenezcan a los grupos especificados, ambos se especifican de forma individual). Ambari sincroniza sAMAccountName como el nombre de usuario y todos los miembros del grupo de forma transitiva.

Los registros deben estar en `/var/log/ambari-server/ambari-server.log`. Para más información, consulte el artículo sobre cómo [configurar el nivel de registro de Ambari](https://docs.cloudera.com/HDPDocuments/Ambari-latest/administering-ambari/content/amb_configure_ambari_logging_level.html).

En los clústeres de Data Lake, el enlace de creación posterior al usuario se usa para crear las carpetas particulares de los usuarios sincronizados y se establecen como los propietarios de las carpetas particulares. Si el usuario no se ha sincronizado con Ambari correctamente, podría encontrarse con errores al obtener acceso al almacenamiento provisional y a otras carpetas temporales.

### <a name="update-groups-to-be-synced-to-ambari"></a>Actualización de los grupos que se van a sincronizar con Ambari

Si no puede administrar la pertenencia a grupos en Azure AD, tiene dos opciones:

* Realice una sincronización de una sola vez como se describe más detalladamente en [Sincronización de usuarios y grupos de LDAP](https://docs.cloudera.com/HDPDocuments/HDP3/latest/ambari-authentication-ldap-ad/content/authe_ldapad_synchronizing_ldap_users_and_groups.html). Cada vez que cambie la pertenencia al grupo, tendrá que volver a realizar esta sincronización.

* Escriba un trabajo Cron y llame a la [API de Ambari periódicamente](https://community.cloudera.com/t5/Support-Questions/How-do-I-automate-the-Ambari-LDAP-sync/m-p/96634) con los nuevos grupos.

## <a name="ranger-user-sync-and-configuration"></a>Sincronización y configuración de usuarios de Ranger

Ranger tiene un motor de sincronización integrado que se ejecuta cada hora para sincronizar a los usuarios. No comparte la base de datos de usuario con Ambari. HDInsight configura el filtro de búsqueda para sincronizar el usuario administrador, el usuario guardián y los miembros del grupo especificado durante la creación del clúster. Los miembros del grupo se sincronizarán de manera transitiva:

* Deshabilite la sincronización incremental.
* Habilite la asignación de sincronización de grupos de usuarios.
* Especifique el filtro de búsqueda para incluir los miembros del grupo transitivo.
* Sincronice sAMAccountName para los usuarios y el atributo name de los grupos.

### <a name="group-or-incremental-sync"></a>Sincronización incremental o de grupos

Ranger es compatible con una opción de sincronización de grupos, pero funciona como una intersección con el filtro de usuario. No es una unión entre pertenencias a grupos y filtros de usuario. Un caso de uso típico para el filtro de sincronización de grupos de Ranger es el filtro de grupo (dn=clusteradmingroup) y el filtro de usuario (city=seattle).

La sincronización incremental solo funciona para los usuarios que ya están sincronizados (la primera vez). El modo incremental no sincronizará ningún usuario nuevo agregado a los grupos después de la sincronización inicial.

### <a name="update-ranger-sync-filter"></a>Actualización del filtro de sincronización de Ranger

El filtro de LDAP se puede encontrar en la interfaz de usuario de Ambari, en la sección de configuración de la sincronización de usuarios de Ranger. El filtro existente tendrá el siguiente formato: `(|(userPrincipalName=bob@contoso.com)(userPrincipalName=hdiwatchdog-core01@CONTOSO.ONMICROSOFT.COM)(memberOf:1.2.840.113556.1.4.1941:=CN=hadoopgroup,OU=AADDC Users,DC=contoso,DC=onmicrosoft,DC=com))`. Asegúrese de agregar el predicado al final y probar el filtro con el comando de búsqueda `net ads` o ldp.exe, o bien algo similar.

## <a name="ranger-user-sync-logs"></a>Registros de sincronización de usuario de Ranger

La sincronización de usuarios de Ranger puede tener lugar en cualquiera de los nodos principales. Los registros están en `/var/log/ranger/usersync/usersync.log`. Para aumentar el nivel de detalle de los registros, siga estos pasos:

1. Inicie sesión en Ambari.
1. Vaya a la sección de configuración de Ranger.
1. Vaya a la sección avanzada **usersync-log4j**.
1. Cambie `log4j.rootLogger` a nivel de `DEBUG` (después del cambio, debería tener el siguiente aspecto: `log4j.rootLogger = DEBUG,logFile,FilterLog`).
1. Guarde la configuración y reinicie Ranger.

## <a name="next-steps"></a>Pasos siguientes

* [Problemas de autenticación en Azure HDInsight](./domain-joined-authentication-issues.md)
* [Sincronización de usuarios de Azure AD con un clúster de HDInsight](../hdinsight-sync-aad-users-to-cluster.md)
