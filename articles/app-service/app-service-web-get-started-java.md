---
title: 'Inicio rápido: Creación de una aplicación Java en Windows'
description: Implementación de su primera aplicación Hola mundo de Java en Azure App Service en Windows en cuestión de minutos. El complemento Aplicación web de Azure para Maven facilita la implementación de aplicaciones Java.
keywords: azure, app service, web app, windows, java, maven, quickstart
author: msangapu-msft
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.devlang: Java
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: jafreebe
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: b07ffe92a5dd0c105188fab55bc679c04f660ed2
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300967"
---
# <a name="quickstart-create-a-java-app-on-azure-app-service-on-windows"></a>Inicio rápido: Creación de una aplicación de Java en Azure App Service en Windows

> [!NOTE]
> En este artículo se implementa una aplicación en App Service en Windows. Para implementarla en App Service en _Linux_, consulte [Creación de una aplicación web de java en Linux](./containers/quickstart-java.md).
>

[Azure App Service](overview.md) proporciona un servicio de hospedaje web muy escalable y con aplicación de revisiones de un modo automático.  En este inicio rápido se muestra cómo usar la [CLI de Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) con el [complemento Aplicación web de Azure para Maven](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) para implementar un archivo web (WAR) de Java.

> [!NOTE]
> También se puede hacer lo mismo con IDE populares, como IntelliJ y Eclipse. Consulte nuestros documentos similares en [Inicio rápido de Azure Toolkit for IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app) o [Inicio rápido de Azure Toolkit for Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app).
>
![Aplicación de ejemplo que se ejecuta en Azure App Service](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Creación de una aplicación Java

Ejecute el siguiente comando de Maven en el símbolo del sistema de Cloud Shell para crear una aplicación denominada `helloworld`:

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp -Dversion=1.0-SNAPSHOT
```

A continuación, cambie el directorio de trabajo a la carpeta del proyecto:

```bash
cd helloworld
```

## <a name="configure-the-maven-plugin"></a>Configuración del complemento Maven

Puede ejecutar el siguiente comando maven en el símbolo del sistema para configurar la implementación, elija **"2"** para el sistema operativo **Windows** en el primer paso y acepte las configuraciones predeterminadas presionando **ENTRAR** hasta que aparezca el mensaje **Confirmar (S/N)** y presione la tecla **S"** ya la configuración habrá terminado. 

```bash
mvn com.microsoft.azure:azure-webapp-maven-plugin:1.9.1:config
```

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

> [!div class="nextstepaction"]
> [He tenido un problema](https://www.research.net/r/javae2e?tutorial=app-service-web-get-started-java&step=config)

## <a name="deploy-the-app"></a>Implementar la aplicación

El proceso de implementación en Azure App Service usa las credenciales de cuenta desde la CLI de Azure. [Inicie sesión con la CLI de Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) antes de continuar.

```azurecli
az login
```
Luego, puede implementar la aplicación Java en Azure mediante el siguiente comando:

```bash
mvn package azure-webapp:deploy
```

Una vez que se haya completado la implementación, vaya a la aplicación implementada mediante la siguiente dirección URL en el explorador web, por ejemplo, `http://<webapp>.azurewebsites.net/`.

![Aplicación de ejemplo que se ejecuta en Azure App Service](./media/app-service-web-get-started-java/java-hello-world-in-browser-azure-app-service.png)

**¡Enhorabuena!** Ha implementado su primera aplicación de Java en App Service en Windows.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Conexión a una base de datos de Azure SQL con Java](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Conexión a Azure DB para MySQL con Java](/azure/mysql/connect-java)

> [!div class="nextstepaction"]
> [Conexión a Azure DB para PostgreSQL con Java](/azure/postgresql/connect-java)

> [!div class="nextstepaction"]
> [Recursos de Azure para desarrolladores de Java](/java/azure/)

> [!div class="nextstepaction"]
> [Asignación de un dominio personalizado](app-service-web-tutorial-custom-domain.md)

> [!div class="nextstepaction"]
> [Más información sobre los complementos de Maven para Azure](https://github.com/microsoft/azure-maven-plugins)
