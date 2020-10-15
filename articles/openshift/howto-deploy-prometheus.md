---
title: Implementación de una instancia de Prometheus en un clúster de Red Hat OpenShift en Azure
description: Cree crear una instancia de Prometheus en un clúster de Red Hat OpenShift en Azure para supervisar las métricas de una aplicación.
author: makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 06/17/2019
keywords: prometheus, aro, openshift, métricas, red hat
ms.openlocfilehash: 7f22df587f51af735e0ea663e53f6eef14d60692
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80886895"
---
# <a name="deploy-a-standalone-prometheus-instance-in-an-azure-red-hat-openshift-cluster"></a>Implementación de una instancia de Prometheus independiente en un clúster de Red Hat OpenShift en Azure

En este artículo se describe cómo configurar una instancia de Prometheus independiente que usa la detección de servicio en un clúster de Red Hat OpenShift en Azure.

> [!NOTE]
> No es necesario acceso de administrador del cliente al clúster de Red Hat OpenShift en Azure.

Configuración de destino:

- Un proyecto (prometheus-project), que contiene Prometheus y Alertmanager.
- Dos proyectos (app-project1 y app-project2), que contienen las aplicaciones que se van a supervisar.

Preparará algunos archivos de configuración de Prometheus localmente. Cree una nueva carpeta para almacenarlos. Los archivos de configuración se almacenan en el clúster como secretos, en caso de que más adelante se agreguen tokens secretos al clúster.

## <a name="sign-in-to-the-cluster-by-using-the-oc-tool"></a>Inicio de sesión en el clúster con la herramienta OC

1. Abra un explorador web y vaya a la consola web de su clúster (https://openshift.*id-aleatorio*.*región*.azmosa.io).
2. Inicie sesión con sus credenciales de Azure.
3. Seleccione su nombre de usuario en la esquina superior derecha y haga clic en **Copy Login Command** (Copiar comando de inicio de sesión).
4. Pegue el nombre de usuario en el terminal que va a usar.

> [!NOTE]
> Para ver verificar si ha iniciado sesión el clúster correcto, ejecute el comando `oc whoami -c`.

## <a name="prepare-the-projects"></a>Preparar los proyectos

Para crear los proyectos, ejecute los comandos siguientes:
```
oc new-project prometheus-project
oc new-project app-project1
oc new-project app-project2
```


> [!NOTE]
> Puede usar el parámetro `-n` o `--namespace`, o seleccionar un proyecto activo con el comando `oc project`.

## <a name="prepare-the-prometheus-configuration-file"></a>Preparación del archivo de configuración de Prometheus
Cree un archivo denominado prometheus.yml con el contenido siguiente:
```
global:
  scrape_interval: 30s
  evaluation_interval: 5s

scrape_configs:
    - job_name: prom-sd
      scrape_interval: 30s
      scrape_timeout: 10s
      metrics_path: /metrics
      scheme: http
      kubernetes_sd_configs:
      - api_server: null
        role: endpoints
        namespaces:
          names:
          - prometheus-project
          - app-project1
          - app-project2
```
Especifique la configuración siguiente para crear un secreto denominado Prom:
```
oc create secret generic prom --from-file=prometheus.yml -n prometheus-project
```

El archivo prometheus.yml es un archivo de configuración básico de Prometheus. Establece los intervalos y configura la detección automática en tres proyectos (prometheus-project, app-project1, app-project2). En el archivo de configuración anterior, los puntos de conexión detectados automáticamente se extraen a través de HTTP sin autenticación.

Para obtener más información sobre la extracción de puntos de conexión, consulte la [configuración de extracción de Prometheus](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config).


## <a name="prepare-the-alertmanager-config-file"></a>Preparación del archivo de configuración de Alertmanager
Cree un archivo denominado alertmanager.yml con el contenido siguiente:
```
global:
  resolve_timeout: 5m
route:
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 12h
  receiver: default
  routes:
  - match:
      alertname: DeadMansSwitch
    repeat_interval: 5m
    receiver: deadmansswitch
receivers:
- name: default
- name: deadmansswitch
```
Especifique la configuración siguiente para crear un secreto denominado Prom-Alerts:
```
oc create secret generic prom-alerts --from-file=alertmanager.yml -n prometheus-project
```

Alertmanager.yml es el archivo de configuración del Administrador de alertas.

> [!NOTE]
> Para comprobar los dos pasos anteriores, ejecute el comando `oc get secret -n prometheus-project`.

## <a name="start-prometheus-and-alertmanager"></a>Iniciar Prometheus y Alertmanager
Vaya al [repositorio de openshift/origin](https://github.com/openshift/origin/tree/release-3.11/examples/prometheus) y descargue la plantilla [prometheus-standalone.yaml](
https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml). Especifique la configuración siguiente para aplicar la plantilla a prometheus-project:
```
oc process -f https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml | oc apply -f - -n prometheus-project
```
El archivo prometheus-standalone.yaml es una plantilla de OpenShift que creará una instancia de Prometheus con un oauth-proxy delante de él y una instancia de Alertmanager, también protegida con el oauth-proxy. En esta plantilla, oauth-proxy está configurado para permitir cualquier usuario que pueda "obtener" el espacio de nombres prometheus-project (consulte la marca `-openshift-sar`).

> [!NOTE]
> Para comprobar si StatefulSet prom tiene réplicas de número DESIRED y CURRENT, ejecute el comando `oc get statefulset -n prometheus-project`. Si quiere comprobar todos los recursos del proyecto, ejecute el comando `oc get all -n prometheus-project`.

## <a name="add-permissions-to-allow-service-discovery"></a>Adición de permisos para permitir la detección de servicios

Cree un archivo denominado prometheus-sdrole.yml con el contenido siguiente:
```
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: prometheus-sdrole
  annotations:
    "openshift.io/display-name": Prometheus service discovery role
    description: |
      Role and rolebinding added permissions required for service discovery in a given project.
    iconClass: fa fa-cogs
    tags: "monitoring,prometheus,alertmanager,time-series"
parameters:
- description: The project name, where a standalone Prometheus is deployed
  name: PROMETHEUS_PROJECT
  value: prometheus-project
objects:
- apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: prometheus-sd
  rules:
  - apiGroups:
    - ""
    resources:
    - services
    - endpoints
    - pods
    verbs:
    - list
    - get
    - watch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: prometheus-sd
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: prometheus-sd
  subjects:
  - kind: ServiceAccount
    name: prom
    namespace: ${PROMETHEUS_PROJECT}
```
Para aplicar la plantilla a todos los proyectos en los que quiere permitir la detección de servicios, ejecute los comandos siguientes:
```
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project1
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project2
oc process -f prometheus-sdrole.yml | oc apply -f - -n prometheus-project
```

> [!NOTE]
> Para comprobar que Role y RoleBinding se han creado correctamente, ejecute los comandos `oc get role` y `oc get rolebinding`.

## <a name="optional-deploy-example-application"></a>Opcional: Implementación de una aplicación de ejemplo

Todo funciona, pero no hay orígenes de métricas. Vaya a la dirección URL de Prometheus (https://prom-prometheus-project.apps.*id-aleatorio*.*región*.azmosa.io/). Puede encontrarla con el comando siguiente:

```
oc get route prom -n prometheus-project
```
> [!IMPORTANT]
> No olvide agregar el prefijo https:// al principio del nombre de host.

La página **Status> Service Discovery** (Estado > Detección de servicios) mostrará 0/0 destinos activos.

Para implementar una aplicación de ejemplo, que expone las métricas de Python básicas en el punto de conexión /metrics, ejecute los comandos siguientes:
```
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example1 -n app-project1

oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example2 -n app-project2
```
Las nuevas aplicaciones deberían aparecer como destinos válidos en la página Detección de servicios en un plazo de 30 segundos a partir de la implementación.

Para obtener más información, seleccione **Estado** > **Destinos**.

> [!NOTE]
> Para cada destino extraído correctamente, Prometheus agrega un punto de datos en la métrica "up". Seleccione **Prometheus** en la esquina superior izquierda, especifique **up** como expresión y haga clic en **Ejecutar**.

## <a name="next-steps"></a>Pasos siguientes

Puede agregar instrumentación personalizada de Prometheus a sus aplicaciones. La biblioteca de cliente de Prometheus, que simplifica la preparación de las métricas de Prometheus, está preparada para diferentes lenguajes de programación.

Para obtener más información, consulte las siguientes bibliotecas de GitHub:

 - [Java](https://github.com/prometheus/client_java)
 - [Python](https://github.com/prometheus/client_python)
 - [Go](https://github.com/prometheus/client_golang)
 - [Ruby](https://github.com/prometheus/client_ruby)
