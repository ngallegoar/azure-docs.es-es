---
title: Enterprise Security con Azure AD DS - Azure HDInsight
description: Aprenda a instalar y configurar un clúster de Enterprise Security Package de HDInsight mediante Azure Active Directory Domain Services.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: seodec18,seoapr2020
ms.date: 04/17/2020
ms.openlocfilehash: 7792ac688ede32155ec32e1f4ba25b328102f86c
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86079843"
---
# <a name="enterprise-security-package-configurations-with-azure-active-directory-domain-services-in-hdinsight"></a>Configuraciones de Enterprise Security Package con Azure Active Directory Domain Services en Azure HDInsight

Los clústeres de Enterprise Security Package (ESP) proporcionan acceso multiusuario en clústeres de Azure HDInsight. Los clústeres de HDInsight con ESP están conectados a un dominio. Esta conexión permite a los usuarios del dominio usar sus credenciales de dominio para autenticarse con los clústeres y ejecutar trabajos de macrodatos.

En este artículo, aprenderá a configurar un clúster de HDInsight con ESP mediante Azure Active Directory Domain Services (Azure AD DS).

> [!NOTE]  
> ESP está disponible con carácter general en HDInsight 3.6 y 4.0 para los tipos de clúster: Apache Spark, Interactive, Hadoop y HBase. ESP para el tipo de clúster Apache Kafka se encuentra en versión preliminar y solo cuenta con soporte técnico según nuestra capacidad técnica. No se admiten los clústeres de ESP creados antes de la fecha de disponibilidad general de ESP (1 de octubre de 2018).

## <a name="enable-azure-ad-ds"></a>Habilitación de Azure AD DS

> [!NOTE]  
> Solo los administradores de inquilinos tienen los privilegios para habilitar Azure AD DS. Si el almacenamiento del clúster es Azure Data Lake Storage Gen1 o Gen2, debe deshabilitar Azure Multi-Factor Authentication solo para los usuarios que necesitarán acceder al clúster mediante la autenticación básica de Kerberos.
>
> Puede usar direcciones [IP de confianza](../../active-directory/authentication/howto-mfa-mfasettings.md#trusted-ips) o el [acceso condicional](../../active-directory/conditional-access/overview.md) para deshabilitar Multi-Factor Authentication para usuarios concretos *solo* cuando estos accedan al intervalo de IP de la red virtual del clúster de HDInsight. Si usa el acceso condicional, asegúrese de que el punto de conexión de servicio de Active Directory esté habilitado en la red virtual de HDInsight.
>
> Si el almacenamiento del clúster es Azure Blob Storage, no deshabilite Multi-Factor Authentication.

La habilitación de Azure AD DS es un requisito previo para poder crear un clúster de HDInsight con ESP. Para obtener más información, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/tutorial-create-instance.md).

Si Azure AD DS está habilitado, todos los usuarios y objetos comienzan a sincronizarse desde Azure Active Directory (Azure AD) con Azure AD DS de forma predeterminada. La longitud de la operación de sincronización depende del número de objetos en Azure AD. La sincronización puede tardar algunos días si existen cientos de miles de objetos.

El nombre de dominio que utilice con Azure AD DS debe tener 39 caracteres como máximo para funcionar con HDInsight.

Puede elegir sincronizar solo los grupos que necesitan acceder a los clústeres de HDInsight. Esta opción de sincronizar solo determinados grupos se conoce como *sincronización con ámbito*. Para obtener instrucciones, consulte [Configuración de la sincronización con ámbito desde Azure AD con un dominio administrado](../../active-directory-domain-services/scoped-synchronization.md).

Al habilitar LDAP seguro, coloque el nombre de dominio en el nombre del firmante. Y el nombre alternativo del firmante en el certificado. Si el nombre de dominio es *contoso100.onmicrosoft.com*, asegúrese de que ese nombre exacto existe en el nombre del firmante y el nombre alternativo del firmante del certificado. Para más información, consulte [Configuración de LDAP seguro para un dominio administrado de Azure AD DS](../../active-directory-domain-services/tutorial-configure-ldaps.md).

En el ejemplo siguiente se crea un certificado autofirmado. El nombre de dominio *contoso100.onmicrosoft.com* se encuentra en `Subject` (nombre del firmante) y `DnsName` (nombre alternativo del firmante).

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
```

## <a name="check-azure-ad-ds-health-status"></a>Comprobación del estado de mantenimiento de Azure AD DS

Para ver el estado de mantenimiento de Azure Active Directory Domain Services, seleccione **Mantenimiento** en la categoría **Administrar**. Asegúrese de que el estado de Azure AD DS esté en verde (en ejecución) y de que la sincronización haya finalizado.

![Estado de Azure AD DS](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="create-and-authorize-a-managed-identity"></a>Creación y autorización de una identidad administrada

Use una *identidad administrada asignada por el usuario* para simplificar las operaciones de servicios de dominio protegidas. Cuando asigna el rol **Colaborador de HDInsight Domain Services** a la identidad administrada, este puede leer, crear, modificar y eliminar operaciones de servicios de dominio.

Ciertas operaciones de servicios de dominio, como la creación de unidades organizativas y entidades de servicio, son necesarias para HDInsight Enterprise Security Package. Las identidades administradas se pueden crear en cualquier suscripción. Para obtener más información sobre las identidades administradas en general, vea [Identidades administradas para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md). Para obtener más información sobre cómo funcionan las identidades administradas en Azure HDInsight, vea [Identidades administradas en Azure HDInsight](../hdinsight-managed-identities.md).

Para configurar clústeres de ESP, cree una identidad administrada asignada por el usuario, si aún no la tiene. Vea [`Create, list, delete, or assign a role to a user-assigned managed identity by using the Azure portal`](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

A continuación, asigne el rol **Colaborador de HDInsight Domain Services** a la identidad administrada en **Control de acceso** para Azure AD DS. Necesita privilegios de administrador de Azure AD DS para establecer esta asignación de rol.

![Control de acceso de Azure Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

La asignación del rol **Colaborador de HDInsight Domain Services** garantiza que esta identidad tiene el acceso correcto (`on behalf of`) para realizar operaciones de servicios de dominio en el dominio Azure AD DS. Estas operaciones incluyen la creación y eliminación de unidades organizativas.

Una vez que se haya dado el rol a la identidad administrada, el administrador de Azure AD DS se encargará de gestionar quién la utiliza. En primer lugar, el administrador selecciona la identidad administrada en el portal. A continuación, selecciona **Access Control (IAM)** en **Información general**. El administrador asigna el rol **Operador de identidad administrada** a los usuarios o grupos que quieran crear clústeres de ESP.

Por ejemplo, el administrador de Azure AD DS puede asignar este rol al grupo **MarketingTeam** para la identidad administrada **sjmsi**. Un ejemplo se muestra en la imagen siguiente. Esta asignación garantiza que las personas adecuadas de la organización puedan usar la identidad administrada para crear clústeres de ESP.

![Asignación de roles del operador de identidades administradas de HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)

## <a name="network-considerations"></a>Consideraciones sobre la red

> [!NOTE]  
> Azure AD DS debe implementarse en una red virtual basada en Azure Resource Manager. No se admiten redes virtuales clásicas para Azure AD DS. Para obtener más información, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/tutorial-create-instance-advanced.md#create-and-configure-the-virtual-network).

Habilite Azure AD DS. Después, se ejecuta un servidor local del Sistema de nombres de dominio (DNS) en las máquinas virtuales (VM) de Active Directory. Configure su red virtual de Azure AD DS para usar estos servidores DNS personalizados. Para buscar las direcciones IP correctas, seleccione **Propiedades** en la categoría **Administrar** y diríjase a **Dirección IP en la red virtual**.

![Localizar las direcciones IP de los servidores DNS locales](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns1.png)

Cambie la configuración de los servidores DNS en la red virtual de Azure AD DS. Para usar estas direcciones IP personalizadas, seleccione **Servidores DNS** en la categoría **Configuración**. Después, seleccione la opción **Personalizado**, escriba la primera dirección IP en el cuadro de texto y seleccione **Guardar**. Agregue más direcciones IP siguiendo los mismos pasos.

![Actualización de la configuración de DNS de la red virtual](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

Es más sencillo colocar tanto la instancia de Azure AD DS como el clúster de HDInsight en la misma red virtual de Azure. Si tiene previsto usar redes virtuales diferentes, se deben emparejar esas redes virtuales para que el controlador de dominio sea visible para las VM de HDInsight. Para más información, consulte [Emparejamiento de redes virtuales](../../virtual-network/virtual-network-peering-overview.md).

Una vez emparejadas las redes virtuales, configure la red virtual de HDInsight para que use un servidor DNS personalizado. Y escriba las direcciones IP privadas de Azure AD DS como direcciones del servidor DNS. Si ambas redes virtuales utilizan los mismos servidores DNS, su nombre de dominio personalizado se resolverá en la IP correcta y será accesible desde HDInsight. Por ejemplo, si el nombre de dominio es `contoso.com`, después de realizar este paso, `ping contoso.com` debe resolverse en la dirección IP correcta de Azure AD DS.

![Configuración de servidores DNS personalizados para una red virtual emparejada](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

Si utiliza reglas de grupos de seguridad de red (NSG) en la subred de HDInsight, debe permitir las [direcciones IP necesarias](../hdinsight-management-ip-addresses.md) para el tráfico entrante y saliente.

Para probar la configuración de la red, debe unir una máquina virtual Windows a la subred o la red virtual de HDInsight y hacer ping en el nombre de dominio. (Debe resolverse en una dirección IP). Ejecute **ldp.exe** para acceder al dominio de Azure AD DS. A continuación, una esta VM Windows al dominio para confirmar que todas las llamadas de RPC necesarias se realizaron correctamente entre el cliente y el servidor.

Use **nslookup** para confirmar el acceso de la red a su cuenta de almacenamiento. O cualquier base de datos externa que puede usar (por ejemplo, metastore de Hive externa o Ranger DB). Asegúrese de que las reglas de grupo de seguridad de red de la subred de Azure AD DS permitan los [puertos necesarios](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers), si un grupo de seguridad de red protege Azure AD DS. Si la unión a un dominio de esta VM Windows se realiza correctamente, puede continuar con el siguiente paso y crear clústeres de ESP.

## <a name="create-an-hdinsight-cluster-with-esp"></a>Crear un clúster de HDInsight con ESP

Después de configurar correctamente los pasos anteriores, el siguiente paso es crear el clúster de HDInsight con ESP habilitado. Cuando cree un clúster de HDInsight, puede habilitar Enterprise Security Package en la pestaña **Seguridad y redes**. En el caso de una plantilla de Azure Resource Manager para la implementación, utilice una vez la experiencia del portal. A continuación, descargue la plantilla rellenada previamente en la página **Revisar y crear** para su reutilización en el futuro.

También puede habilitar la característica [Agente de identidad de HDInsight](identity-broker.md) durante la creación del clúster. La característica Agente de identidad permite iniciar sesión en Ambari con Multi-Factor Authentication y obtener los vales de Kerberos necesarios sin necesidad de hash de contraseña en Azure AD DS.

> [!NOTE]  
> Los seis primeros caracteres de los nombres de clúster ESP deben ser únicos en su entorno. Por ejemplo, si tiene varios clústeres ESP en diferentes redes virtuales, debe elegir una convención de nomenclatura que garantice que los seis primeros caracteres de los nombres de clúster sean únicos.

![Validación del dominio de Enterprise Security Package de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp.png)

Una vez que habilite ESP, se detectarán y validarán las configuraciones erróneas comunes relacionadas con Azure AD DS. Después de corregir estos errores, puede continuar con el paso siguiente.

![Validación de dominio con error de Enterprise Security Package de Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-esp-error.png)

Si crea un clúster de HDInsight mediante ESP, debe proporcionar los siguientes parámetros:

* **Usuario administrador de clúster**: elija un administrador para el clúster desde su instancia sincronizada de Azure AD DS. Esta cuenta de dominio ya debe estar sincronizada y disponible en Azure AD DS.

* **Grupos de acceso de clúster**: los grupos de seguridad cuyos usuarios desea sincronizar con el clúster y que tengan acceso a este deben estar disponibles en Azure AD DS. Un ejemplo es el grupo HiveUsers. Para más información consulte [Creación de un grupo y adición de miembros en Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

* **Dirección URL de LDAPS**: Un ejemplo es `ldaps://contoso.com:636`.

La identidad administrada que se ha creado se puede elegir en la lista desplegable **Identidad administrada asignada por el usuario** al crear un nuevo clúster.

![Identidad administrada de Azure HDInsight ESP Active Directory Domain Services](./media/apache-domain-joined-configure-using-azure-adds/azure-portal-cluster-security-networking-identity.png).

## <a name="next-steps"></a>Pasos siguientes

* Para configurar las directivas de Hive y ejecutar las consultas de Hive, vea [Configurar directivas de Apache Hive en HDInsight unido a un dominio](apache-domain-joined-run-hive.md).
* Para usar SSH para conectarse a clústeres de HDInsight con ESP, consulte [Usar SSH con Apache Hadoop basado en Linux en HDInsight desde Linux, Unix u OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight).
