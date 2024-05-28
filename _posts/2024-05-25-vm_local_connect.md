---

layout: single
title:  "Azure 가상 환경 접속" # 글 제목
categories: 
  -  Azure # category
tags: 
  - [] # tag
toc: true 
toc_sticky: true
toc_label: 목차
author_profile: false
# sidebar: # 왼쪽 사이드 바
    # nav: "docs"
search: true 
use_math: false
typore-root-url: ../
---

**참고**

1. [Install and configure xrdp to use Remote Desktop with Ubuntu](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/use-remote-desktop?tabs=azure-cli)

<br><br>

# Azure VM 접속 방법

## SSH 연결 

1. 생성된 가상 머신에 아래와 같은 명령어로 SSH를 통해 접속 가능

   ```
   ssh -i <key_file_path> <azureuser_name>@<public_IP_address>
   ```

   - SSH key가 저장된 파일이 존재해야 함

<br><br>

## Remote Desktop Protocol (RDP) 방식 연결 

1. SSH 방식을 이용하여 가상 머신에 접속

   <br>

2. 가상 머신에 xfce 설치

   ```
   sudo apt-get update
   sudo DEBIAN_FRONTEND=noninteractive apt-get -y install xfce4
   sudo apt install xfce4-session
   ```

   <br>

3. 가상 머신에  xrdp 설치 후 설정

   ```
   sudo apt-get -y install xrdp
   sudo systemctl enable xrdp
   ```

   ```
   sudo adduser xrdp ssl-cert
   ```

   ```
   echo xfce4-session >~/.xsession
   ```

   ```
   sudo service xrdp restart
   ```

   <br>

4. 가상 머신에서 user에게 Password 부여

   ```
   sudo passwd <azureuser>
   ```

   <br>

5. 포트 열기

   XRDP 서비스는 3389 포트 사용하여 로컬 머신에에서 가상 머신으로 접속

   ```
   az vm open-port --resource-group <mResourceGroup_name> --name <VM_name> --port 3389
   ```

   <br>

   이 때,  결과가 아래와 같이 나오게 되면

   ```
   Please run 'az login' to setup account.
   ```

   `az login` 을 진행하고 명령어 다시 입력

   <br>

   포트를 열고 나면 포트를 확인

   ![17](/images/2024-05-25-vm_local_connect/17.png){:.img-display-c}

   - 3389 포트가 열린 것을 확인
   - 22 포트는 SSH 연결을 위한 포트 

   <br>

6. Local Machine (Ubuntu 환경) 에  freerdp tool 설치

   ```
   sudo apt install freerdp2-x11 
   ```

   <br>

7. 로컬 머신에서 가상 머신으로 접속

   ```
   xfreerdp /u:<userid(=username)> /p:<passwd> /v:<ip> 
   ```

​	<br>

​	위와 같은 명령어 수행하면 다음과 같은 GUI 화면이 나오고 이를 통해서 가상 머신 사용 가능

​	![Screenshot from 2024-05-25 20-24-28](/images/2024-05-25-vm_local_connect/Screenshot from 2024-05-25 20-24-28.png){:.img-display-60}

​	

<br><br>

## Remote tunnel 

1. 로컬과 가상 머신에 VSC를 설치하고Extension 설치

   - Remote - Tunnels
   - Dev Containers

   <br>

2. 가상 머신 VSC 설정

   RDP 로 가상 환경에 접속한 후 진행

   <br>

   **Accounts** > **Turn on Remote Tunnel Access** 선택

   ![18](/images/2024-05-25-vm_local_connect/18.png){:.img-display}

   <br>

   **Install as a service** 선택

   ![19](/images/2024-05-25-vm_local_connect/19.png){:.img-display-60}

   - Turn on for this session
     - 가상 머신에서 VSC가 실행 중일 때, 그 VSC 환경으로 로컬 머신에서 접속 가능
   - Install as a service
     - 가상 머신에 VSC가 실행중이지 않더라도 로컬 머신에서 가상 머신의 VSC 실행 가능

   <br>

   **Sign in with Github** 선택하고 로그인 진행

   - 이를 위해 크롬이나 사파리 등 웹 브라우저 설치가 되어있어야 함

   ![20](/images/2024-05-25-vm_local_connect/20.png){:.img-display-60}

   <br>



3. 가상 머신 재부팅

   <br>

4. SSH로 재 연결

   <br>

5. 로컬 머신 VSC 설정

   가상 머신과 연결이 되어 있어야 함

   <br>

   **Open a Remote Window** 선택

   ![21](/images/2024-05-25-vm_local_connect/21.png){:.img-display}

   <br>

   **Connect to Tunnel...** 선택

   ![22](/images/2024-05-25-vm_local_connect/22.png){:.img-display-60}

   <br>

​	**GitHub** 선택

​	![23](/images/2024-05-25-vm_local_connect/23.png){:.img-display-60}

​	<br>

​	원하는 가상 머신 선택

​	![24](/images/2024-05-25-vm_local_connect/24.png){:.img-display-60}

​	<br>

​	**File** > **Open Folder ...** 선택하면 아래와 같이 가상 머신의 username으로 연결 된 것 확인

​	![25](/images/2024-05-25-vm_local_connect/25.png){:.img-display-c}



