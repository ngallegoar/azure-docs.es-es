---
title: 'Inicio rápido: Creación de una instancia de Data Science Virtual Machine de CentOS'
titleSuffix: Azure Data Science Virtual Machine
description: Configure y cree una instancia de Data Science Virtual Machine para Linux (CentOS) para realizar análisis y aprendizaje automático.
ms.service: machine-learning
ms.subservice: data-science-vm
author: gvashishtha
ms.author: gopalv
ms.topic: quickstart
ms.date: 09/13/2019
ms.openlocfilehash: 73541b31125ee6e99dc2351e26f6a564a1603487
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "77526032"
---
# <a name="quickstart-set-up-a-centos-linux-data-science-virtual-machine-in-azure"></a>Inicio rápido: Configuración de una instancia de Data Science Virtual Machine de CentOS (Linux) en Azure

Empiece a trabajar con una instancia de Data Science Virtual Machine basada en CentOS.

## <a name="prerequisites"></a>Prerrequisitos

Para crear una instancia de Data Science Virtual Machine de CentOS, debe tener una **suscripción a Azure**. [Cree una suscripción gratuita](https://azure.com/free).

## <a name="create-your-centos-data-science-virtual-machine"></a>Creación de una instancia de Data Science Virtual Machine de CentOS

Estos son los pasos necesarios para crear una instancia de Data Science Virtual Machine de CentOS:

1. Vaya a [Azure Portal](https://portal.azure.com). Es posible que se le pida que inicie sesión en su cuenta de Azure, si todavía no lo ha hecho. 
1. Escriba "data science virtual machine" en la barra de búsqueda y seleccione DSVM (CentOS).

    ![Resultado de la búsqueda de CentOS](./media/linux-dsvm-intro/search-centos.png)

1. En la ventana siguiente, seleccione **Crear**.

    [![](media/linux-dsvm-intro/create-centos.png "Button to create a CentOS machine")](media/linux-dsvm-intro/create-centos-expanded.png#lightbox)

1. Debería ser redirigido a la hoja "Crear una máquina virtual".
   
   ![Pestaña Aspectos básicos correspondiente a la máquina virtual con CentOS](./media/linux-dsvm-intro/review-create-centos.png)

1. Escriba la siguiente información para configurar cada paso del asistente:

    1. **Aspectos básicos**:
    
       * **Suscripción**: si tiene más de una suscripción, seleccione aquella en la que se creará y facturará la máquina. Debe tener privilegios de creación de recursos en esta suscripción.
       * **Grupo de recursos**: cree un grupo o use uno existente.
       * **Nombre de la máquina virtual**: escriba el nombre de la máquina virtual. Así es como aparecerá en Azure Portal.
       * **Región**: seleccione el centro de datos más adecuado. Para disfrutar de un acceso más rápido a la red, elija el centro de datos que tenga la mayoría de los datos o el que esté más cerca de su ubicación física. Más información sobre las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).
       * **Imagen**: Deje el valor predeterminado.
       * **Size**: debería rellenarse automáticamente con un tamaño adecuado para cargas de trabajo generales. Obtenga más información sobre los [tamaños de las máquinas virtuales Linux en Azure](../../virtual-machines/linux/sizes.md).
       * **Tipo de autenticación**: para que la configuración sea más rápida, seleccione "Contraseña". 
         
         > [!NOTE]
         > Si tiene previsto usar JupyterHub, asegúrese de seleccionar "Contraseña", ya que JupyterHub *no* está configurado para usar las claves públicas de SSH.

       * **Nombre de usuario**: escriba el nombre de usuario del administrador. Es el nombre de usuario que usará para iniciar sesión en la máquina virtual y no necesariamente debe ser el mismo que el nombre de usuario de Azure. *No* use mayúsculas.
         
         > [!NOTE]
         > Si usa mayúsculas en el nombre de usuario, JupyterHub no funcionará y se producirá un error interno del servidor 500.

       * **Contraseña**: escriba la contraseña que utilizará para iniciar sesión en la máquina virtual.    
    
   1. Seleccione **Revisar + crear**.
   1. **Revisar y crear**
      * Compruebe que toda la información que ha especificado es correcta. 
      * Seleccione **Crear**.
    
    El aprovisionamiento tardará alrededor de 5 minutos. El estado se muestra en Azure Portal.

## <a name="how-to-access-the-centos-data-science-virtual-machine"></a>Acceso a la instancia de Data Science Virtual Machine de CentOS

Puede acceder a la instancia de Data Science Virtual Machine de CentOS de una de estas tres maneras:

  * SSH para sesiones de terminal
  * X2Go para sesiones de gráficas
  * JupyterHub y JupyterLab para instancias de Jupyter Notebook

También puede conectar una instancia de Data Science Virtual Machine a Azure Notebooks para ejecutar cuadernos de Jupyter en la máquina virtual y omitir las limitaciones del nivel de servicio gratuito. Para más información, consulte [Administración y configuración de proyectos de Azure Notebooks](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier).

### <a name="ssh"></a>SSH

Una vez que se ha creado la máquina virtual, si se configuró con acceso a SSH, se puede iniciar sesión en ella mediante SSH. Utilice las credenciales de la cuenta que haya creado en la sección **Aspectos básicos** del paso 3 para la interfaz de shell de texto. En Windows, puede descargar una herramienta de cliente SSH como [PuTTY](https://www.putty.org). Si prefiere un escritorio gráfico (sistema X Window), puede usar el reenvío de X11 en PuTTY.

> [!NOTE]
> El cliente X2Go ha tenido un mejor rendimiento que el reenvío de X11 durante las pruebas. Por lo tanto, se recomienda usar el cliente X2Go para la interfaz gráfica de escritorio.

### <a name="x2go"></a>X2Go

La máquina virtual de Linux ya está provista del servidor X2Go y está preparada para aceptar conexiones de cliente. Para conectarse al escritorio gráfico de la máquina virtual Linux, lleve a cabo el siguiente procedimiento en el cliente:

1. Descargue e instale el cliente X2Go para su plataforma cliente desde [aquí](https://wiki.x2go.org/doku.php/doc:installation:x2goclient).
1. Anote la dirección IP pública de la máquina virtual, que puede encontrar en Azure Portal abriendo la máquina virtual que acaba de crear.

   ![Dirección IP de la máquina CentOS](./media/linux-dsvm-intro/centos-ip-address.png)

1. Ejecute el cliente X2Go. Si la ventana "Nueva sesión" no aparece automáticamente, vaya a Sesión-> Nueva sesión.

1. En la ventana de configuración resultante, escriba los parámetros de configuración siguientes:
   * **Pestaña Sesión**:
     * **Host**: Escriba la dirección IP de la máquina virtual, la que anotó anteriormente.
     * **Inicio de sesión**: escriba el nombre de usuario de la máquina virtual de Linux.
     * **Puerto SSH**: déjelo en 22, el valor predeterminado.
     * **Tipo de sesión**: cambie el valor a **XFCE**. Actualmente, la máquina virtual de Linux solo admite el escritorio XFCE.
   * **Pestaña Multimedia**: puede desactivar la compatibilidad de sonido y la impresión en el cliente si no necesita usarlas.
   * **Carpetas compartidas**: si quiere que los directorios de las máquinas cliente se monten en la VM Linux, agregue en esta pestaña los directorios de máquina cliente que quiere compartir con la VM.

   ![Configuración de X2Go](./media/dsvm-ubuntu-intro/x2go-ubuntu.png)
1. Seleccione **Aceptar**.
1. Haga clic en el cuadro del panel derecho de la ventana de X2Go para que aparezca la pantalla de inicio de sesión de la máquina virtual.
1. Escriba la contraseña de la máquina virtual.
1. Seleccione **Aceptar**.
1. Para finalizar la conexión, es posible que tenga que conceder a X2Go permiso para omitir el firewall.
1. Ahora debería ver la interfaz gráfica de su la instancia de Data Science Virtual Machine de CentOS. 


### <a name="jupyterhub-and-jupyterlab"></a>JupyterHub y JupyterLab

La instancia de DSVM de CentOS ejecuta [JupyterHub](https://github.com/jupyterhub/jupyterhub), un servidor de Jupyter multiusuario. Para establecer la conexión, siga estos pasos:

   1. Tome nota de la dirección IP pública de la máquina virtual; para ello, busque y seleccione la máquina virtual en Azure Portal.

       ![Dirección IP de la máquina CentOS](./media/linux-dsvm-intro/centos-ip-address.png)

   1. En el equipo local, abra un explorador web y vaya a https:\//ip-de-su-máquina-virtual:8000 y sustituya "ip-de-su-máquina-virtual" por la dirección IP que anotó anteriormente.
   1. Escriba el nombre de usuario y la contraseña que usó para crear la máquina virtual e inicie sesión. 

      ![Escriba el inicio de sesión de Jupyter](./media/dsvm-ubuntu-intro/jupyter-login.png)

   1. Examine los numerosos cuadernos de ejemplo que están disponibles.

JupyterLab, la siguiente generación de los cuadernos de Jupyter y JupyterHub, también está disponible. Para acceder a él, inicie sesión en JupyterHub y, después, vaya a la dirección URL https: \//ip-de-su-máquina-virtual: 8000/user/su-nombre-de-usuario/lab y reemplace "su-nombre-de-usuario" por el nombre de usuario que eligió al configurar la máquina virtual.

Puede establecer JupyterLab como el servidor de bloc de notas predeterminado agregando esta línea a `/etc/jupyterhub/jupyterhub_config.py`:

```python
c.Spawner.default_url = '/lab'
```

## <a name="next-steps"></a>Pasos siguientes

A continuación, mostramos cómo puede continuar con las tareas de aprendizaje y exploración:

* En el tutorial [Ciencia de datos en Data Science Virtual Machine para Linux](linux-dsvm-walkthrough.md) se muestra cómo llevar a cabo varias tareas comunes de ciencia de datos con la instancia de DSVM de Linux aprovisionada aquí. 
* Pruebe las herramientas descritas en este artículo para conocer las distintas herramientas de ciencia de datos de la instancia de DSVM. También puede ejecutar `dsvm-more-info` en el shell de la máquina virtual para obtener un introducción básica y referencias para consultar más información sobre las herramientas instaladas en la instancia de DSVM.  
* Aprenda a crear soluciones analíticas completas mediante el uso sistemático del [proceso de ciencia de datos en equipo](https://aka.ms/tdsp).
* Visite la [Azure AI Gallery](https://gallery.azure.ai/) para ver ejemplos de aprendizaje automático y análisis de datos donde se usan los servicios de Azure AI.
* Consulte la [documentación de referencia](./reference-centos-vm.md) adecuada para esta máquina virtual.