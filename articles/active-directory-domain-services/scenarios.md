---
title: Escenarios de implementación comunes para Azure AD Domain Services | Microsoft Docs
description: Obtenga información sobre algunos de los escenarios comunes y casos de uso de Azure Active Directory Domain Services para proporcionar valor y satisfacer las necesidades empresariales.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: iainfou
ms.openlocfilehash: ac67ef64ca4850c6e805b5314ace856114d889a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77917237"
---
# <a name="common-use-cases-and-scenarios-for-azure-active-directory-domain-services"></a>Escenarios y casos de uso comunes para Azure Active Directory Domain Services

Azure Active Directory Domain Services (Azure AD DS) proporciona servicios de dominio administrados como, por ejemplo, unión a un dominio, directivas de grupo, protocolo ligero de acceso a directorios (LDAP) y autenticación Kerberos o NTLM. Azure AD DS se integra con el inquilino de Azure AD existente, lo que posibilita que los usuarios inicien sesión con sus credenciales existentes. Estos servicios de dominio se usan sin necesidad de implementar, administrar y aplicar revisiones a controladores de dominio en la nube, lo que proporciona una migración mediante lift-and-shift más suave de los recursos locales a Azure.

En este artículo se describen algunos escenarios empresariales comunes en los que Azure AD DS proporciona valor y satisface esas necesidades.

## <a name="secure-administration-of-azure-virtual-machines"></a>Administración segura de máquinas virtuales de Azure

Para permitir el uso de un solo conjunto de credenciales de AD, las máquinas virtuales de Azure se pueden unir a un dominio administrado de Azure AD DS. Este enfoque reduce los problemas de administración de credenciales, como el mantenimiento de cuentas de administrador local en cada máquina virtual o la separación de cuentas y contraseñas entre entornos.

Las máquinas virtuales de servidor que están unidas al dominio administrado de Azure AD DS también se pueden administrar y proteger mediante la directiva de grupo. Las líneas de base de seguridad necesarias se pueden aplicar a las máquinas virtuales para bloquearlas con arreglo a las directrices de seguridad corporativa. Por ejemplo, puede utilizar las funcionalidades de administración de directiva de grupo para restringir los tipos de aplicaciones que pueden iniciarse en la máquina virtual.

![Administración simplificada de máquinas virtuales de Azure](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Echemos un vistazo a un escenario de ejemplo común. A medida que los servidores y otras infraestructuras alcanzan el final de su ciclo de vida, Contoso quiere trasladar a la nube muchas aplicaciones que actualmente se hospedan de forma local. Su norma actual de TI dictamina que los servidores que hospedan aplicaciones corporativas se deben unir a un dominio y administrarse mediante la directiva de grupo. El administrador de TI de Contoso preferiría unirse a un dominio de máquinas virtuales implementadas en Azure para facilitar la administración, ya que los usuarios pueden iniciar sesión con sus credenciales corporativas. Cuando están unidas a un dominio, las máquinas virtuales pueden configurarse también para cumplir con las líneas de base de seguridad necesarias mediante objetos de directiva de grupo. Contoso preferiría no tener que implementar, supervisar y administrar sus propios controladores en Azure.

Azure AD DS es una buena opción para este caso de uso. Un dominio administrado de Azure AD DS le permite unirse a un dominio de máquinas virtuales, usar un único conjunto de credenciales y aplicar la directiva de grupo. Como dominio administrado, no tiene que configurar y mantener los controladores de dominio.

### <a name="deployment-notes"></a>Notas de implementación

Las siguientes consideraciones de implementación se aplican a este caso de uso de ejemplo:

* Los dominios administrados Azure AD DS usan de forma predeterminada una única estructura de unidad organizativa (OU) plana. Todas las máquinas virtuales unidas a un dominio se encuentran en una sola unidad organizativa. Si lo desea, puede crear unidades organizativas personalizadas.
* Azure AD DS utiliza un GPO integrado para cada uno de los contenedores de equipos y usuarios. Para un control adicional, puede crear GPO personalizados y dirigirlos a unidades organizativas personalizadas.
* Azure AD DS admite el esquema base del objeto de equipo de AD. No puede extender el esquema del objeto de equipo.

## <a name="lift-and-shift-on-premises-applications-that-use-ldap-bind-authentication"></a>Migración mediante lift-and-shift de las aplicaciones locales que usan la autenticación de enlace LDAP

Como un escenario de ejemplo, Contoso cuenta con una aplicación local que se compró a un ISV hace muchos años. La aplicación está actualmente en modo de mantenimiento por el ISV y resulta excesivamente caro solicitar cambios en la aplicación. Esta aplicación tiene un front-end basado en web que recopila las credenciales de usuario mediante un formulario web y luego autentica a los usuarios mediante la realización de un enlace LDAP al entorno local de AD DS.

![Enlace LDAP](./media/active-directory-domain-services-scenarios/ldap-bind.png)

A Contoso le gustaría migrar esta aplicación a Azure. La aplicación debe continuar funcionando tal cual, sin necesidad de realizar cambios. Además, los usuarios deben poder autenticarse con las credenciales corporativas existentes y sin entrenamiento adicional. Debe ser transparente para los usuarios finales, donde que se ejecuta la aplicación.

En este escenario, Azure AD DS permite a las aplicaciones realizar enlaces LDAP como parte del proceso de autenticación. Las aplicaciones locales heredadas pueden migrar mediante lift-and-shift a Azure y continuar seguir autenticando a los usuarios sin ningún cambio en la configuración o la experiencia del usuario.

### <a name="deployment-notes"></a>Notas de implementación

Las siguientes consideraciones de implementación se aplican a este caso de uso de ejemplo:

* Asegúrese de que la aplicación no necesita modificar o escribir en el directorio. No se admite el acceso de escritura LDAP a un dominio administrado de Azure AD DS.
* No se pueden cambiar las contraseñas directamente en el dominio administrado de Azure AD DS. Los usuarios finales pueden cambiar su contraseña bien mediante el mecanismo de autoservicio de cambio de contraseña de Azure AD o en el directorio local. Estos cambios se sincronizan entonces y están disponibles automáticamente en el dominio administrado de Azure AD DS.

## <a name="lift-and-shift-on-premises-applications-that-use-ldap-read-to-access-the-directory"></a>Migración mediante lift-and-shift de las aplicaciones locales que usan la lectura LDAP para acceder al directorio

Al igual que en el escenario de ejemplo anterior, supongamos que Contoso tiene una aplicación de línea de negocio (LOB) local que se desarrolló hace casi una década. Esta aplicación es compatible con directorios y se ha diseñado para usar LDAP para leer información o atributos acerca de los usuarios de AD DS. La aplicación no modifica los atributos ni escribe en el directorio.

Contoso quiere migrar esta aplicación a Azure y retirar el hardware local antiguo que actualmente hospeda esta aplicación. La aplicación no se puede volver a escribir para que use las modernas API de directorio, como Microsoft Graph API basada en REST. Se necesita una opción de migración mediante lift-and-shift mediante la cual se pueda migrar la aplicación para ejecutarse en la nube, sin modificar el código o volver a escribirla.

Para ayudar con este escenario, Azure AD DS permite a las aplicaciones realizar lecturas LDAP en el dominio administrado para obtener la información de los atributos que necesita. No es necesario volver a escribir la aplicación, por lo que una migración mediante lift-and-shift en Azure permite a los usuarios seguir usando la aplicación sin darse cuenta de que hay un cambio en el lugar donde se ejecuta.

### <a name="deployment-notes"></a>Notas de implementación

Las siguientes consideraciones de implementación se aplican a este caso de uso de ejemplo:

* Asegúrese de que la aplicación no necesita modificar o escribir en el directorio. No se admite el acceso de escritura LDAP a un dominio administrado de Azure AD DS.
* Asegúrese de que la aplicación no necesita un esquema de Active Directory ampliado o personalizado. No se admiten extensiones de esquema en Azure AD DS.

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure"></a>Migración de una aplicación de dominio o servicio local a Azure

Algunas aplicaciones incluyen varios niveles, donde uno de los niveles necesita realizar llamadas autenticadas a un nivel de back-end, como una base de datos. Las cuentas de servicio de AD se usan normalmente en estos escenarios. Al migrar mediante lift-and-shift las aplicaciones en Azure, Azure AD DS le permite seguir usando cuentas de servicio de la misma manera. Puede optar por usar la misma cuenta de servicio que se sincroniza desde el directorio local para Azure AD o crear una unidad organizativa personalizada y, a continuación, crear una cuenta de servicio independiente en esa unidad organizativa. Con cualquiera de estos enfoques, las aplicaciones siguen funcionando de la misma manera para realizar llamadas autenticadas a otros niveles y servicios.

![Cuenta de servicio mediante WIA](./media/active-directory-domain-services-scenarios/wia-service-account.png)

En este escenario de ejemplo, Contoso tiene una aplicación de almacén de software personalizada que incluye un front-end web, un servidor SQL y un servidor FTP de back-end. La autenticación integrada de Windows mediante las cuentas de servicio autentica el web front-end en el servidor FTP. El front-end web está configurado para ejecutarse como una cuenta de servicio. El servidor back-end está configurado para autorizar el acceso de la cuenta de servicio para el front-end web. Contoso no desea implementar y administrar sus propias máquinas virtuales de controlador de dominio en la nube para migrar esta aplicación a Azure.

En este escenario, los servidores que hospedan el front-end web, el servidor SQL y el servidor FTP se pueden migrar a máquinas virtuales de Azure y unirse a un dominio administrado de Azure AD DS. Las máquinas virtuales pueden usar la misma cuenta de servicio en su directorio local para los propósitos de autenticación de la aplicación, que se sincroniza a través de Azure AD mediante Azure AD Connect.

### <a name="deployment-notes"></a>Notas de implementación

Las siguientes consideraciones de implementación se aplican a este caso de uso de ejemplo:

* Asegúrese de que las aplicaciones usan un nombre de usuario y una contraseña para la autenticación. Azure AD DS no admite la autenticación basada en certificado o tarjeta inteligente.
* No se pueden cambiar las contraseñas directamente en el dominio administrado de Azure AD DS. Los usuarios finales pueden cambiar su contraseña bien mediante el mecanismo de autoservicio de cambio de contraseña de Azure AD o en el directorio local. Estos cambios se sincronizan entonces y están disponibles automáticamente en el dominio administrado de Azure AD DS.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Implementaciones de Servicios de Escritorio Remoto de Windows Server en Azure

Puede usar Azure AD DS para proporcionar servicios de dominio administrado a los servidores de escritorio remotos implementados en Azure. Para más información acerca de este escenario de implementación, vea [cómo integrar Azure AD Domain Services con su implementación de RDS][windows-rds].

## <a name="domain-joined-hdinsight-clusters"></a>Usuarios de clústeres de HDInsight unidos a dominio

Puede configurar un clúster de Azure HDInsight que está unido a un dominio administrado de Azure AD DS con Apache Ranger habilitado. Puede crear y aplicar directivas de Hive a través de Apache Ranger y permitir a los usuarios, como los científicos de datos, conectarse a Hive con herramientas basadas en ODBC, como Excel o Tableau. Seguimos trabajando para incluir otras cargas de trabajo, como HBase, Spark y Storm a HDInsight unido a un dominio.

Para más información sobre este escenario de implementación, consulte [cómo configurar clústeres de HDInsight unidos a un dominio][hdinsight]

## <a name="next-steps"></a>Pasos siguientes

Para comenzar, [cree y configure una instancia de Azure Active Directory Domain Services][tutorial-create-instance].

<!-- INTERNAL LINKS -->
[hdinsight]: ../hdinsight/domain-joined/apache-domain-joined-configure.md
[tutorial-create-instance]: tutorial-create-instance.md

<!-- EXTERNAL LINKS -->
[windows-rds]: /windows-server/remote/remote-desktop-services/rds-azure-adds
