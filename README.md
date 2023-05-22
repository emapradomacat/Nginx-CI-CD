# Proyecto Nginx CI/CD - Craftech

Este proyecto tiene como objetivo implementar un flujo de CI/CD utilizando acciones de workflow de GitHub para dockerizar un servidor NGINX y así automatizar el proceso de construcción y despliegue en DockerHub.


## Descripción

Consiste en construir una imagen de Docker que contenga el servidor NGINX con su correspondiente index.html predeterminado. Además, se implementará un pipeline utilizando las acciones de workflow de GitHub para automatizar la construcción y actualización de la imagen en DockerHub cada vez que se realice un cambio en el archivo index.html.


## Estructura 

- CI-CD/
  - README.md
  - docker-compose.yml
  - Dockerfile
  - index.html


## Requisitos Previos

Asegúrate de tener instalado lo siguiente en tu entorno de desarrollo:
- Python v3.10.6
- Docker v20.10.21
- Docker Compose v2.18.1
- Git v2.34.1


## Funcionamiento

1. Clonar el repositorio a nuestra área de trabajo
```
git clone https://github.com/emapradomacat/Nginx-CI-CD.git
```
2. Inicializar git 
```
git init
```
3. Abre el archivo index.html en el directorio raíz del proyecto y realiza los cambios deseados. Puedes personalizar el contenido de la página web según tus necesidades.
4. Guarda los cambios y haz commit
```
git add index.html
git commit -m "Actualización del archivo index.html"
```
5. Push al repositorio remoto
```
git push -u origin main
```


## Despliegue Automatizado con CI/CD

El despliegue automatizado se logra utilizando las acciones de workflow de GitHub. A continuación, se detallan los pasos necesarios para configurar el flujo de CI/CD desde tu consola local.
1. Edita el siguiente archivo con tu editor de texto favorito
```
vim .github/workflows/main.yml
```
2. Reemplazar tu-usuario y nombre-de-la-imagen con tu nombre de usuario y el nombre deseado para la imagen de Docker en DockerHub.
```
name: Docker Build & Push
on:
  push:
    branches:
      - main
jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Log in to Docker Hub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_PASSWORD }}

      - name: Build and push Docker image to Docker Hub
        uses: docker/build-push-action@v2
        with:
          context: .
          push: true
          tags: tu-usuario/nombre-de-la-imagen:latest
```


3. Guarda los cambios y haz commit
4. Configura los secrets en el repositorio:
   - En el repositorio de GitHub, ve a la pestaña "Settings".
   - En el menú lateral izquierdo, selecciona "Secrets".
   - Haz clic en "New repository secret".
   - Configura los siguientes secrets:
   - Nombre del secret: DOCKERHUB_USERNAME
   - Valor: Tu nombre de usuario de DockerHub.
   - Nombre del secret: DOCKERHUB_PASSWORD
   - Valor: Tu contraseña de DockerHub.
   - Haz clic en "Add secret" para guardar los secrets.
5. Realiza cambios en el archivo index.html y ejecuta un commit en el repositorio. El flujo de CI/CD se activará automáticamente y construirá la nueva imagen de Docker con los cambios realizados en el archivo index.html. La imagen actualizada se cargará en tu repositorio de DockerHub.


## Acceso y Uso de Imagen

1. Una vez que el flujo de CI/CD se haya ejecutado correctamente, la nueva imagen de Docker se actualizará en DockerHub. Podrás acceder a ella utilizando el siguiente comando:
```
docker pull tu-usuario/nombre-de-la-imagen:latest
```
(*Reemplaza tu-usuario y nombre-de-la-imagen con tu nombre de usuario y el nombre de la imagen que has utilizado en el flujo de CI/CD.*)

2. Ejecuta el contenedor mapeando el puerto 8080 de tu máquina local al puerto 80 del contenedor:

```
docker run --name Prueba -p 8080:80 -d tu-usuario/nombre-de-la-imagen:latest
```
3. Finalmente podrás acceder a tu página web servida por el contenedor NGINX a través del puerto 8080.
```
http://localhost:8080 
```



## Autor

Este proyecto fue creado por Emanuel Prado Macat.
Cualquier consulta a emapradomacat@gmail.com -
