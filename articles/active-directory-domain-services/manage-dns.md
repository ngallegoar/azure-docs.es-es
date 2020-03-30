---
title: Administración de DNS para Azure AD Domain Services | Microsoft Docs
description: Obtenga información sobre cómo instalar las herramientas del servidor DNS para administrar DNS en un dominio administrado con Azure Active Directory Domain Services.
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: iainfou
ms.openlocfilehash: 2694a5f250b746748a1b42ac4d211aa28ef1ebad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77613693"
---
# <a name="administer-dns-in-an-azure-ad-domain-services-managed-domain"></a>Administración de DNS en un dominio administrado con Azure AD Domain Services

En Azure Active Directory Domain Services (Azure AD DS), un componente clave es DNS (resolución de nombres de dominio). Azure AD DS incluye un servidor DNS que proporciona la resolución de nombres para el dominio administrado. Este servidor DNS incluye registros y actualizaciones de DNS integrados para los componentes clave que permiten la ejecución del servicio.

Cuando ejecute sus propias aplicaciones y servicios, podría tener que crear registros de DNS para las máquinas que no están unidas al dominio, configurar direcciones IP virtuales para los equilibradores de carga o configurar reenviadores DNS externos. Los usuarios que pertenecen al grupo *Administradores del controlador de dominio de AAD* reciben privilegios de administración de DNS en el dominio administrado con Azure AD DS y pueden crear y editar registros de DNS personalizados.

En un entorno híbrido, las zonas DNS y los registros configurados en un entorno de AD DS local no se sincronizan con Azure AD DS. Para definir y usar sus propias entradas DNS, cree registros en el servidor DNS de Azure AD DS o use reenviadores condicionales que apunten a los servidores DNS existentes en el entorno.

En este artículo se muestra cómo se instalan las herramientas del servidor DNS y cómo se usa la consola DNS para administrar registros en Azure AD DS.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Antes de empezar

Para completar este artículo, necesitará los siguientes recursos y privilegios:

* Una suscripción de Azure activa.
    * Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un inquilino de Azure Active Directory asociado a su suscripción, ya sea sincronizado con un directorio en el entorno local o con un directorio solo en la nube.
    * Si es necesario, [cree un inquilino de Azure Active Directory][create-azure-ad-tenant] o [asocie una suscripción a Azure con su cuenta][associate-azure-ad-tenant].
* Un dominio administrado de Azure Active Directory Domain Services habilitado y configurado en su inquilino de Azure AD.
    * Si es necesario, complete el tutorial para [crear y configurar una instancia de Azure Active Directory Domain Services][create-azure-ad-ds-instance].
* Una máquina virtual de administración de Windows Server que esté unida al dominio administrado de Azure AD DS.
    * Si es necesario, complete el tutorial para [crear una máquina virtual de Windows Server y unirla a un dominio administrado][create-join-windows-vm].
* Una cuenta de usuario que sea miembro del grupo de *administradores de Azure AD DC* en el inquilino de Azure AD.

## <a name="install-dns-server-tools"></a>Instalación de las herramientas del servidor DNS

Para crear y modificar registros DNS en Azure AD DS, debe instalar las herramientas del servidor DNS. Estas herramientas se pueden instalar como una característica en Windows Server. Para obtener más información sobre cómo instalar las herramientas administrativas en un cliente de Windows, consulte cómo se instalan las [herramientas de administración remota del servidor (RSAT)][install-rsat].

1. Inicie sesión en la máquina virtual de administración. Si quiere conocer los pasos para conectarse mediante Azure Portal, consulte [Conexión a una máquina virtual de Windows Server][connect-windows-server-vm].
1. Si **Administrador del servidor** no se abre de forma predeterminada al iniciar sesión en la máquina virtual, seleccione el menú **Inicio** y, a continuación, elija **Administrador del servidor**.
1. En el *Panel de información* de la ventana **Administrador del servidor**, seleccione **Agregar roles y características**.
1. En la página **Antes de comenzar** del *Asistente para agregar roles y características*, seleccione **Siguiente**.
1. En *Tipo de instalación*, deje activada la opción **Instalación basada en características o en roles** y seleccione **Siguiente**.
1. En la página **Selección de servidor**, elija la máquina virtual actual del grupo de servidores, por ejemplo *mivm.aaddscontoso.com*, y seleccione **Siguiente**.
1. En la página **Roles de servidor**, haga clic en **Siguiente**.
1. En la página **Características**, expanda el nodo **Herramientas de administración remota del servidor** y el nodo **Herramientas de administración de roles**. Seleccione la característica **Herramientas del servidor DNS** en la lista de herramientas de administración de roles.

    ![Selección para instalar las herramientas del servidor DNS en la lista de herramientas de administración de roles disponibles](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

1. En la página **Confirmación**, seleccione **Instalar**. Las herramientas de Administración de directivas de grupo pueden tardar un minuto o dos en instalarse.
1. Cuando finalice la instalación de la característica, haga clic en **Cerrar** para salir del Asistente para **Agregar roles y características**.

## <a name="open-the-dns-management-console-to-administer-dns"></a>Apertura de la consola de administración de DNS para administrar DNS

Con las herramientas del servidor DNS instaladas, puede administrar los registros de DNS en el dominio administrado con Azure AD DS.

> [!NOTE]
> Para administrar DNS en un dominio administrado con Azure AD DS, debe haber iniciado sesión en una cuenta de usuario que sea miembro del grupo *Administradores del controlador de dominio de AAD*.

1. En la pantalla Inicio, seleccione **Herramientas administrativas**. Se muestra una lista de las herramientas de administración disponibles, incluido **DNS**, instalado en la sección anterior. Seleccione **DNS** para iniciar la consola de administración de DNS.
1. En el cuadro de diálogo **Conectar a servidor DNS**, seleccione **El siguiente equipo** y escriba el nombre de dominio DNS del dominio administrado, como *aaddscontoso.com*:

    ![Conexión al dominio administrado con Azure AD DS en la consola DNS](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

1. La consola DNS se conecta al dominio administrado con Azure AD DS que se ha especificado. Expanda las **Zonas de búsqueda directa** o las **Zonas de búsqueda inversa** para crear las entradas DNS necesarias, o bien edite los registros existentes según sea necesario.

    ![Consola DNS: administración del dominio](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

> [!WARNING]
> Cuando administre registros con las herramientas del servidor DNS, asegúrese de no eliminar ni modificar los registros DNS integrados que usa Azure AD DS. Los registros DNS integrados incluyen registros DNS de dominio, registros de servidor de nombres y otros registros usados para la ubicación del controlador de dominio. Si modifica estos registros, se interrumpirán los servicios de dominio en la red virtual.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la administración de DNS, consulte el artículo [Herramientas de DNS](https://technet.microsoft.com/library/cc753579.aspx)de TechNet.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[create-join-windows-vm]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm

<!-- EXTERNAL LINKS -->
[install-rsat]: /windows-server/remote/remote-server-administration-tools#BKMK_Thresh
