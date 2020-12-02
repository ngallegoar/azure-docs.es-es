---
title: 'Tutorial: Migración de una aplicación Android | Microsoft Azure Maps'
description: Tutorial sobre cómo migrar de una aplicación Android de Google Maps a Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 08/19/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: b096b24acd5cf65f6ad3e9eabb1d536b3aae0168
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96187075"
---
# <a name="tutorial---migrate-an-android-app-from-google-maps"></a>Tutorial: Migración de una aplicación Android desde Google Maps

Android SDK de Azure Maps tiene una interfaz API similar al SDK web. Si ha realizado desarrollos con uno de estos SDK, se aplican muchos de los mismos conceptos, procedimientos recomendados y arquitecturas. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Carga de un mapa
> * Localizar un mapa
> * Agregar marcadores, polilíneas y polígonos.
> * Superposición de una capa de mosaicos
> * Mostrar datos del tráfico

Android SDK de Azure Maps admite una versión mínima para Android de la API 21: Android 5.0.0 (Lollipop).

Todos los ejemplos se proporcionan en Java, pero también puede usar Kotlin con Android SDK de Azure Maps.

Consulte [Guías paso a paso de Android SDK de Azure Maps](how-to-use-android-map-control-library.md) para más información sobre el desarrollo con este SDK.

## <a name="prerequisites"></a>Requisitos previos 

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
2. [Cree una cuenta de Azure Maps](quick-demo-map-app.md#create-an-azure-maps-account).
3. [Obtenga una clave de suscripción principal](quick-demo-map-app.md#get-the-primary-key-for-your-account), también conocida como clave principal o clave de suscripción. Para más información sobre la autenticación en Azure Maps, consulte [Administración de la autenticación en Azure Maps](how-to-manage-authentication.md).

## <a name="load-a-map"></a>Carga de un mapa

La carga de un mapa en una aplicación Android con Google Maps o Azure Maps consta de pasos similares. Al usar cualquier SDK, debe hacer lo siguiente:

* Obtener una API o clave de suscripción para acceder a cualquiera de las plataformas.
* Agregar código XML a una actividad para especificar dónde se debe representar el mapa y cómo se debe diseñar.
* Reemplace todos los métodos de ciclo de vida de la actividad que contienen la vista del mapa por los métodos correspondientes en la clase de mapa. En particular, debe reemplazar los métodos siguientes:
    * `onCreate(Bundle)`
    * `onStart()`
    * `onResume()`
    * `onPause()`
    * `onStop()`
    * `onDestroy()`
    * `onSaveInstanceState(Bundle)`
    * `onLowMemory()`
* Espere a que el mapa esté preparado antes de intentar acceder a él y programarlo.

### <a name="before-google-maps"></a>Antes: Google Maps

Para mostrar un mapa mediante el SDK de Google Maps para Android, debe seguir estos pasos:

1. Asegúrese de que Servicios de Google Play está instalado.
2. Agregue una dependencia del servicio Google Maps al archivo **gradle.build** del módulo:

    `implementation 'com.google.android.gms:play-services-maps:17.0.0'`

3. Agregue una clave de API de Google Maps dentro de la sección de la aplicación del archivo **google\_maps\_api.xml**:

    ```xml
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_GOOGLE_MAPS_KEY"/>
    ```

4. Agregue un fragmento de mapa a la actividad principal:

    ```xml
    <com.google.android.gms.maps.MapView
            android:id="@+id/myMap"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>
    ```

5. En el archivo **MainActivity.java**, tendrá que importar el SDK de Google Maps. Reenvíe todos los métodos de ciclo de vida de la actividad que contienen la vista del mapa a los correspondientes en la clase de mapa. Recupere una instancia de `MapView` desde el fragmento de mapa mediante el método `getMapAsync(OnMapReadyCallback)`. `MapView` inicializa de forma automática el sistema de mapas y la vista. Edite el **MainActivity.java** como sigue:

    ```java
    import com.google.android.gms.maps.GoogleMap;
    import com.google.android.gms.maps.MapView;
    import com.google.android.gms.maps.OnMapReadyCallback;
 
    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    
    public class MainActivity extends AppCompatActivity implements OnMapReadyCallback {
    
        MapView mapView;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapView = findViewById(R.id.myMap);
    
            mapView.onCreate(savedInstanceState);
            mapView.getMapAsync(this);
        }
    
        @Override
    
        public void onMapReady(GoogleMap map) {
            //Add your post map load code here.
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapView.onResume();
        }
    
        @Override
        protected void onStart(){
            super.onStart();
            mapView.onStart();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapView.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapView.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapView.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapView.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapView.onSaveInstanceState(outState);
        }
    }
    ```

Al ejecutar una aplicación, el control de mapa se carga tal como se muestra en la siguiente imagen.

![Google Maps simple](media/migrate-google-maps-android-app/simple-google-maps.png)

### <a name="after-azure-maps"></a>Después: Azure Maps

Para mostrar un mapa mediante el SDK de Azure Maps para Android, debe seguir estos pasos:

1. Abra el archivo **build.gradle** de nivel superior y agregue el siguiente código a la sección de bloques **all projects** (todos los proyectos):

    ```JAVA
    maven {
            url "https://atlas.microsoft.com/sdk/android"
    }
    ```

2. Actualice **app/build.gradle** y agréguele el siguiente código:

    1. Asegúrese de que el valor de **minSdkVersion** del proyecto es API 21 o superior.

    2. Agregue el siguiente código a la sección Android:

        ```java
        compileOptions {
            sourceCompatibility JavaVersion.VERSION_1_8
            targetCompatibility JavaVersion.VERSION_1_8
        }
        ```

    3. Actualice el bloque de dependencias. Agregue una nueva línea de dependencia de implementación para el Android SDK de Azure Maps más reciente:

        ```java
        implementation "com.microsoft.azure.maps:mapcontrol:0.2"
        ```

        > [!Note]
        > Android SDK de Azure Maps se actualiza y mejora periódicamente. Puede consultar [Introducción al control de mapa de Android](how-to-use-android-map-control-library.md) para obtener el número de versión más reciente de Azure Maps. Además, puede establecer el número de versión de "0.2" en "0+" para que el código apunte siempre a la versión más reciente.

    4. Vaya a **Archivo** en la barra de herramientas y haga clic en **Sincronizar proyecto con archivos de Gradle**.

3. Agregue un fragmento de mapa a la actividad principal (resources pwd\> layout \> activity\_main.xml):

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            />
    </FrameLayout>
    ```

4. En el archivo **MainActivity.java**, tendrá que hacer lo siguiente:

    * Importar el SDK de Azure Maps
    * Establecer la información de autenticación de Azure Maps
    * Obtener la instancia del control de mapa en el método **onCreate**

     Establezca la información de autenticación en la clase `AzureMaps` con los métodos `setSubscriptionKey` o `setAadProperties`. Esta actualización global garantiza que agrega la información de autenticación a cada vista.

    El control de mapa contiene sus propios métodos de ciclo de vida para administrar el ciclo de vida de OpenGL de Android. Estos métodos deben llamarse directamente desde la actividad contenida. Para llamar correctamente a los métodos de ciclo de vida del control de mapa, debe invalidar los siguientes métodos de ciclo de vida en la actividad que contiene el control de mapa. Llame al método de control de mapa correspondiente.

    * `onCreate(Bundle)` 
    * `onStart()` 
    * `onResume()` 
    * `onPause()` 
    * `onStop()` 
    * `onDestroy()` 
    * `onSaveInstanceState(Bundle)` 
    * `onLowMemory()`

    Edite el **MainActivity.java** como sigue:

    ```java
    package com.example.myapplication;

    import android.support.v7.app.AppCompatActivity;
    import android.os.Bundle;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.options.MapStyle;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;

    public class MainActivity extends AppCompatActivity {
     
        static {
            AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
        }

        MapControl mapControl;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);

            mapControl = findViewById(R.id.mapcontrol);

            mapControl.onCreate(savedInstanceState);
    
            //Wait until the map resources are ready.
            mapControl.onReady(map -> {
                //Add your post map load code here.
    
            });
        }

        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }

        @Override
        protected void onStart(){
            super.onStart();
            mapControl.onStart();
        }

        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }

        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }

        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }

        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }

        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    }
    ```

Si ejecuta la aplicación, el control de mapa se cargará tal como aparece en la imagen siguiente.


![Azure Maps simple](media/migrate-google-maps-android-app/simple-azure-maps.png)

Tenga en cuenta que el control de Azure Maps permite alejar más y proporciona una mayor vista mundial.

> [!TIP]
> Si usa un emulador de Android en una máquina Windows, es posible que el mapa no se represente debido a conflictos con OpenGL y la representación de gráficos acelerada mediante software. Algunos usuarios han utilizado lo siguiente para resolver este problema. Abra el administrador de AVD y seleccione el dispositivo virtual que se va a editar. Desplácese hacia abajo en el panel **Verify Configuration** (Comprobar configuración). En la sección **Emulated Performance** (Rendimiento emulado,) establezca la opción **Graphics** (Gráficos) en **Hardware**.

## <a name="localizing-the-map"></a>Localización del mapa

La localización es importante si su audiencia se extiende a varios países o regiones y habla distintos idiomas.

### <a name="before-google-maps"></a>Antes: Google Maps

Agregue el código siguiente al método `onCreate` para establecer el idioma del mapa. El código se debe agregar antes de establecer la vista de contexto del mapa. El código de idioma "fr" limita el idioma al francés.

```java
String languageToLoad = "fr";
Locale locale = new Locale(languageToLoad);
Locale.setDefault(locale);
Configuration config = new Configuration();
config.locale = locale;
getBaseContext().getResources().updateConfiguration(config,
        getBaseContext().getResources().getDisplayMetrics());
```

Este es un ejemplo de Google Maps con el idioma establecido en "fr".

![Localización de Google Maps](media/migrate-google-maps-android-app/google-maps-localization.png)

### <a name="after-azure-maps"></a>Después: Azure Maps

Azure Maps proporciona tres formas diferentes de establecer el idioma y la vista regional del mapa. La primera opción consiste en pasar la información del idioma y de la vista regional a la clase `AzureMaps`. Esta opción utiliza los métodos estáticos `setLanguage` y `setView` de forma global. Se establecerán así el idioma y la vista regional predeterminados en todos los controles de Azure Maps cargados en la aplicación. En este ejemplo se establece el francés con el código de idioma "fr-FR".

```java
static {
    //Set your Azure Maps Key.
    AzureMaps.setSubscriptionKey("<Your Azure Maps Key>");

    //Set the language to be used by Azure Maps.
    AzureMaps.setLanguage("fr-FR");

    //Set the regional view to be used by Azure Maps.
    AzureMaps.setView("auto");
}
```

La segunda opción consiste en pasar la información del idioma y de la vista al código XML del control de mapa.

```xml
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_language="fr-FR"
    app:mapcontrol_view="auto"
    />
```

La tercera opción consiste en programar el idioma y la vista regional del mapa mediante el método `setStyle` de Maps. Esta opción actualiza el idioma y la vista regional cada vez que se ejecuta el código.

```java
mapControl.onReady(map -> {
    map.setStyle(StyleOptions.language("fr-FR"));
    map.setStyle(StyleOptions.view("auto"));
});
```

Este es un ejemplo de Azure Maps con el idioma establecido en "fr-FR".

![Localización de Azure Maps](media/migrate-google-maps-android-app/azure-maps-localization.png)

Visualización de la lista completa de los [idiomas admitidos](supported-languages.md).

## <a name="setting-the-map-view"></a>Establecimiento de la vista del mapa

Los mapas dinámicos en Azure Maps y Google Maps se pueden mover mediante programación a nuevas ubicaciones geográficas mediante la llamada a los métodos adecuados. Dispongamos el mapa para que muestre las imágenes aéreas de satélite, lo centramos sobre una ubicación con coordenadas y cambiamos el nivel de zoom. En este ejemplo, usaremos la latitud: 35,0272, la longitud: -111,0225 y un nivel de zoom de 15.

### <a name="before-google-maps"></a>Antes: Google Maps

La cámara del control de mapa de Google Maps se puede mover mediante programación con el método `moveCamera`. El método `moveCamera` permite especificar el centro del mapa y un nivel de zoom. El método `setMapType` cambia el tipo de mapa que se va a mostrar.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.moveCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(35.0272, -111.0225), 15));
    mapView.setMapType(GoogleMap.MAP_TYPE_SATELLITE);
}
```

![Vista establecida de Google Maps](media/migrate-google-maps-android-app/google-maps-set-view.png)

> [!NOTE]
> En Google Maps se usan iconos de 256 píxeles de tamaño mientras que en Azure Maps se usa un icono mayor de 512 píxeles. Esto reduce el número de solicitudes de red que Azure Maps necesita para cargar la misma área del mapa que Google Maps. Para lograr la misma área visible que un mapa en Google Maps, debe restar 1 al nivel de zoom que se usa en Google Maps cuando utilice Azure Maps.

### <a name="after-azure-maps"></a>Después: Azure Maps

Como se ha indicado antes, para lograr la misma área visible en Azure Maps, reste 1 al nivel de zoom que se usa en Google Maps. En este caso, utilice un nivel de zoom de 14.

La vista de mapa inicial se puede establecer en atributos XML en el control de mapa.

```xml
<com.microsoft.azure.maps.mapcontrol.MapControl
    android:id="@+id/myMap"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:mapcontrol_cameraLat="35.0272"
    app:mapcontrol_cameraLng="-111.0225"
    app:mapcontrol_zoom="14"
    app:mapcontrol_style="satellite"
    />
```

La vista del mapa se puede programar mediante los métodos `setCamera` y `setStyle` de Maps.

```java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(35.0272, -111.0225), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

![Vista establecida de Azure Maps](media/migrate-google-maps-android-app/azure-maps-set-view.png)

**Recursos adicionales:**

* [Estilos de mapa admitidos](supported-map-styles.md)

## <a name="adding-a-marker"></a>Adición de un marcador

Los datos de punto se suelen representar mediante una imagen en el mapa. Estas imágenes se denominan marcadores, chinchetas, señales o símbolos. En los ejemplos siguientes se representan los datos de punto como marcadores en el mapa en latitud: 51,5, longitud: -0,2.

### <a name="before-google-maps"></a>Antes: Google Maps

Con Google Maps, los marcadores se agregan mediante el método `addMarker` de los mapas.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33)));
}
```

![Marcador de Google Maps](media/migrate-google-maps-android-app/google-maps-marker.png)

### <a name="after-azure-maps"></a>Después: Azure Maps

En Azure Maps, represente los datos de punto en el mapa agregando primero los datos a un origen de datos. A continuación, debe conectar el origen de datos a una capa de símbolos. El origen de datos optimiza la administración de los datos espaciales del control de mapa. La capa de símbolos especifica cómo representar los datos de punto como una imagen o un texto.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create a point feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

    //Create a symbol layer and add it to the map.
    map.layers.add(new SymbolLayer(dataSource));
});
```

![Marcador de Azure Maps](media/migrate-google-maps-android-app/azure-maps-marker.png)

## <a name="adding-a-custom-marker"></a>Adición de un marcador personalizado

Se pueden usar imágenes personalizadas para representar puntos en un mapa. El mapa de los ejemplos siguientes utiliza una imagen personalizada para mostrar un punto en el mapa. El punto se encuentra en latitud: 51,5 y longitud: -0,2. El delimitador desplaza la posición del marcador, de modo que el punto del icono de chincheta se alinee con la posición correcta en el mapa.

<center>

![imagen de marcador amarillo](media/migrate-google-maps-web-app/yellow-pushpin.png)<br/>
yellow-pushpin.png</center>

En ambos ejemplos, la imagen anterior se agrega a la carpeta drawable de los recursos de las aplicaciones.

### <a name="before-google-maps"></a>Antes: Google Maps

Con Google Maps, se pueden usar imágenes personalizadas para los marcadores. Puede cargar imágenes personalizadas mediante la opción `icon` del marcador. Para alinear el punto de la imagen con la coordenada, utilice la opción `anchor`. El delimitador está relacionado con las dimensiones de la imagen. En este caso, el delimitador tiene 0,2 unidades de ancho y 1 unidad de alto.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.addMarker(new MarkerOptions().position(new LatLng(47.64, -122.33))
    .icon(BitmapDescriptorFactory.fromResource(R.drawable.yellow-pushpin))
    .anchor(0.2f, 1f));
}
```

![Marcador personalizado de Google Maps](media/migrate-google-maps-android-app/google-maps-custom-marker.png)

### <a name="after-azure-maps"></a>Después: Azure Maps

Las capas de símbolos de Azure Maps admiten imágenes personalizadas, pero primero se debe cargar la imagen en los recursos del mapa y debe tener asignado un identificador único. Después, la capa de símbolos debe hacer referencia a este identificador. Desplace el símbolo para que se alinee con el punto correcto de la imagen mediante la opción `iconOffset`. El desplazamiento del icono se realiza en píxeles. De forma predeterminada, el desplazamiento es relativo al centro inferior de la imagen, pero este valor de desplazamiento se puede ajustar mediante la opción `iconAnchor`. En este ejemplo se establece la opción `iconAnchor` en `"center"`. Se usa un desplazamiento de icono para mover la imagen 5 píxeles a la derecha y 15 píxeles hacia arriba hasta alinearla con el punto de la imagen de la chincheta.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create a point feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));

    //Load the custom image icon into the map resources.
    map.images.add("my-yellow-pin", R.drawable.yellow_pushpin);

    //Create a symbol that uses the custom image icon and add it to the map.
    map.layers.add(new SymbolLayer(dataSource,
        iconImage("my-yellow-pin"),
        iconAnchor(AnchorType.CENTER
        iconOffset(new Float[]{5f, -15f})));
});
```

![Marcador personalizado de Azure Maps](media/migrate-google-maps-android-app/azure-maps-custom-marker.png)

## <a name="adding-a-polyline"></a>Adición de una polilínea

Las polilíneas se usan para representar una línea o una ruta en el mapa. En los ejemplos siguientes se muestra cómo crear una polilínea con guiones en el mapa.

### <a name="before-google-maps"></a>Antes: Google Maps

Con Google Maps, representa una polilínea mediante la clase `PolylineOptions`. Agregue la polilínea al mapa mediante el método `addPolyline`. Establezca el color del trazo con la opción `color`. Establezca el ancho del trazo con la opción `width`. Agregue una matriz de guiones del trazo mediante la opción `pattern`.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the polyline.
    PolylineOptions lineOptions = new PolylineOptions()
            .add(new LatLng(46, -123))
            .add(new LatLng(49, -122))
            .add(new LatLng(46, -121))
            .color(Color.RED)
            .width(10f)
            .pattern(Arrays.<PatternItem>asList(
                    new Dash(30f), new Gap(30f)));

    //Add the polyline to the map.
    Polyline polyline = mapView.addPolyline(lineOptions);
}
```

![Polilínea de Google Maps](media/migrate-google-maps-android-app/google-maps-polyline.png)

### <a name="after-azure-maps"></a>Después: Azure Maps

En Azure Maps, las polilíneas se llaman objetos `LineString` o `MultiLineString`. Agregue estos objetos a un origen de datos y represéntelos mediante una capa de línea. Establezca el ancho del trazo con la opción `strokeWidth`. Agregue una matriz de guiones del trazo mediante la opción `strokeDashArray`.

Las unidades de "píxel" de ancho y de matriz de guiones del trazo en el SDK web de Azure Maps son las mismas que en el servicio de Google Maps. Ambos aceptan los mismos valores para generar los mismos resultados.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create an array of points.
    List<Point> points = Arrays.asList(
        Point.fromLngLat(-123, 46),
        Point.fromLngLat(-122, 49),
        Point.fromLngLat(-121, 46));

    //Create a LineString feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(LineString.fromLngLats(points)));

    //Create a line layer and add it to the map.
    map.layers.add(new LineLayer(dataSource,
        strokeColor("red"),
        strokeWidth(4f),
        strokeDashArray(new Float[]{3f, 3f})));
});
```

![Polilínea de Azure Maps](media/migrate-google-maps-android-app/azure-maps-polyline.png)

## <a name="adding-a-polygon"></a>Adición de un polígono

Los polígonos se usan para representar un área en el mapa. En los siguientes ejemplos se muestra cómo crear un polígono. Este polígono forma un triángulo basado en la coordenada central del mapa.

### <a name="before-google-maps"></a>Antes: Google Maps

Con Google Maps, represente un polígono mediante la clase `PolygonOptions`. Agregue el polígono al mapa mediante el método `addPolygon`. Establezca los colores de relleno y trazo con las opciones `fillColor` y `strokeColor`, respectivamente. Establezca el ancho del trazo con la opción `strokeWidth`.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the polygon.
    PolygonOptions polygonOptions = new PolygonOptions()
            .add(new LatLng(46, -123))
            .add(new LatLng(49, -122))
            .add(new LatLng(46, -121))
            .add(new LatLng(46, -123))  //Close the polygon.
            .fillColor(Color.argb(128, 0, 128, 0))
            .strokeColor(Color.RED)
            .strokeWidth(5f);

    //Add the polygon to the map.
    Polygon polygon = mapView.addPolygon(polygonOptions);
}
```

![Polígono de Google Maps](media/migrate-google-maps-android-app/google-maps-polygon.png)

### <a name="after-azure-maps"></a>Después: Azure Maps

En Azure Maps, agregue los objetos `Polygon` y `MultiPolygon` a un origen de datos y represéntelos en el mapa mediante capas. Represente el área de un polígono en una capa de polígono. Represente el contorno de un polígono con una capa de línea. Establezca el color y el ancho del trazo con las opciones `strokeColor` y `strokeWidth`.

Las unidades de "pixel" de ancho y de matriz de guiones del trazo en el SKD web de Azure Maps se corresponden con las unidades respectivas en Google Maps. Ambos aceptan los mismos valores y generan los mismos resultados.

```java
mapControl.onReady(map -> {
    //Create a data source and add it to the map.
    DataSource dataSource = new DataSource();
    map.sources.add(dataSource);

    //Create an array of point arrays to create polygon rings.
    List<List<Point>> rings = Arrays.asList(Arrays.asList(
        Point.fromLngLat(-123, 46),
        Point.fromLngLat(-122, 49),
        Point.fromLngLat(-121, 46),
        Point.fromLngLat(-123, 46)));

    //Create a Polygon feature and add it to the data source.
    dataSource.add(Feature.fromGeometry(Polygon.fromLngLats(rings)));

    //Add a polygon layer for rendering the polygon area.
    map.layers.add(new PolygonLayer(dataSource,
        fillColor("green"),
        fillOpacity(0.5f)));

    //Add a line layer for rendering the polygon outline.
    map.layers.add(new LineLayer(dataSource,
        strokeColor("red"),
        strokeWidth(2f)));
});
```
![Polígono de Azure Maps](media/migrate-google-maps-android-app/azure-maps-polygon.png)

## <a name="overlay-a-tile-layer"></a>Superposición de una capa de mosaicos

 Utilice las capas de mosaicos para superponer imágenes de capas que se han dividido en imágenes en mosaico más pequeñas y que se alinean con el sistema de mosaicos de los mapas. Este enfoque es una manera común de superponer imágenes de capas o conjuntos de datos muy grandes. Las capas de mosaicos se conocen como superposiciones de imágenes en Google Maps.

En los ejemplos siguientes se superpone una capa de mosaicos de radar meteorológico de Iowa Environmental Mesonet of Iowa State University. Los iconos tienen un tamaño de 256 píxeles.

### <a name="before-google-maps"></a>Antes: Google Maps

Con Google Maps, se puede superponer una capa de mosaico encima del mapa. Utilice la clase `TileOverlayOptions`. Agregue la capa de mosaico al mapa mediante el método `addTileLauer`. Para hacer que los iconos sean semitransparentes, la opción `transparency` se establece en 0,2, o en un 20 % de transparencia.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    //Create the options for the tile layer.
    TileOverlayOptions tileLayer = new TileOverlayOptions()
        .tileProvider(new UrlTileProvider(256, 256) {
            @Override
            public URL getTileUrl(int x, int y, int zoom) {

                try {
                    //Define the URL pattern for the tile images.
                    return new URL(String.format("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/%d/%d/%d.png", zoom, x, y));
                }catch (MalformedURLException e) {
                    throw new AssertionError(e);
                }
            }
        }).transparency(0.2f);

    //Add the tile layer to the map.
    mapView.addTileOverlay(tileLayer);
}
```

![Capa de icono de Google Maps](media/migrate-google-maps-android-app/google-maps-tile-layer.png)

### <a name="after-azure-maps"></a>Después: Azure Maps

Se puede agregar una capa de mosaicos al mapa de manera similar a cualquier otra capa. Una dirección URL con formato que tiene marcadores de posición X, Y y zoom (`{x}`, `{y}`, `{z}` respectivamente) se usa para indicar a la capa dónde acceder a los iconos. Asimismo, las capas de mosaicos en Azure Maps admiten los marcadores de posición `{quadkey}`, `{bbox-epsg-3857}` y `{subdomain}`. Para que la capa de icono sea semitransparente, se usa un valor de opacidad de 0,8. La opacidad y la transparencia, aunque son similares, usan valores invertidos. Para realizar la conversión entre las dos opciones, reste su valor del número uno.

> [!TIP]
> En Azure Maps es recomendable representar las capas bajo otras capas, incluidas las capas base del mapa. A menudo es conveniente también representar las capas de mosaicos debajo de las etiquetas del mapa para que resulten fáciles de leer. El método `map.layers.add` toma un segundo parámetro que es el identificador de la capa en la que se va a insertar la siguiente capa nueva. Para insertar una capa de mosaicos debajo de las etiquetas del mapa se puede usar el código siguiente: `map.layers.add(myTileLayer, "labels");`

```java
mapControl.onReady(map -> {
    //Add a tile layer to the map, below the map labels.
    map.layers.add(new TileLayer(
        tileUrl("https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png"),
        opacity(0.8f),
        tileSize(256)
    ), "labels");
});
```

![Capa de mosaicos de Azure Maps](media/migrate-google-maps-android-app/azure-maps-tile-layer.png)

## <a name="show-traffic"></a>Visualización del tráfico

Tanto Azure Maps como Google Maps tienen opciones para superponer datos de tráfico.

### <a name="before-google-maps"></a>Antes: Google Maps

Con Google Maps, los datos de flujo de tráfico se pueden superponer encima del mapa si se pasa true al método `setTrafficEnabled` del mapa.

```java
@Override
public void onMapReady(GoogleMap googleMap) {
    mapView = googleMap;

    mapView.setTrafficEnabled(true);
}
```

![Tráfico de Google Maps](media/migrate-google-maps-android-app/google-maps-traffic.png)

### <a name="after-azure-maps"></a>Después: Azure Maps

Azure Maps proporciona varias opciones diferentes para mostrar el tráfico. Los incidentes de tráfico, como los cierres de carreteras y los accidentes, se pueden mostrar como iconos en el mapa. El flujo de tráfico y las carreteras codificadas en colores se pueden superponer en el mapa. Los colores se pueden modificar para que aparezcan en relación con el límite de velocidad registrado, en relación con el retraso normal esperado o con el retraso absoluto. Los datos de incidentes en Azure Maps se actualizan cada minuto y los datos de flujo se actualizan cada dos minutos.

```java
mapControl.onReady(map -> {
    map.setTraffic(
        incidents(true),
        flow(TrafficFlow.RELATIVE));
});
```

![Tráfico de Azure Maps](media/migrate-google-maps-android-app/azure-maps-traffic.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre Android SDK para Azure Maps:

> [!div class="nextstepaction"]
> [Procedimientos para usar el control de mapa de Android](how-to-use-android-map-control-library.md)

> [!div class="nextstepaction"]
> [Add a symbol layer to an Android map](how-to-add-symbol-to-android-map.md)(Adición de una capa de símbolo a un mapa Android)

> [!div class="nextstepaction"]
> [Adición de formas a un mapa Android](./how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Cambio de estilos de mapa en mapas de Android](./set-android-map-styles.md)