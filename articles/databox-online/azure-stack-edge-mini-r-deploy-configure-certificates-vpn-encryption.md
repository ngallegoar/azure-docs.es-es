---
title: 'Tutorial: Configuración de certificados para un dispositivo Azure Stack Edge Mini R en Azure Portal | Microsoft Docs'
description: Este tutorial para implementar Azure Stack Edge Mini R explica cómo configurar certificados en el dispositivo físico.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/21/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure certificates for Azure Stack Edge Mini R  so I can use it to transfer data to Azure.
ms.openlocfilehash: 1e81ab61b04f9cca6aff57de1736eac25a871c97
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96465072"
---
# <a name="tutorial-configure-certificates-vpn-encryption-for-your-azure-stack-edge-mini-r"></a>Tutorial: Configuración de certificados, la VPN y el cifrado para Azure Stack Edge Mini R

Este tutorial describe cómo configurar los certificados, la VPN y el cifrado en reposo para su dispositivo Azure Stack Edge Mini R mediante la interfaz de usuario web local.

El tiempo necesario para este paso puede variar en función de la opción específica que elija y de cómo se establezca el flujo de certificados en su entorno.

En este tutorial, obtendrá información sobre lo siguiente:

> [!div class="checklist"]
>
> * Requisitos previos
> * Configuración de certificados para el dispositivo físico
> * Configuración de la VPN
> * Configuración del cifrado en reposo

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar e instalar el dispositivo Azure Stack Edge Mini R, asegúrese de lo siguiente:

* Ha instalado el dispositivo físico tal y como se describe en [Instalación de Azure Stack Edge Mini R](azure-stack-edge-mini-r-deploy-install.md).

* Si tiene previsto aportar sus propios certificados:
    - Debe tener los certificados preparados en el formato adecuado, incluido el certificado de la cadena de firma. Para obtener más información sobre el certificado, vaya a [Administración de certificados](azure-stack-edge-j-series-manage-certificates.md).

    - Si el dispositivo se implementa en la nube de Azure Government, Azure Government Secret o Azure Government Top Secret y no se implementa en la nube pública de Azure, se requerirá un certificado de cadena de firma para poder activar el dispositivo. 
    Para más detalles sobre el certificado, consulte [Administración de certificados](azure-stack-edge-j-series-manage-certificates.md).


## <a name="configure-certificates-for-device"></a>Configuración de certificados para el dispositivo

1. En la página **Certificados**, configurará los certificados. En función de si cambió el nombre del dispositivo o el dominio DNS en la página **Dispositivo**, puede elegir una de las siguientes opciones para los certificados.

    - Si no ha cambiado el nombre del dispositivo o del dominio DNS predeterminados en el paso anterior y no desea aportar sus propios certificados, puede omitir este paso y continuar con el paso siguiente. El dispositivo ha generado automáticamente certificados autofirmados para comenzar. 

       <!-- ![Local web UI "Certificates" page](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/certificate-1.png)-->

    - Si cambió el nombre del dispositivo o el dominio DNS, verá que el estado de los certificados se mostrará como **No válido**. 

        ![Página 2 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/certificate-2.png)    

        Seleccione un certificado para ver los detalles del estado.

        <!--![Local web UI "Certificates" page 3](./media/azure-stack-edge-gpu-deploy-configure-certificates/generate-certificate-1a.png)-->  

        El estado del certificado es **No válido** porque los certificados no reflejan el nombre del dispositivo ni el dominio DNS actualizados (que se usan con el nombre del firmante y el nombre alternativo del firmante). Para activar correctamente el dispositivo, puede aportar sus propios certificados de punto de conexión firmados y las cadenas de firma correspondientes. Primero debe agregar la cadena de firma y, a continuación, cargar los certificados de punto de conexión. Para más información, consulte [Aportación de certificados propios en el dispositivo Azure Stack Edge Mini R](#bring-your-own-certificates).


    - Si ha cambiado el nombre del dispositivo o el dominio DNS y no aporta sus propios certificados, **se bloqueará la activación**.


#### <a name="bring-your-own-certificates"></a>Aportación de sus propios certificados

Ya ha agregado la cadena de firma en un paso anterior en este dispositivo. Ahora puede cargar los certificados de punto de conexión, el certificado de nodo, el certificado de interfaz de usuario local y el certificado de VPN. Siga estos pasos para agregar sus propios certificados.

1. Para cargar el certificado, en la página **Certificado**, seleccione **+ Agregar certificado**.

    [![Página 4 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-1.png)](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-1.png#lightbox)


1. Puede cargar otros certificados. Por ejemplo, puede cargar los certificados de punto de conexión de Blob Storage y Azure Resource Manager.

    ![Página 6 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-3.png)

1. También puede cargar el certificado de la interfaz de usuario web local. Después de cargar este certificado, se le pedirá que inicie el explorador y borre la memoria caché. A continuación, tendrá que conectarse a la interfaz de usuario web local del dispositivo.  

    ![Página 7 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-4.png)

1. También puede cargar el certificado de nodo.


    ![Página 8 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-5.png)

1. Por último, puede cargar el certificado de VPN.
        
    ![Página "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-6.png)

1. En cualquier momento, puede seleccionar un certificado y ver sus detalles para asegurarse de que coinciden con el certificado que ha cargado.

    [![Página 9 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-7.png)](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-7.png#lightbox)

    La página del certificado debe actualizarse para reflejar los certificados recién agregados.

    [![Página 10 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-8.png)](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/add-certificate-8.png#lightbox)  

    > [!NOTE]
    > Salvo por la nube pública de Azure, es necesario agregar los certificados de la cadena de firma antes de activar todas las configuraciones de nube (Azure Government o Azure Stack Hub).


## <a name="configure-vpn"></a>Configuración de la VPN

1. En el icono **Security** (Seguridad), seleccione **Configure** (Configurar) para la VPN. 

    Para configurar una VPN, primero debe asegurarse de haber aplicado la configuración necesaria en Azure. Para más detalles, consulte [Configuración de la VPN a través de PowerShell para el dispositivo Azure Stack Edge Mini R](azure-stack-edge-placeholder.md). Después de esto, puede realizar la configuración en la interfaz de usuario local.
    
    1. En la página de la VPN, seleccione **Configure** (Configurar).
        ![Configuración de la interfaz de usuario local de VPN 1](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/configure-vpn-1.png)

    1. En la hoja **Configure VPN** (Configurar VPN):

        1. Aporte la libreta de teléfonos como entrada.
        2. Proporcione el archivo JSON de intervalo IP de centro de datos de Azure como entrada. Descargue el archivo de: [https://www.microsoft.com/download/details.aspx?id=56519](https://www.microsoft.com/download/details.aspx?id=56519).
        3. Seleccione **eastus** como la región.
        4. Seleccione **Aplicar**.

        ![Configuración de la interfaz de usuario local de VPN 2](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/configure-vpn-2.png)
    
    1. Configure los intervalos de direcciones IP a los que se accederá únicamente mediante VPN. 
    
        - En **IP address ranges to be accessed using VPN only** (Intervalos de direcciones IP a los que acceder solo mediante VPN), seleccione **Configure** (Configurar).
        - Escriba el intervalo IPv4 VNET que había elegido para Azure Virtual Network.
        - Seleccione **Aplicar**.
    
        ![Configuración de la interfaz de usuario local de VPN 3](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/configure-vpn-3.png)

El dispositivo está listo ahora para activarse. Configure el cifrado en reposo.


## <a name="enable-encryption"></a>Habilitación del cifrado

1. En el icono **Security** (Seguridad), seleccione **Configure** (Configurar) para el cifrado en reposo. Esta configuración es obligatoria, y el usuario no podrá activar el dispositivo hasta que la configuración sea correcta. 

    ![Página 1 de "Cifrado en reposo" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/encryption-at-rest-1.png)

    En la fábrica, una vez que se ha creado la imagen de los dispositivos, se habilita el cifrado de BitLocker de nivel de volumen. Después de recibir el dispositivo, tiene que configurar el cifrado en reposo. Se volverán a crear el bloque de almacenamiento y los volúmenes, y es posible proporcionar claves de BitLocker para habilitar el cifrado en reposo y, de este modo, crear una segunda capa de cifrado para los datos en reposo.

1. En el panel de **cifrado en reposo**, escriba una clave codificada con Base 64, de 32 caracteres de longitud (AES de 256 bits). Se trata de una configuración que solo se realiza una vez, y esta clave se usa para proteger la clave de cifrado real. 

    ![Página 2 de "Cifrado en reposo" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/encryption-at-rest-2.png)

    También puede optar por generar automáticamente la clave.

    ![Página 3 de "Cifrado en reposo" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/encryption-at-rest-3.png)

1. Seleccione **Aplicar**. Esta operación tarda varios minutos y el estado de la operación se muestra en el icono **Security** (Seguridad).

    ![Página 4 de "Cifrado en reposo" de la interfaz de usuario web local](./media/azure-stack-edge-mini-r-deploy-configure-certificates-vpn-encryption/encryption-at-rest-3.png)

1. Una vez que el estado aparezca como **completado**, vuelva a **Introducción**.

El dispositivo ya está listo para activarse.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, obtendrá información sobre lo siguiente:

> [!div class="checklist"]
>
> * Requisitos previos
> * Configuración de certificados para el dispositivo físico
> * Configuración de la VPN
> * Configuración del cifrado en reposo

Para aprender a activar el dispositivo Azure Stack Edge Mini R, consulte:

> [!div class="nextstepaction"]
> [Activación del dispositivo Azure Stack Edge Mini R](./azure-stack-edge-mini-r-deploy-activate.md)
