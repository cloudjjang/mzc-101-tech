# 실습 : Working with Elastic Load Balancing

## LAB Overview:

이 실습에서는 Elastic Load Balancing의 개념을 소개합니다. 이 실습에서는 Elastic Load Balancing을 사용하여 단일 Availability Zone의 여러 Amazon EC2 (Elastic Compute Cloud) 인스턴스에 트래픽을 로드 밸런싱합니다. 여러 Amazon EC2 인스턴스에 간단한 응용 프로그램을 배포하고 브라우저에서 응용 프로그램을 보고 로드균형을 관찰합니다.

먼저 인스턴스 쌍을 시작하고 부트스트랩하여 웹 서버 및 컨텐트를 설치 한 다음 Amazon EC2 DNS 레코드를 사용하여 인스턴스에 독립적으로 액세스합니다. 그런 다음 Elastic Load Balancing을 설정하고 인스턴스를로드 밸런서에 추가 한 다음 DNS 레코드에 다시 액세스하여 서버간의 요청 로드균형을 관찰합니다. 마지막으로 Amazon CloudWatch에서 Elastic Load Balancing 측정 항목을 볼 수 있습니다.


## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* Amazon EC2에서 다중 서버 웹서버 시작
* 부트스트래핑 기술을 사용하여 Apache, PHP 및 Amazon Simple Storage Service (S3)에서 다운로드 한 간단한 PHP 응용 프로그램으로 Linux 인스턴스를 구성
* Amazon EC2 웹 서버 인스턴스 앞에 로드 밸런서 생성 및 구성
* 로드 밸런서에 대한 Amazon CloudWatch 메트릭보기

## Start Lab:

* 크롬 웹 브라우저를 실행합니다.
* 주소창에 다음 주소를 입력합니다. https://aws.amazon.com 
* 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61859078-5e654000-af02-11e9-9421-0e009586aa95.png)
을 누르고 자신의 계정으로 로그인 합니다.
* Region은 **오레콘(Oregon)**을 선택합니다.

<br>

## 작업 1 : 실습 환경 구성하기

**1. AWS 관리 콘솔에서 서비스를 클릭하고 EC2를 클릭합니다.**

**2. 왼쪽 탐색창에서 “키 페어”를 클릭합니다.**
* 화면 상단의 ![image](https://user-images.githubusercontent.com/48195985/61859173-94a2bf80-af02-11e9-8cd1-ee9f61badf11.png)
을 클릭합니다.
* 키 페어 이름 : mzc-lab-key을 입력하고 ![image](https://user-images.githubusercontent.com/48195985/61859180-9bc9cd80-af02-11e9-837a-ddd76ff9a678.png)
을 누릅니다.
* 생성된 키페어의 pem 파일 다운로드 창이 뜨면 “저장”을 누르고 다운로드 된 mzc-lab-key.pem 파일을 바탕화면에 보관합니다.


**3. 다음을 수행합니다.**
* 주소 : http://bit.ly/2LZr2Y2 을 클릭합니다.
* cf-lab08-alb.yml 파일을 다운로드 합니다.
* 바탕화면에 이 파일을 저장합니다.


**4. AWS 관리콘솔에서 ![image](https://user-images.githubusercontent.com/48195985/61859317-df243c00-af02-11e9-845e-08a9fc85adf5.png)를 누르고 “CloudFormation”을 클릭합니다.**


**5. ![image](https://user-images.githubusercontent.com/48195985/61859362-f4996600-af02-11e9-8833-48c0464f9ef5.png)을 클릭하고 다음으로 구성합니다.**
* 템플릿 준비에서 ![image](https://user-images.githubusercontent.com/48195985/61859413-0c70ea00-af03-11e9-92de-99fe595b5d2d.png)
을 선택합니다.
* 템플릿 지정에서 ![image](https://user-images.githubusercontent.com/48195985/61859432-1397f800-af03-11e9-975f-2a3c7110f29f.png)
를 선택합니다.
* 템플릿 파일 업로드에서 ![image](https://user-images.githubusercontent.com/48195985/61859452-1b579c80-af03-11e9-98d0-a0e95bef62be.png)을 클릭하고 이전에 다운로드 받은 cf-lab08-alb.yml 파일을 엽니다.
* 화면 하단의 ![image](https://user-images.githubusercontent.com/48195985/61859476-26aac800-af03-11e9-9df3-ada8a75919f3.png)을 클릭합니다.


**6. 스택 세부 정보 지정 창에서 다음을 입력합니다.**
* 스택이름 : **mzc-lab**
* ![image](https://user-images.githubusercontent.com/48195985/61859476-26aac800-af03-11e9-9df3-ada8a75919f3.png)을 클릭합니다.


**7. 스택 옵션 구성 창에서 스크롤을 아래로 내려서 ![image](https://user-images.githubusercontent.com/48195985/61859476-26aac800-af03-11e9-9df3-ada8a75919f3.png)을 클릭합니다.**

**8. mzc-lab 검토 창에서 스크롤을 아래로 내려서 ![image](https://user-images.githubusercontent.com/48195985/61859362-f4996600-af02-11e9-8833-48c0464f9ef5.png)을 클릭합니다.**

**9. mzc-lab 스택생성 창에서 “이벤트” 탭을 선택하고 ![image](https://user-images.githubusercontent.com/48195985/61859639-78535280-af03-11e9-9ed6-9c090655e025.png)
 눌러 이벤트를 갱신합니다.** 
> (30초마다 누름)

10. 죄측 창에 스택 상태가 ![image](https://user-images.githubusercontent.com/48195985/61859683-8e611300-af03-11e9-882f-fa3e449e14c4.png)이 될 때까지 기다립니다.
> 최종 완료된 화면은 아래와 같습니다.\
> ![image](https://user-images.githubusercontent.com/48195985/61859705-9ae56b80-af03-11e9-8971-27eb664fb29c.png)

### 실습환경 구성이 완료되었습니다.

<br>

### 실습 토폴로지 : CloudFormation으로 준비된 실습환경입니다.
![image](https://user-images.githubusercontent.com/48195985/61859774-bb152a80-af03-11e9-8b37-4fefe9b2421f.png)

<br>

## 작업 1 : Web Server 시작하기

이 작업에서는 초기화시 Apache PHP 웹 서버와 기본 응용 프로그램이 설치된 두 개의 Amazon Linux EC2 인스턴스를 시작합니다. 또한 Amazon EC2 메타 데이터 서비스를 사용하여 인스턴스를 부트 스트랩하는 간단한 예제를 보여줍니다.

Amazon Machine Images (AMI) 및 인스턴스

Amazon EC2는 소프트웨어 구성(예:운영 체제, 응용 프로그램 서버 및 응용 프로그램)을 포함하는 Amazon Machine Images (AMI) 라고하는 템플릿을 제공합니다. 이 템플릿을 사용하여 클라우드의 가상 서버로 실행되는 AMI의 복사본 인 인스턴스를 시작합니다.

단일 AMI에서 여러 유형의 인스턴스를 실행할 수 있습니다. 인스턴스 유형은 기본적으로 인스턴스에 대한 가상 호스트 컴퓨터의 하드웨어 기능을 결정합니다. 각 인스턴스 유형은 다른 컴퓨팅 및 메모리 기능을 제공합니다. 인스턴스에서 실행하려는 응용 프로그램 또는 소프트웨어에 필요한 메모리 및 컴퓨팅 성능을 기반으로 인스턴스 유형을 선택하십시오. AMI에서 여러 인스턴스를 실행할 수 있습니다.

인스턴스를 중지하거나 종료 할 때까지 또는 실패 할 때까지 인스턴스는 계속 실행됩니다. 인스턴스가 실패하면 AMI에서 새 인스턴스를 시작할 수 있습니다.

인스턴스를 만들 때 인스턴스 유형을 선택하라는 메시지가 표시됩니다. 선택한 인스턴스 유형에 따라 인스턴스에서 사용할 수있는 처리량 및 처리주기가 결정됩니다.


**1. AWS관리 콘솔 상단의  ![image](https://user-images.githubusercontent.com/48195985/61859813-d2ecae80-af03-11e9-9159-e6a481ecfb62.png)메뉴에서 “EC2”를 클릭합니다.**

**2. ![image](https://user-images.githubusercontent.com/48195985/61859847-e4ce5180-af03-11e9-8241-91a215026003.png)을 클릭합니다.**

**3. 첫 번재 행의 Amazon Linux 2 AMI (HVM) 으로 시작되는 이미지에 ![image](https://user-images.githubusercontent.com/48195985/61859861-edbf2300-af03-11e9-9361-5c7a6852a48e.png)
을 누릅니다.**
> 인스턴스 유형 선택창에서 기본값이 t2.micro가 선택되어 있습니다. 

**4. ![image](https://user-images.githubusercontent.com/48195985/61859907-03cce380-af04-11e9-91c8-6817a75d08a1.png)
를 클릭하고 다음을 구성합니다.**
* 인스턴스 수 : **2**
* 네트워크 : **Lab VPC**
* 서브넷 : **Public Subnet 1**

**5.  스크롤을 아래로 내려서 ![image](https://user-images.githubusercontent.com/48195985/61859948-1515f000-af04-11e9-8e9d-6833c2736d8c.png)를 클릭하여 확장합니다.**


**6. 사용자 데이터 항목에 다음 내용을 입력합니다.**

``` bash
#!/bin/sh
yum -y install httpd php
chkconfig httpd on
systemctl start httpd.service
cd /var/www/html
wget https://s3-us-west-2.amazonaws.com/us-west-2-aws-training/awsu-spl/spl-03/scripts/examplefiles-elb.zip
unzip examplefiles-elb.zip
```

> 이렇게하면 인스턴스를 부트 스트랩 할 수 있습니다. 인스턴스가 생성되어 실행될 때 Apache와 PHP 및 이 랩에 필요한 샘플 코드(PHP 스크립트)를 설치합니다. 사용자 데이터는 시작시 액세스 할 수 있는 Amazon 메타 데이터 서비스에 데이터 또는 스크립트를 전달하는 메커니즘을 제공합니다.

> 팁 텍스트를 복사하는 대신 입력하면 Shift + Enter를 눌러 텍스트 상자에 새 행을 만듭니다.

**7. ![image](https://user-images.githubusercontent.com/48195985/61860084-51e1e700-af04-11e9-8420-558115ea7930.png)를 클릭합니다.**
> 기본 스토리지 설정을 사용합니다.

**8. ![image](https://user-images.githubusercontent.com/48195985/61860101-5908f500-af04-11e9-95d0-758f1ae30588.png)를 클릭합니다.**

**9. ![image](https://user-images.githubusercontent.com/48195985/61860116-61f9c680-af04-11e9-97fc-bf462ece3212.png)를 누르고 다음을 구성합니다.**
* 키 : Name
* 값 : MyLBInstances
> 이 이름은 인스턴스로 시작될 때 콘솔에 나타납니다. 복잡한 환경에서 가상머신의 가동 상태를 쉽게 추적 할 수 있습니다. 쉽게 인식하고 기억할 수있는 이름을 사용하십시오.

**10. ![image](https://user-images.githubusercontent.com/48195985/61860194-89509380-af04-11e9-9e92-03ca6e1121e6.png)을 클릭하고 다음을 구성합니다.**
* 보안 그룹 이름 : MyLBInstancesSG
* 설명 : MyLBInstancesSG

> 보안 그룹은 인스턴스의 그룹에 허용 된 트래픽을 제어하는 방화벽 역할을 합니다. Amazon EC2 인스턴스를 실행하면 하나 이상의 보안 그룹에 인스턴스를 할당 할 수 있습니다. 각 보안그룹에 대해 그룹의 인스턴스에 허용 된 인바운드 트래픽을 제어하는 ​​규칙을 추가합니다. 다른 모든 인바운드 트래픽은 삭제됩니다. 보안 그룹에 대한 규칙은 언제든지 수정할 수 있습니다. 새 규칙은 그룹의 모든 기존 및 향후 인스턴스에 대해 자동으로 적용됩니다.


**11. ![image](https://user-images.githubusercontent.com/48195985/61860223-93729200-af04-11e9-8f6e-165f9243aa2e.png)
를 클릭하고 다음을 구성합니다.**
* 유형 : HTTP
* 소스 : 위치 무관
* ![image](https://user-images.githubusercontent.com/48195985/61860258-9ec5bd80-af04-11e9-9fb4-92af2c23335e.png)
을 클릭합니다.
이 화면에서 "보안 그룹 ...이 세계에 개방되어 있습니다"라는 경고가 표시 될 수 있습니다. 이는 앞에서 설명한대로 시스템에 대한 SSH 액세스를 제한하지 않은 결과입니다. 이 실습 목적으로만 사용하므로 경고를 무시할 수 있습니다.


**12. ![image](https://user-images.githubusercontent.com/48195985/61860275-a71df880-af04-11e9-9723-6b2ac3cb6803.png)를 클릭합니다.**

**13. 기존 키 페어 선택 또는 새 키 페어 생성창에서 다음을 구성합니다.**

![image](https://user-images.githubusercontent.com/48195985/61860293-aedd9d00-af04-11e9-8d03-eda22f4cb87b.png)

**14. ![image](https://user-images.githubusercontent.com/48195985/61860324-be5ce600-af04-11e9-9e54-7f98239a2b26.png)을 클릭합니다.**

**15. ![image](https://user-images.githubusercontent.com/48195985/61860341-c452c700-af04-11e9-800c-cef3f3f4954d.png)
를 클릭합니다.**

**16. 인스턴스 상태가 다음과 같이 표시될 때 까지 기다립니다.**

![image](https://user-images.githubusercontent.com/48195985/61860360-cb79d500-af04-11e9-93b9-39919304fb48.png)

> 이는 인스턴스가 현재 완전히 사용할 수 있음을 나타냅니다.

> 이 작업은 몇 분 정도 걸릴 수 있습니다. ![image](https://user-images.githubusercontent.com/48195985/61860416-e0eeff00-af04-11e9-8730-53d0328cf999.png)
(새로고침)을 클릭하여 인스턴스 상태를 새로 고칠 수 있습니다.

<br>

## 작업 2 : 각 웹 서버에 연결하기

이 작업에서는 다음을 수행하게됩니다.

* 두 EC2 인스턴스의 공용 DNS 항목 검색
* 브라우저에서 각 DNS 항목을 가리켜 인스턴스 웹 서버에 액세스하십시오.

모든 Amazon EC2 인스턴스에는 시작시 두 개의 IP 주소, 즉 NAT (Network Address Translation)를 통해 서로 직접 매핑 되는 개인 IP 주소(RFC1918)와 공용 IP 주소가 할당됩니다. 사설 IP 주소는 Amazon EC2 네트워크에서만 접근 할 수 있습니다. 공용 주소는 인터넷을 통해 접근 할 수 있습니다.

또한 Amazon EC2는 내부 DNS 이름 과 개인 및 공용 IP 주소에 각각 매핑 되는 공용 DNS 이름 을 제공합니다. 내부 DNS 이름은 Amazon EC2에서만 해결할 수 있습니다. 공용 DNS 이름은 Amazon EC2 네트워크 외부의 공용 IP 주소와 Amazon EC2 네트워크 내의 개인 IP 주소로 확인됩니다.


**17. 첫 번째 EC2 인스턴스를 선택합니다.**

**18. 설명탭에서 “퍼블릭 DNS(IPv4)”주소를 복사해서 노트패드에 붙여넣습니다.**

> ec2-34-211-15-85.us-west-2.compute.amazonaws.com 유사한 주소입니다.

**19. 새 브라우저를 열고 복사한 퍼블릭 DNS 주소를 주소창에 붙여 넣습니다.** 
> 브라우저에 다음과 같은 화면이 표시됩니다.\
> ![image](https://user-images.githubusercontent.com/48195985/61860495-03811800-af05-11e9-94e0-c7f9d7cdfc02.png)

> 이것은 인스턴스가 시작될 때 설치된 PHP 스크립트에 의해 반환 된 웹 페이지입니다. 이 스크립트는 각 시스템의 메타데이터 서비스를 조사하고 인스턴스ID 및 인스턴스가 실행중인 가용영역 이름을 반환하는 간단한 스크립트입니다.

**20. 두 번째 인스턴스에 대해서도 앞의 두 단계를 반복합니다.**

> 각 시스템은 다른 인스턴스 ID를 표시합니다. 이렇게하면 부하 분산 장치를 앞에 놓았을 때 요청을 처리하는 인스턴스를 식별하는 데 도움이됩니다.

> 브라우저에서 인스턴스에 액세스 할 때 인스턴스 ID 및 가용 영역 대신 오류가 표시되면 몇 분 후에 다시 시도하십시오. 부트스트랩 스크립트가 아직 실행 중이며 아직 웹 서버 및 PHP 응용프로그램 설치 및 시작을 완료하지 않았을 수 있습니다. 오류가 지속되면 인스턴스를 시작할 때 부트스트랩 스크립트를 올 바르게 입력했는지와 보안그룹이 포트 80을 열었는지 확인하십시오.

<br>

## 작업 3 : 로드밸런서 만들기

이제 두 개의 웹 서버가 있습니다. 이제 이 서버 앞에 로드밸런서를 배치해서 사용자에게 둘 다 액세스하고 사용자 요청 간의 균형을 조정할 수 있게 합니다. 이 작업에서는 간단한 HTTP 응용프로그램 부하분산장치를 만듭니다.

**21. 왼쪽 탐색창에서 “로드밸런서”를 클릭합니다.**

**22. ![image](https://user-images.githubusercontent.com/48195985/61861284-7b9c0d80-af06-11e9-9731-6f22fa0d574d.png)
를 클릭합니다.**

**23. 로드밸런서 유형 선택창에서 Application Load Balancer 아래 ![image](https://user-images.githubusercontent.com/48195985/61861306-8c4c8380-af06-11e9-8bc2-c975eb04b5ef.png)
을 클릭하고 다음을 구성합니다.**
* 이름 : **ELB**
* VPC : **Lab VPC**
* 가용 영역 : **모든 가용영역 선택**
* 가용영역 옆 박스에 서브넷은 **Public Subnet**을 선택합니다. (아래와 유사합니다.)
* ![image](https://user-images.githubusercontent.com/48195985/61861334-a0908080-af06-11e9-8f02-c3178fef6fcc.png)
을 클릭합니다.
![image](https://user-images.githubusercontent.com/48195985/61861363-b43be700-af06-11e9-95bd-881fd3230d5c.png)

 
**24. 화면 좌측 하단의 ![image](https://user-images.githubusercontent.com/48195985/61861424-db92b400-af06-11e9-863a-3c268d0f1c98.png)
을 클릭하고 다음을 구성합니다.**
* Default 보안 그룹을 선택을 해제 합니다.
* MyLBInstancesSG 보안 그룹을 선택합니다.
![image](https://user-images.githubusercontent.com/48195985/61861462-ef3e1a80-af06-11e9-89ff-153e44849cbc.png)

**25. ![image](https://user-images.githubusercontent.com/48195985/61861504-0a108f00-af07-11e9-987f-3002fd655e23.png)
을 클릭하고 다음을 구성합니다.**
* 이름 : **myTarget**


**26. ![image](https://user-images.githubusercontent.com/48195985/61861530-185eab00-af07-11e9-8678-b190dc309ff7.png)을 클릭해서 확장하고 다음을 구성합니다.**
* 정상 임계 값 : **3**
* 간격 : **10**
* ![image](https://user-images.githubusercontent.com/48195985/61861553-24e30380-af07-11e9-838e-2cdc8647f345.png)을 클릭합니다.

> ELB는 상태를 결정하기 위해 각 웹 서비스 인스턴스의 ping 경로를 주기적으로 테스트합니다. 200 HTTP 응답 코드는 정상 상태를 나타내고 다른 모든 응답 코드는 비정상 상태를 나타냅니다. 인스턴스가 정상이 아니며 계속되는 수의 검사(비정상적인 임계 값)에 대해 해당 상태로 계속 되면 로드밸런서는 복구 될 때까지 라우팅 대상에서 제외합니다.

> 이 구성에서 ping 경로를 슬래시 ( / ) 로 만들면 이전에 표시된 PHP 생성 페이지인 기본 페이지가 반환됩니다.

**27. 인스턴스 항목에 있는 2개의 인스턴스를 선택하고 ![image](https://user-images.githubusercontent.com/48195985/61861594-3debb480-af07-11e9-8f29-3340830bf913.png)를 클릭합니다.**

![image](https://user-images.githubusercontent.com/48195985/61861608-4512c280-af07-11e9-93a2-eeec3bb860dc.png)

**28. ![image](https://user-images.githubusercontent.com/48195985/61861669-5bb91980-af07-11e9-8e26-e50bdb24af27.png)를 클릭합니다.**


**29. 구성을 검토한 다음 ![image](https://user-images.githubusercontent.com/48195985/61861690-6378be00-af07-11e9-93e7-2e6fb0452005.png)을 클릭합니다.**
> Application Load Balancer가 생성됩니다.


**30. ![image](https://user-images.githubusercontent.com/48195985/61861706-6ecbe980-af07-11e9-8a3c-7c694f8f7f1e.png)를 클릭합니다.**
> 부하 분산 장치를 시작하고 웹 서버를 연결하고 상태 검사를 통과하는데 몇 분이 걸릴 것입니다.


**31. 상태가 Active 로 변경될 때 까지 기다립니다.  화면 우측 상단의 ![image](https://user-images.githubusercontent.com/48195985/61861743-81deb980-af07-11e9-9646-4e33aee2e260.png)을 눌러서 확인합니다.**

> ![image](https://user-images.githubusercontent.com/48195985/61861765-8f943f00-af07-11e9-9f61-7e14dcb44c3f.png)


**32. 설명 탭의 DNS 이름을 노트패드에 복사해서 붙여넣습니다.**

**33. 새 웹브라우저를 열고 DNS이름을 붙여넣고 Enter를 누릅니다.**

**34. 여러번 브라우저를 새로고침합니다.**

> Amazon EC2 인스턴스 ID가 변경되었음을 알 수 있습니다. 즉, 반복되는 응답이 두 개의 다른 웹 서버를 통해 다시 돌아오고 있음을 의미합니다.

> 로드밸런서는 가용영역을 확장 할 수 있으며 수요를 처리하기 위해 필요에 따라 탄력적으로 확장 할 수도 있습니다. 따라서 IP 주소가 아닌 DNS 호스트 이름으로 항상 부하분산 장치에 액세스해야 합니다. 로드밸런서에는 DNS 호스트 이름과 연결된 여러 IP 주소가 있을 수 있습니다.


**35. AWS 관리콘솔의 왼쪽 탐색창에서 “대상 그룹”을 클릭합니다.**
> ![image](https://user-images.githubusercontent.com/48195985/61861797-a33fa580-af07-11e9-8fea-ce41b463562b.png)이 선택되어 있습니다.

**36. 하단창에서 “대상”탭을 클릭하고 등록된 2개의 인스턴스 상태가 Healthy 상태를 확인합니다.**

<br>

## 작업 4 : CloudWatch를 통해 Elastic Load Balancing 메트릭 보기

이 작업에서는 Amazon CloudWatch를 사용하여 ELB를 모니터링합니다. Elastic Load Balancing은 로드밸런서 메트릭을 CloudWatch에 자동으로 보고합니다.

Amazon CloudWatch는 AWS 클라우드 리소스 및 고객이 AWS에서 실행하는 애플리케이션에 대한 모니터링을 제공합니다. 개발자 및 시스템 관리자는 이 도구를 사용하여 메트릭을 수집 및 추적하고 통찰력을 확보하고 즉시 대응하여 응용 프로그램과 비즈니스를 원활하게 운영 할 수 있습니다. CloudWatch는 Amazon EC2 및 Amazon RDS(Relational Database Service) DB 인스턴스와 같은 AWS 리소스를 모니터링하고 애플리케이션 및 서비스에서 생성 된 사용자정의 메트릭을 모니터링 할 수도 있습니다. CloudWatch를 사용하면 리소스 사용, 응용 프로그램 성능 및 운영 상태에 대한 시스템 전반의 가시성을 확보 할 수 있습니다.

Amazon CloudWatch는 신뢰할 수 있고 확장 가능하며 유연한 모니터링 솔루션을 제공하므로 몇 분 안에 시작할 수 있습니다. 더 이상 자신의 모니터링 시스템 및 인프라를 설정, 관리 또는 확장 할 필요가 없습니다. CloudWatch를 사용하면 필요한 만큼 많은 메트릭 데이터를 쉽게 모니터링 할 수 있습니다. CloudWatch를 사용하면 프로그래밍 방식으로 모니터링 데이터를 검색하고 그래프를 보고 알람을 설정하여 문제를 해결하고 추세를 파악하며 클라우드 환경의 상태에 따라 자동화된 작업을 수행 할 수 있습니다.

**37. ![image](https://user-images.githubusercontent.com/48195985/61861834-b81c3900-af07-11e9-845e-9c7c037a6c19.png)에서 CloudWatch를 클릭합니다.**

**38. 왼쪽 탐색창에서 “지표”를 클릭합니다.**

**39. ![image](https://user-images.githubusercontent.com/48195985/61861844-bf434700-af07-11e9-8c97-62ccb138617c.png)
를 선택합니다.**

**40. CloudWatch에 보고된 측정 항목을 탐색합니다.**

> 로드 균형 조정 메트릭에는 대기 시간, 요청 수 및 정상 및 비정상적인 호스트 수가 포함됩니다. 측정 항목은 발생한대로보고되며 CloudWatch에 표시되는 데 몇 분이 걸릴 수 있습니다.

<br>

## 실습을 종료합니다.

**1. 로드밸런서를 삭제합니다.\
2. 대상 그룹을 삭제합니다.\
3. 인스턴스를 삭제합니다.**
