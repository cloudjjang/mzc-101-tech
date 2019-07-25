# 실습 : Introduction to Amazon EC2

## LAB Overview:
이 실습에서는 Amazon EC2 인스턴스의 시작, 크기 조정, 관리 및 모니터링에 대한 기본적인 개요를 제공합니다.

Amazon Elastic Compute Cloud(Amazon EC2)는 클라우드에서 크기 조정 가능한 컴퓨팅 용량을 제공하는 웹 서비스입니다. 개발자가 웹 규모의 클라우드 컴퓨팅을 보다 쉽게 ​​수행 할 수 있도록 설계되었습니다.

Amazon EC2의 간단한 웹 서비스 인터페이스를 사용하면 최소한의 마찰로 용량을 확보하고 구성 할 수 있습니다. 컴퓨팅 리소스를 완벽하게 제어하고 Amazon의 검증된 컴퓨팅 환경에서 실행할 수 있습니다. Amazon EC2를 사용하면 새 서버 인스턴스를 얻고 부팅하는데 필요한 시간이 단 몇 분으로 단축되므로 컴퓨팅 요구 사항이 변함에 따라 용량을 위아래로 빠르게 확장 할 수 있습니다.

Amazon EC2는 실제로 사용하는 용량에 대해서만 비용을 지불함으로써 컴퓨팅의 경제성을 바꿉니다. Amazon EC2는 개발자에게 장애 복구 애플리케이션을 구축하고 일반적인 오류 시나리오로부터 격리 할 수있는 도구를 제공합니다.



## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* 종단 보호가 활성화 된 웹 서버 실행
* EC2 인스턴스 모니터링
* 웹 서버가 HTTP 액세스를 허용하기 위해 사용하는 보안 그룹 수정
* 규모에 맞게 Amazon EC2 인스턴스 크기 조정
* EC2 한계 탐색
* 테스트 종료 보호
* EC2 인스턴스 종료


## Start Lab:

* 크롬 웹 브라우저를 실행합니다.
* 주소창에 다음 주소를 입력합니다. https://aws.amazon.com 
* 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61838147-f7746680-aec2-11e9-902f-51a10480e513.png)을 누르고 자신의 계정으로 로그인 합니다.

<br>
<br>

### 작업 1 : Amazon EC2 인스턴스 시작

이 작업에서는 종료 보호 기능이 있는 Amazon EC2 인스턴스를 시작합니다. 종단 보호는 실수로 EC2 인스턴스를 종료하지 못하게 합니다. 간단한 웹 서버를 배포 할 수 있는 사용자 데이터 스크립트로 인스턴스를 배포합니다.


**1. AWS 관리 콘솔에서 ![image](https://user-images.githubusercontent.com/48195985/61838173-06f3af80-aec3-11e9-8b47-331d955d945a.png)를 클릭하고 EC2를 클릭합니다.**

**2. ![image](https://user-images.githubusercontent.com/48195985/61838182-1541cb80-aec3-11e9-824e-1b0bd0b246e1.png)을 클릭합니다.**

<br>

### 단계 1 : Amazon Machine Image (AMI) 선택

![image](https://user-images.githubusercontent.com/48195985/61838219-3d312f00-aec3-11e9-88f2-3f5f78011755.png)

아마존 머신 이미지 (AMI)는 클라우드에서 가상 서버인 인스턴스를 실행하는 데 필요한 정보를 제공합니다. AMI는 다음을 포함합니다 :

* 인스턴스의 루트 볼륨에 대한 템플릿 (예 : 응용 프로그램이 있는 운영 체제 또는 응용 프로그램 서버)
* 인스턴스를 시작하기 위해 AMI를 사용할 수 있는 AWS 계정을 제어하는 ​​실행 권한
* 인스턴스가 시작될 때 첨부 할 볼륨을 지정하는 블록 장치 매핑

“빠른 시작” 목록은 가장 일반적으로 사용되는 AMI에 포함되어 있습니다. AWS에서 실행되는 소프트웨어를 판매하거나 구입할 수 있는 온라인 상점인 AWS Marketplace에서 직접 AMI를 만들거나 AMI를 선택할 수도 있습니다.

**3. Amazon Linux 2 AMI 옆에 있는 ![image](https://user-images.githubusercontent.com/48195985/61838289-8c775f80-aec3-11e9-8463-a67c98d0cf3b.png)을 클릭합니다.**

<br>

### 단계 2 : 인스턴스 유형 선택

Amazon EC2는 다양한 사용 사례에 맞게 최적화된 다양한 인스턴스 유형을 제공합니다. 인스턴스 유형은 CPU, 메모리, 스토리지 및 네트워킹 용량의 다양한 조합으로 구성되며 응용 프로그램에 적합한 자원 혼합을 선택할 수 있는 유연성을 제공합니다. 각 인스턴스 유형에는 하나 이상의 인스턴스 크기가 포함되므로 대상 워크로드의 요구사항에 맞게 리소스를 확장 할 수 있습니다.

![image](https://user-images.githubusercontent.com/48195985/61838310-a618a700-aec3-11e9-91de-37f5b9f2d2cb.png)

t2.micro 인스턴스를 선택합니다. 기본적으로 이 인스턴스 유형에는 가상 CPU 1개와 메모리 1GiB를 할당합니다.

**4. 화면 오른쪽 하단에 ![image](https://user-images.githubusercontent.com/48195985/61838349-d82a0900-aec3-11e9-8d50-e21adad11cf9.png)을 클릭합니다.**

<br>

### 단계 3 : 인스턴스 세부 정보 구성

이 페이지는 요구사항에 맞게 인스턴스를 구성하는 데 사용됩니다. 여기에는 네트워킹 및 모니터링 설정이 포함됩니다.

가상 사설 클라우드(VPC)는 인스턴스를 실행하고자 하는 네트워크를 분리합니다. 개발, 테스트 및 생산 조직을 다른 네트워크에 배치할 수 있습니다.

**5. “네트워크” 영역에 “Lab VPC”를 선택합니다.**

  > Lab VPC는 ​​실험실 설치 과정에서 CloudFormation 템플릿을 사용하여 만들어졌습니다. 이 VPC에는 두 개의 다른 가용 영역에 두 개의 공용 서브넷이 포함되어 있습니다.

**6. ![image](https://user-images.githubusercontent.com/48195985/61838370-ec6e0600-aec3-11e9-9f99-049db6866144.png)에  ![image](https://user-images.githubusercontent.com/48195985/61838380-f6900480-aec3-11e9-88ac-89adda959df3.png)을 선택합니다.**

  > Amazon EC2 인스턴스가 더 이상 필요하지 않으면 종료 될 수 있습니다. 즉, 인스턴스가 중지되고 해당 리소스가 해제됩니다. 종료 된 인스턴스는 다시 시작할 수 없습니다. 인스턴스가 실수로 종료되는 것을 방지하려면 인스턴스의 종료 보호를 사용 가능으로 설정하여 인스턴스가 종료되지 않도록 하십시오.

**7. 스크롤을 아래로 내려 를 클릭해서 펼치면 “사용자 데이터” 필드가 나타납니다.**

  > 인스턴스를 실행하면 사용자 데이터를 인스턴스에 전달하여 일반적인 자동화 된 구성 작업을 수행하고 인스턴스가 시작된 후에도 스크립트를 실행할 수 있습니다.

  > 인스턴스에 Amazon Linux가 실행 중이므로 인스턴스가 시작될 때 실행할 쉘 스크립트를 제공합니다.

**8. 다음 명령을 사용자 데이터 필드에 입력합니다.**

``` bash
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```

**스크립트는 다음을 수행합니다.**
* Apache 웹 서버 (httpd) 설치
* 부팅 할 때 자동으로 시작되도록 웹 서버 구성
* 웹 서버 활성화
* 간단한 웹 페이지 만들기

![image](https://user-images.githubusercontent.com/48195985/61838635-27bd0480-aec5-11e9-8d59-6e6909794aee.png)

**9. 화면 오른쪽 하단에 ![image](https://user-images.githubusercontent.com/48195985/61838641-36a3b700-aec5-11e9-8fef-a53e19d2df0a.png)를 클릭합니다.**

<br>

### 단계 4 : 스토리지 추가

Amazon EC2는 Elastic Block Store 라는 네트워크 연결 가상 디스크에 데이터를 저장 합니다.

기본 8 GiB 디스크 볼륨을 사용하여 Amazon EC2 인스턴스를 시작합니다. 이것은 루트 볼륨 ( "부트"볼륨이라고도 함)입니다.

**10. 화면 오른쪽 하단의 ![image](https://user-images.githubusercontent.com/48195985/61838685-65ba2880-aec5-11e9-836b-afb1024be00d.png)를 클릭합니다.**

<br>

### 단계 5 : 태그 추가

태그를 사용하면 목적, 소유자 또는 환경과 같은 다양한 방법으로 AWS 리소스를 분류 할 수 있습니다. 이는 동일한 유형의 자원이 많은 경우에 유용합니다. 할당된 태그를 기반으로 특정 자원을 신속하게 식별 할 수 있습니다. 각 태그는 사용자가 정의한 키와 값으로 구성됩니다.

**11. ![image](https://user-images.githubusercontent.com/48195985/61838713-82eef700-aec5-11e9-8650-22b012d8b553.png)를 클릭한 후 다음 정보를 입력합니다.**
* 키 : Name
* 값 : Web Server

**12. 화면 오른쪽 하단의 ![image](https://user-images.githubusercontent.com/48195985/61838717-8c785f00-aec5-11e9-91a6-9d57b9d140d4.png)
을 클릭합니다.**

<br>

### 단계 6 : 보안 그룹 구성

보안 그룹은 하나 이상의 인스턴스에 대한 트래픽을 제어하는 가상 방화벽 역할을 합니다. 인스턴스를 시작하면 하나 이상의 보안 그룹을 인스턴스와 연결시킵니다. 연결된 인스턴스간에 트래픽을 허용하는 규칙을 각 보안 그룹에 추가 합니다. 보안 그룹에 대한 규칙은 언제든지 수정할 수 있습니다. 새 규칙은 보안 그룹과 연관된 모든 인스턴스에 자동으로 적용됩니다.

**13. 보안 그룹 구성에 다음을 구성합니다.**
* 보안 그룹 할당 : 새 보안 그룹 생성 선택
* 보안 그룹 이름 : Web Server Security Group
* 설명 : Security group for my web server

![image](https://user-images.githubusercontent.com/48195985/61838739-a5811000-aec5-11e9-976b-178a65cd9bb3.png)


이 실습에서는 SSH를 사용하여 인스턴스에 로그인하지 않습니다. SSH 액세스를 제거하면 인스턴스 보안이 향상됩니다.

**14. 기존 SSH Role을 제거 합니다.**

  > SSH 규칙의 가장 오른쪽에 ![image](https://user-images.githubusercontent.com/48195985/61838757-ce090a00-aec5-11e9-8401-95e56348de78.png)을 클릭해서 규칙을 삭제 합니다.

**15. 화면 오른쪽 하단의 ![image](https://user-images.githubusercontent.com/48195985/61838764-d95c3580-aec5-11e9-950a-0ee579e9c09b.png)을 클릭합니다.**

<br>

### 단계 7 : 인스턴스 시작 검토

검토 페이지에는 시작하려고 하는 인스턴스의 구성이 표시됩니다.

**16. 화면 오른쪽 하단의 ![image](https://user-images.githubusercontent.com/48195985/61838798-f3961380-aec5-11e9-8c8a-da8a9878bd69.png)를 클릭합니다.**

  > “기존 키 페어 선택 또는 새 키 페어 생성“창이 나타납니다. 

  > Amazon EC2는 공개 키 암호화를 사용하여 로그인 정보를 암호화하고 해독합니다. 인스턴스에 로그인하려면 키 쌍을 작성하고 인스턴스를 시작할 때 키 쌍의 이름을 지정하며 인스턴스에 연결할 때 개인 키를 제공 해야 합니다.

  > 이 실습에서는 인스턴스에 로그인하지 않으므로 키 쌍이 필요하지 않습니다.


**17. ”기존 키 페어 선택“창을 클릭하고 드롭-다운된 목록에서 ”키 페어 없이 계속“을 선택합니다.**

**18. ![image](https://user-images.githubusercontent.com/48195985/61838829-11637880-aec6-11e9-9d74-82bbc3e1386a.png).....을 체크 합니다. (아래 이미지 참고)**

![image](https://user-images.githubusercontent.com/48195985/61839062-14ab3400-aec7-11e9-85e9-c12a4c0ce2d9.png)

**19. ![image](https://user-images.githubusercontent.com/48195985/61839073-24c31380-aec7-11e9-806d-9bf183743e2a.png)을 클릭합니다.**
인스턴스가 시작됩니다.

**20. ![image](https://user-images.githubusercontent.com/48195985/61839085-2ab8f480-aec7-11e9-9d56-2fb4cc1ef589.png)를 클릭합니다.**

  > 인스턴스가 보류 중 상태로 나타나면 인스턴스가 시작되고 있음을 나타냅니다. 그런 다음 인스턴스가 부팅을 시작 했음을 나타내는 ![image](https://user-images.githubusercontent.com/48195985/61839121-4d4b0d80-aec7-11e9-9991-79e86f5fca42.png)으로 변경됩니다. 인스턴스에 액세스하기 전에 잠깐 시간이 걸릴 것입니다.
  
  > 인스턴스는 인터넷에서 인스턴스에 연결하는 데 사용할 수 있는 공용 DNS 이름을 받습니다.

  > 생성된 Web Server 인스턴스를 선택해야 합니다. 설명탭은 인스턴스에 대한 자세한 정보를 표시합니다.

  > 설명 탭에 표시된 정보를 검토 하십시오. 여기에는 인스턴스 유형, 보안 설정 및 네트워크 설정에 대한 정보가 포함됩니다.


**21. 인스턴스가 다음과 같이 표시될 때 까지 기다립니다.**

  > 인스턴스 상태 : ![image](https://user-images.githubusercontent.com/48195985/61839121-4d4b0d80-aec7-11e9-9991-79e86f5fca42.png)\
  > 상태 검사 : ![image](https://user-images.githubusercontent.com/48195985/61839184-9c913e00-aec7-11e9-954e-3ff60ec7dd59.png)

**<span style="color:red"> 축하합니다. 첫 번째 Amazon EC2 인스턴스를 성공적으로 시작했습니다.**</span>

<br>

### 작업 2 : 인스턴스 모니터링

모니터링은 Amazon EC2(Amazon Elastic Compute Cloud) 인스턴스 및 AWS 솔루션의 안정성, 가용성 및 성능을 유지 관리하는 중요한 부분입니다.


**22.  ”상태 검사“ 탭을 클릭 하십시오.**

  > 인스턴스 상태 모니터링을 사용하면 인스턴스가 응용 프로그램을 실행하지 못하게 하는 문제를 Amazon EC2에서 감지했는지 여부를 신속하게 확인할 수 있습니다. Amazon EC2는 실행중인 모든 EC2 인스턴스에 대해 자동화된 검사를 수행하여 하드웨어 및 소프트웨어 문제를 식별합니다.

  > 시스템 상태 검사 와 인스턴스 상태 검사가 모두 통과되었는지 확인합니다.

**23. ”모니터링“ 탭을 클릭 하십시오.**

  > 이 탭에는 인스턴스에 대한 CloudWatch 메트릭이 표시됩니다. 현재 인스턴스가 최근에 시작되었기 때문에 표시할 메트릭이 많지 않습니다.

  > 그래프를 클릭하면 확장된 보기를 볼 수 있습니다.

  > Amazon EC2는 Amazon CloudWatch에 EC2 인스턴스에 대한 메트릭을 보냅니다. 기본 (5분) 모니터링은 기본적으로 사용됩니다. 상세한 (1분) 모니터링을 활성화 할 수 있습니다.


**24. 화면 좌측 상단의 ![image](https://user-images.githubusercontent.com/48195985/61839314-15909580-aec8-11e9-8157-7faea0493b00.png)메뉴에서 ”인스턴스 설정“을 선택하고 ”시스템 로그 가져오기“를 클릭합니다.**

  > 시스템 로그는 문제 진단을 위한 유용한 도구인 인스턴스의 콘솔 출력을 표시합니다. 특히 SSH 데몬을 시작하기 전에 인스턴스가 종료되거나 도달 할 수 있는 커널 문제 및 서비스 구성 문제를 해결하는데 유용 합니다. **시스템 로그가 보이지 않으면 몇 분 후에 다시 시도하십시오.**


**25. 출력창을 아래로 스크롤해서 인스턴스를 만들 때 추가한 ”사용자 데이터“에서 HTTP 패키지가 설치되었는지 확인 합니다.**

![image](https://user-images.githubusercontent.com/48195985/61839357-36f18180-aec8-11e9-9904-457af00e5f78.png)

26. ![image](https://user-images.githubusercontent.com/48195985/61839372-41ac1680-aec8-11e9-9c0b-2488a13d45f6.png)
를 클릭합니다.

27. ![image](https://user-images.githubusercontent.com/48195985/61839391-4c66ab80-aec8-11e9-8203-320094a199d6.png)을 클릭하고 ”인스턴스 설정“에 ”인스턴스 스크린샷 가져오기“를 클릭합니다.

  > 이 화면은 화면이 연결된 경우 Amazon EC2 인스턴스 콘솔의 모습을 보여줍니다.

  > ![image](https://user-images.githubusercontent.com/48195985/61839406-5ab4c780-aec8-11e9-8209-ec5ec716b083.png)

  > SSH 또는 RDP를 통해 인스턴스에 연결할 수 없는 경우 인스턴스의 스크린샷을 캡처하여 이미지로 볼 수 있습니다. 이를 통해 인스턴스 상태에 대한 가시성을 확보하고 보다 신속하게 문제를 해결할 수 있습니다.

28. ![image](https://user-images.githubusercontent.com/48195985/61839427-6e602e00-aec8-11e9-9604-cd66f3a9fdab.png)를 클릭합니다.

<br>

### 작업 3 : 보안 그룹 업데이트 및 웹 서버 액세스

EC2 인스턴스를 시작할 때 웹 서버를 설치하고 간단한 웹 페이지를 작성한 스크립트를 제공했습니다. 이 작업에서는 웹 서버의 컨텐츠에 액세스합니다.

**29. ”설명“ 탭을 클릭합니다.**

**30. 인스턴스의 “IPv4 공인 IP” 주소를 클립보드로 복사합니다.**

**31. 웹 브라우저에서 새탭을 열고 방금 복사한 IP 주소를 붙여 넣은 다음 Enter를 누릅니다.**

  > 질문 : 웹 서버에 액세스 할 수 있습니까? 없으면 그이유는 무엇일까요?

  > 인스턴스를 생성할 때 보안그룹(SecurityGroup)에서 HTTP 트래픽을 허용하는 규칙을 만들지 않고 진행했습니다. 인스턴스 방화벽인 보안그룹의 사용으로 트래픽이 허용되지 않기 때문입니다.

  > 이 문제를 해결하기 위해서 HTTP 트래픽을 허용하도록 보안그룹을 업데이트 합니다.

**32. “페이지 읽기 오류” 페이지는 열어둔 상태에서 “EC2 관리 콘솔” 탭으로 돌아갑니다.**

**33. 왼쪽 탐색창에서 “보안 그룹”을 클릭합니다.**

**34. 그룹이름이 “Web Server Security Group”을 선택합니다.**

  > ![image](https://user-images.githubusercontent.com/48195985/61839497-b8491400-aec8-11e9-9f7c-641f4dd40d2a.png)

**35. 인바운드 탭을 클릭합니다.**

  > 현재 선택된 “Web Server Security Group” 보안그룹은 보안정책이 아무것도 없는 상태입니다.

**36. ![image](https://user-images.githubusercontent.com/48195985/61839796-e4b16000-aec9-11e9-8829-2c9579d4c3e9.png)을 클릭하고 다음을 구성합니다.**
  > * 유형 : HTTP
  > * 소스 : 위치 무관

  > 새 인바운드 HTTP 규칙은 IPv4 IP 주소 (0.0.0.0/0)와 IPv6 IP 주소 (::/0) 항목을 만듭니다.

  > 참고 : "위치 무관“ 또는 ”사용자지정“으로 0.0.0.0/0 또는 ::/0을 사용하는 것은 프로덕션 작업 부하에 권장되는 최선의 방법은 아닙니다.


**37. 이전에 열고 에러난 페이지로 돌아가 ”새로고침“ 합니다.**
  > Hello From Your Web Server! 라는 메시지가 나타납니다.

<br>

### 작업 4 : 인스턴스 크기 조정 : 인스턴스 유형 및 EBS 볼륨

요구 사항이 변하면 인스턴스가 과도하게 (너무 작게) 또는 과소 사용 (너무 많음) 될 수 있습니다. 그렇다면 인스턴스 유형을 변경할 수 있습니다. 예를 들어 t2.micro 인스턴스가 작업 부하에 비해 용량이 너무 작은 경우 더 많은 용량을 갖는 m5.medium으로 변경할 수 있습니다. 마찬가지로 디스크 크기도 변경할 수 있습니다.


### 인스턴스 중지

인스턴스 크기를 변경하기 전에 인스턴스를 중지해야 합니다.

인스턴스를 중지하면 종료됩니다. 중지된 EC2 인스턴스에는 요금이 부과되지 않지만 연결된 Amazon EBS 볼륨의 저장 비용은 그대로 유지됩니다.

**38. EC2 관리 콘솔에서 “인스턴스”를 클릭합니다.**
  > Web Server 인스턴스를 선택합니다. \
  > ![image](https://user-images.githubusercontent.com/48195985/61839886-407be900-aeca-11e9-8a1c-0bb2d9dc7e3f.png)

**39.  ![image](https://user-images.githubusercontent.com/48195985/61839909-55f11300-aeca-11e9-926e-362ee069c55a.png)메뉴를 클릭하고 “인스턴스 상태” ‣ “중지”를 선택합니다.**

**40. 인스턴스 중지 확인창에서 ![image](https://user-images.githubusercontent.com/48195985/61839925-60131180-aeca-11e9-9473-e477a22d9888.png)를 클릭합니다.**
  > 인스턴스가 정상적으로 종료되고 실행이 중지됩니다.

**41. 인스턴스 상태가  ![image](https://user-images.githubusercontent.com/48195985/61839931-6ef9c400-aeca-11e9-8f1d-b44af4b7dd2e.png)가 될 때 까지 기다립니다.**

### 인스턴스 타입 변경

**41. ![image](https://user-images.githubusercontent.com/48195985/61839947-889b0b80-aeca-11e9-833a-c3b0cbb33b08.png)메뉴를 클릭하고 “인스턴스 설정” ▶ “인스턴스 유형 변경”을 선택하고 다음과 같이 구성합니다.**
  > * 인스턴스 유형 : t2.small
  > * ![image](https://user-images.githubusercontent.com/48195985/61839973-9ea8cc00-aeca-11e9-93b9-067710b8b879.png)을 클릭합니다.

  > 인스턴스가 다시 시작되면 t2.small이 되며 t2.micro 인스턴스의 메모리가 두배가 됩니다.


### EBS 볼륨 조정하기

**42. 왼쪽 탐색창에서 “볼륨”을 클릭합니다.**

**43.  ![image](https://user-images.githubusercontent.com/48195985/61840013-c5ff9900-aeca-11e9-86e7-724bdee8f57f.png)메뉴에서 “볼륨 수정”을 선택합니다.**
  > 현재 디스크 볼륨의 크기는 8GiB 입니다. 이제 이 디스크의 크기를 늘릴 것입니다.

**44. 변경할 디스크 크기 : 10**

**45. ![image](https://user-images.githubusercontent.com/48195985/61840036-da439600-aeca-11e9-8af6-3c15c683acf4.png)을 클릭합니다.**

**46. 볼륨 수정 창에서 ![image](https://user-images.githubusercontent.com/48195985/61840027-ce57d400-aeca-11e9-90d3-a5bbe042856d.png)를 클릭합니다.**

**47. 볼륨 수정이 요청 성공 메시지 창에서 ![image](https://user-images.githubusercontent.com/48195985/61840043-e4fe2b00-aeca-11e9-8eb4-da1b02c1a978.png)를 클릭합니다.**

**48. 화면 오른쪽 상단의  ![image](https://user-images.githubusercontent.com/48195985/61840080-f9422800-aeca-11e9-9567-1733904d904f.png)새로고침을 눌러 볼륨의 크기가 10GiB로 변경되었는지 확인합니다.**

![image](https://user-images.githubusercontent.com/48195985/61840108-0a8b3480-aecb-11e9-8e85-f29839259b4a.png)


### 크기조정된 인스턴스 시작

이제 인스턴스를 다시 시작합니다. 이제는 더 많은 메모리와 디스크 공간이 생깁니다.

**49. 왼쪽 탐색창에서 “인스턴스”를 클릭합니다.**

**50. “Web Server” 인스턴스를 선택합니다.**

**51.  ![image](https://user-images.githubusercontent.com/48195985/61840152-34dcf200-aecb-11e9-92fe-9a2b3c522ba9.png)메뉴를 클릭하고 “인스턴스 상태” ▶ “시작”을 선택합니다.**

**52. ![image](https://user-images.githubusercontent.com/48195985/61840167-432b0e00-aecb-11e9-8101-a0cd70e55c5e.png)을 클릭합니다.
참고 : 수정되는 EBS 볼륨은 상태 수정, 최적화 및 최종 완료 시퀀스를 거칩니다.**

<br>

### 작업 5 : EC2 한계 탐색

Amazon EC2는 사용할 수 있는 다양한 리소스를 제공합니다. 이러한 리소스에는 이미지, 인스턴스, 볼륨 및 스냅 샷이 포함됩니다. AWS 계정을 만들 때 지역별로 이러한 리소스에 대한 기본 제한이 있습니다.

**53. 왼쪽 탐색창에서 “제한”을 클릭합니다.**

**54. t2.small 인스턴스를 사용 중이므로 목록을 아래로 스크롤하여 “t2.small 인스턴스 온디맨드 실행 중”을 찾습니다. 현재 Region의 현재 제한 수가 표시됩니다.**

  > 참고 : 이 영역에서 실행할 수 있는 인스턴스 수에는 제한이 있습니다. 인스턴스를 시작할 때 요청으로 인해 해당 영역에서 현재 인스턴스 한계를 초과하지 않아야 합니다.

  > 이러한 한도는 “제한 증가 요청”을 통해 증가 요청을 할 수 있습니다.

<br>

### 작업 6 : 테스트 종료 보호

더 이상 필요 없는 인스턴스는 삭제할 수 있습니다. 이를 인스턴스 종료라고 합니다. 인스턴스가 종료 된 후에는 인스턴스에 연결하거나 인스턴스를 다시 시작할 수 없습니다.

이 작업에서는 종료 방지 기능의 사용방법을 배웁니다.

**55. 왼쪽 탐색창에서 “인스턴스”를 클릭합니다.**

**56. 화면 상단의  ![image](https://user-images.githubusercontent.com/48195985/61840264-8f764e00-aecb-11e9-9181-4e12184e1941.png)메뉴에서 “인스턴스 상태” ▶ “종료”를 선택합니다.**
  > 다음과 같은 메시지가 나타납니다.\
  > “이 인스턴스는 종료 방지 기능이 있으므로 종료할 수 없습니다. [인스턴스] 화면의 [작업] 메뉴에 있는 [종료 방지 기능 변경] 옵션을 사용하여 이 인스턴스의 종료를 허용할 수 있습니다.”\
 ![image](https://user-images.githubusercontent.com/48195985/61840313-ad43b300-aecb-11e9-8d8c-b50f48461017.png)버튼이 흐리게 표시되어 있어 클릭할 수 없습니다.

**57. “취소”를 클릭합니다.**

58.  ![image](https://user-images.githubusercontent.com/48195985/61840152-34dcf200-aecb-11e9-92fe-9a2b3c522ba9.png)메뉴를 클릭하고 “인스턴스 설정” ▶ “종료 방지 기능 변경”을 선택합니다.

59. “종료 방지 기능 비활성화” 창이 나타나면 ![image](https://user-images.githubusercontent.com/48195985/61840638-ce58d380-aecc-11e9-8d65-e37c71d70783.png)를 클릭합니다.
이제 인스턴스를 종료할 수 있습니다.

60.  ![image](https://user-images.githubusercontent.com/48195985/61840152-34dcf200-aecb-11e9-92fe-9a2b3c522ba9.png)메뉴에서 “인스턴스 상태” ▶ “종료”를 선택합니다.

61. ![image](https://user-images.githubusercontent.com/48195985/61840698-f9432780-aecc-11e9-9f07-c352b5451ab1.png)를 클릭합니다.

<br>

**<span style="color:red">수고하셨습니다. 실습을 완료하셨습니다.</span>**


**참고 :**
* 인스턴스가 종료되는지 확인합니다.
* CloudFormation 스택을 삭제합니다.




