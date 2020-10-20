---
title: Instalación de un cliente independiente de Service Fabric
description: En este tutorial aprenderá a instalar el cliente independiente de Service Fabric en el clúster.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: ae0b343be986f4d8d5176c1f39eef6b23ca81278
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840649"
---
# <a name="tutorial-install-and-create-service-fabric-cluster"></a>Tutorial: instalación y creación del clúster de Service Fabric

Los clústeres de Service Fabric independientes ofrecen la opción de elegir un entorno propio y crear un clúster como parte del enfoque "Cualquier sistema operativo, cualquier nube" que toma Service Fabric. En esta serie de tutoriales se crea un clúster independiente hospedado en AWS o Azure y se instala en él una aplicación.

Este tutorial es la segunda parte de una serie. Este tutorial le guía por los pasos para crear un clúster independiente de Service Fabric.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Descargar e instalar del paquete independiente de Service Fabric
> * Crear el clúster de Service Fabric
> * Conectarse al clúster de Service Fabric

## <a name="download-the-service-fabric-for-windows-server-package"></a>Descarga del paquete de Service Fabric para Windows Server

Service Fabric proporciona un paquete de instalación para crear clústeres independientes de Service Fabric.  [Descargue el paquete de instalación](https://go.microsoft.com/fwlink/?LinkId=730690) en el equipo local.  Una vez descargado correctamente, cópielo a través de la conexión RDP en la máquina virtual y péguelo en el escritorio.

Seleccione el archivo ZIP, abra el menú contextual y seleccione **Extract All** > **Extract** (Extraer todos > Extraer).  Al extraer los archivos se generará una carpeta en el escritorio con el mismo nombre que el del archivo ZIP.

Más detalles sobre el [contenido del paquete de instalación](service-fabric-cluster-standalone-package-contents.md).

## <a name="set-up-your-configuration-file"></a>Configuración del archivo de configuración

Está compilando un clúster Windows de tres nodos, por lo que necesita modificar el archivo `ClusterConfig.Unsecure.MultiMachine.json`.

A continuación, actualice las tres líneas de dirección IP del archivo en las líneas 8, 15 y 22 con las direcciones IP de las instancias.

Después de actualizar los nodos, aparecen como se indica a continuación:

```json
        {
            "nodeName": "vm0",
            "ipAddress": "172.31.27.1",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        }
```

A continuación, debe actualizar un par de propiedades.  En la línea 34, debe modificar la cadena de conexión para el almacén de diagnóstico; debe ser similar a la siguiente `"connectionstring": "C:\\ProgramData\\SF\\DiagnosticsStore"`

Por último, en la sección `nodeTypes` de la configuración, agregue una nueva sección para asignar los puertos efímeros que usará Windows.  El archivo de configuración debería tener un aspecto similar al siguiente:

```json
"applicationPorts": {
    "startPort": "20001",
    "endPort": "20031"
},
"ephemeralPorts": {
    "startPort": "20606",
    "endPort": "20861"
},
"isPrimary": true
```

## <a name="validate-the-environment"></a>Validación del entorno

El script *TestConfiguration.ps1* del paquete independiente se usa como analizador de procedimientos recomendados para validar si se puede implementar un clúster en un entorno determinado. En la [preparación para la implementación](service-fabric-cluster-standalone-deployment-preparation.md) se enumeran los requisitos previos y los requisitos del entorno. Ejecute el script para comprobar si puede crear el clúster de desarrollo:

```powershell
cd .\Desktop\Microsoft.Azure.ServiceFabric.WindowsServer.6.2.274.9494\
.\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
```

La salida debe ser parecida al ejemplo siguiente. Si el campo inferior "Passed" se devuelve como `True`, se han superado las comprobaciones de integridad y, según la configuración de entrada el clúster se puede implementar.

```powershell
Trace folder already exists. Traces will be written to existing trace folder: C:\Users\Administrator\Desktop\Microsoft.Azure.ServiceFabric.WindowsServer.6.2.274.9494\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 :
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
DataDrivesAvailable        : True
NoDomainController         : True
Passed                     : True
```

## <a name="create-the-cluster"></a>Creación del clúster

Una vez validada correctamente la configuración del clúster, ejecute el script *CreateServiceFabricCluster.ps1* para implementar el clúster de Service Fabric en las máquinas virtuales del archivo de configuración.

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

Si todo funciona, obtendrá una salida similar a esta:

```powershell
Your cluster is successfully created! You can connect and manage your cluster using Microsoft Azure Service Fabric Explorer or PowerShell. To connect through PowerShell, run 'Connect-ServiceFabricCluster [ClusterConnectionEndpoint]'.
```

> [!NOTE]
> Se escriben seguimientos de implementación en la máquina virtual/máquina física en la que ejecutó el script CreateServiceFabricCluster.ps1 de PowerShell. Estos seguimientos se pueden encontrar en la subcarpeta DeploymentTraces, que se encuentra en el directorio desde el que se ejecutó el script. Para ver si Service Fabric se ha implementado correctamente en una máquina, busque los archivos instalados en el directorio FabricDataRoot, como se detalla en la sección FabricSettings del archivo de configuración del clúster (de forma predeterminada c:\ProgramData\SF). También, los procesos FabricHost.exe y Fabric.exe se pueden ver ejecutando el Administrador de tareas.
>
>

### <a name="open-service-fabric-explorer"></a>Apertura de Service Fabric Explorer

Ahora puede conectarse al clúster con Service Fabric Explorer directamente desde una de las máquinas con http:\//localhost:19080/Explorer/index.html o de forma remota con http:\//<*IPAddressofaMachine*>:19080/Explorer/index.html.

## <a name="add-and-remove-nodes"></a>Agregar y quitar nodos

Puede agregar o quitar nodos del clúster de Service Fabric independiente a medida que cambien las necesidades empresariales. Lea [Incorporación o eliminación de nodos de un clúster de Service Fabric independiente](service-fabric-cluster-windows-server-add-remove-nodes.md) para obtener pasos detallados.

## <a name="next-steps"></a>Pasos siguientes

En este artículo aprendió no solo cómo cargar grandes cantidades de datos aleatorios en una cuenta de almacenamiento en paralelo, si no también lo siguiente:

> [!div class="checklist"]
> * Configurar la cadena de conexión
> * Compilar la aplicación
> * Ejecución de la aplicación
> * Validar el número de conexiones

Vaya a la tercera parte de la serie para instalar una aplicación en el clúster que ha creado.

> [!div class="nextstepaction"]
> [Instalación de la aplicación en el clúster de Service Fabric](service-fabric-tutorial-standalone-install-an-application.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
