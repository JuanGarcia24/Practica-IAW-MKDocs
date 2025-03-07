# Practica-IAW-MKDocs: Creación de un sitio web estático con MkDocs y GitHub Pages

## Estructura de la Práctica

![Estructura](./Imagenes/Estructura.png)


## Objetivo de la Práctica

El objetivo de esta práctica es aprender a crear, personalizar y desplegar un sitio web utilizando MkDocs y el theme "Material for MkDocs". Para ello, se utilizarán herramientas como Docker para la ejecución del entorno, y GitHub Pages para la publicación del sitio web.

Además, llevaremos a cabo el proceso de integración y despliegue continuo (CI/CD) con GitHub Actions, permitiendo que la documentación se actualice automáticamente con cada cambio en el repositorio.


## Desarrollo de la práctica 

En primer lugar, crearemos nuestro proyecto con MKDocs y el tema Material usando Docker (Asegurándonos de tener Docker instalado en nuestro sistema). 


![Creación de Proyecto](./Imagenes/Creacion_Proyecto.png)


Realizado lo anterior, configuraremos el archivo "mkdocs.yml" para personalizar a nuestro gusto el nombre del sitio, el tema...


- **`mkdocs.yml`:** En este archivo se guardará la configuración principal de un proyecto MkDocs. Su función es definir cómo se estructurará y personalizará el sitio web de la documentación generada.



```bash

    site_name: IAW

    nav:
        - Principal: index.md
        - Acerca de: about.md
        - Practica 1: practica1.md
        - Práctica 5.4: practica5.4.md
        - Práctica 4.5: practica4.5.md
        - Práctica 3.1: practica3.1.md

    theme: material

```


Ahora, iniciaremos el servidor, esto nos servirá para poder visualizar el sitio a través del navegador.

![Creación de Proyecto](./Imagenes/iniciar_servidor.png)



Para comprobar el resultado, abriremos nuestro navegador y introducimos la IP de la máquina dónde estemos realizando la práctica y al final introducimos :8000.

![Sitio Funcionando](./Imagenes/sitio_funcionando.png)



En siguiente lugar, generaremos la documentación de nuestro sitio web de forma que no haga falta tener que iniciar el servidor local de desarrollo. Creará un directorio llamado "site" con el sitio web generado.

![Archivos Estáticos](./Imagenes/sitio_estático.png)



Por último, publicaremos el sitio web en GitHub Pages, publicándose en la rama "gh-pages".


![Publicar en GitHub Pages](./Imagenes/publicar_githubpages.png)



Nos aseguraremos tras realizar este paso, de tener configurados los permisos de WorkFlow marcados en la sección "Reaad and write permissions". Para dirigirnos a este apartado, habrá que situarse sobre las opciones del repositorio de la práctica y desplegamos la sección "Actions".

![Ajuste de WorkFlow](./Imagenes/Workflow.png)




- **`build-push-mkdocs.yml`:** En este archivo configuramos el workflow de GitHub Actions. Cuyo propósito es automatizar la publicación de la documentación creada con MKDocs en GitHub Pages cada vez que realizamos un push en la rama "main".


**Contenido del archivo:** 
   
```bash

    name: build-push-mkdocs

# Eventos que desescandenan el workflow
on:
  push:
    branches: ["main"]

  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:

  # Job para crear la documentación de mkdocs
  build:
    # Indicamos que este job se ejecutará en una máquina virtual con la última versión de ubuntu
    runs-on: ubuntu-latest
    
    # Definimos los pasos de este job
    steps:
      - name: Clone repository
        uses: actions/checkout@v4

      - name: Install Python3
        uses: actions/setup-python@v4
        with:
          python-version: 3.x

      - name: Install Mkdocs
        run: |
          pip install mkdocs
          pip install mkdocs-material 

      - name: Build MkDocs
        run: |
          mkdocs build

      - name: Push the documentation in a branch
        uses: s0/git-publish-subdir-action@develop
        env:
          REPO: self
          BRANCH: gh-pages # The branch name where you want to push the assets
          FOLDER: site # The directory where your assets are generated
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # GitHub will automatically add this - you don't need to bother getting a token
          MESSAGE: "Build: ({sha}) {msg}" # The commit message

   ```


### Comprobaciones generales

Una vez redactados los comandos útiles, probaremos el resultado de la publicación del sitio en GitHub Pages.

![Publicación en Github Pages](./Imagenes/GitHub_Pages.png)


También nos dirigiremos a GitHub, y en la sección "Actions" nos mostrará un resumen de la ejecución del WorkFlow, dónde indica que se ejecutó correctamente. 

https://juangarcia24.github.io/Practica-IAW-MKDocs/