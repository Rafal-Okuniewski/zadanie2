
# Programowanie full-stack w chmurze obliczeniowej - Sprawozdanie 3

Rafał Okuniewski

Nr albumu: 99175


## Zadanie 2
Zadanie podzielone jest na dwie części : obowiązkową i dodatkową (nieobowiązkową)
W zadaniu należy wykorzystać prostą aplikację, przygotowaną w ramach zadania nr 1. (zadanie 1, część obowiązkowa, punkt 1)

### CZĘŚĆ OBOWIĄZKOWA

1. Proszę uruchomić przygotowaną aplikację na platformie AWS, usługa EBS. W tym celu należy wykorzystać przykład przedstawiony na laboratorium nr 3 (pliki: Lab3_AWS.pdf oraz Lab3_AWS_sources.zip).

2. Wdrożenie aplikacji ma byż zrealizowane w oparciu o GitHub Action i załączony przykład pliku konfiguracyjnego, który jest dostępny na moodle w katalogu Zadanie 2 (plik: zad2_GHActions.zip)

```
name: GitHub Action

on:
  push:
    branches: [master]

jobs:
  build-push-images:
    name: Budowa i publikacja obrazu z zadania 2 na repozytorium
    runs-on: ubuntu-latest

    steps:

      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v2
        with:
          platforms: linux/arm/v7,linux/arm64/v8,linux/amd64

      - name: Set up Docker Buildx
        id: buildx
        uses: docker/setup-buildx-action@master

      - name: Login to DockerHub
        uses: docker/login-action@v1 
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_ACCESS_TOKEN }}

      - name: Login to Github Packages
        uses: docker/login-action@v1
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GHCR_PAT }}

      - name: Build and push
        id: docker_build
        uses: docker/build-push-action@v2
        with:
          context: ./
          file: ./Dockerfile
          push: true
          platforms: linux/arm/v7,linux/arm64/v8,linux/amd64
          tags: |
            s99175/zadanie2:v1
            ghcr.io/rafal-okuniewski/zadanie2:v1

      - name: Generate deployment package
        run: zip -r deploy.zip . -x '*.git*'

      - name: Deploy to EB
        uses: einaregilsson/beanstalk-deploy@v20
        with:
          aws_access_key: ${{ secrets.AWS_ACCESS_KEY }}
          aws_secret_key: ${{ secrets.AWS_SECRET_KEY }}
          application_name: Zadanie 2
          environment_name: Zadanie2-env
          existing_bucket_name: elasticbeanstalk-us-east-1-035784104592
          version_label: "docker-app-${{ steps.format-time.outputs.replaced }}"
          region: us-east-1
          deployment_package: deploy.zip
```



3. W sprawozdaniu proszę podać link do repozytorium GitHub oraz link do uruchomionej aplikacji w chmurze AWS.

Repozytorium GitHub: https://github.com/Rafal-Okuniewski/zadanie2

Aplikacja w chmurze AWS: http://zadanie2-env.eba-dq3zwxg2.us-east-1.elasticbeanstalk.com/

### CZĘŚĆ DODATKOWA

Zadanie w tej części polega na rozbudowaniu pliku wdrożenia w GitHub Action. W ramach tego rozszerzenia:

a. GitHub Ations ma zbudować obraz aplikacji i przesłać go na repozytorium DockerHub. Proszę w tym celu wykorzystać wiedzę i konfigurację przygotowaną w ramach zadania nr 1.

```
  - name: Login to DockerHub
    uses: docker/login-action@v1 
    with:
      username: ${{ secrets.DOCKER_HUB_USERNAME }}
      password: ${{ secrets.DOCKER_HUB_ACCESS_TOKEN }}

  - name: Login to Github Packages
    uses: docker/login-action@v1
    with:
      registry: ghcr.io
      username: ${{ github.actor }}
      password: ${{ secrets.GHCR_PAT }}

  - name: Build and push
    id: docker_build
    uses: docker/build-push-action@v2
    with:
      context: ./
      file: ./Dockerfile
      push: true
      platforms: linux/arm/v7,linux/arm64/v8,linux/amd64
      tags: |
        s99175/zadanie2:v1
        ghcr.io/rafal-okuniewski/zadanie2:v1
```


b. Plik Docker Compose ma korzystać z gotowego obrazu (zbudowanego na wcześniejszych etapach pracy GHAction)

```
version: '3'
services:
  web:
    image: s99175/zadanie2:v1
    stdin_open: true
    environment:
      - CHOKIDAR_USEPOLLING=true
    ports:
      - "5000:5000"
```

c. Aplikacja ma zostać wdrożona w chmurze AWS, usługa EBS.
W sprawozdaniu proszę podać link do repozytorium GitHub, DockerHub oraz link do uruchomionej aplikacji w chmurze AWS. 

Repozytorium GitHub: https://github.com/Rafal-Okuniewski/zadanie2

Repozytorium DockerHub: https://hub.docker.com/repository/docker/s99175/zadanie2

Aplikacja w chmurze AWS: http://zadanie2-env.eba-dq3zwxg2.us-east-1.elasticbeanstalk.com/

