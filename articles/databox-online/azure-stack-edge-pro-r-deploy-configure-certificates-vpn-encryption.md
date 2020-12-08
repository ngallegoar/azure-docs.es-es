---
title: 'Tutorial: Configuración de certificados para un dispositivo Azure Stack Edge Pro R en Azure Portal | Microsoft Docs'
description: Este tutorial para implementar Azure Stack Edge Pro R explica cómo configurar certificados en el dispositivo físico.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/19/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to configure certificates for Azure Stack Edge Pro R so I can use it to transfer data to Azure.
ms.openlocfilehash: fad3e5dcb0ecda82f3fb35cadf1719a62c99bd97
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96464693"
---
# <a name="tutorial-configure-certificates-for-your-azure-stack-edge-pro-r"></a>Tutorial: Configuración de certificados para un dispositivo Azure Stack Edge Pro R

En este tutorial se describe cómo puede configurar certificados para el dispositivo Azure Stack Edge Pro R mediante la interfaz de usuario web local.

El tiempo necesario para este paso puede variar en función de la opción específica que elija y de cómo se establezca el flujo de certificados en su entorno.

En este tutorial, obtendrá información sobre lo siguiente:

> [!div class="checklist"]
>
> * Requisitos previos
> * Configuración de certificados para el dispositivo físico
> * Configuración de la VPN
> * Configuración del cifrado en reposo

## <a name="prerequisites"></a>Requisitos previos

Antes de configurar e instalar el dispositivo Azure Stack Edge Pro R, asegúrese de lo siguiente:

* Ha instalado el dispositivo físico tal y como se describe en [Instalación de Azure Stack Edge Pro R](azure-stack-edge-pro-r-deploy-install.md).
* Si tiene previsto aportar sus propios certificados:
    - Debe tener los certificados preparados en el formato adecuado, incluido el certificado de la cadena de firma. Para obtener más información sobre el certificado, vaya a [Administración de certificados](azure-stack-edge-j-series-manage-certificates.md).



## <a name="configure-certificates-for-device"></a>Configuración de certificados para el dispositivo

1. En la página **Certificados**, configurará los certificados. En función de si cambió el nombre del dispositivo o el dominio DNS en la página **Dispositivo**, puede elegir una de las siguientes opciones para los certificados.

    - Si no ha cambiado el nombre de dispositivo o el dominio DNS en el paso anterior, puede omitir este paso y continuar en el paso siguiente. El dispositivo ha generado automáticamente certificados autofirmados para comenzar. 

    - Si cambió el nombre del dispositivo o el dominio DNS, verá que el estado de los certificados se mostrará como **No válido**. 

        ![Página 2 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/generate-certificate-1.png)    

        Seleccione un certificado para ver los detalles del estado.

        ![Página 3 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/generate-certificate-1a.png)  

        Esto se debe a que los certificados no reflejan el nombre del dispositivo y el dominio DNS actualizados (que se usan en el nombre del firmante y el nombre alternativo del firmante). Para activar correctamente el dispositivo, puede aportar sus propios certificados de punto de conexión firmados y las cadenas de firma correspondientes. Primero debe agregar la cadena de firma y, a continuación, cargar los certificados de punto de conexión. Para más información, vaya a [Aportación de sus propios certificados](#bring-your-own-certificates).

    - Si ha cambiado el nombre de dispositivo o el dominio DNS y no aporta sus propios certificados, **se bloqueará la activación**.

    
#### <a name="bring-your-own-certificates"></a>Aportación de sus propios certificados

Siga estos pasos para agregar sus propios certificados, incluida la cadena de firma.

1. Para cargar el certificado, en la página **Certificado**, seleccione **+ Agregar certificado**.

    ![Página 4 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/add-certificate-1.png)

2. Cargue primero la cadena de firma y seleccione **Validar y agregar**.

    ![Página 5 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/add-certificate-2.png)

3. Ahora puede cargar otros certificados. Por ejemplo, puede cargar los certificados de punto de conexión de Blob Storage y Azure Resource Manager.

    ![Página 6 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/add-certificate-3.png)

    También puede cargar el certificado de la interfaz de usuario web local. Después de cargar este certificado, se le pedirá que inicie el explorador y borre la memoria caché. A continuación, tendrá que conectarse a la interfaz de usuario web local del dispositivo.  

    ![Página 7 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/add-certificate-5.png)

    También puede cargar el certificado de nodo.

    ![Página 8 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/add-certificate-4.png)

    Por último, puede cargar el certificado de VPN.

    ![Página 9 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/add-certificate-4.png)

    En cualquier momento, puede seleccionar un certificado y ver sus detalles para asegurarse de que coinciden con el certificado que ha cargado.

    La página del certificado debe actualizarse para reflejar los certificados recién agregados.

    ![Página 10 de "Certificados" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/add-certificate-7.png)  

    > [!NOTE]
    > A excepción de la nube pública de Azure, es necesario que los certificados de la cadena de firma se agreguen antes de la activación de todas las configuraciones de nube (Azure Government o Azure Stack).

1. Seleccione **< Atrás para comenzar**.

## <a name="configure-vpn"></a>Configuración de la VPN

1. En el icono **Security** (Seguridad), seleccione **Configure** (Configurar) para la VPN.

    ![Página "VPN" de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/configure-vpn-local-ui-1.png)  

    Para configurar una VPN, primero debe asegurarse de haber aplicado la configuración necesaria en Azure. Para más información, consulte [Configuración de los requisitos previos](azure-stack-edge-placeholder.md) y [Configuración de los recursos de Azure para la VPN](azure-stack-edge-placeholder.md). Después de esto, puede realizar la configuración en la interfaz de usuario local.
    
    1. En la página de la VPN, seleccione **Configure** (Configurar).
    2. En la hoja **Configure VPN** (Configurar VPN):

    - Habilite **VPN settings** (Configuración de VPN).
    - Proporcione el valor de **VPN shared secret** (Secreto compartido de VPN). Esta es la clave compartida que proporcionó al crear el objeto de conexión de la VPN de Azure.
    - Proporcione la dirección para **VPN gateway IP** (Dirección IP de VPN Gateway). Esta es la dirección IP de la puerta de enlace de red local de Azure.
    - En **PFS group** (Grupo PFS), seleccione **None** (Ninguno). 
    - En **DH group** (Grupo DH), seleccione **Group2**.
    - Para **IPsec integrity method** (Método de integridad de IPsec), seleccione **SHA256**.
    - Para **IPsec cipher transform constants** (Constantes de transformación de cifrado de IPsec), seleccione **GCMAES256**.
    - Para **IPsec authentication transform constants** (Constantes de transformación de autenticación de IPsec), seleccione **GCMAES256**.
    - Para **IKE encryption method** (Método de cifrado de IKE), seleccione **AES256**.
    - Seleccione **Aplicar**.

        ![Configuración de la interfaz de usuario local 2](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/configure-vpn-local-ui-2.png)

    3. Para cargar el archivo de configuración de la ruta de VPN, seleccione **Upload** (Cargar). 
    
        ![Configuración de la interfaz de usuario local 3](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/configure-vpn-local-ui-3.png)
    
        - Vaya al archivo *json* de configuración de VPN que descargó en el sistema local en el paso anterior.
        - Seleccione la región como la región de Azure asociada con el dispositivo, la red virtual y las puertas de enlace.
        - Seleccione **Aplicar**.
    
            ![Configuración de la interfaz de usuario local 4](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/configure-vpn-local-ui-4.png)
    
    4. Para agregar rutas específicas del cliente, configure los intervalos de direcciones IP a los que se va a tener acceso solo mediante VPN. 
    
        - En **IP address ranges to be accessed using VPN only** (Intervalos de direcciones IP a los que acceder solo mediante VPN), seleccione **Configure** (Configurar).
        - Proporcione un intervalo IPv4 válido y seleccione **Add** (Agregar). Repita los pasos para agregar otros intervalos.
        - Seleccione **Aplicar**.
    
            ![Configuración de la interfaz de usuario local 5](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/configure-vpn-local-ui-5.png)

1. Seleccione **< Atrás para comenzar**.

## <a name="configure-encryption-at-rest"></a>Configuración del cifrado en reposo

1. En el icono **Security** (Seguridad), seleccione **Configure** (Configurar) para el cifrado en reposo. Esta configuración es obligatoria y no podrá activar el dispositivo hasta que la configuración sea correcta. 

    En la fábrica, una vez que se ha creado la imagen de los dispositivos, se habilita el cifrado de BitLocker de nivel de volumen. Después de recibir el dispositivo, tiene que configurar el cifrado en reposo. Se volverán a crear el bloque de almacenamiento y los volúmenes, y es posible proporcionar claves de BitLocker para habilitar el cifrado en reposo y, de este modo, crear una segunda capa de cifrado para los datos en reposo.

1. En el panel **Encryption-at-rest** (Cifrado en reposo), proporcione una clave codificada en Base 64 de 32 caracteres de longitud. Se trata de una configuración que solo se realiza una vez y esta clave se usa para proteger la clave de cifrado real. Puede optar por generar automáticamente esta clave o escribir una.

    ![Panel "Encryption at rest" (Cifrado en reposo) de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/encryption-key-1.png)

    La clave se guarda en un archivo de claves en la página **Cloud details** (Detalles de la nube) después de que el dispositivo esté activado.

1. Seleccione **Aplicar**. Esta operación tarda varios minutos y el estado de la operación se muestra en el icono **Security** (Seguridad).

    ![Página "Encryption at rest" (Cifrado en reposo) de la interfaz de usuario web local](./media/azure-stack-edge-pro-r-deploy-configure-certificates-vpn-encryption/encryption-at-rest-status-1.png)

1. Una vez que el estado aparezca como **Completed** (Completado), seleccione **< Back to Get started** (< Atrás para Comenzar).

El dispositivo ya está listo para activarse. 


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, obtendrá información sobre lo siguiente:

> [!div class="checklist"]
>
> * Requisitos previos
> * Configuración de certificados para el dispositivo físico
> * Configuración de la VPN
> * Configuración del cifrado en reposo

Para obtener información sobre cómo activar el dispositivo Azure Stack Edge Pro R, consulte:

> [!div class="nextstepaction"]
> [Activación del dispositivo Azure Stack Edge Pro R](./azure-stack-edge-pro-r-deploy-activate.md)
