---
title: Conversión de modelos estándar del sector
titleSuffix: Azure Digital Twins
description: Información sobre el patrón de conversión de modelos estándar del sector (RDF/OWL) a DTDL
author: baanders
ms.author: baanders
ms.date: 10/28/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: b9e298e55f06501bbbb4271f0643012c3cb6c5fe
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "92918337"
---
# <a name="convert-industry-standard-models-to-dtdl-for-azure-digital-twins"></a>Conversión de modelos estándar del sector a DTDL con Azure Digital Twins

Los modelos de Azure Digital Twins se representan mediante el [**lenguaje de definición de Digital Twins (DTDL)**](concepts-models.md), que se basa en JSON-LD. Si tiene modelos existentes fuera de Azure Digital Twins que se basen en un estándar del sector, como OWL o RDF, deberá **convertirlos a DTDL** para usarlos con Azure Digital Twins. La versión en DTDL se convertirá en el origen confiable del modelo en Azure Digital Twins.

En este artículo se describe un patrón de conversión de modelos del sector o personalizados basados en RDF a DTDL. Incluye:
* **Instrucciones de nivel de estrategia** que se pueden aplicar a diversos estándares y tipos de modelos
* [**Código de ejemplo** para un convertidor específico de RDF](#sample-converter-application)

## <a name="industry-models"></a>Modelos del sector  

El uso de modelos del sector ofrece un punto de partida completo al diseñar un modelo de Azure Digital Twins. Asimismo, el uso de modelos del sector también contribuye a la estandarización y el uso compartido de la información. 

Algunos modelos comunes del sector son los siguientes:  

| Segmento de mercado vertical | Modelo |
| --- | --- | 
| Administración de edificios e instalaciones | [RealEstateCore](https://www.realestatecore.io/)<br>[BRICK Schema](https://brickschema.org/ontology/1.1/)<br>[Building Topology Ontology (BOT)](https://w3c-lbd-cg.github.io/bot/)<br>[Semantic Sensor Network](https://www.w3.org/TR/vocab-ssn/)<br>[buildingSmart Industry Foundation Classes (IFC)](https://technical.buildingsmart.org/standards/ifc/ifc-schema-specifications/) |
| Ciudades inteligentes | [ETSI NGSI-LD](https://www.etsi.org/deliver/etsi_gr/CIM/001_099/008/01.01.01_60/gr_CIM008v010101p.pdf)<br>[Smart Applications REFerence (SAREF)](https://saref.etsi.org/)<br>[Fiware](https://www.fiware.org/)<br>[Open & Agile Smart Cities (OASC)](https://oascities.org/) |
| Red eléctrica | [CIM](https://cimug.ucaiug.org/)/[IEC 61968](https://en.wikipedia.org/wiki/IEC_61968)<br>[Modelos ADRM](http://www.adrm.com/) para el comercio energético y de mercancías |
| Automoción | [Especificación de la señal del vehículo](https://github.com/GENIVI/vehicle_signal_specification/tree/master/spec) |

En función de sus necesidades, también puede usar DTDL para personalizar o ampliar modelos del sector, o bien desarrollar su propio modelo personalizado desde cero. 

## <a name="create-and-edit-models"></a>Creación y edición de modelos

El primer paso del modelado es crear los modelos. Puede crear y completar la edición de los modelos estándar del sector antes de convertirlos a DTDL, o bien convertirlos a DTDL al empezar y seguir editándolos como documentos en DTDL.

### <a name="with-industry-standards"></a>Con estándares del sector

La mayoría de los modelos del sector (también denominados **ontologías** ) se basan en estándares web semánticos como [OWL](https://www.w3.org/OWL/[), [RDF](https://www.w3.org/2001/sw/wiki/RDF) y [RDFS](https://www.w3.org/2001/sw/wiki/RDFS). 

En algunos casos, puede que quiera crear o editar un modelo mediante herramientas de modelos que se basen en OWL. A la hora de diseñar un modelo basado en OWL, puede usar todas las herramientas de creación de modelos que quiera, incluidas las siguientes.
* [WebProtégé](https://protege.stanford.edu/products.php#web-protege) y [Protégé Desktop](https://protege.stanford.edu/products.php#desktop-protege) son ejemplos populares. Puede importar modelos del sector en varios formatos, editar o ampliar un modelo, y exportarlo. 
* [WebVOWL](http://www.visualdataweb.de/webvowl/) es otra herramienta popular para visualizar modelos. 

Si crea un modelo con un estándar del sector que no sea DTDL, tendrá que convertirlo a DTDL y cargarlo en Azure Digital Twins. 

#### <a name="common-model-file-formats"></a>Formatos de archivo de los modelos comunes 

RDF, OWL y RDFS son los bloques de creación básicos de la Web semántica. 

**RDF** proporciona una estructura conceptual para describir cosas, en forma de **tripletas**. Las tripletas constan de tres partes: **sujeto** , **predicado** y **objeto**. Los objetos se pueden crear a partir de los identificadores URI. 

Estos son algunos ejemplos de tripletas de RDF:

```
<LogicalDevice> <hasCapabiity> <Temperature>
<Chiller> <locatedIn> <Level1>
<Asset> <isPartOf> <Asset>  
```

Estos ejemplos son todos tripletas de RDF válidas, pero la última instrucción no es válida semánticamente. En esta situación es donde la especificación OWL entra en escena. **OWL** define lo que se puede escribir con RDF para tener una ontología válida.

Este es un ejemplo de uso de OWL: 

```
<Asset> <isPartOf> <Building>
<TemperatureSensor> <isType> <Sensor>
```

**RDFS** proporciona una semántica de vocabulario adicional que le ayuda a definir y describir las clases. Por ejemplo, una clase de este tipo es `rdfs:subClassOf`:

```
<Equipment> <subClassOf> <Asset>
```

Los modelos se pueden guardar, importar y exportar en muchos formatos de archivo, entre los que se incluyen los siguientes:  
* RDF/XML 
* Turtle (TTL) 
* OWL/XML 

### <a name="with-dtdl"></a>Con DTDL

Azure Digital Twins usa el  **lenguaje de definición de Digital Twins (DTDL)** , que se basa en JSON-LD, para la creación de modelos. Todo modelo que se vaya a usar con Azure Digital Twins deberá escribirse originalmente en DTDL o convertirse a este lenguaje.

Al trabajar con modelos en DTDL, puede usar la [**extensión de DTDL**](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-dtdl) disponible para  [Visual Studio Code](https://code.visualstudio.com/), que proporciona validación de la sintaxis y otras características para facilitar la escritura de modelos en DTDL.

Puede obtener más información sobre los modelos de Azure Digital Twins y sus componentes en [*Conceptos: Modelos personalizados*](concepts-models.md) y [*Procedimiento: Administración de modelos personalizados*](how-to-manage-model.md).

## <a name="convert-models-to-dtdl"></a>Conversión de modelos a DTDL

Para usar un modelo con Azure Digital Twins, debe tener el formato DTDL. En esta sección se explica cómo convertir modelos basados en RDF a DTDL para que se puedan usar con Azure Digital Twins.

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

## <a name="validate-and-upload-dtdl-models"></a>Validación y carga de modelos en DTDL

[!INCLUDE [Azure Digital Twins: validate models info](../../includes/digital-twins-validate.md)]

Una vez que un modelo se convierte y valida, puede **cargarlo en la instancia de Azure Digital Twins**. Para obtener más información sobre este proceso, consulte la sección [*Carga de modelos*](how-to-manage-model.md#upload-models) de *Procedimiento: Administración de modelos personalizados*.

## <a name="sample-converter-application"></a>Aplicación de conversión de ejemplo 

Hay una aplicación de ejemplo disponible que convierte un archivo de modelo basado en RDF a [DTDL (versión 2)](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) para su uso por parte del servicio Azure Digital Twins. El ejemplo es una aplicación de línea de comandos de .NET Core denominada **RdfToDtdlConverter**.

Aquí puede obtener el ejemplo: [**RdfToDtdlConverter**](https://docs.microsoft.com/samples/azure-samples/rdftodtdlconverter/digital-twins-model-conversion-samples/). 

Para descargar el código en la máquina, pulse el botón *Descargar archivo ZIP* que se encuentra debajo del título en la página de aterrizaje del ejemplo. Se descargará un archivo *ZIP* de nombre *RdfToDtdlConverter_sample_application_to_convert_RDF_to_DTDL.zip* , que podrá descomprimir y explorar.

Puede usar este ejemplo para ver los patrones de conversión en contexto y tener como bloque de creación de sus propias aplicaciones la realización de conversiones de modelos de acuerdo con sus propias necesidades específicas.

## <a name="next-steps"></a>Pasos siguientes 

Más información sobre el diseño y la administración de modelos de gemelos digitales:
 
* [*Conceptos: Modelos personalizados*](concepts-models.md)
* [*Procedimiento: Administración de modelos personalizados*](how-to-manage-model.md)
* [*Procedimiento: Análisis y validación de modelos*](how-to-parse-models.md)