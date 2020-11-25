---
title: 'Creación y asociación de directivas de punto de conexión de servicio: Azure Portal'
titlesuffix: Azure Virtual Network
description: En este artículo, aprenderá cómo configurar y asociar directivas de punto de conexión de servicio mediante Azure Portal.
services: virtual-network
documentationcenter: virtual-network
author: RDhillon
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 02/21/2020
ms.author: rdhillon
ms.openlocfilehash: bdf0e87c92a55d0dbb5bbe34334a6de4580cb350
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96004966"
---
# <a name="create-change-or-delete-service-endpoint-policy-using-the-azure-portal"></a>Creación, cambio o eliminación de la directiva de punto de conexión de servicio mediante Azure Portal

Las directivas de puntos de conexión de servicio le permiten filtrar el tráfico de red virtual a recursos específicos de Azure, sobre puntos de conexión de servicio. Si no está familiarizado con estas directivas, consulte [Directivas de punto de conexión de servicio](virtual-network-service-endpoint-policies-overview.md) para más información.

 En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una directiva de punto de conexión de servicio
> * Definición de Crear una directiva de punto de conexión de servicio
> * Creación de una red virtual con una subred
> * Asociación una directiva de punto de conexión de servicio a una subred

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure 

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-service-endpoint-policy"></a>Creación de una directiva de punto de conexión de servicio

1. Seleccione **+ Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. En el panel de búsqueda, escriba "directiva de punto de conexión de servicio" y seleccione **Directiva de punto de conexión de servicio** y, a continuación, seleccione **Crear**.

![Creación de una directiva de punto de conexión de servicio](./media/virtual-network-service-endpoint-policies-portal/create-sep-resource.png)

3. Escriba o seleccione la siguiente información en **Básico**. 

   - Suscripción: Seleccione la suscripción por directiva.
   - Grupo de recursos: Seleccione **Crear nuevo** y escriba *myResourceGroup*
   - Nombre: myEndpointPolicy
   - Ubicación: Centro de EE. UU.
 
   ![Conceptos básicos de Crear una directiva de punto de conexión de servicio](./media/virtual-network-service-endpoint-policies-portal/create-sep-basics.png)

4. Seleccione **+ Agregar** en **Recursos** y escriba o seleccione la siguiente información en el panel **Agregar un recurso**.

   - Servicio: Solo **Microsoft. Storage** está disponible con las directivas de punto de conexión de servicio
   - Ámbito: Seleccione una de las opciones **Cuenta única** , **Todas las cuentas de la suscripción**  y **Todas las cuentas del grupo de recursos** .
   - Suscripción: seleccione la suscripción para la cuenta de almacenamiento. Las cuentas de almacenamiento y las directivas pueden estar en distintas suscripciones.
   - Grupo de recursos: Seleccione el grupo de recursos que necesite. Requerido, si el ámbito está establecido como "Todas las cuentas del grupo de recursos" o "Cuenta única".  
   - Recurso: Seleccione el recurso de Azure Storage en la suscripción o el grupo de recursos seleccionado
   - Haga clic en el botón **Agregar** en la parte inferior para terminar de agregar el recurso.

   ![Definición de la directiva de punto de conexión de servicio: recurso](./media/virtual-network-service-endpoint-policies-portal/create-sep-add-resource.png)

   - Agregue más recursos repitiendo los pasos anteriores según sea necesario.

5. Opcional: escriba o seleccione la siguiente información en **Etiquetas**:
   
   - Clave: seleccione la clave de la directiva. Por ejemplo: Dept     
   - Valor: escriba el par de valor de la clave. Por ejemplo: Finance

6. Seleccione **Revisar + crear**. Valide la información y haga clic en **Crear**. Para hacer más modificaciones, haga clic en **Anterior**. 

   ![Validaciones finales de Crear una directiva de punto de conexión de servicio](./media/virtual-network-service-endpoint-policies-portal/create-sep-review-create.png)
  
## <a name="view-endpoint-policies"></a>Visualización de directivas de punto de conexión de servicio 

1. En el cuadro *Todos los servicios* del portal, comience a escribir *directivas de punto de conexión de servicio*. Seleccione **Directivas de punto de conexión de servicio**.
2. En **Suscripciones**, seleccione la suscripción y grupo de recursos, tal como se muestra en la siguiente imagen.

   ![Muestra de directiva](./media/virtual-network-service-endpoint-policies-portal/sep-view.png)
       
3. Seleccione la directiva y haga clic en **Definiciones de directiva** para ver o agregar más definiciones.

   ![Muestra de definiciones de directivas](./media/virtual-network-service-endpoint-policies-portal/sep-policy-definition.png)

4. Seleccione **Subredes asociadas** para ver las subredes asociadas a la directiva. Si todavía no hay ninguna subred asociada, siga las instrucciones del paso siguiente.

   ![Subredes asociadas](./media/virtual-network-service-endpoint-policies-portal/sep-associated-subnets.png)
 
5. Asociación de una directiva a una subred

>[!WARNING] 
> Asegúrese de que todos los recursos a los que se accede desde la subred se agregan a la directiva antes de asociarla a la subred especificada. Una vez que la directiva está asociada, solo se permitirá el acceso a los recursos de la *lista de permitidos*  en los puntos de conexión de servicio. 
>
> Asegúrese también de que no existe ningún servicio de Azure administrado en la subred que se está asociando a la directiva de punto de conexión de servicio.

- Para poder asociar una directiva a una subred, debe crear una red virtual y una subred. Consulte el artículo sobre la [creación de una red virtual](./quick-create-portal.md) para obtener ayuda.

- Cuando haya configurado la red virtual y la subred, debe configurar los puntos de conexión de servicio de red virtual para Azure Storage. En la hoja Red virtual, seleccione **Puntos de conexión de servicio** y, en el panel siguiente, seleccione **Microsoft.Storage** y en **Subredes**, seleccione la red virtual o la subred deseada.

- Ahora, puede seleccionar la directiva de punto de conexión de servicio en la lista desplegable del panel anterior, si ya ha creado directivas de punto de conexión de servicio antes de configurar el punto de conexión de servicio para la subred, tal como se muestra a continuación.

    ![Asociación de la subred al crear el punto de conexión de servicio](./media/virtual-network-service-endpoint-policies-portal/vnet-config-service-endpoint-add-sep.png)

- O bien, si va a asociar las directivas de punto de conexión de servicio después de configurar los puntos de conexión de servicio, puede elegir asociar la subred desde la hoja Directiva de punto de conexión de servicio. Para ello, vaya al panel **Subredes asociadas** como se muestra a continuación.

    ![Asociación de la subred mediante SEP](./media/virtual-network-service-endpoint-policies-portal/sep-edit-subnet-association.png)

>[!WARNING] 
>El acceso a los recursos de Azure Storage en todas las regiones se restringe según la directiva de punto de conexión de servicio de esta subred.

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha creado una directiva de punto de conexión de servicio y la ha asociado a una subred. Para saber más de las directivas de punto de conexión de servicio, consulte [Directivas de punto de conexión de servicio de redes virtuales (versión preliminar).](virtual-network-service-endpoint-policies-overview.md)
