---
title: 'Inicio rápido: Configuración de una máquina virtual Windows en Azure con Chef'
description: En este inicio rápido aprenderá a usar Chef para implementar y configurar una máquina virtual Windows en Azure
keywords: chef, azure, devops, virtual machine
ms.topic: tutorial
ms.service: chef
author: tomarchermsft
ms.author: tarcher
ms.date: 02/22/2020
ms.openlocfilehash: 824e4df7662ee67c3f0786877053c39a8d952d49
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "77589499"
---
# <a name="quickstart---configure-a-windows-virtual-machine-in-azure-using-chef"></a>Inicio rápido: Configuración de una máquina virtual Windows en Azure con Chef

Chef permite ofrecer automatización y las configuraciones de estado que desee.

Con la versión de API de nube más reciente, Chef proporciona una perfecta integración con Azure, lo que ofrece la posibilidad de aprovisionar e implementar los estados de configuración mediante un único comando.

En este artículo configuraremos un entorno de Chef para aprovisionar las máquinas virtuales de Azure y le guiaremos con la creación de una directiva o guía, que después implementaremos en una máquina virtual de Azure.

## <a name="chef-basics"></a>Conceptos básicos de Chef

Antes de comenzar con este artículo, [revise los conceptos básicos de Chef](https://www.chef.io/chef).

El diagrama siguiente muestra la arquitectura de Chef de alto nivel.

![Arquitectura de Chef](media/chef-automation/chef-architecure.png)

La arquitectura de Chef tiene tres componentes principales: 
- Servidor de Chef: punto de administración con dos opciones: una solución hospedada o una solución local.
- Cliente de Chef (nodo): agente que se encuentra en los servidores usted administra.
- Estación de trabajo de Chef: nombre tanto de la estación de trabajo de administración en la que se crean directivas y se ejecutan comandos de administración como del paquete de software de las herramientas de Chef.

Por lo general, verá **su estación de trabajo** como la ubicación en la que se ejecutan comandos y la **estación de trabajo de Chef** para el paquete de software.

Por ejemplo, el comando de knife se descarga como parte de la **estación de trabajo de Chef**, pero los comandos de knife que administran la infraestructura se ejecutan desde **su estación de trabajo**.

Chef también usa los conceptos de *guías* y *recetas*. Estos términos son las directivas que se definen y se aplican a los servidores, respectivamente.

## <a name="preparing-your-workstation"></a>Preparación de la estación de trabajo

En primer lugar, prepare la estación de trabajo mediante la creación de un directorio que almacene archivos de configuración de Chef y libros de recetas.

Cree un directorio llamado `C:\Chef`.

Descargue e instale la última versión de la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) en su estación de trabajo.

## <a name="configure-azure-service-principal"></a>Configuración de una entidad de servicio de Azure

Usaremos una entidad de servicio para ayudarnos a crear recursos de Azure desde nuestra estación de trabajo de Chef.  Para crear la entidad de servicio pertinente con los permisos necesarios, ejecute los siguientes comandos en PowerShell:
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

Apunte los valores de SubscriptionID, TenantID, ClientID y Secreto de cliente (la contraseña que ha establecido antes en este tutorial), los necesitará más adelante. 

## <a name="setup-chef-server"></a>Configuración del servidor Chef

En esta guía se da por supuesto que se registrará en una solución de Chef hospedada.

Si no usa un servidor Chef, puede:

* Registrarse para obtener [Hosted Chef](https://manage.chef.io/signup), que es la forma más rápida de empezar a usar Chef.
* Instalar un servidor Chef independiente en el equipo con Linux siguiendo las [instrucciones de instalación](https://docs.chef.io/install_server.html) de [Chef Docs](https://docs.chef.io/).

### <a name="creating-a-hosted-chef-account"></a>Creación de una cuenta de Hosted Chef

Para disfrutar de una cuenta de Hosted Chef, regístrese [aquí](https://manage.chef.io/signup).

Durante el proceso de registro, se le pedirá que cree una organización.

![Ventana de creación de la organización](media/chef-automation/create-organization.png)

Cuando se cree la organización, descargue el starter kit.

![Configuración de Chef](media/chef-automation/configure-chef.png)

> [!NOTE]
> Si recibe un mensaje que le advierte que se restablecerán las claves, está bien continuar como si no tuviéramos ninguna infraestructura existente configurada todavía.
>

El archivo zip de Starter Kit contiene los archivos de configuración y la clave de usuario de la organización que están en el directorio `.chef`.

`organization-validator.pem` debe descargarse por separado, porque es una clave privada y las claves privadas no se deben almacenar en el servidor de Chef. En [Chef Manage](https://manage.chef.io/), vaya a la sección de administración, seleccione "Reset Validation Key" (Restablecer clave de validación) y se le proporcionará un archivo que se descarga por separado. Guarde el archivo en c:\chef.

### <a name="configuring-your-chef-workstation"></a>Configuración de la estación de trabajo Chef

Extraiga el contenido de `chef-starter.zip` en `c:\chef`.

Copie todos los archivos de `chef-starter\chef-repo\.chef` en el directorio `c:\chef`.

Copie el archivo de `organization-validator.pem` en `c:\chef`, si se ha guardado en `c:\Downloads`.

El directorio debería tener ahora un aspecto similar al siguiente ejemplo.

```powershell
    Directory: C:\Users\username\chef

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
d-----    12/6/2018   6:41 PM           .chef
d-----    12/6/2018   5:40 PM           chef-repo
d-----    12/6/2018   5:38 PM           cookbooks
d-----    12/6/2018   5:38 PM           roles
-a----    12/6/2018   5:38 PM       495 .gitignore
-a----    12/6/2018   5:37 PM      1678 azuredocs-validator.pem
-a----    12/6/2018   5:38 PM      1674 user.pem
-a----    12/6/2018   5:53 PM       414 knife.rb
-a----    12/6/2018   5:38 PM      2341 README.md
```

Ahora debería tener cinco archivos y cuatro directorios (incluido el directorio chef-repo vacío) en la raíz de c:\chef.

### <a name="edit-kniferb"></a>Edición de knife.rb

Los archivos PEM contienen sus claves privadas de organización y administración para la comunicación y el archivo knife.rb contiene la configuración de knife. Tendremos que editar el archivo knife.rb.

Abra el archivo knife.rb en el editor que prefiera. El archivo sin modificar debe ser similar a este:

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

Agregue la siguiente información a knife.rb y reemplace los marcadores de posición por su información:

```rb
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg.pem"
knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] =     "#1234p$wdchef19"
```

Estas líneas garantizarán que Knife haga referencia al directorio cookbooks de `c:\chef\cookbooks`.

El archivo `knife.rb` debería ser ahora similar al del siguiente ejemplo:

![Ejemplo de un archivo knife](./media/chef-automation/knife-file-example.png)

```rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "myorg"
client_key               "#{current_dir}/myorg.pem"
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg-validator.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
knife[:azure_tenant_id] = "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] = "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] = "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] = "#1234p$wdchef19"
```

## <a name="install-chef-workstation"></a>Instalación de la estación de trabajo Chef

A continuación, [descargue e instale](https://downloads.chef.io/chef-workstation/) la estación de trabajo de Chef.

Instale la estación de trabajo de Chef en la ubicación predeterminada.

En el escritorio verá CW PowerShell. Esta herramienta se usa para interactuar con los productos de Chef. CW PowerShell permite usar los nuevos comandos, como `chef-run`, así como comandos de la CLI de Chef, como `chef`. Consulte la versión instalada de la estación de trabajo Chef y de las herramientas de Chef con `chef -v`. También puede consultar la versión de la estación de trabajo. Para ello, debe seleccionar **About Chef Workstation** (Acerca de la estación de trabajo de Chef) en la aplicación de la estación de trabajo de Chef.

`chef --version` debe devolver algo similar a esto:

```
Chef Workstation: 0.4.2
  chef-run: 0.3.0
  chef-client: 15.0.300
  delivery-cli: 0.0.52 (9d07501a3b347cc687c902319d23dc32dd5fa621)
  berks: 7.0.8
  test-kitchen: 2.2.5
  inspec: 4.3.2
```

> [!NOTE]
> El orden de la ruta de acceso es importante. Si las rutas de acceso de opscode no están en el orden correcto, se producirán problemas.
>

Reinicie la estación de trabajo antes de continuar.

### <a name="install-knife-azure"></a>Instalación de Knife Azure

En este tutorial se da por supuesto que usa Azure Resource Manager para interactuar con su máquina virtual.

Instale la extensión de Knife para Azure, que incluye el complemento de Azure.

Ejecute el siguiente comando:

    chef gem install knife-azure ––pre

> [!NOTE]
> El argumento `–-pre` garantiza que recibe la versión candidata para lanzamiento más reciente del complemento de Azure para Knife, que ofrece acceso al conjunto más reciente de API.
>
>

Es probable que también se instalen varias dependencias al mismo tiempo.

![Salida de instalación de knife-azure](./media/chef-automation/install-knife-azure.png)

Para garantizar que todo esté configurado correctamente, ejecute el siguiente comando.

    knife azurerm server list

Si todo está configurado correctamente, verá una lista de imágenes de Azure disponibles en desplazamiento.

¡Enhorabuena! La estación de trabajo está configurada.

## <a name="creating-a-cookbook"></a>Creación de una guía

Chef usa las guías para definir un conjunto de comandos que desee ejecutar en el cliente administrado. La creación de una guía es un proceso sencillo, solo haya que usar el comando `chef generate cookbook` para generar la plantilla de guía. Dicho libro de recetas es para un servidor web que implementa IIS automáticamente.

En el `C:\Chef directory`, ejecute el siguiente comando.

    chef generate cookbook webserver

Este comando genera un conjunto de archivos en el directorio C:\Chef\cookbooks\webserver. A continuación, defina el conjunto de comandos que el cliente Chef ejecutara en la máquina virtual administrada.

Los comandos se almacenan en el archivo default.rb. En este archivo, defina un conjunto de comandos que instale IIS, inicie IIS y copie un archivo de plantilla en la carpeta `wwwroot`.

Modifique el archivo C:\chef\cookbooks\webserver\recipes\default.rb y agregue las siguientes líneas.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Cuando haya terminado, guarde el archivo.

## <a name="creating-a-template"></a>Crear una plantilla

En este paso generará un archivo de plantilla que se usará como página `default.html`.

Ejecute el siguiente comando para generar la plantilla:

    chef generate template webserver Default.htm

Vaya al archivo `C:\chef\cookbooks\webserver\templates\default\Default.htm.erb`. Para editar el archivo, agregue código HTML sencillo de *Hello World* y guárdelo.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Carga de la guía en el servidor de Chef

En este paso se realiza una copia de la guía que se ha creado en la máquina local y se carga en el servidor de Chef hospedado. Una vez cargada, la guía aparece en la pestaña **Policy** (Directiva).

    knife cookbook upload webserver

![Resultados de la instalación de la guía en el servidor de Chef](./media/chef-automation/cookbook-installation-under-policy-tab.png)

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Implementar una máquina virtual con Knife Azure

Implemente una máquina virtual de Azure y aplique la guía de `Webserver` con el comando `knife`.

El comando `knife` instalará también el servicio web de IIS y la página web predeterminada.

```bash
    knife azurerm server create `
    --azure-resource-group-name rg-chefdeployment `
    --azure-storage-account store `
    --azure-vm-name chefvm `
    --azure-vm-size 'Standard_DS2_v2' `
    --azure-service-location 'westus' `
    --azure-image-reference-offer 'WindowsServer' `
    --azure-image-reference-publisher 'MicrosoftWindowsServer' `
    --azure-image-reference-sku '2016-Datacenter' `
    --azure-image-reference-version 'latest' `
    -x myuser -P myPassword123 `
    --tcp-endpoints '80,3389' `
    --chef-daemon-interval 1 `
    -r "recipe[webserver]"
```

En el ejemplo del comando `knife` se crea una máquina virtual *Standard_DS2_v2* con Windows Server 2016 instalado en la región Oeste de EE. UU. Modifique estos valores según las necesidades de la aplicación.

Cuando haya ejecutado el comando, vaya a Azure Portal y verá que la máquina comienza a realizar el aprovisionamiento.

![Máquina virtual con aprovisionamiento en curso](./media/chef-automation/virtual-machine-being-provisioned.png)

A continuación aparecerá el símbolo del sistema:

![Salida de knife al crear la máquina virtual](./media/chef-automation/knife-output-when-creating-vm.png)

Una vez completada la implementación, se muestra la dirección IP pública de la nueva máquina virtual. Pegue este valor en un explorador web para ver el nuevo sitio web. Al implementar la máquina virtual, hemos abierto el puerto 80, por lo que debería estar disponible externamente.   

![Prueba de la máquina virtual](./media/chef-automation/testing-the-virtual-machine.png)

En este ejemplo se utiliza código HTML creativo.

También puede ver el estado del nodo en [Chef Manage](https://manage.chef.io/). 

![Visualización del estado del nodo](./media/chef-automation/viewing-node-status.png)

No olvide que también puede conectarse mediante una sesión RDP desde Azure Portal a través del puerto 3389.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Chef en Azure](/azure/chef/)