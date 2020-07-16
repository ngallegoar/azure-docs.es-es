---
title: Referencia del SDK del Lector inmersivo
titleSuffix: Azure Cognitive Services
description: El SDK del Lector inmersivo contiene una biblioteca de JavaScript que permite integrar dicho lector en la aplicación.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 6dfcd8d56232f893f881f310b33f3f849e2364a7
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475958"
---
# <a name="immersive-reader-javascript-sdk-reference-v11"></a>Referencia del SDK de JavaScript de Lector inmersivo (v 1.1)

El SDK del Lector inmersivo contiene una biblioteca de JavaScript que permite integrar dicho lector en la aplicación.

## <a name="functions"></a>Functions

El SDK expone las funciones:

- [`ImmersiveReader.launchAsync(token, subdomain, content, options)`](#launchasync)

- [`ImmersiveReader.close()`](#close)

- [`ImmersiveReader.renderButtons(options)`](#renderbuttons)

## <a name="launchasync"></a>launchAsync

Inicia el Lector inmersivo dentro de un `iframe` en la aplicación web. Tenga en cuenta que el tamaño del contenido está limitado a un máximo de 50 MB.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<LaunchResponse>;
```

### <a name="parameters"></a>Parámetros

| Nombre | Tipo | Descripción |
| ---- | ---- |------------ |
| `token` | string | Token de autenticación de Azure AD |
| `subdomain` | string | El subdominio personalizado del recurso Lector inmersivo en Azure. |
| `content` | [Contenido](#content) | Un objeto que contiene el contenido que se mostrará en el Lector inmersivo. |
| `options` | [Opciones](#options) | Opciones para configurar ciertos comportamientos del Lector inmersivo. Opcional. |

### <a name="returns"></a>Devuelve

Devuelve `Promise<LaunchResponse>`, que se resuelve cuando el lector inmersivo se carga. El objeto `Promise` se resuelve en un objeto [`LaunchResponse`](#launchresponse).

### <a name="exceptions"></a>Excepciones

El valor devuelto de `Promise` se rechazará con un objeto [`Error`](#error) si el Lector inmersivo no se puede cargar. Para más información, consulte los [códigos de error](#error-codes).

## <a name="close"></a>close

Cierra el lector inmersivo.

Un caso de uso de ejemplo de esta función se produce si el botón para salir se oculta al establecer ```hideExitButton: true``` en [options](#options). Después, un botón distinto (por ejemplo, la flecha atrás de un encabezado para dispositivos móviles) puede llamar a esta función ```close``` cuando se hace clic en ella.

```typescript
close(): void;
```

## <a name="renderbuttons"></a>renderButtons

Esta función crea los estilos y actualiza los elementos del botón de lector inmersivo del documento. Si se proporciona ```options.elements```, esta función representará los botones dentro de ```options.elements```. De lo contrario, los botones se representarán dentro de los elementos del documento que tengan la clase ```immersive-reader-button```.

Esta función es invocada automáticamente por el SDK cuando se cierra la aplicación.

Consulte [Atributos opcionales](#optional-attributes) para más opciones de representación.

```typescript
renderButtons(options?: RenderButtonsOptions): void;
```

### <a name="parameters"></a>Parámetros

| Nombre | Tipo | Descripción |
| ---- | ---- |------------ |
| `options` | [RenderButtonsOptions](#renderbuttonsoptions) | Opciones para configurar ciertos comportamientos de la función renderButtons. Opcional. |

## <a name="types"></a>Tipos

### <a name="content"></a>Contenido

Contiene el contenido que se mostrará en el Lector inmersivo.

```typescript
{
    title?: string;    // Title text shown at the top of the Immersive Reader (optional)
    chunks: Chunk[];   // Array of chunks
}
```

### <a name="chunk"></a>Fragmento

Un único fragmento de datos, que se pasará al contenido del lector inmersivo.

```typescript
{
    content: string;        // Plain text string
    lang?: string;          // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
    mimeType?: string;      // MIME type of the content (optional). Currently 'text/plain', 'application/mathml+xml', and 'text/html' are supported. Defaults to 'text/plain' if not specified.
}
```

#### <a name="supported-mime-types"></a>Tipos MIME compatibles

| Tipo MIME | Descripción |
| --------- | ----------- |
| text/plain | Texto sin formato. |
| text/html | Contenido HTML. [Más información](#html-support)|
| application/mathml+xml | Lenguaje de marcado matemático (MathML). [Más información](./how-to/display-math.md).
| application/vnd.openxmlformats-officedocument.wordprocessingml.document | Documento de Microsoft Word con formato .docx.

### <a name="options"></a>Opciones

Contiene propiedades que configuran ciertos comportamientos del Lector inmersivo.

```typescript
{
    uiLang?: string;           // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;          // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number;         // Z-index of the iframe that will be created (default is 1000).
    useWebview?: boolean;      // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
    onExit?: () => any;        // Executes when the Immersive Reader exits.
    customDomain?: string;     // Reserved for internal use. Custom domain where the Immersive Reader webapp is hosted (default is null).
    allowFullscreen?: boolean; // The ability to toggle fullscreen (default is true).
    hideExitButton?: boolean;  // Whether or not to hide the Immersive Reader's exit button arrow (default is false). This should only be true if there is an alternative mechanism provided to exit the Immersive Reader (e.g a mobile toolbar's back arrow).
    cookiePolicy?: CookiePolicy; // Setting for the Immersive Reader's cookie usage (default is CookiePolicy.Disable). It's the responsibility of the host application to obtain any necessary user consent in accordance with EU Cookie Compliance Policy.
    disableFirstRun?: boolean; // Disable the first run experience.
    readAloudOptions?: ReadAloudOptions; // Options to configure Read Aloud.
    translationOptions?: TranslationOptions; // Options to configure translation.
    displayOptions?: DisplayOptions; // Options to configure text size, font, etc.
    preferences?: string; // String returned from onPreferencesChanged representing the user's preferences in the Immersive Reader.
    onPreferencesChanged?: (value: string) => any; // Executes when the user's preferences have changed.
}
```

```typescript
enum CookiePolicy { Disable, Enable }
```

```typescript
type ReadAloudOptions = {
    voice?: string;      // Voice, either 'male' or 'female'. Note that not all languages support both genders.
    speed?: number;      // Playback speed, must be between 0.5 and 2.5, inclusive.
    autoplay?: boolean;  // Automatically start Read Aloud when the Immersive Reader loads.
};
```

> [!NOTE]
> Debido a las limitaciones del explorador, la reproducción automática no se admite en Safari.

```typescript
type TranslationOptions = {
    language: string;                         // Set the translation language, e.g. fr-FR, es-MX, zh-Hans-CN. Required to automatically enable word or document translation.
    autoEnableDocumentTranslation?: boolean;  // Automatically translate the entire document.
    autoEnableWordTranslation?: boolean;      // Automatically enable word translation.
};
```

```typescript
type DisplayOptions = {
    textSize?: number;          // Valid values are 14, 20, 28, 36, 42, 48, 56, 64, 72, 84, 96.
    increaseSpacing?: boolean;  // Set whether increased spacing is enabled.
    fontFamily?: string;        // Valid values are 'Calibri', 'ComicSans', and 'Sitka'.
};
```

### <a name="launchresponse"></a>LaunchResponse

Contiene la respuesta de la llamada a `ImmersiveReader.launchAsync`. Tenga en cuenta que se puede acceder a una referencia al elemento `iframe` que contiene el Lector inmersivo a través de `container.firstChild`.

```typescript
{
    container: HTMLDivElement;    // HTML element which contains the Immersive Reader iframe
    sessionId: string;            // Globally unique identifier for this session, used for debugging
}
```
 
### <a name="error"></a>Error

Contiene información sobre el error.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Códigos de error

| Código | Descripción |
| ---- | ----------- |
| BadArgument | El argumento proporcionado no es válido; consulte `message` para detalles. |
| Tiempo de espera | El Lector inmersivo no se pudo cargar en el tiempo de espera especificado. |
| TokenExpired | El token suministrado en caché ha expirado. |
| Limitado | Se ha superado el límite de frecuencia de llamadas. |

### <a name="renderbuttonsoptions"></a>RenderButtonsOptions

Opciones para representar los botones del lector inmersivo.

```typescript
{
    elements: HTMLDivElement[];    // Elements to render the Immersive Reader buttons in
}
```

## <a name="launching-the-immersive-reader"></a>Inicio del Lector inmersivo

El SDK proporciona el estilo predeterminado para el botón para iniciar el Lector inmersivo. Use el atributo de clase `immersive-reader-button` para habilitar este estilo. Consulte [este artículo](./how-to-customize-launch-button.md) para obtener más detalles.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>Atributos opcionales

Use los siguientes atributos para configurar la apariencia y sensibilidad del botón.

| Atributo | Descripción |
| --------- | ----------- |
| `data-button-style` | Establece el estilo del botón. Puede ser `icon`, `text` o `iconAndText`. Su valor predeterminado es `icon`. |
| `data-locale` | Establece la configuración regional. Por ejemplo, `en-US` o `fr-FR`. El valor predeterminado es el idioma inglés `en`. |
| `data-icon-px-size` | Establece el tamaño del icono en píxeles. El valor predeterminado es 20 píxeles. |

## <a name="html-support"></a>Compatibilidad con HTML

| HTML | Contenido admitido |
| --------- | ----------- |
| Estilos de fuente | Negrita, cursiva, subrayado, código, tachado, superíndice, subíndice |
| Listas sin ordenar | Disco, círculo, cuadrado |
| Listas ordenadas | Decimal, alfabético en mayúsculas, alfabético en minúsculas, romano superior, romano inferior |

Las etiquetas no admitidas se representarán de la misma forma. Las imágenes y las tablas actualmente no se admiten.

## <a name="browser-support"></a>Compatibilidad con exploradores

Use las versiones más recientes de los siguientes exploradores para disfrutar de la mejor experiencia con el Lector inmersivo.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Pasos siguientes

* Explorar el [SDK del Lector inmersivo en GitHub](https://github.com/microsoft/immersive-reader-sdk)
* [Inicio rápido: Creación de una aplicación web que inicia el Lector inmersivo (C#)](./quickstarts/client-libraries.md?pivots=programming-language-csharp)
