---
title: Habilitación del acceso a un sitio privado para Azure Functions
description: Aprenda a configurar el acceso a un sitio privado de la red virtual de Azure para Azure Functions.
author: mcollier
ms.author: mcollier
ms.service: azure-functions
ms.topic: tutorial
ms.date: 06/17/2020
ms.openlocfilehash: 930b691019d5cb56e487c58e3ca01c4f7ee4ef98
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2020
ms.locfileid: "85261169"
---
# <a name="tutorial-establish-azure-functions-private-site-access"></a>Tutorial: Establecimiento del acceso a un sitio privado de Azure Functions

En este tutorial se muestra cómo habilitar el [acceso a un sitio privado](./functions-networking-options.md#private-site-access) con Azure Functions. Mediante el acceso a un sitio privado, puede requerir que el código de la función solo se desencadene desde una red virtual específica.

El acceso a un sitio privado es útil en escenarios en los que es necesario limitar el acceso a la aplicación de funciones a una red virtual específica. Por ejemplo, la aplicación de funciones puede ser aplicable solo a los empleados de una organización específica o a los servicios que se encuentran en la red virtual especificada (por ejemplo, otra función de Azure, una máquina virtual de Azure o un clúster de AKS).

Si una aplicación de funciones necesita acceder a los recursos de Azure dentro de la red virtual o conectarse mediante [puntos de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md), se necesita la [integración de red virtual](./functions-create-vnet.md).

En este tutorial, aprenderá a configurar el acceso a un sitio privado para la aplicación de funciones:

> [!div class="checklist"]
> * Creación de una máquina virtual
> * Creación de un servicio de Azure Bastion
> * Creación de una aplicación de Azure Functions
> * Configuración de un punto de conexión de servicio de red virtual
> * Creación e implementación de una función de Azure
> * Invocación de la función desde fuera y dentro de la red virtual

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="topology"></a>Topología

El siguiente diagrama muestra la arquitectura de la solución que se va a crear:

![Diagrama de arquitectura de alto nivel de la solución de acceso a un sitio privado](./media/functions-create-private-site-access/topology.png)

## <a name="prerequisites"></a>Requisitos previos

En este tutorial, es importante que comprenda el direccionamiento IP y las subredes. Puede empezar con [este artículo que trata los aspectos básicos del direccionamiento y las subredes](https://support.microsoft.com/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Hay muchos más artículos y vídeos disponibles en línea.

## <a name="sign-in-to-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

El primer paso de este tutorial consiste en crear una nueva máquina virtual dentro de una red virtual.  La máquina virtual se usará para acceder a la función una vez que se haya restringido el acceso para que solo esté disponible desde dentro de la red virtual.

1. Seleccione el botón **Crear un recurso**.

1. En el campo de búsqueda, escriba **Windows Server** y seleccione **Windows Server** en los resultados de la búsqueda.

1. Seleccione **Windows Server 2019 Datacenter** en la lista de opciones de Windows Server y presione el botón **Crear**.

1. En la pestaña _Básico_, utilice la configuración de máquina virtual que se especifica en la tabla debajo de la imagen:

    >[!div class="mx-imgBorder"]
    >![Pestaña Básico de una nueva máquina virtual Windows](./media/functions-create-private-site-access/create-vm-3.png)

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | _Suscripción_ | Su suscripción | Suscripción en la que se han creado los recursos. |
    | [_Grupo de recursos_](../azure-resource-manager/management/overview.md) | myResourceGroup | Elija el grupo de recursos que contendrá todos los recursos de este tutorial.  Usar el mismo grupo de recursos facilita la limpieza de los recursos cuando termine el tutorial. |
    | _Nombre de la máquina virtual_ | myVM | El nombre de la máquina virtual debe ser único en el grupo de recursos |
    | [_Región_](https://azure.microsoft.com/regions/) | (EE. UU.) Centro-norte de EE. UU. | Elija una región cerca de usted o cerca de las funciones a las que se va a acceder. |
    | _Puertos de entrada públicos_ | None | Seleccione **Ninguno** para asegurarse de que no hay conectividad de entrada a la máquina virtual desde Internet. El acceso remoto a la máquina virtual se configurará mediante el servicio Azure Bastion. |

1. Elija la pestaña _Redes_ y seleccione **Crear nueva** para configurar una nueva red virtual.

    >[!div class="mx-imgBorder"]
    >![Creación de una nueva red virtual para la nueva máquina virtual](./media/functions-create-private-site-access/create-vm-networking.png)

1. En _Crear red virtual_, use la configuración de la tabla que aparece debajo de la imagen:

    >[!div class="mx-imgBorder"]
    >![Creación de una nueva red virtual para la nueva máquina virtual](./media/functions-create-private-site-access/create-vm-vnet-1.png)

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | _Nombre_ | myResourceGroup-vnet | Puede usar el nombre predeterminado generado para la red virtual. |
    | _Intervalo de direcciones_ | 10.10.0.0/16 | Use un rango de dirección único para la red virtual. |
    | _Nombre de subred_ | Tutorial | Nombre de la subred. |
    | _Rango de dirección_ (subred) | 10.10.1.0/24 | El tamaño de la subred define la cantidad de interfaces que se pueden agregar a la subred. La máquina virtual usa esta subred. Una subred de /24 proporciona 254 direcciones de host. |

1. Seleccione **Aceptar** para crear la red virtual.
1. De vuelta en la pestaña _Redes_, asegúrese de que se ha seleccionado **Ninguna** para _Dirección IP pública_.
1. Elija la pestaña _Administración_ y, a continuación, en _Cuenta de almacenamiento de diagnóstico_, elija **Crear nueva** para crear una nueva cuenta de almacenamiento.
1. Deje los valores predeterminados para las secciones _Identidad_, _Apagado automático_ y _Copia de seguridad_.
1. Seleccione _Revisar + crear_. Una vez completada la validación, seleccione **Crear**. El proceso de creación de la máquina virtual tarda unos minutos.

## <a name="configure-azure-bastion"></a>Configuración de Azure Bastion

[Azure Bastion](https://azure.microsoft.com/services/azure-bastion/) es un servicio de Azure totalmente administrado que proporciona acceso seguro mediante RDP y SSH a las máquinas virtuales directamente desde Azure Portal. El uso del servicio Azure Bastion elimina la necesidad de establecer la configuración de red relacionada con el acceso mediante RDP.

1. En el portal, elija **Agregar** en la parte superior de la vista de grupo de recursos.
1. En el campo de búsqueda, escriba **Bastion**.
1. Seleccione **Bastion** en los resultados de la búsqueda.
1. Seleccione **Crear** para iniciar el proceso de creación de un nuevo recurso de Azure Bastion. Verá un mensaje de error en la sección _Red virtual_, dado que todavía no existe una subred AzureBastionSubnet. La subred se crea en los pasos siguientes. Utilice la configuración de la tabla debajo de la imagen:

    >[!div class="mx-imgBorder"]
    >![Inicio de la creación del recurso de Azure Bastion](./media/functions-create-private-site-access/create-bastion-basics-1.png)

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | _Nombre_ | myBastion | nombre del nuevo recurso de Bastion. |
    | _Región_ | Centro-Norte de EE. UU | Elija una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que tendrán acceso las funciones. |
    | _Red virtual_ | myResourceGroup-vnet | Red virtual en la que se creará el recurso de Bastion. |
    | _Subred_ | AzureBastionSubnet | subred de la red virtual en la que se implementará el nuevo recurso de host de Bastion. Debe crear una subred con el valor de nombre **AzureBastionSubnet**. Este valor permite a Azure saber en qué subred se deben implementar los recursos de Bastion. Debe usar una subred de al menos **/27** o mayor (/27,/26, etc.). |

    > [!NOTE]
    > Para obtener una guía paso a paso detallada sobre la creación de un recurso de Azure Bastion, consulte el tutorial [Creación de un host de Azure Bastion](../bastion/bastion-create-host-portal.md).

1. Cree una subred en la que Azure pueda aprovisionar el host de Azure Bastion. Al elegir **Administrar la configuración de subred**, se abre un nuevo panel en el que puede definir una nueva subred.  Elija **+ Subred** para crear una nueva subred.
1. La subred debe tener el nombre **AzureBastionSubnet** y el prefijo de subred debe ser al menos **/27**.  Seleccione **Aceptar** para crear la subred.

    >[!div class="mx-imgBorder"]
    >![Creación de una subred para el host de Azure Bastion](./media/functions-create-private-site-access/create-bastion-subnet-2.png)

1. En la página _Crear una instancia de Bastion_, seleccione la que se acaba de crear, **AzureBastionSubnet**, en la lista de subredes disponibles.

    >[!div class="mx-imgBorder"]
    >![Creación de un host de Azure Bastion con una subred específica](./media/functions-create-private-site-access/create-bastion-basics-2.png)

1. Seleccione **Revisar y crear**. Una vez finalice la validación, seleccione **Crear**. Se tardará unos minutos en crear el recurso de Azure Bastion.

## <a name="create-an-azure-functions-app"></a>Creación de una aplicación de Azure Functions

El siguiente paso consiste en crear una aplicación de funciones en Azure con el [Plan de consumo](functions-scale.md#consumption-plan). El código de la función se implementará en este recurso más adelante en el tutorial.

1. En el portal, elija **Agregar** en la parte superior de la vista de grupo de recursos.
1. Seleccione **Proceso > Aplicación de funciones**.
1. En la sección _Básico_, utilice la configuración de la aplicación de funciones que se especifica en la tabla siguiente.

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | _Grupo de recursos_ | myResourceGroup | Elija el grupo de recursos que contendrá todos los recursos de este tutorial.  Usar el mismo grupo de recursos para la aplicación de funciones y la máquina virtual facilita la limpieza de los recursos cuando termine el tutorial. |
    | _Nombre de la aplicación de función_ | Nombre único globalmente | Nombre que identifica la nueva Function App. Los caracteres válidos son a-z (no se distinguen mayúsculas de minúsculas), 0-9 y -. |
    | _Publicar_ | Código | Opción para publicar archivos de código o un contenedor de Docker. |
    | _Pila en tiempo de ejecución_ | Lenguaje preferido | Elija un tiempo de ejecución que admita su lenguaje de programación de funciones preferido. |
    | _Región_ | Centro-Norte de EE. UU | Elija una [región](https://azure.microsoft.com/regions/) cerca de usted o cerca de otros servicios a los que tendrán acceso las funciones. |

    Seleccione el botón **Siguiente: Hospedaje >** .
1. En la sección _Hospedaje_, seleccione los valores adecuados de _Cuenta de almacenamiento_, _Sistema operativo_ y _Plan_, según se describe en la tabla siguiente.

    | Configuración      | Valor sugerido  | Descripción      |
    | ------------ | ---------------- | ---------------- |
    | _Cuenta de almacenamiento_ | Nombre único globalmente | Cree una cuenta de almacenamiento que use la aplicación de función. Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula. También puede usar una cuenta existente que debe cumplir los [requisitos de la cuenta de almacenamiento](./functions-scale.md#storage-account-requirements). |
    | _Sistema operativo_ | Sistema operativo preferido | Se preselecciona un sistema operativo en función de la selección de pila en tiempo de ejecución, pero puede cambiar esta configuración si es necesario. |
    | _Plan_ | Consumo | El [Plan de hospedaje](./functions-scale.md) determina cómo se escala la aplicación de funciones y los recursos disponibles para cada instancia. |
1. Seleccione **Revisar y crear** para revisar las selecciones de configuración de la aplicación.
1. Seleccione **Crear** para aprovisionar e implementar la aplicación de función.

## <a name="configure-access-restrictions"></a>Configuración de las restricciones de acceso

El siguiente paso consiste en configurar [restricciones de acceso](../app-service/app-service-ip-restrictions.md) para asegurarse de que solo los recursos de la red virtual pueden invocar a la función.

El acceso a un [sitio privado](functions-networking-options.md#private-site-access) se habilita mediante la creación de un [punto de conexión de servicio](../virtual-network/virtual-network-service-endpoints-overview.md) de red virtual de Azure entre la aplicación de funciones y la red virtual especificada. Las restricciones de acceso se implementan mediante puntos de conexión de servicio. Los puntos de conexión de servicio garantizan que solo el tráfico que se origina desde dentro de la red virtual especificada puede acceder al recurso designado. En este caso, el recurso designado es la función de Azure.

1. Dentro de la aplicación de funciones, seleccione el vínculo **Redes** en el encabezado de la sección _Configuración_.
1. La página _Redes_ es el punto de partida para configurar Azure Front Door, Azure CDN y las restricciones de acceso.
1. Seleccione **Configurar restricciones de acceso** para configurar el acceso al sitio privado.
1. En la página _Restricciones de acceso_, solo verá la restricción predeterminada. El valor predeterminado no impone restricciones sobre el acceso a la aplicación de funciones.  Seleccione **Agregar regla** para crear una configuración de restricción de acceso al sitio privado.
1. En el panel _Agregar restricción de acceso_, proporcione los valores para _Nombre_, _Prioridad_ y _Descripción_ de la nueva regla.
1. Seleccione **Red virtual** en el cuadro desplegable _Tipo_ y, luego, elija la red virtual creada anteriormente y la subred **Tutorial**. 
    > [!NOTE]
    > La habilitación del punto de conexión de servicio puede tardar varios minutos.
1. La página _Restricciones de acceso_ ahora muestra que hay una nueva restricción. Puede que transcurran algunos segundos antes de que el _estado del punto de conexión_ cambie a Habilitado, tras pasar por Deshabilitado y Aprovisionando.

    >[!IMPORTANT]
    > Cada aplicación de funciones tiene un [sitio de Herramientas avanzadas (Kudu)](../app-service/app-service-ip-restrictions.md#scm-site) que se usa para administrar las implementaciones de la aplicación de funciones. Al sitio se accede mediante una dirección URL como: `<FUNCTION_APP_NAME>.scm.azurewebsites.net`. Habilitar las restricciones de acceso en el sitio de Kudu impide la implementación del código de proyecto desde una estación de trabajo de desarrollador local, y se necesita entonces un agente dentro de la red virtual para realizar la implementación.

## <a name="access-the-functions-app"></a>Acceso a la aplicación de funciones

1. Vuelva a la aplicación de funciones creada anteriormente.  En la sección _Información general_, copie la dirección URL.

    >[!div class="mx-imgBorder"]
    >![Obtención de la dirección URL de la aplicación de funciones](./media/functions-create-private-site-access/access-function-overview.png)

    Si intenta acceder ahora a la aplicación de funciones desde el equipo que está fuera de la red virtual, recibirá una página HTTP 403 que indica que se prohíbe el acceso.
1. Vuelva al grupo de recursos y seleccione la máquina virtual creada anteriormente. Para acceder al sitio desde la máquina virtual, necesitará conectarse a esta mediante el servicio Azure Bastion.
1. Seleccione **Conectar** y, luego, elija **Bastion**.
1. Proporcione el nombre de usuario y la contraseña necesarios para iniciar sesión en la máquina virtual.
1. Seleccione **Conectar**. Aparecerá una nueva ventana del explorador para que pueda interactuar con la máquina virtual.
Dado que esta máquina virtual tiene acceso al sitio mediante una red virtual, es posible acceder al sitio desde el explorador web de la máquina virtual.  Aunque el sitio solo es accesible desde dentro de la red virtual designada, se mantiene una entrada de DNS pública.

## <a name="create-a-function"></a>Creación de una función

El siguiente paso de este tutorial consiste en crear una función de Azure desencadenada por HTTP. La invocación de la función mediante una solicitud HTTP GET o POST debe dar como resultado una respuesta "Hello, {nombre}".  

1. Siga una de estas guías de inicio rápido para crear e implementar la aplicación de Azure Functions.

    * [Visual Studio Code](./functions-create-first-function-vs-code.md)
    * [Visual Studio](./functions-create-your-first-function-visual-studio.md)
    * [Línea de comandos](./functions-create-first-azure-function-azure-cli.md)
    * [Maven (Java)](./functions-create-first-java-maven.md)

1. Al publicar el proyecto de Azure Functions, elija el recurso de la aplicación de funciones que creó anteriormente en este tutorial.
1. Compruebe que la función está implementada.

    >[!div class="mx-imgBorder"]
    >![Función implementada en la lista de funciones](./media/functions-create-private-site-access/verify-deployed-function.png)

## <a name="invoke-the-function-directly"></a>Invocación directa de la función

1. Para probar el acceso a la función, debe copiar la dirección URL de la función. Seleccione la función implementada y, luego, elija **Obtener la dirección URL de la función**. A continuación, haga clic en el botón **Copiar** para copiar la dirección URL en el Portapapeles.

    >[!div class="mx-imgBorder"]
    >![Copia de la dirección URL de la función](./media/functions-create-private-site-access/get-function-url.png)

1. Pegue la dirección URL en un explorador web. Si intenta acceder ahora a la aplicación de funciones desde un equipo que está fuera de la red virtual, recibirá una respuesta HTTP 403 que indica que el acceso a la aplicación está prohibido.

## <a name="invoke-the-function-from-the-virtual-network"></a>Invocación de la función desde la red virtual

El acceso a la función mediante un explorador web (mediante el servicio Azure Bastion) de la máquina virtual configurada en la red virtual da lugar a una respuesta correcta.

>[!div class="mx-imgBorder"]
>![Acceso a la función de Azure mediante Azure Bastion](./media/functions-create-private-site-access/access-function-via-bastion-final.png)

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Pasos siguientes


> [!div class="nextstepaction"]
> [Más información sobre las opciones de redes en Functions](./functions-networking-options.md)
