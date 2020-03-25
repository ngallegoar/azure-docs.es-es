---
title: Administración del portal del Contrato Enterprise de Azure
description: En este artículo se explican las tareas comunes que un administrador lleva a cabo en el portal del Contrato Enterprise de Azure.
author: bandersmsft
ms.author: banders
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: boalcsva
ms.openlocfilehash: 79225d4dfe9e53da6936f8647c9f5a1dff0b4909
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "78301479"
---
# <a name="azure-ea-portal-administration"></a>Administración del portal del Contrato Enterprise de Azure

En este artículo se explican las tareas comunes que un administrador lleva a cabo en el portal del Contrato Enterprise de Azure (https://ea.azure.com). El portal del Contrato Enterprise de Azure es un portal de administración en línea que ayuda a los clientes a administrar el costo de los servicios del Contrato Enterprise de Azure. Para obtener información introductoria acerca del portal del Contrato Enterprise de Azure, consulte el artículo [Introducción al portal del Contrato Enterprise de Azure](ea-portal-get-started.md).

## <a name="add-a-new-enterprise-administrator"></a>Adición de un nuevo administrador de empresa

Los administradores de empresa tienen los máximos privilegios al administrar una inscripción del Contrato Enterprise de Azure. Se creó el administrador del Contrato Enterprise de Azure inicial cuando se configuró el acuerdo del Contrato Enterprise. Sin embargo, puede agregar o quitar nuevos administradores en cualquier momento. Solo los administradores existentes agregan nuevos administradores. Para más información sobre cómo agregar administradores de empresa adicionales, consulte [Creación de otro administrador de empresa](ea-portal-get-started.md#create-another-enterprise-administrator). Para más información acerca de los roles del perfil de facturación, consulte [Tareas y roles del perfil de facturación](understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="update-user-state-from-pending-to-active"></a>Actualización del estado de usuario de pendiente a activo

Cuando se agregan nuevos propietarios de la cuenta a una inscripción del Contrato Enterprise de Azure por primera vez, su estado aparece como _pendiente_. Cuando un nuevo propietario de la cuenta recibe el correo electrónico de bienvenida de activación, puede iniciar sesión para activar su cuenta. Al activar su cuenta, el estado de la cuenta se actualiza de _pendiente_ a _activa_. El propietario de la cuenta tiene que leer el mensaje de advertencia y seleccionar **Continuar**. Es posible que se pida a los nuevos usuarios que escriban su nombre y apellidos para crear una cuenta comercial. En ese caso, deben agregar la información necesaria para continuar y, acto seguido, se activará la cuenta.

## <a name="add-a-department-admin"></a>Adición de un administrador de departamento

Cuando un administrador de Contrato Enterprise de Azure crea un departamento, el administrador puede agregar administradores de departamento y asociar cada uno de ellos a un departamento. Un administrador de departamento puede crear nuevas cuentas. Se necesitan nuevas cuentas para que se creen las suscripciones del Contrato Enterprise de Azure.

Para más información sobre cómo agregar un administrador de departamento, consulte [Creación de un administrador de departamento del Contrato Enterprise de Azure](ea-portal-get-started.md#add-a-department-administrator).

## <a name="associate-an-account-to-a-department"></a>Asociar una cuenta a un departamento

Los administradores de empresa pueden asociar cuentas existentes a los departamentos de la inscripción.

### <a name="to-associate-an-account-to-a-department"></a>Para asociar una cuenta a un departamento

1. Inicie sesión en Azure EA Portal como administrador de empresa.
1. Seleccione **Administrar** en el panel de navegación izquierdo.
1. Seleccione **Departamento**.
1. Mantenga el puntero sobre la fila en que se encuentra la cuenta y seleccione el icono de lápiz de la derecha.
1. Seleccione el departamento en el menú desplegable.
1. Seleccione **Guardar**.

## <a name="department-spending-quotas"></a>Cuota de gasto de departamento

Los clientes con Contrato Enterprise pueden establecer o cambiar las cuotas de gasto de cada departamento de una inscripción. El importe de la cuota de gasto se establece para el período de compromiso actual. Al final del período de compromiso actual, el sistema ampliará la cuota de gasto existente al siguiente período de compromiso a menos que se actualicen los valores.

El administrador del departamento puede ver la cuota de gasto, pero solo el administrador de la empresa puede actualizar el importe de la cuota. El administrador de la empresa y el administrador del departamento reciben notificaciones una vez que la cuota ha alcanzado el 50 %, 75 %, 90 % y 100 %.

### <a name="enterprise-administrator-to-set-the-quota"></a>Para que el administrador de la empresa establezca la cuota:

 1. Abra Azure EA Portal.
 1. Seleccione **Administrar** en el panel de navegación izquierdo.
 1. Seleccione la pestaña **Departamento**.
 1. Seleccione el departamento.
 1. Seleccione el icono del lápiz de la sección Department Details (Detalles del departamento) o seleccione el símbolo **+ Add Department** (Agregar departamento) para agregar una cuota de gasto junto con un nuevo departamento.
 1. En Department Details (Detalles del departamento), escriba el importe de la cuota de gasto en la moneda de la inscripción en el cuadro Spending Quota $ (Cuota de gasto) (debe ser mayor que 0).
    - El nombre del departamento y el centro de costos también se pueden editar en este momento.
 1. Seleccione **Guardar**.

La cuota de gasto del departamento ahora estará visible en la vista de lista de departamentos en la pestaña Department (Departamento). Al final del compromiso actual, Azure EA Portal mantendrá las cuotas de gasto para el siguiente período de compromiso.

El importe de la cuota de departamento es independiente del compromiso monetario actual y el importe de la cuota y las alertas solo se aplican al uso de la primera parte. La cuota de gasto del departamento solo tiene fines informativos, no supone una exigencia de los límites de gasto.

### <a name="department-administrator-to-view-the-quota"></a>Para que el administrador de departamento pueda ver la cuota:

1. Abra Azure EA Portal.
1. Seleccione **Administrar** en el panel de navegación izquierdo.
1. Seleccione la pestaña **Department** (Departamento) y accederá a la vista de lista de departamentos con sus cuotas de gasto.

Si es un cliente indirecto, el partner de canal debe habilitar las características de costos.

## <a name="enterprise-user-roles"></a>Roles de los usuarios de empresa

El portal del Contrato Enterprise de Azure le ayuda a administrar los costos y el uso del Contrato Enterprise de Azure. Hay tres roles principales en el portal del Contrato Enterprise de Azure:

- Administrador del Contrato Enterprise
- Administrador de departamentos
- Propietario de cuenta

Cada rol tiene un nivel de acceso y autoridad diferente.

Para más información sobre los roles de usuario, consulte [Roles de usuario de empresa](https://docs.microsoft.com/azure/billing/billing-ea-portal-get-started#enterprise-user-roles).

## <a name="add-an-azure-ea-account"></a>Adición de una cuenta del Contrato Enterprise de Azure

La cuenta del Contrato Enterprise de Azure es una unidad organizativa del portal del Contrato Enterprise de Azure. Se utiliza para administrar las suscripciones y los informes. Para acceder a los servicios de Azure y usarlos, debe crear una cuenta o tener una ya creada.

Para más información sobre las cuentas de Azure, consulte la sección sobre cómo agregar una cuenta.

## <a name="enterprise-devtest-offer"></a>Oferta Desarrollo/pruebas - Enterprise

Como administrador de empresa de Azure, puede permitir a los propietarios de cuentas de su organización crear suscripciones basadas en la oferta Desarrollo/pruebas del Contrato Enterprise. Para ello, seleccione el cuadro Desarrollo/pruebas del propietario de la cuenta en Azure EA Portal.

Una vez que haya activado la casilla Dev/Test (Desarrollo/pruebas), comuníqueselo al propietario de la cuenta para que pueda configurar las suscripciones de Desarrollo/pruebas del Contrato Enterprise necesarias para sus equipos de suscriptores de desarrollo y pruebas.

La oferta permite a los suscriptores activos de Visual Studio ejecutar cargas de trabajo de desarrollo y pruebas en Azure a las tarifas especiales de desarrollo y pruebas. Proporciona acceso a toda la galería de imágenes de desarrollo y pruebas incluido Windows 8.1 y Windows 10.

### <a name="to-set-up-the-enterprise-devtest-offer"></a>Para configurar la oferta Desarrollo/pruebas - Enterprise:

1. Inicie sesión como administrador de empresa.
1. Seleccione **Administrar** en el panel de navegación izquierdo.
1. Seleccione la pestaña **Cuenta**.
1. Seleccione la fila de la cuenta en la que desea habilitar el acceso a Desarrollo/pruebas.
1. Seleccione el símbolo del lápiz situado a la derecha de la fila.
1. Seleccione la casilla Dev/Test (Desarrollo/pruebas).
1. Seleccione **Guardar**.

Cuando se agrega un usuario como propietario de la cuenta mediante Azure EA Portal, las suscripciones de Azure asociadas con el propietario de la cuenta que se basan en la oferta Desarrollo/pruebas de pago por uso, o en las ofertas de crédito mensuales para suscriptores de Visual Studio, se convertirán en la oferta Desarrollo/pruebas - Enterprise. Las suscripciones basadas en otros tipos de oferta, como Pago por uso, asociados al propietario de la cuenta se convertirán en ofertas de Microsoft Azure Enterprise.

Los clientes de Azure Gov no pueden disfrutar de la oferta Desarrollo/pruebas en este momento.

## <a name="transfer-an-enterprise-account-to-a-new-enrollment"></a>Transferencia de una cuenta empresarial a una nueva inscripción

Una transferencia de cuenta mueve un propietario de la cuenta de una inscripción a otra. Todas las suscripciones relacionadas en el propietario de la cuenta se moverán a la inscripción de destino. Use una transferencia de cuentas cuando tenga varias inscripciones activas y solo desee mover los propietarios de la cuenta seleccionada.

Esta sección tiene solo carácter informativo, ya que el administrador de la empresa no puede realizar la acción. Para transferir una cuenta de empresa a una nueva inscripción, se necesita una solicitud de soporte técnico.

Tenga en cuenta los puntos siguientes cuando transfiera una cuenta de empresa a una nueva inscripción:

- Solo se transfieren las cuentas especificadas en la solicitud. Si se eligen todas las cuentas, se transferirán todas ellas.
- La inscripción de origen mantiene su estado como activo o extendido. Puede seguir usando la inscripción hasta que expire.

### <a name="prerequisites"></a>Prerrequisitos

Al solicitar una transferencia de cuentas, proporcione la siguiente información:

- El número de la inscripción de destino, el nombre de cuenta y el correo electrónico del propietario de la cuenta que se va a transferir.
- Para la inscripción de origen, el número de inscripción y la cuenta que se va a transferir

Otros puntos que hay que tener en cuenta antes de transferir una cuenta:

- Es necesaria la aprobación de un administrador del Contrato Enterprise para la inscripción de origen y destino.
- Si la transferencia de una cuenta no cumple los requisitos, considere la posibilidad de realizar una transferencia de inscripción.
- La transferencia de la cuenta transfiere todos los servicios y suscripciones relacionados con las cuentas específicas.
- Una vez completada la transferencia, la cuenta transferida aparece como inactiva en la inscripción de origen y como activa en la inscripción de destino.
- La cuenta muestra la fecha de finalización correspondiente a la fecha de transferencia efectiva en la inscripción de origen y la fecha de inicio en la inscripción de destino.
- Cualquier uso que se haya realizado de la cuenta antes de la fecha de transferencia efectiva permanecerá bajo la inscripción de origen.


## <a name="transfer-enterprise-enrollment-to-a-new-one"></a>Transferencia de la inscripción empresarial a una nueva

Una transferencia de inscripción se tiene en cuenta cuando:

- Ha finalizado el plazo de compromiso de la inscripción actual.
- Una inscripción está en estado expirado o extendido y se negocia un nuevo contrato.
- Tiene varias inscripciones y desea combinar todas las cuentas y la facturación en una sola inscripción.

Esta sección tiene solo carácter informativo, ya que el administrador de la empresa no puede realizar la acción. Para transferir una inscripción de empresa a una nueva, se necesita una solicitud de soporte técnico.

Cuando se solicita la transferencia de una inscripción empresarial completa a una inscripción, se producen las siguientes acciones:

- Todos los servicios, suscripciones, cuentas, departamentos y la estructura de inscripción de Azure completa, incluidos todos los administradores de departamento del Contrato Enterprise, se transfieren a una nueva inscripción de destino.
- El estado de inscripción se establece en _Transferido_. La inscripción transferida solo está disponible con fines de informe de historial de uso.
- No se pueden agregar roles o suscripciones a una inscripción transferida. El estado transferido evita el uso adicional en la inscripción.
- Se pierde cualquier saldo de compromiso monetario restante del contrato, incluidos los términos futuros.
-    Si la inscripción desde la que va a realizar la transferencia tiene compras de instancias reservadas, el precio de compra permanecerá en la inscripción de origen; sin embargo, todas las ventajas de las instancias reservadas se transferirán para su uso en la nueva inscripción.
-    La cuota de compra única de Marketplace y las tarifas fijas mensuales que ya se hayan realizado en la inscripción antigua no se transferirán a la nueva inscripción. Se transferirán los cargos de Marketplace basados en el consumo.

### <a name="effective-transfer-date"></a>Fecha de transferencia efectiva

La fecha de transferencia efectiva puede ser la fecha de inicio de la suscripción de destino o una fecha posterior.

El uso de la inscripción de origen se cobra en el compromiso monetario o como uso por encima del límite. El uso que se produce después de la fecha de transferencia efectiva se transfiere a la nueva inscripción y se cobra en consecuencia.

### <a name="prerequisites"></a>Prerrequisitos

Al solicitar una transferencia de inscripciones, proporcione la siguiente información:

- Para la inscripción de origen, el número de inscripción.
- Para la inscripción de destino, el número de inscripción al que se va a transferir.
- Para la fecha de vigencia de la transferencia de inscripción, puede ser la fecha de inicio de la inscripción de destino o después de ella. La fecha elegida no puede afectar al uso de ninguna factura por uso por encima del límite ya emitida.

Otros puntos que hay que tener en cuenta antes de una transferencia de inscripciones:

- Es necesaria la aprobación de un administrador del Contrato Enterprise para la inscripción de origen y destino.
- Si una transferencia de inscripción no cumple sus requisitos, considere la posibilidad de transferir una cuenta.
- El estado de inscripción de origen se actualizará al estado transferido y solo estará disponible para fines de informes de uso históricos.

### <a name="monetary-commitment"></a>Compromiso monetario

El compromiso monetario no se transfiere entre inscripciones. Los saldos de compromiso monetario se asocian contractualmente a la inscripción en la que se solicitaron. El compromiso monetario no se transfiere como parte del proceso de transferencia de la cuenta o inscripción.

### <a name="no-services-affected-for-account-and-enrollment-transfers"></a>No hay servicios afectados para las transferencias de cuentas e inscripciones.

No hay ningún tiempo de inactividad durante la transferencia de la cuenta o inscripción. Se puede completar el mismo día de la solicitud si se proporciona toda la información necesaria.

## <a name="change-account-owner"></a>Cambiar el propietario de la cuenta

El portal del Contrato Enterprise de Azure puede transferir suscripciones de un propietario de cuenta a otro. Para más información, consulte [Cambio del propietario de la cuenta](ea-portal-get-started.md#change-account-owner).

## <a name="subscription-transfer-effects"></a>Efectos de la transferencia de la suscripción

Si se transfiere una suscripción de Azure a una cuenta en el mismo inquilino de Azure Active Directory, todos los usuarios, grupos y entidades de servicio que tenían [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md) para administrar recursos mantendrán el acceso.

Para ver los usuarios con acceso RBAC a la suscripción:

1. En Azure Portal, abra **Suscripciones**.
2. Seleccione la suscripción que desee ver y, después, seleccione **Control de acceso (IAM)** .
3. Seleccione **Asignaciones de roles**. La página de asignación de roles enumera todos los usuarios que tienen acceso RBAC a la suscripción.

Si transfiere la suscripción a una cuenta en otro inquilino de Azure AD, todos los usuarios, grupos y entidades de servicio que tenían [RBAC](../../role-based-access-control/overview.md) para administrar recursos _perderán_ el acceso. Aunque el acceso con RBAC no está presente, el acceso a la suscripción puede estar disponible mediante mecanismos de seguridad, entre los que se incluyen:

- Certificados de administración que conceden al usuario derechos administrativos a los recursos de la suscripción. Para obtener más información, consulte [Crear y cargar un certificado de administración para Azure](../../cloud-services/cloud-services-certs-create.md).
- Claves de acceso para servicios como Almacenamiento. Para más información, vea [Introducción a las cuentas de Azure Storage](../../storage/common/storage-account-overview.md).
- Credenciales de acceso remoto para servicios como Azure Virtual Machines.

Si el destinatario necesita restringir el acceso a sus recursos de Azure, debería plantearse la posibilidad de actualizar todos los secretos asociados al servicio. La mayoría de los recursos se pueden actualizar mediante el uso de los siguientes pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. En el menú Concentrador, seleccione **Todos los recursos**.
3. Seleccione el recurso.
4. En la página de recursos, seleccione **Settings** (Configuración) para ver y actualizar los secretos existentes.

## <a name="delete-subscription"></a>Eliminar suscripción

Para eliminar una suscripción en la que es el propietario de la cuenta:

1. Inicie sesión en Azure Portal con las credenciales asociadas a su cuenta.
1. En el menú de concentrador, seleccione **Suscripción**.
1. En la pestaña de suscripciones de la esquina superior izquierda de la página, seleccione la suscripción que desea cancelar y, después, **Cancel Sub** (Cancelar suscripción) para iniciar la pestaña de cancelación.
1. Escriba el nombre de la suscripción, elija el motivo de la cancelación y seleccione el botón **Cancel Sub** (Cancelar suscripción).

Solo los administradores de la cuenta pueden cancelar las suscripciones.

Para más información, consulte [¿Qué ocurre después de cancelar la suscripción?](cancel-azure-subscription.md#what-happens-after-i-cancel-my-subscription).

## <a name="delete-an-account"></a>Eliminación de una cuenta

Solo se puede completar la eliminación de cuentas de las cuentas activas sin suscripciones activas.

1. En Enterprise Portal, seleccione **Manage** (Administrar) en el panel de navegación izquierdo.
1. Seleccione la pestaña **Cuenta**.
1. En la tabla de cuentas, seleccione la cuenta que desea eliminar.
1. Seleccione el símbolo X de la derecha de la fila de la cuenta.
1. Una vez que no haya suscripciones activas en la cuenta, seleccione **Yes** (Sí) en la fila de la cuenta para confirmar la eliminación de la cuenta.

## <a name="update-notification-settings"></a>Actualización de la configuración de notificación

Los administradores de empresa se inscriben automáticamente para recibir notificaciones de uso asociadas a la inscripción. Cada administrador de empresa puede cambiar el intervalo de las notificaciones individuales o puede desactivarlas completamente.

Los contactos de notificación se muestran en el portal del Contrato Enterprise de Azure en la sección **Notification Contact** (Contacto para notificaciones). La administración de los contactos de notificaciones garantiza que las personas adecuadas de la organización obtienen notificaciones del Contrato Enterprise de Azure.

Para ver la configuración actual de notificaciones:

1. En el portal del Contrato Enterprise de Azure, vaya a **Manage** > **Notification Contact** (Administrar > Contacto para notificaciones).
2. Dirección de correo electrónico: la dirección de correo electrónico asociada a la cuenta de Microsoft del administrador de empresa, la cuenta profesional o educativa que recibe las notificaciones.
3. Unbilled Balance Notification Frequency (Frecuencia de notificación de saldo sin facturar): frecuencia con la que las notificaciones están configuradas para enviarse a cada administrador de empresa individual.

Para agregar un contacto:

1. Seleccione **+Add Contact** (Agregar contacto).
2. Especifique la dirección de correo electrónico y confírmela.
3. Seleccione **Guardar**.

El nuevo contacto de notificación se muestra en la sección **Notification Contact** (Contacto para notificaciones). Para cambiar la frecuencia de notificación, seleccione el contacto de la notificación y, después, el símbolo del lápiz situado a la derecha de la fila seleccionada. Establezca la frecuencia en **diaria**, **semanal**, **mensual** o **ninguna**.

Puede suprimir _que se aproxima a la fecha de finalización del período de cobertura_ y _que se aproxima a la fecha de deshabilitación y desaprovisionamiento_ de las notificaciones del ciclo de vida. Al deshabilitar las notificaciones del ciclo de vida, se suprimen las notificaciones sobre el período de cobertura y la fecha de finalización del contrato.

## <a name="manage-partner-administrators"></a>Administración de administradores de asociados

Cada administrador de asociados de Azure EA Portal tiene la capacidad de agregar o eliminar otros administradores de asociados. Estos administradores se asocian a las organizaciones de asociados de inscripciones indirectas, no directamente a las inscripciones.

### <a name="add-a-partner-administrator"></a>Adición de un administrador de asociados

Para ver una lista de todas las inscripciones asociadas a la misma organización de asociados que el usuario actual, seleccione la pestaña **Enrollment** (Inscripción) y, después, la casilla de la inscripción que desee.

1. Inicie sesión como administrador de asociados.
1. Seleccione **Administrar** en el panel de navegación izquierdo.
1. Seleccione la pestaña **Partner** (Asociado).
1. Seleccione **+ Add Administrator** (+ Agregar administrador) y rellene la dirección de correo electrónico, el contacto de notificaciones y los detalles de la notificación.
1. Seleccione **Agregar**.

### <a name="remove-a-partner-administrator"></a>Eliminación de un administrador de asociados

Para ver una lista de todas las inscripciones asociadas a la misma organización de asociados que el usuario actual, seleccione la pestaña **Enrollment** (Inscripción) y, después, la casilla de la inscripción que desee.

1. Inicie sesión como administrador de asociados.
1. Seleccione **Administrar** en el panel de navegación izquierdo.
1. Seleccione la pestaña **Partner** (Asociado).
1. En la sección Administrator (Administrador), seleccione la fila correspondiente al administrador que desea eliminar.
1. Seleccione el símbolo X de la derecha.
1. Confirme que desea eliminar.

## <a name="manage-partner-notifications"></a>Administración de notificaciones de asociados

Los administradores de asociados pueden administrar la frecuencia con la que reciben las notificaciones de uso para las inscripciones. Reciben automáticamente notificaciones semanales del saldo no facturado. Pueden cambiar la frecuencia de las notificaciones individuales a mensual, semanal, diaria o desactivarlas completamente.

Si un usuario no recibe una notificación, compruebe que la configuración de notificaciones del usuario es correcta con los pasos siguientes.

1. Inicie sesión en el portal del Contrato Enterprise de Azure como administrador de usuarios.
2. Seleccione **Manage** (Administrar) y, después, seleccione la pestaña **Partner** (Asociado).
3. Consulte la lista de administradores en la sección Administrator (Administrador).
4. Para editar las preferencias de notificación, mantenga el cursor sobre el administrador adecuado y seleccione el símbolo del lápiz.
5. Aumente la frecuencia de notificación y las notificaciones del ciclo de vida tanto como sea necesario.
6. Agregue un contacto si es necesario y seleccione **Add** (Agregar).
7. Seleccione **Guardar**.

![Ejemplo que muestra el cuadro Agregar contacto ](./media/ea-portal-administration/create-ea-manage-partner-notification.png)

## <a name="view-enrollments-for-partner-administrators"></a>Consulta de las inscripciones de los administradores de asociados

Los administradores de asociados pueden ver una lista con todas sus inscripciones directas e indirectas en Azure EA Portal. Se mostrarán cuadros que contienen información general de cada inscripción con el número de inscripción, el nombre de la inscripción, el saldo y los importes de uso por encima del límite.

### <a name="view-a-list-of-enrollments"></a>Consulta de una lista de inscripciones

1. Inicie sesión como administrador de asociados.
1. Seleccione **Manage** (Administrar) en la barra de navegación del lado izquierdo de la página.
1. Seleccione la pestaña **Inscripción**.
1. Seleccione la casilla correspondiente a la inscripción.

En la parte superior de la página permanece una vista de todas las inscripciones con cuadros para cada inscripción. Además, puede desplazarse entre inscripciones; para ello, seleccione el número de la inscripción actual en la barra de navegación en el lado izquierdo de la página. Aparecerá un elemento emergente que le permitirá buscar inscripciones o seleccionar otra inscripción, para lo que debe seleccionar el cuadro correspondiente.

## <a name="azure-sponsorship-offer"></a>Oferta Patrocinio de Azure

La oferta Patrocinio de Azure es una cuenta de Microsoft Azure patrocinada limitada. Está disponible mediante invitación por correo electrónico solo para un número limitado de clientes seleccionados por Microsoft. Si es apto para la oferta Patrocinio de Microsoft Azure, recibirá una invitación por correo electrónico a su identificador de cuenta.

Para más información, cree una [solicitud de soporte técnico para la activación del patrocinio](https://aka.ms/azrsponsorship).

## <a name="conversion-to-work-or-school-account-authentication"></a>Conversión a la autenticación de la cuenta profesional o educativa

Los usuarios de Azure Enterprise pueden convertir desde una cuenta de Microsoft (MSA o Live ID) al tipo de autenticación de una cuenta profesional o educativa (que usa Active Directory en Azure).

Para empezar:

1. Agregue la cuenta profesional o educativa a Azure EA Portal en los roles necesarios.
1. Si recibe errores, es posible que la cuenta no sea válida en Active Directory.  Azure usa el nombre principal de usuario (UPN), que no es siempre idéntico a la dirección de correo electrónico.
1. Inicie sesión en Azure EA Portal con la cuenta profesional o educativa.

### <a name="to-convert-subscriptions-from-microsoft-accounts-to-work-or-school-accounts"></a>Para convertir las suscripciones de las cuentas de Microsoft en cuentas profesionales o educativas:

1. Inicie sesión en el portal de administración con la cuenta de Microsoft que es propietaria de las suscripciones.
1. Use la transferencia de propiedad de la cuenta para pasar a la nueva cuenta.
1. Ahora la cuenta de Microsoft debe estar libre de suscripciones activas y se puede eliminar.
1. Cualquier cuenta eliminada permanecerá en la vista en el portal en un estado inactivo por razones del historial de facturación.  Puede filtrarla de la vista activando una casilla de verificación para mostrar solo las cuentas activas.

## <a name="account-subscription-ownership-faq"></a>Preguntas más frecuentes sobre la propiedad de la suscripción de la cuenta

En este documento se responden las preguntas más frecuentes relacionadas con la propiedad de la suscripción de la cuenta.

### <a name="how-many-azure-account-owners-can-you-have-per-subscription"></a>¿Cuántos propietarios de cuenta de Azure se pueden tener por suscripción?

Solo se permite un propietario de cuenta por suscripción.  Se pueden agregar roles adicionales mediante el acceso basado en rol o (Access Control (IAM)) en la pestaña suscripción en la esquina superior izquierda de la página en [portal.azure.com]](https://portal.azure.com).

### <a name="can-an-azure-account-owner-be-listed-under-more-than-one-department"></a>¿Puede un propietario de cuenta de Azure aparecer en más de un departamento?

No, un propietario de una cuenta solo se puede asociar a un único departamento. La directiva ayuda a garantizar una supervisión precisa y un prorrateo de los costos y el gasto asociados al departamento con el que está alineado en la inscripción de Contrato Enterprise en Azure EA Portal.

### <a name="can-an-azure-account-owner-be-listed-as-a-security-group"></a>¿Puede un propietario de cuenta de Azure aparecer como un grupo de seguridad?

No, el propietario de una suscripción debe ser una cuenta de Microsoft (MSA) única o una autenticación de Azure Active Directory (AAD). Para tener en cuenta la sucesión dentro de la organización, puede considerar la creación de cuentas genéricas y el uso de AAD para administrar el acceso a las suscripciones.

### <a name="can-an-individual-user-own-multiple-subscriptions"></a>¿Puede un usuario individual ser propietario de varias suscripciones?

Un propietario de cuenta de Azure puede crear y administrar un número ilimitado de suscripciones.

### <a name="how-can-i-accessview-all-my-organizations-subscriptions"></a>¿Cómo puedo acceder o ver todas las suscripciones de mi organización?

En la actualidad, esto debe realizarse mediante una directiva; es decir, es necesario requerir que, para cada suscripción creada, su cuenta se agregue a un rol de suscripción mediante el acceso basado en rol.

### <a name="where-do-i-go-to-create-a-subscription"></a>¿Dónde puedo crear una suscripción?

Para poder crear una suscripción de la oferta Enterprise de Azure (EA), el administrador de la inscripción de Contrato Enterprise debe agregar su cuenta al rol del propietario de la cuenta en Azure EA Portal. Tendrá que iniciar sesión en Azure EA Portal para obtener el derecho a crear suscripciones del tipo de oferta Contrato Enterprise. Se recomienda crear la primera suscripción de Contrato Enterprise desde el vínculo "+ Add Subscription" (+ Agregar suscripción) de la pestaña de la suscripción de EA Portal.  Sin embargo, una vez que la cuenta tenga derecho, puede ser más fácil crear suscripciones en portal.azure.com en la pestaña Suscripción en la esquina superior izquierda de la página, donde puede crear y cambiar el nombre de la suscripción en un solo paso.

### <a name="who-can-create-a-subscription"></a>¿Quién puede crear una suscripción?

Para crear una suscripción del tipo de oferta Contrato Enterprise, debe tener derecho en el rol del propietario de cuenta en [EA Portal](https://ea.azure.com).

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información acerca de cómo las [reservas de máquinas virtuales](ea-portal-vm-reservations.md) pueden ayudarle a ahorrar dinero.
- Si necesita ayuda para solucionar problemas con el portal del Contrato Enterprise de Azure, consulte [Solución de problemas de acceso al portal del Contrato Enterprise de Azure](ea-portal-troubleshoot.md).
