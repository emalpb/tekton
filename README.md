# README

Documentación de pipelines 2021 con Tekton para Openshift 4.6

 
 # Alta de pipelines TEKTON en proyectos nuevos:
 
 Validar si ya no existe la SA pipeline en el namespaces.

    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: pipeline
    secrets:
      - name: bitbucket-credentials


Importar YAML's en la carpeta clustertasks y luego de la carpeta pipelines:
cicd.yaml para ambiente dev
cicd-qa.yaml para ambiente qa
cicd-prd.yaml para ambiente prd

# Webhooks
Para conectar bitbucket con los pipelines de Openshift. 
Por namespaces se debe importar ( modificar namespaces en cada YAML ) de la carpeta webhooks:

tt.yaml 	- TriggerTemplate
tb.yaml  - TriggerBinding
eventlistener.yaml  -  EventListener - luego crearle una route 

Luego configurar en bitbucket el webhook con la route del EventListener.


# Pipelines TEKTON

  

##  Dev:

  

oc-tasks: Crea elementos Openshift deploymentconfig, service y route.

fetch: Descarga codigo fuente desde bitbucket.

maven: Compila el codigo y empaqueta en /target

sonarqube: Analisa el codigo.

build: Crea la imagen con el .jar del paso maven.

deploy: Deploy de la aplicacion con la ultima imagen.

scale: Escala a la replica seleccionada.

  

##  Qa / Prd:

  

oc-tasks: Crea elementos Openshift deploymentconfig, service y route.

tag: Tagea la version latest de DEV con el numero informado.

patch: Patche del dc con la version informada.

deploy: Deploy de la aplicacion con la ultima imagen.

scale: Escala a la replica seleccionada.