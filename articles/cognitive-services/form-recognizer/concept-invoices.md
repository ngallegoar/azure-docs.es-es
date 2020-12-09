---
title: 'Facturas: Form Recognizer'
titleSuffix: Azure Cognitive Services
description: 'Conozca los conceptos relacionados con el análisis de facturas mediante Form Recognizer API: uso y límites.'
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 11/18/2020
ms.author: pafarley
ms.openlocfilehash: 9a3a6bd6489baea90ed4143b42a09e7d697bbc50
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2020
ms.locfileid: "96602451"
---
# <a name="form-recognizer-prebuilt-invoice-model"></a>Modelo de factura precompilado de Form Recognizer

Azure Form Recognizer puede analizar y extraer información de facturas de compra mediante sus modelos de factura precompilados. Invoice API permite a los clientes tomar facturas en varios formatos y devolver datos estructurados para automatizar el procesamiento de facturas. Combina nuestras eficaces funcionalidades de [reconocimiento óptico de caracteres (OCR)](../computer-vision/concept-recognizing-text.md) con modelos de aprendizaje profundo de reconocimiento de facturas para extraer información clave de facturas en inglés. Extrae el texto, las tablas y la información como el cliente, el proveedor, el identificador de la factura, la fecha de vencimiento de la factura, el total, el importe debido de la factura, el importe de los impuestos, la dirección de envío, la dirección de facturación, etc. La instancia precompilada de Invoice API está disponible públicamente en la versión preliminar de Form Recognizer v2.1.

## <a name="what-does-the-invoice-service-do"></a>¿Qué hace el servicio Invoice?

Invoice API extrae campos clave de las facturas y los devuelve en una respuesta JSON estructurada organizada. Las facturas pueden ser de varios formatos y tener distintos grados de calidad, lo que incluye imágenes capturadas por un teléfono, documentos digitalizados y archivos PDF digitales. Invoice API extraerá la salida estructurada de todas estas facturas. 

![Ejemplo de factura de Contoso](./media/invoice-example.jpg)

## <a name="try-it-out"></a>Prueba

Para probar el servicio Invoice de Form Recognizer, vaya a la herramienta de interfaz de usuario de ejemplo en línea:

> [!div class="nextstepaction"]
> [Uso de modelos precompilados](https://fott-preview.azurewebsites.net/)

Necesitará una suscripción de Azure ([cree una gratis](https://azure.microsoft.com/free/cognitive-services)) y un punto de conexión de [recursos de Form Recognizer](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer), así como la clave para probar el servicio Invoice de Form Recognizer. 

![Ejemplo de factura analizada](./media/analyze-invoice.png)


### <a name="input-requirements"></a>Requisitos de entrada 

[!INCLUDE [input reqs](./includes/input-requirements-receipts.md)]

## <a name="the-analyze-invoice-operation"></a>La operación Analyze Invoice

La operación [Analyze Invoice](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/5ed8c9843c2794cbb1a96291) toma una imagen o un archivo PDF de una factura como entrada y extrae los valores de interés. La llamada devuelve un campo de encabezado de respuesta denominado `Operation-Location`. El valor `Operation-Location` es una dirección URL que contiene el id. de resultado que se va a usar en el paso siguiente.

|Encabezado de respuesta| Dirección URL del resultado |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.1-preview.2/prebuilt/invoice/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="the-get-analyze-invoice-result-operation"></a>La operación Get Analyze Invoice Result

El segundo paso consiste en llamar a la operación [Get Analyze Invoice Result](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/5ed8c9acb78c40a2533aee83). Esta operación toma como entrada el id. de resultado que la operación Analyze Invoice ha creado. Devuelve una respuesta JSON que contiene un campo de **estado** con los siguientes valores posibles. Llamará a esta operación de forma iterativa hasta que se devuelva con el valor **correcto**. Use un intervalo de 3 a 5 segundos para evitar superar la tasa de solicitudes por segundo (RPS).

|Campo| Tipo | Valores posibles |
|:-----|:----:|:----|
|status | string | notStarted: la operación de análisis no se ha iniciado.<br /><br />running: la operación de análisis está en curso.<br /><br />failed: error en la operación de análisis.<br /><br />succeeded: la operación de análisis se realizó correctamente.|

Cuando el campo de **estado** tiene el valor **succeeded**, la respuesta de JSON incluirá los resultados del reconocimiento de la factura, las tablas extraídas y los resultados del reconocimiento de texto opcional, en caso de que se solicite. El resultado del reconocimiento de la factura se organiza como un diccionario de valores de campo con nombre, en el que cada valor contiene el texto extraído, el valor normalizado, el rectángulo delimitador, el nivel de confianza y los elementos de la palabra correspondiente. El resultado del reconocimiento de texto se organiza como una jerarquía de líneas y palabras, con texto, rectángulo delimitador e información de confianza.

### <a name="sample-json-output"></a>Salida de JSON de ejemplo

La respuesta a la operación Get Analyze Invoice Result será la representación estructurada de la factura con toda la información extraída. Aquí encontrará un [archivo con una factura de ejemplo](./media/sample-invoice.jpg) y su salida estructurada, una [salida de una factura de ejemplo](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-invoice-output.json).

La salida JSON tiene tres partes: 
* El nodo `"readResults"` contiene todo el texto reconocido y las marcas de selección. El texto se organiza por página, después, por líneas y, finalmente, por palabras individuales. 
* El nodo `"pageResults"` contiene las tablas y celdas extraídas con sus rectángulos delimitadores, su confianza y una referencia a las líneas y palabras de "readResults".
* El nodo `"documentResults"` contiene los valores específicos de la factura que el modelo ha detectado. Aquí es donde encontrará todos los campos de la factura, como el identificador de la factura, la dirección de envío, la de facturación, el cliente y el total, entre otros.

## <a name="example-output"></a>Salida de ejemplo

El servicio Invoice extraerá el texto, las tablas y los 26 campos de la factura. A continuación se muestran los campos extraídos de una factura en la respuesta de salida JSON (la siguiente salida usa esta [factura de ejemplo](./media/sample-invoice.jpg))  

|Nombre| Tipo | Descripción | Texto | Valor (salida estándar) |
|:-----|:----|:----|:----| :----|
| CustomerName | string | Cliente al que se va a facturar | Microsoft Corp |  |
| CustomerId | string | Identificador de referencia del cliente | CID-12345 |  |
| PurchaseOrder | string | Número de referencia del pedido | PO-3333 | |  |
| InvoiceId | string | Identificador de esta factura concreta (a menudo "Número de factura") | INV-100 | |  |
| FechaFactura | date | Fecha en que se generó la factura | 15/11/2019 | 
| DueDate | date | Fecha de vencimiento de esta factura | 15/12/2019 | 15-12-2019 | 15-11-2019 |
| VendorName | string | Proveedor que ha creado esta factura | CONTOSO LTD. | |
| VendorAddress | string | Dirección de correo del proveedor | 123 456th St New York, NY, 10001 | |
| VendorAddressRecipient | string | Nombre asociado a VendorAddress | Oficina central de Contoso | |
| CustomerAddress | string | Dirección de correo del cliente | 123 Other St, Redmond WA, 98052 | |
| CustomerAddressRecipient | string | Nombre asociado a CustomerAddress | Microsoft Corp | |
| BillingAddress | string | Dirección facturación del cliente | 123 Bill St, Redmond WA, 98052 | |
| BillingAddressRecipient | string | Nombre asociado a BillingAddress | Servicios Microsoft | |
| ShippingAddress | string | Dirección de envío explícita del cliente | 123 Ship St, Redmond WA, 98052 | |
| ShippingAddressRecipient | string | Nombre asociado a ShippingAddress | Microsoft Delivery | |
| SubTotal | number | Campo de subtotal identificado en esta factura | 100,00 USD | 100 | 
| TotalTax | number | Campo de total de impuestos identificado en esta factura | 10,00 USD | 10 |
| InvoiceTotal | number | Total de cargos nuevos asociados a esta factura | 110,00 USD | 110 |
| AmountDue |  number | Importe total debido al proveedor | 610,00 USD | 610 |
| ServiceAddress | string | Dirección de servicio o dirección de propiedad explícitas para el cliente | 123 Service St, Redmond WA, 98052 | |
| ServiceAddressRecipient | string | Nombre asociado a ServiceAddress | Servicios Microsoft | |
| RemittanceAddress | string | Dirección de remite o de pago explícitas del cliente | 123 Remit St New York, NY, 10001 |  |
| RemittanceAddressRecipient | string | Nombre asociado a RemittanceAddress | Facturación de Contoso |  |
| ServiceStartDate | date | Primera fecha del período de servicio (por ejemplo, un período de servicio de factura de la utilidad) | 14/10/2019 | 2019-10-14 |
| ServiceEndDate | date | Fecha de finalización del período de servicio (por ejemplo, un período de servicio de factura de la utilidad) | 14/11/2019 | 14-11-2019 |
| PreviousUnpaidBalance | number | Saldo explícito no pagado previamente | 500,00 USD | 500 |


## <a name="next-steps"></a>Pasos siguientes

- Pruebe sus propias facturas y ejemplos en la interfaz de usuario de ejemplo de [Form Recognizer](https://fott-preview.azurewebsites.net/).
- Lea el [inicio rápido de la biblioteca cliente de Form Recognizer](quickstarts/client-library.md) para empezar a escribir una aplicación de procesamiento de facturas con Form Recognizer en el lenguaje de su elección.
- O bien, siga el inicio rápido [Extracción de datos de facturas](./quickstarts/python-invoices.md) para implementar la extracción de datos de facturas con Python y la API REST.
## <a name="see-also"></a>Consulte también

* [¿Qué es Form Recognizer?](./overview.md)
* [Documentos de referencia de la API de REST](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-2/operations/5ed8c9843c2794cbb1a96291)
