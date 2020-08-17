---
title: 'Inicio rápido: Creación de una aplicación de Java en Azure App Service'
description: Implemente su primera aplicación Hola mundo de Java en Azure App Service en cuestión de minutos. El complemento Aplicación web de Azure para Maven facilita la implementación de aplicaciones Java.
keywords: azure, app service, web app, windows, linux, java, maven, quickstart
author: jasonfreeberg
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 08/01/2020
ms.author: jafreebe
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
zone_pivot_groups: app-service-platform-windows-linux
ms.openlocfilehash: 8289b21da5009459d2eb7ddc8d26b549f0920317
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2020
ms.locfileid: "88084346"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service"></a>Inicio rápido: Creación de una aplicación de Java en Azure App Service

[Azure App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático.  En este inicio rápido se muestra cómo usar la [CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) con el [complemento Aplicación web de Azure para Maven](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) para implementar un archivo web (WAR) de Java.

> [!NOTE]
> También se puede hacer lo mismo con IDE populares, como IntelliJ y Eclipse. Consulte nuestros documentos similares en [Inicio rápido de Azure Toolkit for IntelliJ](/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app) o [Inicio rápido de Azure Toolkit for Eclipse](/azure/developer/java/toolkit-for-eclipse/create-hello-world-web-app).
>
![Aplicación de ejemplo que se ejecuta en Azure App Service](./media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Creación de una aplicación Java

Ejecute el siguiente comando de Maven en el símbolo del sistema de Cloud Shell para crear una aplicación denominada `helloworld`:

```bash
mvn archetype:generate "-DgroupId=example.demo" "-DartifactId=helloworld" "-DarchetypeArtifactId=maven-archetype-webapp" "-Dversion=1.0-SNAPSHOT"
```

A continuación, cambie el directorio de trabajo a la carpeta del proyecto:

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>Configuración del complemento Maven

El proceso de implementación en Azure App Service puede elegir sus credenciales de Azure desde la CLI de Azure automáticamente. El complemento Maven iniciará sesión con OAuth o el inicio de sesión del dispositivo si la CLI de Azure no está instalada localmente. Consulte los detalles sobre la [autenticación con complementos de Maven](https://github.com/microsoft/azure-maven-plugins/wiki/Authentication) si fuera necesario.

Puede ejecutar el siguiente comando de Maven para configurar la implementación.
```bash
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```

::: zone pivot="platform-windows"  
Se le pedirá que seleccione 
* **SO (valor predeterminado: `linux`)**
* **Versión de Java (valor predeterminado: `1.8`)**
* **Contenedor Web (valor predeterminado: `tomcat 8.5`)** 

No olvide escribir **`2`** para elegir el sistema operativo **Windows** en el primer paso. En las restantes opciones se pueden dejar los valores predeterminados, para lo que se debe presionar **Entrar**. Por último, presione **`Y`** en el aviso **Confirmar (S/N)** para completar la configuración.

Así es un proceso de ejemplo:

```console
~@Azure:~/helloworld$ mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------------< example.demo:helloworld >-----------------------
[INFO] Building helloworld Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.9.1:config (default-cli) @ helloworld ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use: 2
Define value for javaVersion(Default: 1.8): 
1. 1.7
2. 1.7.0_191_ZULU
3. 1.7.0_51
4. 1.7.0_71
5. 1.7.0_80
6. 1.8 [*]
7. 1.8.0_102
8. 1.8.0_111
9. 1.8.0_144
10. 1.8.0_172
11. 1.8.0_172_ZULU
12. 1.8.0_181
13. 1.8.0_181_ZULU
14. 1.8.0_202
15. 1.8.0_202_ZULU
16. 1.8.0_25
17. 1.8.0_60
18. 1.8.0_73
19. 1.8.0_92
20. 11
21. 11.0.2_ZULU
Enter index to use:
Define value for webContainer(Default: tomcat 8.5): 
1. jetty 9.1
2. jetty 9.1.0.20131115
3. jetty 9.3
4. jetty 9.3.13.20161014
5. tomcat 7.0
6. tomcat 7.0.50
7. tomcat 7.0.62
8. tomcat 8.0
9. tomcat 8.0.23
10. tomcat 8.5 [*]
11. tomcat 8.5.20
12. tomcat 8.5.31
13. tomcat 8.5.34
14. tomcat 8.5.37
15. tomcat 8.5.6
16. tomcat 9.0
17. tomcat 9.0.0
18. tomcat 9.0.12
19. tomcat 9.0.14
20. tomcat 9.0.8
Enter index to use:
Please confirm webapp properties
AppName : helloworld-1590394316693
ResourceGroup : helloworld-1590394316693-rg
Region : westeurope
PricingTier : PremiumV2_P1v2
OS : Windows
Java : 1.8
WebContainer : tomcat 8.5
Deploy to slot : false
Confirm (Y/N)? :
[INFO] Saving configuration to pom.
```
::: zone-end
::: zone pivot="platform-linux"  
Así es un proceso de ejemplo:

```cmd
~@Azure:~/helloworld$ mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------------< example.demo:helloworld >-----------------------
[INFO] Building helloworld Maven Webapp 1.0-SNAPSHOT
[INFO] --------------------------------[ war ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.9.1:config (default-cli) @ helloworld ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: jre8):
1. Java 11
2. Java 8 [*]
Enter index to use:
Define value for runtimeStack(Default: TOMCAT 8.5):
1. TOMCAT 9.0
2. TOMCAT 8.5 [*]
Enter index to use:
Please confirm webapp properties
AppName : helloworld-1558400876966
ResourceGroup : helloworld-1558400876966-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : TOMCAT 8.5-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```
::: zone-end

> [!NOTE]
> En este artículo solo se trabaja con aplicaciones Java empaquetadas en archivos WAR. El complemento también admite aplicaciones web JAR, visite [Implementación de un archivo JAR de SE de Java en App Service en Linux](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) para probarlo.

Abra `pom.xml` para ver la configuración actualizada.

```bash
code pom.xml
```

Las configuraciones de App Service se pueden modificar directamente en el archivo POM si fuera necesario. A continuación se muestran algunas configuraciones comunes:

 Propiedad | Obligatorio | Descripción | Versión
---|---|---|---
`<schemaVersion>` | false | Especifique la versión del esquema de configuración. Los valores admitidos son: `v1`, `v2`. | 1.5.2
`<resourceGroup>` | true | Grupo de recursos de Azure para la aplicación web. | 0.1.0+
`<appName>` | true | El nombre de la aplicación web. | 0.1.0+
`<region>` | true | Especifica la región donde se hospedará la aplicación web. El valor predeterminado es **westeurope**. Todas las regiones válidas en la sección [Regiones admitidas](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+
`<pricingTier>` | false | El plan de tarifa de la aplicación web. El valor predeterminado es **P1V2**.| 0.1.0+
`<runtime>` | true | La configuración del entorno en tiempo de ejecución. Puede ver los detalles [aquí](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+
`<deployment>` | true | La configuración de implementación. Puede ver los detalles [aquí](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme). | 0.1.0+

::: zone pivot="platform-windows"
Tenga cuidado con los valores de `<appName>` y `<resourceGroup>`(`helloworld-1590394316693` y `helloworld-1590394316693-rg` en la demostración), ya que se usarán más adelante.
::: zone-end
::: zone pivot="platform-linux"
::: zone-end
> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/javae2e?tutorial=quickstart-java&step=config)

## <a name="deploy-the-app"></a>Implementar la aplicación

El proceso de implementación en Azure App Service usa las credenciales de cuenta desde la CLI de Azure. [Inicie sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) antes de continuar.

```azurecli
az login
```
Luego, puede implementar la aplicación Java en Azure mediante el siguiente comando:

```bash
mvn package azure-webapp:deploy
```

::: zone pivot="platform-windows"
Una vez finalizada la implementación, la aplicación estará lista en `http://<appName>.azurewebsites.net/`(`http://helloworld-1590394316693.azurewebsites.net` en la demostración). Abra la dirección URL con un explorador web local; debería ver

![Aplicación de ejemplo que se ejecuta en Azure App Service](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

**¡Enhorabuena!** Ha implementado su primera aplicación de Java en App Service en Windows.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]
::: zone-end
::: zone pivot="platform-linux"
Una vez que se haya completado la implementación, vaya a la aplicación implementada mediante la siguiente dirección URL en el explorador web, por ejemplo, `http://<webapp>.azurewebsites.net`. 

![Aplicación de ejemplo que se ejecuta en Azure App Service](media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**¡Enhorabuena!** Ha implementado su primera aplicación Java en App Service en Linux.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/javae2e?tutorial=app-service-linux-quickstart&step=deploy)

## <a name="clean-up-resources"></a>Limpieza de recursos

En los pasos anteriores, creó recursos de Azure en un grupo de recursos. Si prevé que no va a necesitar estos recursos en el futuro, elimine el grupo de recursos del portal, para lo que debe ejecutar el siguiente comando en Cloud Shell:

```azurecli-interactive
az group delete --name <your resource group name; for example: helloworld-1558400876966-rg> --yes
```

Este comando puede tardar varios segundos en ejecutarse.
::: zone-end

Una vez finalizada la implementación, la aplicación estará lista en `http://<appName>.azurewebsites.net/`(`http://helloworld-1590394316693.azurewebsites.net` en la demostración). Abra la dirección URL con un explorador web local; debería ver

![Aplicación de ejemplo que se ejecuta en Azure App Service](./media/quickstart-java/java-hello-world-in-browser-azure-app-service.png)

**¡Enhorabuena!** Ha implementado su primera aplicación Java en App Service.

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/javae2e?quickstart-java&step=deploy)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Conexión a Azure SQL Database con Java](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Conexión a Azure DB para MySQL con Java](/azure/mysql/connect-java)

> [!div class="nextstepaction"]
> [Conexión a Azure DB para PostgreSQL con Java](/azure/postgresql/connect-java)

> [!div class="nextstepaction"]
> [Recursos de Azure para desarrolladores de Java](/java/azure/)

> [!div class="nextstepaction"]
> [Configuración de una aplicación Java](configure-language-java.md)

> [!div class="nextstepaction"]
> [CI/CD con Jenkins](/azure/jenkins/deploy-jenkins-app-service-plugin)

> [!div class="nextstepaction"]
> [Asignación de un dominio personalizado](app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Más información sobre los complementos de Maven para Azure](https://github.com/microsoft/azure-maven-plugins)