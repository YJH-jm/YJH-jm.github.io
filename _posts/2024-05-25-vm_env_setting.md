---

layout: single
title:  "Azure GPU 기반 VM 환경 설정" # 글 제목
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

1. [SSH 키 이용 시 bad permission 발생](https://www.deok.me/entry/SSH-%ED%82%A4-%EC%9D%B4%EC%9A%A9-%EC%8B%9C-bad-permissions-ignore-key-%EC%97%90%EB%9F%AC%EA%B0%80-%EB%B0%9C%EC%83%9D%ED%95%A0-%EA%B2%BD%EC%9A%B0)
2. [NVIDIA GPU-Optimized VMI overview](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/nvidia.ngc_azure_17_11?tab=overview)

<br><br>

# Azure GPU 기반 가상 머신 환경 설정

Azure에서 GPU로 딥러닝 학습, 추론을 하기 위한 환경 설정을 위함

Hardware, Software setting 필요

아래는 설정 예시 중 하나이고 원하는 HW, SW 설정에 따라 변경 가능 

<br>

## Virtual Machine (VM) 생성

1. **가상 머신** 선택

   ![1](/images/2024-05-25-vm_env_setting/1.png)

   <br>

2. 새로운 VM을 만들이 위해 **만들기** > **Azure 가상 머신** 선택

    ![2](/images/2024-05-25-vm_env_setting/2.png)

   <br>

3. 가상 머신 만들기에서 필요한 정보 입력 

   ![3](/images/2024-05-25-vm_env_setting/3.png)

   - **가상 머신 이름** 생성하면 리소스 그룹 이름 자동으로 생성됨
   - **지역** 에 Korea Central 선택
     - Korea South와 Central 두 가지 존재하는데 GPU 사용 가능한 HW 설정을 위해서는 Central 선택

   - **가용성 옵션**은 가용성 영역으로 설정함
   - **가용성 영역**은 지역의 서브 리전
     - 1 영역으로 설정했다가 VMI가 2, 3 영역에서만 사용할 수 있다고 해서 변경
   - **이미지**는 VMI (Virtual Machine Image) 를 의미
     - Nvidia GPU Optimized VMI 선택 
       - ubuntu, docker ,nvidia driver. nvidia toolkit이 모두 설정

    <br>

   ![4](/images/2024-05-25-vm_env_setting/4.png)

   - **Azure Spot 할인** 을 선택
   - **크기** 에서 HW 설정
     - Nvidia에서 제공하는 T4 GPU 모델을 사용하겠다는 의미
     - 또한 4개의 가상의 CPU 사용
     - 여기서 Quotas (할당량) 문제가 발생할 수 있는데 아래 방법으로 진행

   - SSH로 접속하기 위해 해당 설정 완료

   <br>

   위의 과정을 마치면 **다음**을 선택하여 디스크 설정으로 이동

   <br>

   ![5](/images/2024-05-25-vm_env_setting/5.png)

   - **OS 디스크 크기** 선택
   - **OS 디스크 유형** 표준 SSD 선택
   - 다른 값들은 기본 값을 변경하지 않고 사용 

   <br>

   <br>

   위의 과정을 마치면 **다음**을 선택하여 네트워크 설정으로 이동

   <br>

   ![6](/images/2024-05-25-vm_env_setting/6.png)

   - 다음과 같은 내용을 확인

   <br>

   이상이 없으면 **검토 + 만들기** 선택하여 VM 생성

   <br>

   ![7](/images/2024-05-25-vm_env_setting/7.png)

   - 유효성 검사를 통과하면 **만들기** 선택

   <br>

   ![8](/images/2024-05-25-vm_env_setting/8.png){:.img-display-60}

   - **프라이빗 키 다운로드 및 리소스 만들기** 를 클릭하면 key가 다운로드 받아지고 VM 생성됨 

   <br>

   ![9](/images/2024-05-25-vm_env_setting/9.png)

   <br>

4. 생성한 VM 확인

   **리소스 그룹** 선택

   ![10](/images/2024-05-25-vm_env_setting/10.png){:.img-display-60}

   <br>

   새로 생성한 VM이 생성됨을 확인하고 선택

   ![11](/images/2024-05-25-vm_env_setting/11.png)

   <br>

   리소스 그룹의 리소스 확인

   ![12](/images/2024-05-25-vm_env_setting/12.png)

   <br>

5. SSH를 이용하여 VM  접속

   **가상 머신**에서 새로 만든 VM 선택하여 **연결 > 연결** 선택하고 

   원시 SSH 선택하여 접속 방법 확인

   ![13](/images/2024-05-25-vm_env_setting/13.png)

   <br>

   그 설명에 따라 SSH에 접속

   <br>

   ```
   ssh -i <key_file_path> <azureuser_name>@<public_IP_address>
   ```

   - 다운로드 받은 key 파일의 경로, 설정한 azureuser, 공용 IP 주소를 위에 명령어에 넣어 실행

   <br>

   이 때,  아래와 같은 에러가 발생한 경우 

   ```
   @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
   @         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
   @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
   Permissions 0664 for './{key_file_name}.pem' are too open.
   It is required that your private key files are NOT accessible by others.
   This private key will be ignored.
   Load key "./{key_file_name}.pem": bad permissions
   azureuser_jh@4.230.8.74: Permission denied (publickey).
   ```

   <br>

   Key 파일의 권한을 변경하면 해결 

   ```
   chmod 400 ./{key_file_name}.pem
   ```

   <br>

6. 자동 종료 설정 (Option)

   가상머신에서 **개요** > **자동종료** 선택

   ![26](/images/2024-05-25-vm_env_setting/26.png){:.img-display-c}

   <br>

   자동 종료를 원하는 시간 등을 설정하고 **저장** 선택

   ![27](/images/2024-05-25-vm_env_setting/27.png){:.img-display-60}

<br><br>



### 할당량 (Quatas) 로 인한 문제 발생하는 경우

VM을 만들면서 유효성 검사를 통과하지 못해서 VM을 생성할 수 없었음

메세지를 바탕으로 확인해보니 



![14](/images/2024-05-25-vm_env_setting/14.png){:.img-display-c}

Korea Central 지역에서 사용할 수 있는 vCPU의 총 개수는 3개, 지금 사용하는 HW에는 4개의 vCPU 사용하기 때문에 유효성 검사 통과 못함

따라서 4개의 vCPU를 사용할 수 있도록 설정해야 함

조정 가능하기 때문에 **연필 모양** 선택

<br>

![15](/images/2024-05-25-vm_env_setting/15.png){:.img-display-60}

새 한도 값을 설정한 후 **제출** 선택

<br>

![16](/images/2024-05-25-vm_env_setting/16.png){:.img-display-60}

<br>

**Create a support request**를 선택 후 나오는 설정들을 완료하면  support request가 만들어지고 

요청이 승인되면 할당량이 늘어남

<br>

### Azure Spot 할인 

Vender 관리하고 제공하는 리소스들이 존재하고 사용자들은 그 리소스를 대여해서 사용

- Virtual Machine, ... 

spot discount option은 추가적인 리소스가 존재하지 않을 때 

추가적 수요가 발생하면 일반 리소스로 구매한 고객들의 리소스는 완벽하게 보호 

spot discount로 구매한 고객은 완벽하게 보호가 되지 않음 

Spot discount로 구매한 옵션을 지우고 일반 리소스로 바꾸어서 새로운 수요자에게 대여, 공급 

즉, 가격은 저렴하나 신뢰성이 100% 보장이 되지 않음

<br>

세부 옵션 2가지 

- 용량만 (Capacity Only)
  - Azure의 초과 용량이 사라지면 가상 머신이 제거
- 가격 또는 용량
  - 시간 당 지불하려는 최대 가격을 설정
  - 이 가격이 낮은 사람들이 추가적인 수요가 들어오면 우선적으로 제거됨
  - 지불하게 되더라도 일반 가격보다는 저렴
    - 일반 가격보다는 높지 않게 최대 가격을 설정하므로

<br><br>

## Software 확인

VMI에서 제공하는 Software, OS 설치 확인하는 것이 좋음 

아래에는 NVIDIA GPU-OPTIMIZED 에서 제공하는 SW 확인하기 위함

<br>

- OS version 확인

  ```
  lsb_release -a
  ```

  <br>

- Nvidia Driver 확인

  ```
  nvidia-smi
  ```

  <br>

- Docker 설치 확인

  ```
  sudo docker run hello-world
  sudo usermod -aG docker <username>
  ```

​	<br>

- Nvidia Container toolkit 설치 확인

  ```
  docker run --rm --gpus all ubuntu nvidia-smi
  ```

​	<br>

- Git 설치 확인

  ```
  git --version
  ```

  <br>

- Miniconda 설치 확인

  ```
  python3 -V
  ```

  

