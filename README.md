# Apache Spark

## Lab Environment
### Server : Amazon EC2
- Instance Type : m5a.large
- Instance Count : Total 4 EA (Master 1EA / Worker 3EA)
- Core : 2 CPUs
- Memory : 8 GiB
- Disk : 50 GiB
- OS : Ubuntu Server (20.04 LTS), 64 bits (x86)

<br/>

* * *

<br/>

## Set Environment Configuration
- [x] (1) EC2 Instance 생성
- [x] (2) 각 서버에 OS User 생성 및 sudo 권한 설정
- [x] (3) 각 서버에 Utils 설치
- [x] (4) 각 서버에 Host Name 설정
- [x] (5) 각 서버 간 SSH Connection(w/o) 가능하도록 설정
- [x] (6) Apache Spark 설치 및 실행

<br/>

***

<br/>

## Step-by-Step Details

<br/>

### (1) EC2 Instance 생성
- 총 4개의 EC2 Instance 생성
![images/image1.png](images/image1.png)
- SSH Key 파일 저장 및 저장한 경로 기억하기
    - Chrome에서 .pem 파일이 다운로드 되지 않는 이슈 (.cer 파일로 저장됨)
    - Safari에서 .pem 파일 다운로드
- 각각의 서버마다 SSh Client Connection Test
    - `ssh -i "*********.pem" ubuntu@<Public DNS>`

<br/>

### (2) 각 서버에 OS User 생성 및 sudo 권한 설정
- `sudo useradd {OS User Name} -m -s /bin/bash`
    - '-m' : OS User 생성 시, 홈 디렉토리 생성하는 옵션
    - '-s' : OS User가 사용할 Shell 지정
- 계정 전환 시, 패스워드 입력하지 않도록 설정
    - `sudo vi /etc/sudoers`
    - > {OS User Name} ALL=(ALL) NOPASSWD:ALL
- 디렉토리 생성 및 신규 생성한 OS User로 소유자 변경
    - `sudo mkdir {Directory Name}`
    - `sudo chown {Owner: OS User Name}:{Group: OS User Name} {Directory Name}`

<br/>

### (3) 각 서버에 Utils 설치
- wget, unzip, bzip2, net-tools 설치
    - `sudo apt install -y wget unzip bzip2 net-tools`
    - wget : web get의 약어로, web 상의 파일을 다운로드 받을 때 사용
    - unzip : zip으로 압축된 파일을 풀 때 사용
    - bzip2 : 단일 파일을 .bzip2의 확장자로 압축 및 해제할 때 사용
    - net-tools : ifconfig, router 등 네트워크 명령어 포함

<br/>

### (4) 각 서버에 Host Name 설정
- 각 서버 별로 Host Name을 다르게 설정
    - `sudo hostnamectl set-hostname {Master Node Host Name}`
    - `sudo hostnamectl set-hostname {Worker Node 1 Host Name}`
    - `sudo hostnamectl set-hostname {Worker Node 2 Host Name}`
    - `sudo hostnamectl set-hostname {WorkerNode 3 Host Name}`
- 서버 간 통신에 ip 대신 Host Name으로 통신할 수 있도록 구성
    - 각 서버 별로 Private IP 주소를 모두 확인
        - `ifconfig`
    - Host Name으로 IP를 확인할 수 있도록 각 서버 별 아래와 동일한 내용으로 /etc/hosts 파일 편지
```text
{Master Node Private IP} {Master Node Host Name}
{Worker Node 1 Private IP} {Worker Node 1 Host Name}
{Worker Node 2 Private IP} {Worker Node 2 Host Name}
{Worker Node 3 Private IP} {Worker Node 3 Host Name}
```
