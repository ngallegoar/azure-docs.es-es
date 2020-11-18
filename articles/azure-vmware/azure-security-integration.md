---
title: Protección de máquinas virtuales de Azure VMware Solution con la integración de Azure Security Center
description: Obtenga información sobre cómo proteger las máquinas virtuales de Azure VMware Solution con las herramientas de seguridad nativas de Azure desde un solo panel en Azure Security Center.
ms.topic: how-to
ms.date: 11/06/2020
ms.openlocfilehash: 53669f2988a7ff7ab1150b155a65c7a187c6f1c8
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94369904"
---
# <a name="protect-your-azure-vmware-solution-vms-with-azure-security-center-integration"></a>Protección de máquinas virtuales de Azure VMware Solution con la integración de Azure Security Center

Las herramientas de seguridad nativas de Azure proporcionan una infraestructura segura para un entorno híbrido de Azure, Azure VMware Solution y máquinas virtuales (VM) locales. En este artículo se muestra cómo configurar las herramientas de Azure para la seguridad del entorno híbrido. Usará varias herramientas para identificar y abordar diferentes tipos de amenazas.

## <a name="azure-native-services"></a>Servicios nativos de Azure

A continuación se muestra un resumen rápido de cada servicio nativo de Azure:

- **Área de trabajo de Log Analytics**: un área de trabajo de Log Analytics es un entorno único para almacenar los datos de registro. Cada área de trabajo tiene su propio repositorio y configuración de datos. Los orígenes de datos y las soluciones se configuran para almacenar sus datos en un área de trabajo específica.
- **Azure Security Center:** Azure Security Center es un sistema de administración de la seguridad de infraestructura unificada. Fortalece la postura de seguridad de los centros de datos y proporciona protección contra amenazas avanzada en las cargas de trabajo híbridas tanto en la nube como a nivel local.
- **Azure Sentinel**: Azure Sentinel es una solución de administración de eventos, información de seguridad (SIEM) y respuesta automatizada de orquestación de seguridad (SOAR) que es nativa de la nube. Ofrece análisis de seguridad inteligentes e inteligencia sobre amenazas en un entorno. Se trata de solución única para la detección de alertas, la visibilidad de amenazas, la búsqueda proactiva y la respuesta a amenazas.

## <a name="topology"></a>Topología

![Diagrama que muestra la arquitectura de la seguridad integrada de Azure.](media/azure-security-integration/azure-integrated-security-architecture.png)

El agente de Log Analytics habilita la recopilación de datos de registro de Azure, Azure VMware Solution y las máquinas virtuales locales. Los datos de registro se envían a los registros de Azure Monitor y se almacenan en un área de trabajo de Log Analytics. Puede implementar el agente de Log Analytics mediante la [compatibilidad con la extensión de máquina virtual](../azure-arc/servers/manage-vm-extensions.md) de servidores habilitados para Arc para máquinas virtuales nuevas y existentes. 

Una vez que los registros se recopilan en el área de trabajo de Log Analytics, puede configurar el área de trabajo de Log Analytics con Azure Security Center. Azure Security Center evaluará el estado de vulnerabilidades de las máquinas virtuales de Azure VMware Solution y generará una alerta para cualquier vulnerabilidad crítica. Por ejemplo, se evalúan las revisiones del sistema operativo que faltan, los errores de configuración de seguridad y [Endpoint Protection](../security-center/security-center-services.md).

Puede configurar el área de trabajo de Log Analytics con Azure Sentinel para detección de alertas, visibilidad de amenazas, búsqueda proactiva y respuesta a amenazas. En el diagrama anterior, Azure Security Center se conecta a Azure Sentinel mediante el conector de Azure Security Center. Azure Security Center reenviará la vulnerabilidad del entorno a Azure Sentinel para crear un incidente y asignarlo a otras amenazas. También puede crear la consulta de reglas programadas para detectar la actividad no deseada y convertirla en incidentes.

## <a name="benefits"></a>Ventajas

- Los servicios nativos de Azure se pueden usar para la seguridad del entorno híbrido en Azure, Azure VMware Solution y servicios locales.
- Con un área de trabajo de Log Analytics, puede recopilar los datos o los registros en un solo punto y presentar los mismos datos en diferentes servicios nativos de Azure.
- Azure Security Center proporciona características de seguridad, como la supervisión de la integridad de los archivos, la detección de ataques sin archivos, la evaluación de revisiones del sistema operativo, la evaluación de los errores de configuración de seguridad y la evaluación de Endpoint Protection.
- Azure Sentinel le permite lo siguiente:
    - Recopile datos a escala de nube de todos los usuarios, dispositivos, aplicaciones y de toda la infraestructura, tanto en el entorno local como en diversas nubes.
    - Detecte amenazas previamente no detectadas.
    - Investigue amenazas con inteligencia artificial y busque actividades sospechosas a gran escala.
    - Responda a los incidentes con rapidez con la orquestación y la automatización de tareas comunes integradas.

## <a name="create-a-log-analytics-workspace"></a>Creación de un área de trabajo de Log Analytics

Necesitará un área de trabajo de Log Analytics para recopilar datos de varios orígenes. Vea los pasos de [Creación de un área de trabajo de Log Analytics en Azure Portal](../azure-monitor/learn/quick-create-workspace.md). 

## <a name="deploy-security-center-and-configure-azure-vmware-solution-vms"></a>Implementación de Security Center y configuración de máquinas virtuales de Azure VMware Solution

Azure Security Center es una herramienta preconfigurada y no requiere implementación. En Azure Portal, busque **Security Center** y selecciónelo.

### <a name="enable-azure-defender"></a>Habilitación de Azure Defender

Azure Defender amplía Advanced Threat Protection de Azure Security Center en las cargas de trabajo híbridas tanto a nivel local como en la nube. Por tanto, para proteger las máquinas virtuales de Azure VMware Solution, debe habilitar Azure Defender. 

1. En Security Center, seleccione **Introducción**.

2. Seleccione la pestaña **Actualizar** y, después, seleccione la suscripción o el área de trabajo. 

3. Seleccione **Actualizar** para habilitar Azure Defender.

## <a name="add-azure-vmware-solution-vms-to-security-center"></a>Adición de máquinas virtuales de Azure VMware Solution en Security Center

1. En Azure Portal, busque **Azure Arc** y selecciónelo.

2. En Recursos, seleccione **Servidores** y, después, **+Agregar**.

    :::image type="content" source="media/azure-security-integration/add-server-to-azure-arc.png" alt-text="Una captura de pantalla en la que se muestra la página de servidores de Azure Arc para agregar una máquina virtual de Azure VMware Solution a Azure":::.

3. Seleccione **Generar scripts**.
 
    :::image type="content" source="media/azure-security-integration/add-server-using-script.png" alt-text="Una captura de pantalla de la página de Azure Arc en la que se muestra la opción para agregar un servidor con el script interactivo":::. 
 
4. En la pestaña **Requisitos previos**, seleccione **Siguiente**.

5. En la pestaña **Detalles de recursos**, rellene los siguientes detalles: 
    - Suscripción
    - Grupo de recursos
    - Region 
    - Sistema operativo
    - Detalles del servidor proxy
    
    Después, seleccione **Next: Etiquetas**.

6. En la pestaña **Etiquetas**, seleccione **Siguiente**.

7. En la pestaña **Descargar y ejecutar el script**, seleccione **Descargar**.

8. Especifique el sistema operativo y ejecute el script en la máquina virtual de Azure VMware Solution.

## <a name="view-recommendations-and-passed-assessments"></a>Visualización de recomendaciones y evaluaciones superadas

1. En Azure Security Center, seleccione **Inventario** en el panel izquierdo.

2. En Tipo de recurso, seleccione **Servidores: Azure Arc**.
 
     :::image type="content" source="media/azure-security-integration/select-resource-in-security-center.png" alt-text="Una captura de pantalla de la página de inventario de Azure Security Center que muestra Servidores: Azure Arc seleccionado en Tipo de recurso":::.

3. Seleccione el nombre del recurso. Se abre una página que muestra los detalles del estado de seguridad del recurso.

4. En **Lista de recomendaciones**, seleccione las pestañas **Recomendaciones**, **Evaluaciones correctas**, y **Evaluaciones no disponibles** para ver estos detalles.

    :::image type="content" source="media/azure-security-integration/view-recommendations-assessments.png" alt-text="Captura de pantalla de Azure Security Center que muestra las recomendaciones de seguridad y las evaluaciones":::.

## <a name="deploy-an-azure-sentinel-workspace"></a>Implementación de un área de trabajo de Azure Sentinel

Azure Sentinel se basa en un área de trabajo de Log Analytics. El primer paso en la incorporación de Azure Sentinel es seleccionar el área de trabajo de Log Analytics que quiere usar para ese propósito.

1. En Azure Portal, busque **Azure Sentinel** y selecciónelo.

2. En la página Áreas de trabajo de Azure Sentinel, seleccione **+Agregar**.

3. Seleccione el área de trabajo de Log Analytics y, después, **Agregar**.

## <a name="enable-data-collector-for-security-events-on-azure-vmware-solution-vms"></a>Habilitación del recopilador de datos para eventos de seguridad en máquinas virtuales de Azure VMware Solution

Ahora está listo para conectar Azure Sentinel con sus orígenes de datos, en este caso, los eventos de seguridad.

1. En la página Áreas de trabajo de Azure Sentinel, seleccione el área de trabajo configurada.

2. En Configuración, seleccione **Conectores de datos**.

3. En la columna Nombre del conector, seleccione **Eventos de seguridad** en la lista y, después, seleccione **Abrir página del conector**.

4. En la página del conector, seleccione los eventos que desea transmitir y, después, seleccione **Aplicar cambios**.

    :::image type="content" source="media/azure-security-integration/select-events-you-want-to-stream.png" alt-text="Captura de pantalla de la página de eventos de seguridad de Azure Sentinel, donde puede seleccionar los eventos que desea transmitir":::.

## <a name="connect-azure-sentinel-with-azure-security-center"></a>Conexión de Azure Sentinel con Azure Security Center  

1. En la página del área de trabajo de Azure Sentinel, seleccione el área de trabajo configurada.

2. En Configuración, seleccione **Conectores de datos**.

3. Seleccione **Azure Security Center** en la lista y, después, seleccione **Abrir página del conector**.

    :::image type="content" source="media/azure-security-integration/connect-security-center-with-azure-sentinel.png" alt-text="Captura de pantalla de la página de conectores de datos de Azure Sentinel que muestra la selección para conectar Azure Security Center con Azure Sentinel":::.

4. Seleccione **Conectar** para conectar Azure Security Center con Azure Sentinel.

5. Habilite **Crear incidente** para generar un incidente para Azure Security Center.

## <a name="create-rules-to-identify-security-threats"></a>Creación de reglas para identificar amenazas de seguridad

Después de conectar los orígenes de datos a Azure Sentinel, puede crear reglas para generar alertas basadas en amenazas detectadas. En el ejemplo siguiente, crearemos una regla para identificar los intentos de inicio de sesión en Windows Server con una contraseña incorrecta.

1. En la página de información general de Azure Sentinel, en Configuraciones, seleccione **Analytics**.

2. En Configuraciones, seleccione **Analytics**.

3. Seleccione **+Crear** y, en el menú desplegable, seleccione **Regla de consulta programada**.

4. En la pestaña **General**, escriba la información necesaria.

    - Nombre
    - Descripción
    - Tácticas
    - Gravedad
    - Status

    Seleccione **Siguiente: Establecer la lógica de la regla >** .

5. En la pestaña **Establecer la lógica de la regla**, escriba la información necesaria.

    - Consulta de la regla (aquí se muestra nuestra consulta de ejemplo)
    
        ```
        SecurityEvent
        |where Activity startswith '4625'
        |summarize count () by IpAddress,Computer
        |where count_ > 3
        ```
        
    - Asignar entidad
    - Programación de consultas
    - Umbral de alerta
    - Agrupación de eventos
    - Supresión

    Seleccione **Siguiente**.

6. En la pestaña **Configuración de los incidentes**, habilite **Crear incidentes a partir de las alertas desencadenadas por esta regla de análisis** y seleccione **Siguiente: Respuesta automatizada >** .
 
    :::image type="content" source="media/azure-security-integration/create-new-analytic-rule-wizard.png" alt-text="Captura de pantalla del Asistente para reglas de análisis para crear una regla en Azure Sentinel que muestra habilitada la opción Crear incidentes a partir de alertas desencadenadas por esta regla de análisis":::.

7. Seleccione **Siguiente: revisión >** .

8. En la pestaña **Revisar y crear**, revise la información y seleccione **Crear**.

Después del tercer intento fallido de iniciar sesión en Windows Server, la regla creada desencadena un incidente para cada intento fallido.

## <a name="view-generated-alerts"></a>Visualización de alertas generadas

Puede ver los incidentes generados con Azure Sentinel. También puede asignar incidentes y cerrarlos una vez resueltos, todo ello desde Azure Sentinel.

1. Vaya a la página de información general de Azure Sentinel.

2. En Administración de amenazas, seleccione **Incidentes**.

3. Seleccione un incidente. Después, puede asignar el incidente a un equipo para su resolución.

    :::image type="content" source="media/azure-security-integration/assign-incident.png" alt-text="Captura de pantalla de la página de incidentes de Azure Sentinel con el incidente seleccionado y la opción para asignar el incidente para su resolución":::.

    Después de resolver el problema, puede cerrarlo.

## <a name="hunt-security-threats-with-queries"></a>Búsqueda de amenazas de seguridad con consultas

Puede crear consultas o usar la consulta predefinida disponible en Azure Sentinel para identificar amenazas en su entorno. En los pasos siguientes, se ejecuta una consulta predefinida.

1. Vaya a la página de información general de Azure Sentinel.

2. En Administración de amenazas, seleccione **Búsqueda**. Se muestra una lista de consultas predefinidas.

3. Seleccione una consulta y, después, seleccione **Ejecutar consulta**.

4. Seleccione **Ver resultados** para comprobar los resultados.

### <a name="create-a-new-query"></a>Creación de una consulta

1.  En Administración de amenazas, seleccione **Búsqueda** y después **+Nueva consulta**.

    :::image type="content" source="media/azure-security-integration/create-new-query.png" alt-text="Captura de pantalla de la página de búsqueda de Azure Sentinel con la opción + Nueva consulta resaltada":::.

2. Rellene la información siguiente para crear una consulta personalizada.

    - Nombre
    - Descripción
    - Consulta personalizada
    - Especificar asignación
    - Tácticas
    
3. Seleccione **Crear**. Después, puede seleccionar la consulta creada, **Ejecutar consulta** y **Ver resultados**.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo usar el [panel de Azure Defender](../security-center/azure-defender-dashboard.md).
- Explore toda la gama de protección que ofrece [Azure Defender](../security-center/azure-defender.md).
- Obtenga información sobre la [Detección avanzada de ataques de varias fases en Azure Sentinel](../azure-monitor/learn/quick-create-workspace.md).
