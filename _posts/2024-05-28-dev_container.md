---

layout: single
title:  "Dev Containers을 이용한 container 생성" # 글 제목
categories: 
  - Docker # category
tags: 
  - [Dev Containers, ] # tag
toc: true 
toc_sticky: true
toc_label: 목차
author_profile: false
# sidebar: # 왼쪽 사이드 바
    # nav: "docs"
search: true 
use_math: False
typore-root-url: ../
---



## Dev Containers

Dockerfile만 정의하면 container 자동을 생성해주는 VSC의 Extension



<br><br>

## Dockerfile을 이용한 Dev Containers 구동

Python 관련 container 생성 예시

<br>



1. Dockerfile 생성

   ```dockerfile
   FROM pytorch/pytorch:2.1.0-cuda11.8-cudnn8-devel 
   RUN pip install -U openmim
   ```

   <br>

2. ****

   Container Configuration File 생성

   - **Ctrl+Shift+P**를 눌러 명령 팔레트 열어 **Dev Containers: Add Dev Container Configuration Files...** 실행
   - **Open a Remote Window** 선택 후 **Add Dev Container Configuration Files...** 실행

   <br>

   ![Screenshot from 2024-05-29 00-08-35](/images/2024-05-28-dev_container/Screenshot from 2024-05-29 00-08-35.png){:.img-display-60}

   <br>

   **Add configuration to workspace** 선택

   ![Screenshot from 2024-05-29 00-09-14](/images/2024-05-28-dev_container/Screenshot from 2024-05-29 00-09-14.png){:.img-display-60}

   - Azure 가상 머신과 연결하여 진행했을 때는 이 과정 없었음

   <br>

   **From 'Dockerfile'** 선택

   ![Screenshot from 2024-05-29 00-09-34](/images/2024-05-28-dev_container/Screenshot from 2024-05-29 00-09-34.png){:.img-display-60}

   <br>

   필요한 feature 선택하고 OK 선택

   ![Screenshot from 2024-05-29 00-09-55](/images/2024-05-28-dev_container/Screenshot from 2024-05-29 00-09-55.png){:.img-display-60}

   - 이 예시에서는 진행하지 않음

   <br>

   모든 과정을 거치면 

   ![Screenshot from 2024-05-28 22-45-07](/images/2024-05-28-dev_container/Screenshot from 2024-05-28 22-45-07.png){:.img-display}

   - `.devcontainer` 폴더에 `devcontainer.json` 설정 파일 생성

   <br>

   생성된 설정 파일의 예시는 아래와 같음

   ```json
   // For format details, see https://aka.ms/devcontainer.json. For config options, see the
   // README at: https://github.com/devcontainers/templates/tree/main/src/docker-existing-dockerfile
   {
   	"name": "Existing Dockerfile",
   	"build": {
   		// Sets the run context to one level up instead of the .devcontainer folder.
   		"context": "..",
   		// Update the 'dockerFile' property if you aren't using the standard 'Dockerfile' filename.
   		"dockerfile": "../Dockerfile"
   	}
   
   	// Features to add to the dev container. More info: https://containers.dev/features.
   	// "features": {},
   
   	// Use 'forwardPorts' to make a list of ports inside the container available locally.
   	// "forwardPorts": [],
   
   	// Uncomment the next line to run commands after the container is created.
   	// "postCreateCommand": "cat /etc/os-release",
   
   	// Configure tool-specific properties.
   	// "customizations": {},
   
   	// Uncomment to connect as an existing user other than the container default. More info: https://aka.ms/dev-containers-non-root.
   	// "remoteUser": "devcontainer"
   }
   
   ```

   <br>

3. `devcontainer.json` 설정 파일을 이용한 container 구동

   - **Ctrl+Shift+P**를 눌러 명령 팔레트 열어 **Dev Containers: Rebuild & Reopen in Container** 실행
   - **Open a Remote Window** 선택 후 **Add Dev Container Configuration Files...** 실행

   <br>

   ![Screenshot from 2024-05-29 00-41-16](/images/2024-05-28-dev_container/Screenshot from 2024-05-29 00-41-16.png){:.img-display}

   <br>

   이 과정을 통해 container 구동

   <br>



### `devcontainer.json` 설정 파일 수정 되는 경우

Container를 생성하고 구동 중에  설정 파일을 수정하는 경우 rebuild 진행



1. `devcontainer.json` 설정 파일 수정

   ```json
   // For format details, see https://aka.ms/devcontainer.json. For config options, see the
   // README at: https://github.com/devcontainers/templates/tree/main/src/docker-existing-dockerfile
   {
   	"name": "Existing Dockerfile",
   	"build": {
   		// Sets the run context to one level up instead of the .devcontainer folder.
   		"context": "..",
   		// Update the 'dockerFile' property if you aren't using the standard 'Dockerfile' filename.
   		"dockerfile": "../Dockerfile"
   	},
   	"runArgs": ["--gpus=all"],
   	"customizations": {
   		"vscode": {
   			"extensions": [
   				"ms-toolsai.jupyter",
   				"github.copilot",
   				"ms-python.python"
   			]
   		}
   	}
   
   	// Features to add to the dev container. More info: https://containers.dev/features.
   	// "features": {},
   
   	// Use 'forwardPorts' to make a list of ports inside the container available locally.
   	// "forwardPorts": [],
   
   	// Uncomment the next line to run commands after the container is created.
   	// "postCreateCommand": "cat /etc/os-release",
   
   	// Configure tool-specific properties.
   	// "customizations": {},
   
   	// Uncomment to connect as an existing user other than the container default. More info: https://aka.ms/dev-containers-non-root.
   	// "remoteUser": "devcontainer"
   }
   
   ```

   - "runArgs", "customizations" 추가

   <br>

2. Rebuild 진행

   - **Ctrl+Shift+P**를 눌러 명령 팔레트 열어 **Dev Containers: Rebuild Container** 실행
   - **Open a Remote Window** 선택 후 **Rebuild Container** 실행

   <br>

   ![Screenshot from 2024-05-29 00-56-16](/images/2024-05-28-dev_container/Screenshot from 2024-05-29 00-56-16.png){:.img-display-60}
