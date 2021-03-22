---
title: Instalação
weight: 20
description: >-
 Nesta seção, você vai encontrar todas as orientações para instalar o Horusec-CLI.
---

É possível instalar o Horusec-CLI de 4 maneiras:

1. Instalação Local
2. Instalação Manual 
3. Instalação via Imagem Docker 
4. Instalação via Pipeline

A seguir, você irá entender melhor cada uma delas.

## **Instalação Local**
A instalação feita direto no seu computador é ideal para quem quer usar logo o Horusec, seja fazer análises ou verificar as vulnerabilidades de um projeto.

Confira, a seguir, o comando necessário para instalar o Horusec localmente de acordo com o sistema operacional:

### MAC ou Linux
Para instalar o Horusec nos sistemas MacOS ou Linux, basta rodar o comando abaixo no seu terminal:

```bash
curl -fsSL https://horusec.io/bin/install.sh | bash
```

### Windows
Para instalar o Horusec-CLI no Windows, basta rodar o comando abaixo no seu terminal:

{{% alert color="warning" %}}

No caso do windows, você terá que executar o comando sempre no local onde foi feito o download que está o executável.

{{% /alert %}}

```bash
curl "https://horusec.io/bin/latest/win_x64/horusec.exe" -o "./horusec.exe" && ./horusec.exe version
```


{{% alert color="info" %}}
Caso você precise fazer o download para uma versão e/ou sistema operacional específico, os sistemas que o Horusec suporta são:

- linux_x86
- linux_x64
- mac_x64
- win_x86
- win_x64

👉[**A última versão disponível**](https://horusec.io/bin/version-cli-latest.txt)

👉[**Todas as versões disponíveis** ](https://horusec.io/bin/all-version-cli.txt)

{{% /alert %}}

## **Instalação Manual**

A instalação manual é feita de acordo com o sistema operacional e a versão que deseja fazer download. 

Os links abaixo abaixo um dos links para realizar download da última versão. 

{{% alert color="info" %}}
Caso queira uma versão específica, basta trocar a palavra `latest` no link pela versão que você desejar.
{{% /alert %}}

- Windows x64:

    📥 https://horusec.io/bin/latest/win_x64/horusec.exe

- Windows x64:
  - 📥 https://horusec.io/bin/latest/win_x64/horusec.exe
- Windows x86:
  - 📥 https://horusec.io/bin/latest/win_x86/horusec.exe
- Linux x64:
  - 📥 https://horusec.io/bin/latest/linux_x64/horusec
- Linux x86:
  - 📥 https://horusec.io/bin/latest/linux_x86/horusec
- Mac x64:
  - 📥 https://horusec.io/bin/latest/mac_x64/horusec


👉[**A última versão disponível**](https://horusec.io/bin/version-cli-latest.txt)

👉[**Todas as versões disponíveis** ](https://horusec.io/bin/all-version-cli.txt)


## **Instalação via Imagem Docker**

Esta forma de instalação permite que você realize suas análises por meio de uma imagem docker, que você roda localmente ou utilizando sua pipeline. 

Veja alguns casos de uso:


### **Iniciando imagem com comando run:**

Quando você inicializa a imagem com o comando run, basta executar o Horusec com o comando que você deseja:

```bash
docker run -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd):/src horuszup/horusec-cli:latest horusec start -p /src -P $(pwd)
```

{{% alert color="warning" %}}
Verifique se o ambiente que você está trabalhando permite a criação de um volume bidirecional, pois isso é necessário para usar o Horusec em imagem docker.
{{% /alert %}}

## **Instalação via Pipeline**

Este tipo de instalação garante que a entrega do seu projeto em produção seja segura, já que o Horusec é adicionado à sua pipeline. 

Veja a seguir as formas de instalação considerando diferentes tipos de pipeline:

### Github Actions

```yaml
name: SecurityPipeline

on: [push]

jobs:
  horusec-security:
    name: horusec-security
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v2
    - name: Running Horusec Security
      run: |
        curl -fsSL https://horusec.io/bin/install.sh | bash
        horusec start -p="./" -e="true"
```

### AWS Code Build

* **Environment:**

  * Imagem de ambiente: `Imagem gerenciada`
  * Sistema operational: `Ubuntu`
  * Tempo(s) de execução: `Standard`
  * Imagem: `aws/codebuild/standard:3.0`
  * Versão de imagem:  `Latest`
  * Tipo de ambiente:  `Linux`
  * Habilite esse indicador se quiser criar imagens do Docker ou quiser que suas compilações obtenham privilégios elevados:  `true`

* Buildspec:

```yaml
version: 0.2

phases:
  install:
    runtime-versions:
      docker: 18
  build:
    commands:
      - docker run -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd):/src/horusec horuszup/horusec-cli:latest horusec start -p /src/horusec -P $(pwd)
```

### Circle CI

```yaml
version: 2.1

executors:
  horusec-executor:
    machine:
      image: ubuntu-1604:202004-01

jobs:
  horusec:
    executor: horusec-executor
    steps:
      - checkout
      - run:
          name: Horusec Security Test
          command: |
            curl -fsSL https://horusec.io/bin/install.sh | bash
            horusec start -p="./" -e="true"
workflows:
  pipeline:
    jobs:
      - horusec
```

### Jenkins

```groovy
stages {
        stage('Security') {
            agent {
                docker { image 'docker:dind' }
            }
            steps {
                sh 'curl -fsSL https://horusec.io/bin/install.sh | bash'
                sh 'horusec start -p="./" -e="true"'
            }
        }
    }
```

### Azure DevOps Pipeline

```yaml
pool:
  vmImage: 'ubuntu-18.04'

steps:
- script: curl -fsSL https://horusec.io/bin/install.sh | bash && horusec start -p ./
```

### GitLab CI/CD

```yaml
image: docker:latest

services:
  - docker:dind

build-code-job:
  stage: build
  script:
    - docker run -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd):/src/horusec horuszup/horusec-cli:latest horusec start -p /src/horusec -P $(pwd)
```