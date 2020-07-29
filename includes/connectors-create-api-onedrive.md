---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: 8b5f72f18ee5bf9c1e4dbaaa87d6705f55b09bc5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86524221"
---
## <a name="prerequisites"></a>Prerequisites

* Una cuenta de Azure; puede crear una [gratuita](https://azure.microsoft.com/free)
* Una cuenta de [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3). 

Para poder usar su cuenta de OneDrive en una aplicación lógica, debe autorizar a la aplicación lógica a conectarse a dicha cuenta.  Esto lo puede hacer fácilmente dentro de la aplicación lógica en el Portal de Azure. 

Autorice a la aplicación lógica a conectarse a su cuenta de OneDrive con los pasos siguientes:

1. Cree una aplicación lógica. En el diseñador de Logic Apps, seleccione **Mostrar API administradas por Microsoft** en la lista desplegable y, luego, escriba "onedrive" en el cuadro de búsqueda. Seleccione uno de los desencadenadores o una de las acciones:  
   ![El cuadro de diálogo titulado "Mostrar las API administradas por Microsoft" tiene un cuadro de búsqueda que contiene "onedrive". A continuación se muestra una lista de cuatro desencadenadores. El primero de la lista es "OneDrive: Cuando se crea un archivo", que está seleccionado.](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Si no ha creado previamente las conexiones a OneDrive, se le pedirá que inicie sesión con sus credenciales de OneDrive:  
   ![Un cuadro de diálogo titulado "OneDrive: Cuando se crea un archivo" tiene un botón denominado "Iniciar sesión".](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Seleccione **Iniciar sesión** y escriba su nombre de usuario y contraseña. Seleccione **Iniciar sesión**:  
   ![Un cuadro de diálogo titulado "Iniciar sesión" muestra la instrucción "Usar tu cuenta de Microsoft". Tiene dos cuadros de texto llamados "Correo electrónico o teléfono" y "Contraseña". También tiene una casilla denominada "Mantener la sesión iniciada" y un botón titulado "Iniciar sesión".](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Estas credenciales se usan para autorizar a la aplicación lógica a conectarse y tener acceso a los datos de su cuenta de OneDrive. 
4. Seleccione **Sí** para autorizar a la aplicación lógica a usar la cuenta de OneDrive:  
   ![Un cuadro de diálogo titulado "¿Permites el acceso a tu información?" solicita permiso para hacer las siguientes cuatro cosas: 1) "Iniciar sesión automáticamente", 2) "Obtener acceso a tus direcciones de correo electrónico", 3) "Obtener acceso a tu información en cualquier momento" y 4) "Acceder a archivos de OneDrive". Hay un botón "Sí" para dar permiso y un botón "No" para rechazarlo. Hay un vínculo para cambiar estos permisos de aplicación.](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Observe que la conexión se ha creado en el portal. Ahora, continúe con el resto de los pasos en la aplicación lógica:  
   ![Un cuadro de diálogo titulado "Cuando se crea un archivo" tiene un cuadro de texto denominado "CARPETA" con un botón de búsqueda asociado.](./media/connectors-create-api-onedrive/onedrive-5.png)

