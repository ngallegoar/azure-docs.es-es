---
title: Integración de modelos estándar del sector
titleSuffix: Azure Digital Twins
description: Descripción de cómo se integran modelos los estándar del sector en DTDL para Azure Digital Twins, ya sea mediante el uso de ontologías de DTDL especiales o mediante la conversión de ontologías existentes
author: baanders
ms.author: baanders
ms.date: 11/04/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: f5bfe128ddc04e8048bb89a8e39035434dfd2b92
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96352888"
---
# <a name="integrate-industry-standard-models-with-dtdl-for-azure-digital-twins"></a>Integración de modelos estándar del sector con DTDL para Azure Digital Twins

El uso de modelos que se basan en estándares del sector o que usan una representación estándar de la ontología, como RDF u OWL, proporciona un excelente punto de partida para diseñar modelos de Azure Digital Twins. Asimismo, el uso de modelos del sector también contribuye a la estandarización y el uso compartido de la información.

Para usarse con Azure Digital Twins, los modelos se deben representar mediante el [**lenguaje de definición de Digital Twins (DTDL)**](concepts-models.md), basado en JSON-LD. Por consiguiente, en este artículo se describe cómo representar los modelos estándar del sector en DTDL mediante la integración de los conceptos existentes del sector con la semántica de DTDL para que Azure Digital Twins pueda usarlos. El modelo de DTDL sirve como origen confiable del modelo en Azure Digital Twins.

Hay tres rutas posibles para integrar modelos estándar del sector con DTDL:
* **Adoptar**: puede iniciar la solución con una ontología de DTDL de código abierto que se ha creado a partir de estándares del sector profusamente adoptados. 
* **Convertir:** si ya tiene modelos existentes, deberá convertirlos al lenguaje DTDL.
* **Crear**: siempre puede desarrollar sus propios modelos de DTDL personalizados desde cero, como se describe en [ *Administración de modelos personalizados*](how-to-manage-model.md).

## <a name="adopt-an-open-source-dtdl-ontology"></a>Adopción de una ontología de DTDL de código abierto

A menudo es más fácil empezar con una ontología de DTDL de código abierto que desde una página en blanco. 

Por ejemplo, las soluciones de edificios inteligentes pueden aprovechar la [ontología RealEstateCore basada en DTDL **de código abierto**](https://github.com/Azure/opendigitaltwins-building), que proporciona un terreno común para el modelado de edificios inteligentes, al tiempo que se aprovechan los estándares del sector para evitar la reinvención. 

Estas ontologías de DTDL de código abierto también proporcionan procedimientos recomendados para consumir y extender correctamente los modelos. 

## <a name="convert-existing-models-to-dtdl"></a>Conversión de los modelos existentes a DTDL

La mayoría de los modelos del sector (también denominados **ontologías**) se basan en estándares web semánticos como [OWL](https://www.w3.org/OWL/), [RDF](https://www.w3.org/2001/sw/wiki/RDF) y [RDFS](https://www.w3.org/2001/sw/wiki/RDFS). 

Para usar un modelo con Azure Digital Twins, debe tener el formato DTDL. En esta sección se describen las instrucciones de diseño generales en forma de **patrón de conversión** para convertir modelos basados en RDF al lenguaje DTDL, con el fin de que se puedan usar con Azure Digital Twins. 

También contiene [ **código de ejemplo** para un convertidor de RDF](#sample-converter-application), que se ha validado para el esquema de [Brick](https://brickschema.org/ontology/) y se puede extender a otros esquemas del sector de la construcción.

### <a name="conversion-pattern"></a>Patrón de conversión

Hay varias bibliotecas de terceros que se pueden usar al convertir modelos basados en RDF a DTDL. Algunas de estas bibliotecas le permiten cargar el archivo del modelo en un grafo. Puede recorrer el grafo en bucle en busca de construcciones específicas de RDFS y OWL, y realizar la conversión a DTDL.   

La tabla siguiente es un ejemplo de cómo se pueden asignar las construcciones RDFS y OWL a DTDL. 

| Concepto RDFS/OWL | Construcción RDFS/OWL | Concepto DTDL | Construcción DTDL |
| --- | --- | --- | --- |
| Clases | `owl:Class`<br>Sufijo IRI<br>``rdfs:label``<br>``rdfs:comment`` | Interfaz | `@type:Interface`<br>`@id`<br>`displayName`<br>`comment` 
| Subclases  | `owl:Class`<br>Sufijo IRI<br>`rdfs:label`<br>`rdfs:comment`<br>`rdfs:subClassOf` | Interfaz | `@type:Interface`<br>`@id`<br>`displayName`<br>`comment`<br>`extends` 
| Propiedades de tipos de datos | `owl:DatatypeProperty`<br>`rdfs:label` o `INode`<br>`rdfs:label`<br>`rdfs:range` | Propiedades de interfaz | `@type:Property`<br>`name`<br>`displayName`<br>`schema` 
| Propiedades de objeto | `owl:ObjectProperty`<br>`rdfs:label` o `INode`<br>`rdfs:range`<br>`rdfs:comment`<br>`rdfs:label` | Relación | `type:Relationship`<br>`name`<br>`target` (o se omite si no hay `rdfs:range`)<br>`comment`<br>`displayName`<br>

En el siguiente fragmento de código en C# se muestra cómo se carga un archivo de modelo de RDF en un grafo y se convierte a DTDL, mediante la biblioteca [**dotNetRDF**](https://www.dotnetrdf.org/). 

```csharp
using VDS.RDF.Ontology; 
using VDS.RDF.Parsing; 
using Microsoft.Azure.DigitalTwins.Parser; 

//...

Console.WriteLine("Reading file..."); 

FileLoader.Load(_ontologyGraph, rdfFile.FullName); 

// Start looping through for each owl:Class 
foreach (OntologyClass owlClass in _ontologyGraph.OwlClasses) 
{ 

    // Generate a DTMI for the owl:Class 
    string Id = GenerateDTMI(owlClass); 

    if (!String.IsNullOrEmpty(Id)) 
    { 

        Console.WriteLine($"{owlClass.ToString()} -> {Id}"); 

        // Create Interface
        DtdlInterface dtdlInterface = new DtdlInterface 
        { 
            Id = Id, 
            Type = "Interface", 
            DisplayName = GetInterfaceDisplayName(owlClass), 
            Comment = GetInterfaceComment(owlClass), 
            Contents = new List<DtdlContents>() 
        }; 

        // Use DTDL 'extends' for super classes 
        IEnumerable<OntologyClass> foundSuperClasses = owlClass.DirectSuperClasses; 

        //... 
     }

     // Add interface to the list of interfaces 
     _interfaceList.Add(dtdlInterface); 
} 

// Serialize to JSON 
var json = JsonConvert.SerializeObject(_interfaceList); 

//...
``` 

### <a name="sample-converter-application"></a>Aplicación de conversión de ejemplo 

Hay una aplicación de ejemplo disponible que convierte un archivo de modelo basado en RDF a [DTDL (versión 2)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) para su uso por parte del servicio Azure Digital Twins. Se ha validado para el esquema de [Brick](https://brickschema.org/ontology/) y se puede extender a otros esquemas del sector de la construcción (como [Building Topology Ontology (BOT)](https://w3c-lbd-cg.github.io/bot/), [Semantic Sensor Network](https://www.w3.org/TR/vocab-ssn/) o [buildingSmart Industry Foundation Classes (IFC)](https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/)).

El ejemplo es una aplicación de línea de comandos de .NET Core denominada **RdfToDtdlConverter**.

Aquí puede obtener el ejemplo: [**RdfToDtdlConverter**](/samples/azure-samples/rdftodtdlconverter/digital-twins-model-conversion-samples/). 

Para descargar el código en la máquina, pulse el botón *Descargar archivo ZIP* que se encuentra debajo del título en la página de aterrizaje del ejemplo. Se descargará un archivo *ZIP* de nombre *RdfToDtdlConverter_sample_application_to_convert_RDF_to_DTDL.zip*, que podrá descomprimir y explorar.

Puede usar este ejemplo para ver los patrones de conversión en contexto y tener como bloque de creación de sus propias aplicaciones la realización de conversiones de modelos de acuerdo con sus propias necesidades específicas.

## <a name="validate-and-upload-dtdl-models"></a>Validación y carga de modelos en DTDL

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

Una vez que un modelo se convierte y valida, puede **cargarlo en la instancia de Azure Digital Twins**. Para obtener más información sobre este proceso, consulte la sección [*Carga de modelos*](how-to-manage-model.md#upload-models) de *Procedimiento: Administración de modelos personalizados*.

## <a name="next-steps"></a>Pasos siguientes 

Más información sobre el diseño y la administración de modelos de gemelos digitales:
 
* [*Conceptos: Modelos personalizados*](concepts-models.md)
* [*Procedimiento: Administración de modelos personalizados*](how-to-manage-model.md)
* [*Procedimiento: Análisis y validación de modelos*](how-to-parse-models.md)