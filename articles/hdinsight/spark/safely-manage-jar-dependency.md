---
title: Administración segura de dependencias de JAR en Azure HDInsight
description: En este artículo se describen los procedimientos recomendados para administrar las dependencias de los archivos Java (JAR) en las aplicaciones de HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 02/05/2020
ms.openlocfilehash: 5834d3512c95e77e6ce45472ff8f2e1b4b2ed456
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92545589"
---
# <a name="safely-manage-jar-dependencies"></a>Administración segura de dependencias de JAR

Los componentes instalados en los clústeres de HDInsight tienen dependencias de bibliotecas de terceros. Normalmente, estos componentes integrados hacen referencia a una versión específica de módulos comunes como Guava. Cuando se envía una aplicación con sus dependencias, puede producirse un conflicto entre las distintas versiones del mismo módulo. Si la versión del componente a la que se hace referencia está en primer lugar en la ruta de clases, los componentes integrados pueden producir excepciones debido a la incompatibilidad de versiones. Sin embargo, si los componentes integrados insertan sus dependencias primero en la ruta de clases, la aplicación puede producir errores como `NoSuchMethod`.

Para evitar conflictos de versiones, considere la posibilidad de sombrear las dependencias de la aplicación.

## <a name="what-does-package-shading-mean"></a>¿Qué significa sombrear los paquetes?
El sombreado permite incluir y cambiar el nombre de las dependencias. Reubica las clases y reescribe el código de bytes y los recursos afectados para crear una copia privada de las dependencias.

## <a name="how-to-shade-a-package"></a>Cómo sombrear un paquete

### <a name="use-uber-jar"></a>Uso de Uber-Jar
Uber-Jar es un único archivo jar que contiene el código jar de la aplicación y sus dependencias. De forma predeterminada, las dependencias de uber-jar no se sombrean. En algunos casos, esto puede producir un conflicto de versión si otros componentes o aplicaciones hacen referencia a una versión diferente de esas bibliotecas. Para evitarlo, puede crear un archivo Uber-Jar con algunas (o todas) las dependencias sombreadas.

### <a name="shade-package-using-maven"></a>Sombreado de paquetes con Maven
Maven puede compilar aplicaciones escritas tanto en Java como en Scala. Maven-shade-plugin puede ayudar a crear fácilmente un archivo Uber-Jar sombreado.

En el ejemplo siguiente se muestra un archivo `pom.xml` que se ha actualizado para sombrear un paquete mediante maven-shade-plugin.  La sección XML `<relocation>…</relocation>` mueve las clases del paquete `com.google.guava` al paquete `com.google.shaded.guava`, mueve las entradas del archivo JAR correspondientes y reescribe el código de bytes afectado.

Después de cambiar `pom.xml`, puede ejecutar `mvn package` para compilar el archivo Uber-Jar sombreado.

```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.1</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
            <configuration>
              <relocations>
                <relocation>
                  <pattern>com.google.guava</pattern>
                  <shadedPattern>com.google.shaded.guava</shadedPattern>
                </relocation>
              </relocations>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```

### <a name="shade-package-using-sbt"></a>Sombreado de paquetes con SBT
SBT también es una herramienta de compilación para Scala y Java. SBT no tiene un complemento de sombreado como maven-shade-plugin. Puede modificar el archivo `build.sbt` para sombrear paquetes. 

Por ejemplo, para sombrear `com.google.guava`, puede agregar el comando siguiente al archivo `build.sbt`:

```scala
assemblyShadeRules in assembly := Seq(
  ShadeRule.rename("com.google.guava" -> "com.google.shaded.guava.@1").inAll
)
```

Después, puede ejecutar `sbt clean` y `sbt assembly` para compilar el archivo Jar sombreado. 

## <a name="next-steps"></a>Pasos siguientes

* [Uso de las herramientas de HDInsight para IntelliJ](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)

* [Creación de una aplicación de Scala Maven para Spark en IntelliJ](./apache-spark-create-standalone-application.md)