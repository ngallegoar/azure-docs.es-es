---
title: Sincronización de LDAP en Ranger y Apache Ambari en Azure HDInsight
description: Aborde la sincronización de LDAP en Ranger y Ambari y proporcione directrices generales.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 0779ac261fbb4ee91bf63021bb0cc685a371c2b2
ms.sourcegitcommit: bbd66b477d0c8cb9adf967606a2df97176f6460b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93234076"
---
# <a name="ldap-sync-in-ranger-and-apache-ambari-in-azure-hdinsight"></a>Sincronización de LDAP en Ranger y Apache Ambari en Azure HDInsight

Los clústeres de Enterprise Security Package (ESP) de HDInsight usan Ranger para la autorización. Apache Ambari y Ranger sincronizan usuarios y grupos de forma independiente y funcionan de un modo ligeramente diferente. El objetivo de este artículo es abordar la sincronización de LDAP en Ranger y Ambari.

## <a name="general-guidelines"></a>Directrices generales

* Implemente siempre los clústeres con uno o varios grupos.
* Si desea utilizar más grupos en el clúster, compruebe si tiene sentido actualizar las pertenencias a grupos en Azure Active Directory (Azure AD).
* Si desea cambiar los grupos de clústeres, puede cambiar los filtros de sincronización mediante Ambari.
* Todos los cambios de pertenencia a grupos en Azure AD se reflejan en el clúster en las sincronizaciones posteriores. Los cambios deben sincronizarse primero con Azure AD Domain Services (Azure AD DS) y, a continuación, con los clústeres.
* Los clústeres de HDInsight utilizan Samba/Winbind para proyectar las pertenencias a grupos en los nodos del clúster.
* Los miembros del grupo se sincronizan de forma transitiva (todos los subgrupos y sus miembros) en Ambari y Ranger. 

## <a name="users-are-synced-separately"></a>Los usuarios se sincronizan por separado

 * Ambari y Ranger no comparten la base de datos de usuario porque tienen dos fines diferentes. 
   * Si un usuario necesita utilizar la interfaz de usuario de Ambari, el usuario debe sincronizarse con Ambari. 
   * Si el usuario no está sincronizado con Ambari, la API o la interfaz de usuario de Ambari lo rechazará, pero otras partes del sistema funcionarán (es Ranger o Resource Manager quien las protege, no Ambari).
   * Para incluir usuarios o grupos en las directivas de Ranger, las entidades de seguridad se deben sincronizar explícitamente en Ranger.

## <a name="ambari-user-sync-and-configuration"></a>Sincronización y configuración de usuarios de Ambari

Desde los nodos principales, un trabajo Cron, `/opt/startup_scripts/start_ambari_ldap_sync.py`, se ejecuta cada hora para programar la sincronización de usuarios. El trabajo Cron llama a las API REST de Ambari para realizar la sincronización. El script envía una lista de usuarios y grupos para sincronizar (ya que es posible que los usuarios no pertenezcan a los grupos especificados, ambos se especifican de forma individual). Ambari sincroniza sAMAccountName como el nombre de usuario y todos los miembros del grupo de forma transitiva.

Los registros deben estar en `/var/log/ambari-server/ambari-server.log`. Para más información, consulte el artículo sobre cómo [configurar el nivel de registro de Ambari](https://docs.cloudera.com/HDPDocuments/Ambari-latest/administering-ambari/content/amb_configure_ambari_logging_level.html).

En los clústeres de Data Lake, el enlace de creación posterior al usuario se usa para crear las carpetas particulares de los usuarios sincronizados y se establecen como los propietarios de las carpetas particulares. Si el usuario no se ha sincronizado con Ambari correctamente, podría encontrarse con errores en la ejecución de los trabajos, ya que es posible que la carpeta de inicio no esté correctamente configurada.

## <a name="ranger-user-sync-and-configuration"></a>Sincronización y configuración de usuarios de Ranger

Ranger tiene un motor de sincronización integrado que se ejecuta cada hora para sincronizar a usuarios. No comparte la base de datos de usuario con Ambari. HDInsight configura el filtro de búsqueda para sincronizar el usuario administrador, el usuario guardián y los miembros del grupo especificado durante la creación del clúster. Los miembros del grupo se sincronizarán de manera transitiva:

1. Deshabilite la sincronización incremental.
1. Habilite la asignación de sincronización de los grupos de usuarios.
1. Especifique el filtro de búsqueda para incluir los miembros del grupo transitivo.
1. Sincronice el atributo sAMAccountName para los usuarios y el atributo name para los grupos.

### <a name="group-or-incremental-sync"></a>Sincronización incremental o de grupos

Ranger es compatible con una opción de sincronización de grupos, pero funciona como una intersección con el filtro de usuario, no como una unión entre las pertenencias a grupos y el filtro de usuario. Un caso de uso típico para el filtro de sincronización de grupos de Ranger es el filtro de grupo (dn=clusteradmingroup) y el filtro de usuario (city=seattle).

La sincronización incremental solo funciona para los usuarios que ya están sincronizados (la primera vez). El modo incremental no sincronizará ningún usuario nuevo agregado a los grupos después de la sincronización inicial.

### <a name="update-ranger-sync-filter"></a>Actualización del filtro de sincronización de Ranger

El filtro de LDAP se puede encontrar en la interfaz de usuario de Ambari, en la sección de configuración de la sincronización de usuarios de Ranger. El filtro existente tendrá el siguiente formato: `(|(userPrincipalName=bob@contoso.com)(userPrincipalName=hdiwatchdog-core01@CONTOSO.ONMICROSOFT.COM)(memberOf:1.2.840.113556.1.4.1941:=CN=hadoopgroup,OU=AADDC Users,DC=contoso,DC=onmicrosoft,DC=com))`. Asegúrese de agregar el predicado al final y probar el filtro con el comando de búsqueda `net ads` o ldp.exe, o bien algo similar.

## <a name="ranger-user-sync-logs"></a>Registros de sincronización de usuario de Ranger

La sincronización de usuarios de Ranger puede tener lugar en cualquiera de los nodos principales. Los registros están en `/var/log/ranger/usersync/usersync.log`. Para aumentar el nivel de detalle de los registros, siga estos pasos:

1. Inicie sesión en Ambari.
1. Vaya a la sección de configuración de Ranger.
1. Vaya a la sección avanzada **usersync-log4j**.
1. Cambie `log4j.rootLogger` al nivel `DEBUG`. (Después de cambiarlo, debería ser similar a `log4j.rootLogger = DEBUG,logFile,FilterLog`).
1. Guarde la configuración y reinicie Ranger.

## <a name="known-issues-with-ranger-user-sync"></a>Problemas conocidos de la sincronización de usuarios de Ranger
* Si el nombre de grupo tiene caracteres Unicode, la sincronización de Ranger no podrá sincronizar ese objeto. Si un usuario pertenece a un grupo que tiene caracteres internacionales, Ranger sincroniza la pertenencia parcial a grupos.
* El nombre de usuario (sAMAccountName) y el nombre de grupo (name) deben tener una longitud de 20 caracteres o menos. Si el nombre de grupo es más largo, el usuario se tratará como si no perteneciera al grupo, al calcular los permisos.

## <a name="next-steps"></a>Pasos siguientes

* [Problemas de autenticación en Azure HDInsight](./domain-joined-authentication-issues.md)
* [Sincronización de usuarios de Azure AD con un clúster de HDInsight](../hdinsight-sync-aad-users-to-cluster.md)
