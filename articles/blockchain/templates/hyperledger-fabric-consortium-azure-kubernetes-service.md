---
title: Consorcio Hyperledger Fabric en Azure Kubernetes Service (AKS)
description: Implementación y configuración de una red del consorcio de Hyperledger Fabric en Azure Kubernetes Service
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: v-umha
ms.openlocfilehash: 2312c002e5c2e0b813f8acbdc3e3bff597f204d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476447"
---
# <a name="hyperledger-fabric-consortium-on-azure-kubernetes-service-aks"></a>Consorcio Hyperledger Fabric en Azure Kubernetes Service (AKS)

Puede usar la plantilla de Hyperledger Fabric (HLF) en Azure Kubernetes Service (AKS) para implementar y configurar una red del consorcio de Hyperledger Fabric en Azure.

Después de leer este artículo, habrá aprendido lo siguiente:

- Obtendrá conocimiento práctico de Hyperledger Fabric y de los distintos componentes que forman los bloques de creación de la red de la cadena de bloques de Hyperledger Fabric.
- Aprenderá a implementar y configurar un consorcio de Hyperledger Fabric en Azure Kubernetes Service para sus escenarios de producción.

## <a name="hyperledger-fabric-consortium-architecture"></a>Arquitectura del consorcio de Hyperledger Fabric

Para compilar la red de Hyperledger Fabric en Azure, debe implementar el servicio de ordenación y la organización con nodos del mismo nivel. Los diferentes componentes fundamentales que se crean como parte de la implementación de la plantilla son:

- **Nodos solicitantes**: Un nodo responsable del orden de las transacciones en el libro de contabilidad. Junto con otros nodos, los nodos ordenados forman el servicio de ordenación de la red de Hyperledger Fabric.

- **Nodos del mismo nivel**: Un nodo que hospeda principalmente libros de contabilidad y contratos inteligentes, estos elementos fundamentales de la red.

- **Fabric CA**: Fabric CA es la entidad de certificación (CA) para Hyperledger Fabric. Fabric CA permite inicializar el proceso de servidor que hospeda la entidad de certificación. Permite administrar identidades y certificados. Cada clúster de AKS implementado como parte de la plantilla tendrá un pod de Fabric CA de forma predeterminada.

- **CouchDB o LevelDB**: La base de datos de estado mundial de los nodos del mismo nivel se puede almacenar en LevelDB o CouchDB. LevelDB es la base de datos de estado predeterminada incrustada en el nodo del mismo nivel y almacena los datos del código de cadena como pares clave-valor simples y admite solo consultas de clave, intervalo de claves y clave compuesta. CouchDB es una base de datos de estado alternativa opcional que admite consultas enriquecidas cuando los valores de los datos del código de cadena se modelan como JSON.

La plantilla de la implementación pone en marcha varios recursos de Azure en su suscripción. Los diferentes recursos de Azure implementados son:

- **Clúster de AKS**: Clúster de Azure Kubernetes que se configura según los parámetros de entrada proporcionados por el cliente. El clúster de AKS tiene varios pods configurados para ejecutar los componentes de red de Hyperledger Fabric. Los distintos pods creados son:

  - **Herramientas de Fabric**: La herramienta de Fabric es responsable de la configuración de los componentes de Hyperledger Fabric.
  - **Pods solicitantes/del mismo nivel**: Los nodos de la red HLF.
  - **Proxy**: Un pod de proxy de NGNIX a través del cual las aplicaciones cliente pueden interactuar con el clúster de AKS.
  - **Fabric CA**: El pod que ejecuta la entidad Fabric CA.
- **PostgreSQL**: Se implementa una instancia de PostgreSQL para mantener las identidades de Fabric CA.

- **Azure Key Vault**: Se implementa una instancia de Key Vault para guardar las credenciales de Fabric CA y los certificados raíz proporcionados por el cliente, que se usa en caso de reintento de implementación de plantilla, para administrar la mecánica de la plantilla.
- **Disco administrado de Azure**: El disco administrado de Azure es para el almacenamiento persistente de la base de datos de estado mundial del nodo del mismo nivel y el libro de contabilidad.
- **Dirección IP pública**: Un punto de conexión de dirección IP pública del clúster de AKS implementado para interactuar con el clúster.

## <a name="hyperledger-fabric-blockchain-network-setup"></a>Configuración de red de cadena de bloques de Hyperledger Fabric

Para comenzar, necesita una suscripción a Azure que pueda admitir la implementación de varias máquinas virtuales y cuentas de almacenamiento estándar. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita de Azure](https://azure.microsoft.com/free/).

Configure la red de cadena de bloques de Hyperledger Fabric mediante los pasos siguientes:

- [Implementación de la organización solicitante/del mismo nivel](#deploy-the-ordererpeer-organization)
- [Compilación del consorcio](#build-the-consortium)
- [Ejecución de operaciones HLF nativas](#run-native-hlf-operations)

## <a name="deploy-the-ordererpeer-organization"></a>Implementación de la organización solicitante/del mismo nivel

Para empezar a trabajar con la implementación de componentes de red de HLF, vaya a [Azure Portal](https://portal.azure.com). Seleccione **Crear un recurso > Cadena de bloques** > busque **Hyperledger Fabric en Azure Kubernetes Service**.

1. Seleccione **Crear** para iniciar la implementación de la plantilla. Se muestra la opción de **crear Hyperledger Fabric en Azure Kubernetes Service**.

    ![Plantilla de Hyperledger Fabric en Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-fabric-aks.png)

2. Escriba los detalles del proyecto en la página **Datos básicos**.

    ![Plantilla de Hyperledger Fabric en Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-basics.png)

3. Escriba la siguiente información:
    - **Suscripción**: Elija el nombre de la suscripción en la que desea implementar los componentes de red de HLF.
    - **Grupo de recursos**: Cree un nuevo grupo de recursos o elija un grupo de recursos vacío existente, el grupo de recursos contendrá todos los recursos implementados como parte de la plantilla.
    - **Región**: Elija la región de Azure en la que desea implementar el clúster de Azure Kubernetes para los componentes de HLF. La plantilla está disponible en todas las regiones en las que AKS está disponible. Asegúrese de elegir una región en la que la suscripción no alcance el límite de cuota de máquina virtual (VM).
    - **Prefijo de recurso**: Prefijo para el nombre de los recursos que se implementan. El prefijo de recurso debe tener menos de seis caracteres de longitud y la combinación de caracteres debe incluir números y letras.
4. Seleccione la pestaña **Configuración de Fabric** para definir los componentes de la red de HLF que se implementarán.

    ![Plantilla de Hyperledger Fabric en Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-settings.png)

5. Escriba la siguiente información:
    - **Nombre de la organización**: El nombre de la organización de Fabric, que es necesario para varias operaciones del plano de datos. El nombre de la organización debe ser único en cada implementación. 
    - **Componente de red de Fabric**: Elija el servicio de ordenación o los nodos del mismo nivel basados en el componente de red de la cadena de bloques que desea configurar.
    - **Número de nodos**: Estos son los dos tipos de nodos:
        - Servicio de ordenación: Seleccione el número de nodos para proporcionar tolerancia a errores a la red. Solo se admiten 3, 5 o 7 nodos solicitantes.
        - Nodos del mismo nivel: Puede elegir entre 1 y 10 nodos, según sus necesidades.
    - **Base de datos de estado mundial del nodo del mismo nivel**: Elija entre LevelDB y CoucbDB. Este campo se muestra cuando el usuario elige el nodo del mismo nivel en el menú desplegable del componente de la red de Fabric.
    - **Nombre de usuario de Fabric**: Escriba el nombre de usuario que se usa para la autenticación de Fabric CA.
    - **Contraseña de Fabric CA**: Escriba la contraseña para la autenticación de Fabric CA.
    - **Confirmar contraseña**: Confirme la contraseña de Fabric CA.
    - **Certificados**: Si desea usar sus propios certificados raíz para inicializar Fabric CA, elija la opción de cargar certificado raíz para la opción Fabric CA; de lo contrario, Fabric CA crea certificados autofirmados.
    - **Certificado raíz**: Cargue el certificado raíz (clave pública) con el que se debe inicializar Fabric CA. Se admiten los certificados con formato. pem; los certificados deben ser válidos en la zona horaria UTC.
    - **Clave privada de certificado raíz**: Cargue la clave privada del certificado raíz. Si tiene un certificado. pem, que tiene una clave pública y privada combinada, cárguela aquí también.


6. Seleccione la pestaña **AKS cluster Settings** (Configuración del clúster de AKS) para definir la configuración del clúster de Azure Kubernetes, que es la infraestructura subyacente en la que se van a configurar los componentes de la red de Fabric.

    ![Plantilla de Hyperledger Fabric en Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/create-for-hyperledger-fabric-aks-cluster-settings-1.png)

7. Escriba la siguiente información:
    - **Nombre del clúster de Kubernetes**: Nombre del clúster de AKS que se crea. Este campo se rellena previamente según el prefijo de recurso proporcionado; puede cambiarlo si es necesario.
    - **Versión de Kubernetes**: La versión de Kubernetes que se implementará en el clúster. En función de la región seleccionada en la pestaña **Datos básicos**, las versiones admitidas pueden cambiar.
    - **Prefijo de DNS**: Prefijo del nombre del sistema de nombres de dominio (DNS) para el clúster de AKS. Usará DNS para conectarse a la API de Kubernetes al administrar los contenedores después de crear el clúster.
    - **Tamaño del nodo**: El tamaño del nodo de Kubernetes, puede elegir en la lista de referencia de almacén (SKU) de máquinas virtuales disponible en Azure. Para obtener un rendimiento óptimo, se recomienda el estándar DS3 v2.
    - **Node count** (Número de nodos): Cantidad de nodos de Kubernetes que se van a implementar en el clúster. Se recomienda mantener este número de nodos al menos igual o superior que el número de nodos de HLF especificados en la configuración de Fabric.
    - **Identificador de cliente de la entidad de servicio**: Escriba el identificador de cliente de una entidad de servicio existente o cree uno nuevo, que es necesario para la autenticación de AKS. Vea los pasos para la [creación de la entidad de servicio](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-3.2.0#create-a-service-principal).
    - **Secreto de cliente de la entidad de servicio**: Escriba el secreto de cliente de la entidad de servicio proporcionada en el identificador del cliente de la entidad de servicio.
    - **Confirmar secreto del cliente**: Confirme el secreto de cliente proporcionado en el secreto de cliente de la entidad de servicio.
    - **Habilitar la supervisión de contenedores**: Opte por habilitar la supervisión de AKS, que permite que los registros de AKS se inserten en el área de trabajo de Log Analytics especificada.
    - **Área de trabajo de Log Analytics**: El área de trabajo de Log Analytics se rellenará con el área de trabajo predeterminada que se crea si la supervisión está habilitada.

8. Después de proporcionar todos los detalles anteriores, seleccione la pestaña **Revisar y crear**. La revisión y la creación desencadenan la validación de los valores proporcionados.
9. Una vez que se supera la validación, puede seleccionar **Crear**.
La implementación normalmente tarda 10-12 minutos, puede variar en función del tamaño y el número de nodos de AKS especificados.
10. Cuando la implementación se complete correctamente, recibirá una notificación a través de las notificaciones de Azure en la esquina superior derecha.
11. Seleccione **Ir al grupo de recursos** para comprobar todos los recursos creados como parte de la implementación de la plantilla. Todos los nombres de los recursos comenzarán con el prefijo proporcionado en la configuración de los **Datos básicos**.

## <a name="build-the-consortium"></a>Compilación del consorcio

Para compilar la publicación del consorcio de la cadena de bloques que implementa el servicio de ordenación y los nodo del mismo nivel, tiene que ejecutar los siguientes pasos en orden. Cree el script para **compilar su propia red** (byn.sh), que le ayuda a configurar el consorcio, crear el canal e instalar el código de cadena.

> [!NOTE]
> El script para compilar su propia red (byn) que se proporciona debe usarse estrictamente en escenarios de demostración y de desarrollo y pruebas. En el caso de una configuración en el nivel de producción, se recomienda usar las API nativas de HLF.

Todos los comandos para ejecutar el script byn se pueden ejecutar a través de la interfaz de la línea de comandos (CLI) de Azure Bash. Puede iniciar sesión en la versión web de Azure Shell a través de la opción ![Plantilla de Hyperledger Fabric en Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/arrow.png) en la esquina superior derecha de Azure Portal. En el símbolo del sistema, escriba bash y escriba para cambiar a la CLI de Bash.

Para más información, consulte [Introducción a Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

![Plantilla de Hyperledger Fabric en Azure Kubernetes Service](./media/hyperledger-fabric-consortium-azure-kubernetes-service/hyperledger-powershell.png)


Descargue el archivo byn.sh y fabric-admin.yaml.

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/consortiumScripts/byn.sh -o byn.sh; chmod 777 byn.sh
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/consortiumScripts/fabric-admin.yaml -o fabric-admin.yaml
```
**Establezca las siguientes variables de entorno en el shell de Bash de la CLI de Azure**:

Establezca la información de canal y la información de la organización solicitante.

```bash
SWITCH_TO_AKS_CLUSTER() { az aks get-credentials --resource-group $1 --name $2 --subscription $3; }
ORDERER_AKS_SUBSCRIPTION=<ordererAKSClusterSubscriptionID>
ORDERER_AKS_RESOURCE_GROUP=<ordererAKSClusterResourceGroup>
ORDERER_AKS_NAME=<ordererAKSClusterName>
ORDERER_DNS_ZONE=$(az aks show --resource-group $ORDERER_AKS_RESOURCE_GROUP --name $ORDERER_AKS_NAME --subscription $ORDERER_AKS_SUBSCRIPTION -o json | jq .addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName | tr -d '"')
ORDERER_END_POINT="orderer1.$ORDERER_DNS_ZONE:443"
CHANNEL_NAME=<channelName>
```
Establezca la información de la organización del mismo nivel.

```bash
PEER_AKS_RESOURCE_GROUP=<peerAKSClusterResourceGroup>
PEER_AKS_NAME=<peerAKSClusterName>
PEER_AKS_SUBSCRIPTION=<peerAKSClusterSubscriptionID>
#Peer organization name is case-sensitive. Specify exactly the same name, which was provided while creating the Peer AKS Cluster.
PEER_ORG_NAME=<peerOrganizationName>
```

Cree un recurso compartido de archivos de Azure para compartir varios certificados públicos entre la organización del mismo nivel y la organización solicitante.

```bash
STORAGE_SUBSCRIPTION=<subscriptionId>
STORAGE_RESOURCE_GROUP=<azureFileShareResourceGroup>
STORAGE_ACCOUNT=<azureStorageAccountName>
STORAGE_LOCATION=<azureStorageAccountLocation>
STORAGE_FILE_SHARE=<azureFileShareName>

az account set --subscription $STORAGE_SUBSCRIPTION
az group create -l $STORAGE_LOCATION -n $STORAGE_RESOURCE_GROUP
az storage account create -n $STORAGE_ACCOUNT -g  $STORAGE_RESOURCE_GROUP -l $STORAGE_LOCATION --sku Standard_LRS
STORAGE_KEY=$(az storage account keys list --resource-group $STORAGE_RESOURCE_GROUP  --account-name $STORAGE_ACCOUNT --query "[0].value" | tr -d '"')
az storage share create  --account-name $STORAGE_ACCOUNT  --account-key $STORAGE_KEY  --name $STORAGE_FILE_SHARE
SAS_TOKEN=$(az storage account generate-sas --account-key $STORAGE_KEY --account-name $STORAGE_ACCOUNT --expiry `date -u -d "1 day" '+%Y-%m-%dT%H:%MZ'` --https-only --permissions lruwd --resource-types sco --services f | tr -d '"')
AZURE_FILE_CONNECTION_STRING="https://$STORAGE_ACCOUNT.file.core.windows.net/$STORAGE_FILE_SHARE?$SAS_TOKEN"
```
**Comandos de administración de canal**

Vaya al clúster de AKS de la organización solicitante y emita el comando para crear un nuevo canal.

```bash
SWITCH_TO_AKS_CLUSTER $ORDERER_AKS_RESOURCE_GROUP $ORDERER_AKS_NAME $ORDERER_AKS_SUBSCRIPTION
./byn.sh createChannel "$CHANNEL_NAME"
```

**Comandos de administración del consorcio**

Ejecute los comandos siguientes en el orden indicado para agregar una organización del mismo nivel en un canal y un consorcio.

1. Vaya al clúster de AKS de la organización del mismo nivel y cargue su proveedor de servicios miembro en una unidad de Azure File Storage.

    ```bash
    SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
    ./byn.sh uploadOrgMSP "$AZURE_FILE_CONNECTION_STRING"
    ```

2. Vaya al clúster de AKS de la organización solicitante y agregue la organización del mismo nivel en el canal y el consorcio.

    ```bash
    SWITCH_TO_AKS_CLUSTER $ORDERER_AKS_RESOURCE_GROUP $ORDERER_AKS_NAME $ORDERER_AKS_SUBSCRIPTION
    #add peer in consortium
    ./byn.sh addPeerInConsortium "$PEER_ORG_NAME" "$AZURE_FILE_CONNECTION_STRING"
    #add peer in channel
    ./byn.sh addPeerInChannel "$PEER_ORG_NAME" "$CHANNEL_NAME" "$AZURE_FILE_CONNECTION_STRING"
    ```

3. Vuelva a la organización del mismo nivel y emita el comando para unir los nodos del mismo nivel en el canal.

    ```bash
    SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
    ./byn.sh joinNodesInChannel "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
    ```

Del mismo modo, para agregar más organizaciones del mismo nivel en el canal, actualice las variables de entorno de AKS del mismo nivel según la organización del mismo nivel requerida y ejecute los pasos de 1 a 3.

**Comandos de administración de código de cadena**

Ejecute el siguiente comando para realizar la operación relacionada con el código de cadena. Estos comandos realizan todas las operaciones en un código de cadena de demostración. Este código de cadena de demostración tiene dos variables: "a" y "b". Al crear una instancia del código de cadena, "a" se inicializa con 1000 y "b" se inicializa con 2000. En cada invocación del código de cadena, 10 unidades se transfieren de "a" a "b". La operación de consulta en el código de cadena muestra el estado mundial de la variable "a".

Ejecute los siguientes comandos ejecutados en el clúster de AKS de la organización del mismo nivel.

```bash
# switch to peer organization AKS cluster. Skip this command if already connected to the required Peer AKS Cluster
SWITCH_TO_AKS_CLUSTER $PEER_AKS_RESOURCE_GROUP $PEER_AKS_NAME $PEER_AKS_SUBSCRIPTION
```
**Comandos de operación de código de cadena**

```bash
PEER_NODE_NAME="peer<peer#>"
./byn.sh installDemoChaincode "$PEER_NODE_NAME"
./byn.sh instantiateDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
./byn.sh invokeDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME" "$ORDERER_END_POINT" "$AZURE_FILE_CONNECTION_STRING"
./byn.sh queryDemoChaincode "$PEER_NODE_NAME" "$CHANNEL_NAME"
```

## <a name="run-native-hlf-operations"></a>Ejecución de operaciones HLF nativas

Para ayudar a los clientes a empezar a ejecutar comandos nativos de Hyperledger en la red HLF en AKS. Se proporciona la aplicación de ejemplo que usa el SDK de NodeJS de Fabric para realizar las operaciones de HLF. Los comandos se proporcionan para crear una nueva identidad de usuario e instalar su propio código de cadena.

### <a name="before-you-begin"></a>Antes de empezar

Siga los siguientes comandos para la configuración inicial de la aplicación:

- Descarga de los archivos de la aplicación
- Generación del perfil de conexión y el perfil de administrador
- Importación de la identidad del usuario administrador

Después de completar la configuración inicial, puede usar el SDK para lograr las operaciones siguientes:

- Generación de identidad de usuario
- Operaciones de código de cadena

Los comandos mencionados anteriormente se pueden ejecutar desde Azure Cloud Shell.

### <a name="download-application-files"></a>Descarga de los archivos de la aplicación

La primera configuración de la aplicación en ejecución consiste en descargar todos los archivos de la aplicación en una carpeta.

**Cree la carpeta de la aplicación y escriba en la carpeta**:

```bash
mkdir app
cd app
```
Ejecute el comando siguiente para descargar todos los archivos y paquetes necesarios:

```bash-interactive
curl https://raw.githubusercontent.com/Azure/Hyperledger-Fabric-on-Azure-Kubernetes-Service/master/application/setup.sh | bash
```
Este comando tarda tiempo en cargar todos los paquetes. Después de ejecutar correctamente el comando, puede ver una carpeta `node_modules` en el directorio actual. Todos los paquetes necesarios se cargan en la carpeta `node_modules`.

### <a name="generate-connection-profile-and-admin-profile"></a>Generación del perfil de conexión y el perfil de administrador

Cree el directorio `profile` dentro de la carpeta `app`.

```bash
cd app
mkdir ./profile
```
Establezca estas variables de entorno en Azure Cloud Shell.

```bash
# Organization name whose connection profile is to be generated
ORGNAME=<orgname>
# Organization AKS cluster resource group
AKS_RESOURCE_GROUP=<resourceGroup>
```

Ejecute el comando siguiente para generar el perfil de conexión y el perfil de administrador de la organización

```bash
./getConnector.sh $AKS_RESOURCE_GROUP | sed -e "s/{action}/gateway/g"| xargs curl > ./profile/$ORGNAME-ccp.json
./getConnector.sh $AKS_RESOURCE_GROUP | sed -e "s/{action}/admin/g"| xargs curl > ./profile/$ORGNAME-admin.json
```

Creará el perfil de conexión y el `profile` de administración de la organización en la carpeta de perfil con el nombre `<orgname>-ccp.json` y `<orgname>-admin.json`, respectivamente.

Del mismo modo, genere el perfil de conexión y el perfil de administrador para cada organización solicitante y del mismo nivel.


### <a name="import-admin-user-identity"></a>Importación de la identidad del usuario administrador

El último paso consiste en importar la identidad del usuario administrador de la organización en la cartera.

```bash
npm run importAdmin -- -o <orgName>

```
El comando anterior ejecuta importAdmin.js para importar la identidad del usuario administrador en la cartera. El script lee la identidad de administrador del perfil de administrador `<orgname>-admin.json` y lo importa en la cartera para ejecutar operaciones de HLF.

Los scripts usan la cartera del sistema de archivos para almacenar las identidades. Crea una cartera según la ruta de acceso especificada en el campo ".wallet" en el perfil de conexión. De forma predeterminada, el campo ".wallet" se inicializa con `<orgname>`, lo que significa que se crea una carpeta denominada `<orgname>` en el directorio actual para almacenar las identidades. Si desea crear la cartera en otra ruta de acceso, modifique el campo ".wallet" en el perfil de conexión antes de ejecutar la inscripción del usuario administrador y cualquier otra operación de HLF.

Del mismo modo, importe la identidad del usuario administrador para cada organización.

Consulte la ayuda del comando para obtener más detalles sobre los argumentos pasados en el comando.

```bash
npm run importAdmin -- -h

```

### <a name="user-identity-generation"></a>Generación de identidad de usuario

Ejecute los comandos siguientes en el orden indicado para generar nuevas identidades de usuario para la organización de HLF.

> [!NOTE]
> Antes de comenzar con los pasos de generación de identidad de usuario, asegúrese de que se ha realizado la configuración inicial de la aplicación.

Establezca las siguientes variables de entorno en Azure Cloud Shell.

```bash
# Organization name for which user identity is to be generated
ORGNAME=<orgname>
# Name of new user identity. Identity will be registered with the Fabric-CA using this name.
USER_IDENTITY=<username>

```

Registro e inscripción de un nuevo usuario

Para registrar e inscribir un nuevo usuario, ejecute el comando siguiente que ejecuta registerUser.js. Guarda la identidad del usuario generada en la cartera.

```bash
npm run registerUser -- -o $ORGNAME -u $USER_IDENTITY

```

> [!NOTE]
> La identidad del usuario administrador se usa para emitir el comando de registro para el nuevo usuario. Por lo tanto, es obligatorio tener la identidad del usuario administrador en la cartera antes de ejecutar este comando. De lo contrario, se producirá un error en este comando.

Consulte la ayuda del comando para obtener más detalles sobre los argumentos pasados en el comando.

```bash
npm run registerUser -- -h

```

### <a name="chaincode-operations"></a>Operaciones de código de cadena


> [!NOTE]
> Antes de comenzar con la operación del código de cadena, asegúrese de que se ha realizado la configuración inicial de la aplicación.

Establezca las siguientes variables de entorno en Azure Cloud Shell:

```bash
# peer organization name where chaincode is to be installed
ORGNAME=<orgName>
USER_IDENTITY="admin.$ORGNAME"
CC_NAME=<chaincodeName>
CC_VERSION=<chaincodeVersion>
# Language in which chaincode is written. Supported languages are 'node', 'golang' and 'java'
# Default value is 'golang'
CC_LANG=<chaincodeLanguage>
# CC_PATH contains the path where your chaincode is place. In case of go chaincode, this path is relative to 'GOPATH'.
# For example, if your chaincode is present at path '/opt/gopath/src/chaincode/chaincode.go'.
# Then, set GOPATH to '/opt/gopath' and CC_PATH to 'chaincode'
CC_PATH=<chaincodePath>
# 'GOPATH' environment variable. This needs to be set in case of go chaincode only.
export GOPATH=<goPath>
# Channel on which chaincode is to be instantiated/invoked/queried
CHANNEL=<channelName>

````

Se pueden realizar las siguientes operaciones de código de cadena:

- Instalación del código de cadena
- Creación de instancia del código de cadena
- Invocación del código de cadena
- Consulta del código de cadena

### <a name="install-chaincode"></a>Instalación del código de cadena

Ejecute el comando siguiente para instalar el código de cadena en la organización del mismo nivel.

```bash
npm run installCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -l $CC_LANG -v $CC_VERSION

```
Esto instalará el código de cadena en todos los nodos del mismo nivel de la organización establecidos en la variable de entorno `ORGNAME`. Si hay dos o más organizaciones del mismo nivel en el canal y desea instalar el código de cadena en todas ellas, ejecute los comandos por separado para cada organización del mismo nivel.

Siga estos pasos:

- Establezca `ORGNAME` en `<peerOrg1Name>` y emita el comando `installCC`.
- Establezca `ORGNAME` en `<peerOrg2Name>` y emita el comando `installCC`.

  Ejecútelo para cada organización del mismo nivel.

Consulte la ayuda del comando para obtener más detalles sobre los argumentos pasados en el comando.

```bash
npm run installCC -- -h

```

### <a name="instantiate-chaincode"></a>Creación de instancia del código de cadena

Ejecute el comando siguiente para crear una instancia del código de cadena en el mismo nivel.

```bash
npm run instantiateCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -p $CC_PATH -v $CC_VERSION -l $CC_LANG -c $CHANNEL -f <instantiateFunc> -a <instantiateFuncArgs>

```
Pase el nombre de la función de creación de instancias y la lista de argumentos separados por comas en `<instantiateFunc>` y `<instantiateFuncArgs>`, respectivamente. Por ejemplo, en [fabrcar chaincode](https://github.com/hyperledger/fabric-samples/blob/release/chaincode/fabcar/fabcar.go), para crear una instancia del código de cadena, establezca `<instantiateFunc>` en `"Init"` y `<instantiateFuncArgs>` en la cadena vacía `""`.

> [!NOTE]
> Ejecute el comando para una vez desde una organización del mismo nivel en el canal.
> Una vez que la transacción se envía correctamente al solicitante, el solicitante distribuye esta transacción a todas las organizaciones del mismo nivel del canal. Por lo tanto, se crea una instancia del código de cadena en todos los nodos del mismo nivel en todas las organizaciones del mismo nivel del canal.

Consulte la ayuda del comando para obtener más detalles sobre los argumentos pasados en el comando.

```bash
npm run instantiateCC -- -h

```

### <a name="invoke-chaincode"></a>Invocación del código de cadena

Ejecute el siguiente comando para invocar la función del código de cadena:

```bash
npm run invokeCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL -f <invokeFunc> -a <invokeFuncArgs>

```
Pase el nombre de la función de invocación y la lista de argumentos separados por comas en `<invokeFunction>` y `<invokeFuncArgs>`, respectivamente. Siguiendo con el ejemplo del código de cadena fabcar, para invocar la función initLedger, establezca `<invokeFunction>` en `"initLedger"` y `<invokeFuncArgs>` en `""`.

> [!NOTE]
> Ejecute el comando para una vez desde una organización del mismo nivel en el canal.
> Una vez que la transacción se envía correctamente al solicitante, este distribuye esta transacción a todas las organizaciones del mismo nivel del canal. Por lo tanto, el estado mundial se actualiza en todos los nodos del mismo nivel de todas las organizaciones del mismo nivel del canal.

Consulte la ayuda del comando para obtener más detalles sobre los argumentos pasados en el comando.

```bash
npm run invokeCC -- -h

```

### <a name="query-chaincode"></a>Consulta del código de cadena

Ejecute el siguiente comando para consultar el código de cadena:

```bash
npm run queryCC -- -o $ORGNAME -u $USER_IDENTITY -n $CC_NAME -c $CHANNEL -f <queryFunction> -a <queryFuncArgs>

```

Pase el nombre de la función de consulta y la lista de argumentos separados por comas en `<queryFunction>` y `<queryFuncArgs>`, respectivamente. Una vez más, tomando el código de cadena `fabcar` como referencia, para consultar todos los automóviles del estado mundial, configure `<queryFunction>` en `"queryAllCars"` y `<queryArgs>` en `""`.

Consulte la ayuda del comando para obtener más detalles sobre los argumentos pasados en el comando.

```bash
npm run queryCC -- -h

```
