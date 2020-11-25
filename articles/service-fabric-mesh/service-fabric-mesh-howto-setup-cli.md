---
title: Configuración de la CLI de Azure Service Fabric Mesh
description: La interfaz de la línea de comandos (CLI) de Service Fabric Mesh es necesaria para implementar y administrar recursos de Azure Service Fabric Mesh. Así es como se configura.
author: georgewallace
ms.author: gwallace
ms.date: 11/28/2018
ms.topic: conceptual
ms.openlocfilehash: ea4a7764cf1ede1cfaf53b1097034c5894660376
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94660685"
---
# <a name="set-up-service-fabric-mesh-cli"></a>Configuración de la CLI de Service Fabric Mesh
La interfaz de la línea de comandos (CLI) de Service Fabric Mesh es necesaria para implementar y administrar recursos de Azure Service Fabric Mesh. Así es como se configura.

Hay tres tipos de interfaces de la línea de comandos que se pueden utilizar y que se resumen en la siguiente tabla.

| Módulo de la CLI | Entorno de destino |  Descripción | 
|---|---|---|
| az mesh | Azure Service Fabric Mesh | La CLI principal que le permite implementar las aplicaciones y administrar recursos en el entorno de Azure Service Fabric Mesh. 
| sfctl | Clústeres locales | CLI de Service Fabric que permite la implementación y la prueba de los recursos de Service Fabric frente a los clústeres locales.  
| CLI de Maven | Clústeres locales y Azure Service Fabric Mesh | Un contenedor alrededor de `az mesh` y `sfctl` que permite a los desarrolladores de Java utilizar una experiencia de línea de comandos familiar para la experiencia de desarrollo local y de Azure.  

En la versión preliminar, la CLI de Azure Service Fabric Mesh está escrita como una extensión de la CLI de Azure. Puede instalarla en Azure Cloud Shell o como una instalación local de la CLI de Azure. 

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- En este artículo se necesita la versión 2.0.67 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="install-the-azure-service-fabric-mesh-cli"></a>Instalación de la CLI de Azure Service Fabric Mesh

Si aún no lo ha hecho, instale el módulo de extensión de la CLI de Azure Service Fabric Mesh mediante el siguiente comando: 
 
```azurecli-interactive
az extension add --name mesh
```

Si ya está instalado, actualice un módulo existente de la CLI de Azure Service Fabric Mesh mediante el comando siguiente:

```azurecli-interactive
az extension update --name mesh
```

## <a name="install-the-service-fabric-cli-sfctl"></a>Instalación de la CLI de Service Fabric (sfctl) 

Siga las instrucciones de [Configuración de la CLI de Service Fabric](../service-fabric/service-fabric-cli.md). El módulo **sfctl** puede utilizarse para la implementación de aplicaciones basadas en el modelo de recursos contra clústeres de Service Fabric en su máquina local. 

## <a name="install-the-maven-cli"></a>Instalación de la CLI de Maven 

Para poder usar la CLI de Maven, se debe instalar lo siguiente en la máquina: 

* [Java](https://www.azul.com/downloads/zulu/)
* [Maven](https://maven.apache.org/download.cgi)
* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* CLI de Azure Mesh (az mesh): para seleccione el destino en Azure Service Fabric Mesh 
* SFCTL (sfctl): para dirigirse a los clústeres locales 

La CLI de Maven para Service Fabric sigue en versión preliminar. 

Para usar el complemento de Maven en la aplicación Java de Maven, agrega el siguiente fragmento al archivo pom.xml:

```XML
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-sfmesh-maven-plugin</artifactId>
          <version>0.1.0</version>
          <configuration>
            ...
          </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

Lea la sección [Referencia de la CLI de Maven](service-fabric-mesh-reference-maven.md) para obtener más información sobre el uso.

## <a name="next-steps"></a>Pasos siguientes

También puede configurar su [entorno de desarrollo de Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

Busque respuestas a [preguntas y problemas comunes](service-fabric-mesh-faq.md).

[azure-cli-install]: /cli/azure/install-azure-cli
