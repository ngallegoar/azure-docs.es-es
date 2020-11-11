---
title: Transferencia de la propiedad de facturación de una suscripción de Azure
description: Se describe cómo transferir la propiedad de facturación de una suscripción de Azure a otra cuenta.
keywords: transferencia de la suscripción de Azure, suscripción de transferencia de Azure, traslado de la suscripción de Azure a otra cuenta, cambio de propietario de la suscripción de Azure, transferencia de la suscripción de Azure a otra cuenta, facturación de transferencia de Azure
author: bandersmsft
ms.reviewer: amberb
tags: billing,top-support-issue
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 10/05/2020
ms.author: banders
ms.custom: contperfq1
ms.openlocfilehash: 1ce461f50a6775bf7884fedc4cfa4dc7cb41bb8c
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94408151"
---
# <a name="transfer-billing-ownership-of-an-azure-subscription-to-another-account"></a>Transferencia de la propiedad de facturación de una suscripción de Azure a otra cuenta

En este artículo se muestran los pasos necesarios para transferir la propiedad de facturación de una suscripción de Azure a otra cuenta. Antes de transferir la propiedad de facturación de una suscripción, lea [Acerca de cómo transferir la propiedad de facturación de una suscripción de Azure](../understand/subscription-transfer.md).

Si quiere mantener la propiedad de facturación, pero cambiar el tipo de suscripción, consulte [Cambio de la suscripción de Azure a otra oferta](switch-azure-offer.md). Para controlar quién puede acceder a los recursos de la suscripción, consulte [Roles integrados en Azure](../../role-based-access-control/built-in-roles.md).

Si es un cliente de Contrato Enterprise (EA), el administrador de la empresa puede transferir la propiedad de facturación de las suscripciones entre cuentas. Para más información, consulte [Cambio del propietario de la cuenta](ea-portal-administration.md#change-account-owner).

Solo el administrador de facturación de una cuenta puede transferir la propiedad de una suscripción.

## <a name="transfer-billing-ownership-of-an-azure-subscription"></a>Transferencia de la propiedad de facturación de una suscripción de Azure

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta de facturación que tiene la suscripción que quiere transferir. Si no está seguro de si es administrador, o si necesita determinar quién es, consulte [Determinación del administrador de facturación de la cuenta](../understand/subscription-transfer.md#whoisaa).
1. Busque **Administración de costos + facturación**.  
   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-subscription-transfer/billing-search-cost-management-billing.png)
1. Seleccione **Suscripciones** en el panel izquierdo. Según el acceso, es posible que deba seleccionar un ámbito de facturación y, a continuación, seleccione **Suscripciones** o **Suscripciones de Azure**.
1. Seleccione **Transferir propiedad de la facturación** para la suscripción que quiera transferir.  
   ![Seleccione la suscripción que va a transferir.](./media/billing-subscription-transfer/billing-select-subscription-to-transfer.png)
1. Escriba la dirección de correo electrónico de un usuario que sea administrador de facturación de la cuenta que será el nuevo propietario de la suscripción.
1. Si va a transferir la suscripción a la cuenta de otro inquilino de Azure AD, seleccione si desea trasladar la suscripción al inquilino de la nueva cuenta. Para más información, consulte [Transferencia de una suscripción a una cuenta en otro inquilino de Azure AD](#transfer-a-subscription-to-another-azure-ad-tenant-account).
    > [!IMPORTANT]
    > Si opta por mover la suscripción al inquilino de Azure AD de la nueva cuenta, todas las [asignaciones de roles de Azure](../../role-based-access-control/role-assignments-portal.md) para acceder a los recursos de la suscripción se quitan de forma permanente. Solo el usuario de la nueva cuenta que acepte la solicitud de transferencia tendrá acceso para administrar los recursos de la suscripción. Como alternativa, puede desactivar la opción **Inquilino de la suscripción de Azure AD** para transferir la propiedad de facturación sin mover la suscripción al inquilino de la nueva cuenta. Si lo hace, se mantendrán las asignaciones de roles de Azure existentes para acceder a los recursos de Azure.  
    ![Envío de la página de transferencia](./media/billing-subscription-transfer/billing-send-transfer-request.png)
1. Seleccione **Enviar solicitud de transferencia**.
1. El usuario recibe un correo electrónico con instrucciones para revisar la solicitud de transferencia.  
   ![Correo electrónico de transferencia de suscripción enviado al destinatario](./media/billing-subscription-transfer/billing-receiver-email.png)
1. Para aprobar la solicitud de transferencia, el usuario selecciona el vínculo en el correo electrónico y sigue las instrucciones. El usuario selecciona entonces un método de pago que se usará para pagar la suscripción. Si el usuario no tiene una cuenta de Azure, tendrá que registrarse para obtener una cuenta nueva.  
   ![Primera página web de transferencia de suscripción](./media/billing-subscription-transfer/billing-accept-ownership-step1.png)
   ![Segunda página web de transferencia de suscripción](./media/billing-subscription-transfer/billing-accept-ownership-step2.png)
   ![Tercera página web de transferencia de suscripción](./media/billing-subscription-transfer/billing-accept-ownership-step3.png)
1. ¡Hecho! La suscripción ya está transferida.

## <a name="transfer-a-subscription-to-another-azure-ad-tenant-account"></a>Transferencia de una suscripción a otra cuenta de inquilino de Azure AD

Cuando se registra en Azure, se crea un inquilino de Azure Active Directory (AD) para usted. El inquilino representa su cuenta. Use el inquilino para administrar el acceso a sus suscripciones y recursos.

Cuando se crea una suscripción nueva, se hospeda en el inquilino de Azure AD de la cuenta. Si quiere dar a otros usuarios acceso a la suscripción o a sus recursos, deberá invitarlos a unirse al inquilino. De esta forma, podrá controlar el acceso a sus suscripciones y recursos.

Cuando transfiere la propiedad de facturación de la suscripción a una cuenta en otro inquilino de Azure AD, puede mover la suscripción al inquilino de la nueva cuenta. En ese caso, todos los usuarios, grupos o entidades de servicio que tenían [asignaciones de roles de Azure](../../role-based-access-control/role-assignments-portal.md) para administrar suscripciones y sus recursos perderán el acceso. Solo el usuario de la cuenta nueva que acepte la solicitud de transferencia tendrá acceso para administrar los recursos. El nuevo propietario debe agregar manualmente estos usuarios a la suscripción para proporcionar acceso al usuario que la perdió. Para más información, consulte [Transferencia de una suscripción de Azure a otro directorio de Azure AD](../../role-based-access-control/transfer-subscription.md).

## <a name="transfer-visual-studio-and-partner-network-subscriptions"></a>Transferencia de suscripciones de Visual Studio y de Partner Network

Las suscripciones de Visual Studio y Microsoft Partner Network tienen crédito de Azure periódico mensual asociado. Al transferir estas suscripciones, el crédito no está disponible en la cuenta de facturación de destino. La suscripción usa el crédito de la cuenta de facturación de destino. Por ejemplo, si Roberto transfiere una suscripción de Visual Studio Enterprise a la cuenta de Julia el 9 de septiembre y Julia acepta a la transferencia. Una vez completada la transferencia, la suscripción empieza a usar el crédito de la cuenta de Julia. El crédito se restablecerá cada noveno día del mes.

## <a name="next-steps-after-accepting-billing-ownership"></a>Pasos siguientes después de aceptar la propiedad de facturación

Si ha aceptado la propiedad de facturación de una suscripción de Azure, se recomienda que revise los pasos siguientes:

1. Revise y actualice el administrador del servicio, los coadministradores y las asignación de roles de Azure. Para más información, consulte [Adición o cambio de los administradores de la suscripción de Azure](add-change-subscription-administrator.md) y [Adición o eliminación de asignaciones de roles de Azure mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md).
1. Actualice las credenciales asociadas a los servicios de esta suscripción:
   1. Certificados de administración que conceden al usuario derechos administrativos a los recursos de la suscripción. Para obtener más información, consulte [Crear y cargar un certificado de administración para Azure](../../cloud-services/cloud-services-certs-create.md)
   1. Claves de acceso para servicios como Almacenamiento. Para más información, consulte [Acerca de las cuentas de almacenamiento de Azure](../../storage/common/storage-account-create.md).
   1. Credenciales de acceso remoto para servicios como Azure Virtual Machines.
1. Si trabaja con un asociado, considere la posibilidad de actualizar el id. de asociado en esta suscripción. Puede actualizar el identificador de asociado en [Azure Portal](https://portal.azure.com). Para más información, consulte [Vinculación de un Id. de partner a cuentas de Azure](link-partner-id.md).

## <a name="troubleshooting"></a>Solución de problemas

Use la siguiente información de solución de problemas si tiene problemas para transferir suscripciones.

### <a name="the-transfer-subscription-option-is-unavailable"></a>La opción "Transferir suscripción" no está disponible

<a name="no-button"></a> 

La transferencia de suscripción de autoservicio no está disponible para su cuenta de facturación. Actualmente, no se admite la transferencia de la propiedad de facturación de suscripciones en cuentas de Contrato Enterprise (EA) en Azure Portal. Además, las cuentas de Contrato de cliente de Microsoft que se crean mientras trabaja con un representante de Microsoft no admiten la transferencia de la propiedad de facturación.

###  <a name="not-all-subscription-types-can-transfer"></a>No todos los tipos de suscripción se pueden transferir

No todos los tipos de suscripciones admiten la transferencia de la propiedad de facturación. Para ver la lista de tipos de suscripción que admiten transferencias, consulte [Tipos de suscripción admitidos](../understand/subscription-transfer.md#supported-subscription-types).

###  <a name="access-denied-error-shown-when-trying-to-transfer-subscription-billing-ownership"></a>Se muestra un error de acceso denegado al intentar transferir la propiedad de facturación de suscripción

Verá este error si intenta transferir una suscripción de un plan de Microsoft Azure y no tiene el permiso necesario. Para transferir una suscripción de un plan de Microsoft Azure, debe ser propietario o colaborador en la sección de la factura en la que se factura la suscripción. Para más información, consulte [Administrar las suscripciones para la sección de factura](../manage/understand-mca-roles.md#manage-subscriptions-for-invoice-section).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- Revise y actualice el administrador del servicio, los coadministradores y las asignación de roles de Azure. Para más información, consulte [Adición o cambio de los administradores de la suscripción de Azure](add-change-subscription-administrator.md) y [Adición o eliminación de asignaciones de roles de Azure mediante Azure Portal](../../role-based-access-control/role-assignments-portal.md).