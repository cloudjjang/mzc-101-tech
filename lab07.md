# 실습 : Introduction to Amazon Virtual Private Cloud (VPC)

## LAB Overview:

이 실습에서는 Amazon VPC (Amazon Virtual Private Cloud)에 대해 소개합니다. 이 실습에서는 아마존 VPC 마법사를 사용하여 VPC를 만들고, 인터넷 게이트웨이를 연결하고, 서브넷을 추가 한 다음 트래픽이 서브넷과 인터넷 게이트웨이간에 흐르도록 VPC 라우팅을 정의합니다.


## Topics Covered:

이 LAB에서는 다음의 내용을 학습합니다.

* VPC 마법사를 사용하여 Amazon VPC 만들기 
* VPC의 기본 구성 요소는 다음과 같습니다. 
* 공용 및 개인 서브넷
* 경로표 및 경로
* NAT 게이트웨이
* Network ACL
* Elastic IP

**Amazon Virtual Private Cloud (VPC) 란 무엇입니까?**\
Amazon VPC (Amazon Virtual Private Cloud)를 사용하면 정의한 가상 네트워크에서 AWS 리소스를 시작할 수 있는 AWS(Amazon Web Services) 클라우드의 논리적으로 격리된 섹션을 프로비저닝 할 수 있습니다. 자체 IP 주소 범위 선택, 서브넷 작성 및 라우트 테이블 및 네트워크 게이트웨이 구성을 포함하여 가상 네트워킹 환경을 완벽하게 제어 할 수 있습니다. VPC에서 IPv4 및 IPv6을 모두 사용하여 리소스 및 응용 프로그램에 안전하고 쉽게 액세스 할 수 있습니다.

<br>

## Start Lab:

* 크롬 웹 브라우저를 실행합니다.
* 주소창에 다음 주소를 입력합니다. https://aws.amazon.com 
* 우측 상단에 ![image](https://user-images.githubusercontent.com/48195985/61857120-789d1f00-aefe-11e9-978d-2d8e385ce3af.png)을 누르고 자신의 계정으로 로그인 합니다.
* Region은 오레콘(Oregon)을 선택합니다.

<br>

## 작업 1 : 탄력적 IP 주소 만들기

VPC는 개인 자원에 대한 인터넷 액세스를 제공하기 위해 NAT 게이트웨이를 시작합니다. NAT 게이트웨이에는 “탄력적 IP” 주소라고 하는 고정 IP 주소가 할당됩니다. 이 작업에서는 “탄력적 IP” 주소를 만듭니다.

![image](https://user-images.githubusercontent.com/48195985/61857159-8652a480-aefe-11e9-847c-74757c695529.png)


탄성 IP 주소는 공용 IPv4 주소로, 인터넷에서 연결할 수 있습니다. 그것은 정적 IP 주소로 IP 주소를 변경하지 않을 것을 의미한다. 탄력적 IP 주소를 VPC의 리소스(예:NAT 게이트웨이 또는 Amazon EC2 인스턴스)와 연결할 수 있습니다. 탄력적 IP 주소를 AWS로 다시 릴리스하기 전까지 Elastic IP 주소를 계속 제어 할 수 있습니다.


**1. AWS 관리 콘솔에서 ![image](https://user-images.githubusercontent.com/48195985/61857181-923e6680-aefe-11e9-891a-c45397e42396.png)를 클릭하고 VPC를 클릭합니다.**

**2. 왼쪽 탐색창에서 “탄력적 IP”를 클릭합니다.**

**3. ![image](https://user-images.githubusercontent.com/48195985/61857217-a3877300-aefe-11e9-9f40-d9effb249222.png)
을 클릭합니다.**

**4. 새 주소 할당에서 기본값으로 ![image](https://user-images.githubusercontent.com/48195985/61857235-b00bcb80-aefe-11e9-9522-86b7f264cf3f.png)
을 클릭합니다.**
> 할당된 탄력적 IP 주소가 표시됩니다. 다음 작업에서 사용하게 됩니다.\
![image](https://user-images.githubusercontent.com/48195985/61857258-be59e780-aefe-11e9-90ff-8b3529d666f9.png)

**5. ![image](https://user-images.githubusercontent.com/48195985/61857299-d29de480-aefe-11e9-97fa-53057c569b6e.png)를 클릭합니다.**

<br>

## 작업 2 : Amazon VPC 만들기

이 작업에서는 VPC 마법사를 사용하여 Amazon VPC를 만듭니다. 마법사는 지정한 매개 변수를 기반으로 VPC를 자동으로 만듭니다. VPC 마법사를 사용하면 VPC의 각 구성 요소를 수동으로 만드는 것보다 훨씬 간단합니다.

다음은 만드는 VPC의 개요입니다.

![image](https://user-images.githubusercontent.com/48195985/61857357-fcefa200-aefe-11e9-9206-55c0ba35bcb6.png)

각 구성 요소는 본 실습의 뒷부분에서 자세히 설명합니다.

**6. 화면의 왼쪽 상단 모서리에 “VPC 대시보드”를 클릭합니다.**

**7. ![image](https://user-images.githubusercontent.com/48195985/61857414-142e8f80-aeff-11e9-827e-ed5b0135388f.png)
을 클릭합니다.**

> 마법사는 미리 정의 된 네 가지 구성을 제공합니다. 마법사에서 각 옵션을 클릭하여 정의를 봅니다.
> * 단일 퍼블릭 서브넷이 있는 VPC 
> * 퍼블릭 및 프라이빗 서브넷이 있는 VPC 
> * 퍼블릭 및 프라이빗 서브넷 이 있고 하드웨어 VPN 액세스를 제공하는 VPC
> * 프라이빗 서브넷만 잇고 하드웨어 VPN 액세스를 제공하는 VPC 


이 랩에서 “퍼블릭 및 프라이빗 서브넷이 있는 VPC”를 사용합니다 .

**8. 1단계 :VPC 구성 선택에서 “퍼블릭 및 프라이빗 서브넷이 있는 VPC”를 클릭합니다.**

![image](https://user-images.githubusercontent.com/48195985/61857493-3f18e380-aeff-11e9-9445-202bf95465b1.png)


**9. ![image](https://user-images.githubusercontent.com/48195985/61857554-6079cf80-aeff-11e9-9f58-3c4ef647afc0.png)을 클릭합니다.**

**10. 2단계 : 퍼블릭 및 프라이빗 서브넷이 있는 VPC 에서 다음 항목을 설정합니다.**
* VPC이름 : My VPC
* 퍼블릭 서브넷의 IPv4 CIDR：10.0.25.0/24
* 가용영역 : 첫 번째 가용역영 선택
* 퍼블릭 서브넷 이름 : Public Subnet
* 프라이빗 서브넷의 IPv4 CIDR : 10.0.50.0/24
* 가용 영역 : 첫 번째 가용영역 선택
* 프라이빗 서브넷 이름 : Private Subnet
* 탄력적 IP 할당 ID : 박스를 클릭하고 이전에 생성한 탄력적 IP를 선택한다.

![image](https://user-images.githubusercontent.com/48195985/61857635-7be4da80-aeff-11e9-8bba-1c77872e85c4.png)


**11. ![image](https://user-images.githubusercontent.com/48195985/61857656-87380600-aeff-11e9-87ea-60a77c766b24.png)
을 클릭합니다.**
> 이제 VPC가 생성됩니다. 상태 창에 진행 상태가 표시됩니다. VPC가 완료되면 상태 창에서 VPC가 성공적으로 만들어졌음을 확인합니다. 생성하는 데 몇 분이 걸릴 수 있습니다.

**12. ![image](https://user-images.githubusercontent.com/48195985/61857691-9454f500-aeff-11e9-8d02-41f34687431f.png)
을 클릭하여 상태 창을 닫고 VPC 대시보드로 돌아갑니다.**

<br>

## 작업 3 : VPC 탐색하기

이 작업에서는 VPC 마법사로 만든 VPC 구성요소를 탐색합니다.

**13. 왼쪽 상단의 “VPC별로 필터링: 박스를 클릭하고 ”My VPC“를 선택합니다.
이렇게 하면 필터링한 VPC와 관련된 구성요소만 표시되도록 콘솔 디스플레이가 제한됩니다.**

**14. 만약 VPC 필터링 항목에 My VPC가 표시되지 않으면 ”서비스“ 메뉴에 ”VPC“를 다시 선택합니다.**

**15. 왼쪽 탐색창에서 ”인터넷 게이트웨이“를 클릭합니다.**
> My VPC용 인터넷 게이트웨이가 표시됩니다.

> 인터넷 게이트웨이가 VPC를 인터넷에 연결합니다. 인터넷 게이트웨이가 없으면 VPC는 인터넷에 연결 되지 않습니다.

> 인터넷 게이트웨이는 수평적으로 확장되고 중복되며 고가용성 VPC 구성 요소입니다. 따라서 네트워크 트래픽에 가용성 위험이나 대역폭 제약을 부과하지 않습니다.

**16. 왼쪽 탐색창에서 서브넷을 클릭합니다.**
> 서브넷은 VPC의 하위 집합입니다.
* 특정 VPC에 포함되어 있어야 합니다.
* 단일 가용영역에 존재합니다. (VPC는 여러 가용영역을 포함할 수 있습니다.)
* IP 주소 범위 (CIDR 주소 범위)
> VPC에 퍼블릭 서브넷과 프라이빗 서브넷이 표시됩니다.

**17. ![image](https://user-images.githubusercontent.com/48195985/61857746-afc00000-aeff-11e9-943b-f920d39cffcd.png)
을 선택하고 아래에 표시된 창에서 다음을 확인합니다.**
* 각 서브넷은 고유한 Subnet ID가 할당됩니다.
* IPv4 CIDR 주소범위는 10.0.25.0/24 가 할당되어 있습니다. 이는 10.0.25.0 ~ 10.0.25.255 범위의 주소를 포함합니다.
* 사용 가능한 IPv4 주소 항목에는 250개의 값이 표시되어 있습니다. (이는 각 서브넷에 예약된 주소 4개와 NAT 게이트웨이에 1개의 주소가 예약되어 사용되기 때문에 250개로 표시됩니다.)

**18. ![image](https://user-images.githubusercontent.com/48195985/61857770-b8183b00-aeff-11e9-961a-6554ac5f34b4.png)
탭을 클릭합니다.**
> 각 서브넷은 서브넷을 나가는 아웃바운드 트래픽의 경로를 지정하는 라우팅테이블과 연결됩니다. 목적지를 기반으로 트래픽을 보낼 곳을 나열하는 주소록과 같다고 생각하십시오.

> ![image](https://user-images.githubusercontent.com/48195985/61857782-bfd7df80-aeff-11e9-92a9-c88245dde302.png)


퍼블릿 서브넷과 연결된 라우팅테이블에는 2개의 경로가 있습니다.
* “10.0.0.0/16 | local”로 표시된 경로는 VPC내의 로컬 경로로 전달합니다.
* “0.0.0.0/0 | igw-”로 시작하는 경로는 모든 트래픽을 인터넷 게이트웨이로 보냅니다.
> 라우팅 규칙은 더 많은 주소를 지정하는 정보가 먼저 적용되므로 16bit가 지정된 10.0.*.*/16 주소가 먼저 적용되면 나머지 모든 주소는 0.0.0.0/0 주소를 적용받습니다.

> 이 서브넷에 Defatul 경로 (0.0.0.0/0)에 대한 정보로 인터넷 게이트웨이를 지정하고 있으므로 퍼블릭서브넷이 됩니다.

**19. 아래 탐색창에서 ![image](https://user-images.githubusercontent.com/48195985/61857821-d2eaaf80-aeff-11e9-9742-1948aadf4eba.png)
탭을 클릭합니다.**
> 네트워크 액세스 제어 목록 (ACL)은 서브넷 안팎으로 흐르는 트래픽을 제어하기 위한 방화벽 역할을 하는 VPC용 보안계층​​(선택 사항)입니다. 네트워크 ACL은 일반적으로 서브넷 안팎의 모든 트래픽을 허용하는 기본 설정으로 유지됩니다.
* 인바운드 규칙 아래 규칙# 100은 서브넷으로 인바운드 되는 모든 트래픽을 허용하는 정책입니다.
* 아웃바운드 규칙 아래 규칙# 100은 서브넷으로 아웃바운드 되는 모든 트래픽을 허용하는 정책입니다. 
* 각 Rule의 두 번째 줄에는 트래픽이 이전 규칙중 하나와 일치하지 않는 경우 모두 적용할 규칙으로 별표(*)가 표시됩니다.

> ![image](https://user-images.githubusercontent.com/48195985/61857844-dbdb8100-aeff-11e9-8eeb-03f0f346a283.png)


2**0. ![image](https://user-images.githubusercontent.com/48195985/61857872-e6961600-aeff-11e9-8391-fdc92ac680a5.png)탭을 클릭합니다.**
> 서브넷에 Public Subnet 값을 갖는 Name 키가 태그로 설정되어 있습니다. 태그는 AWS 리소스를 관리하고 식별하는데 도움이 됩니다.

**21. 창 상단에서 Public Subnet 선택을 지우고, Private Submet을 선택합니다.**

> (아래와 같이 표시됩니다.)\
> ![image](https://user-images.githubusercontent.com/48195985/61857885-ee55ba80-aeff-11e9-8a42-755fee7d9fda.png)


**22. ![image](https://user-images.githubusercontent.com/48195985/61857907-fd3c6d00-aeff-11e9-92ab-83521fb35493.png)
탭을 클릭합니다.**

> 프라이빗 서브넷의 라우팅테이블은 다음과 같이 구성되어 있습니다.
* “10.0.0.0/16 | local”은 퍼블릭 서브넷과 동일합니다.
* “0.0.0.0/0 | nat-”로 시작하는 경로는 NAT 게이트웨이로 트래픽을 보냅니다.
> 이 서브넷은 인터넷 게이트웨이로 보내는 경로가 없으므로 프라이빗 서브넷입니다.

> ![image](https://user-images.githubusercontent.com/48195985/61857919-075e6b80-af00-11e9-974e-385177e922e9.png)


**23. 왼쪽 탐색창에서 “NAT 게이트웨이”를 클릭합니다.**
> NAT 게이트웨이를 표시합니다.

> NAT (Network Address Translation) 게이트웨이를 사용하면 프라이빗 서브넷의 리소스가 인터넷 및 VPC 외부의 다른 리소스에 연결할 수 있습니다. 이것은 아웃바운드 전용연결입니다. 즉, 연결은 프라이빗서브넷 내에서 시작되어야 합니다. 인터넷 리소스가 인바운드 연결을 할 수 없습니다. 따라서 리소스를 비공개로 유지하고 VPC 리소스의 보안을 향상시키는 수단입니다.

> ![image](https://user-images.githubusercontent.com/48195985/61857936-10e7d380-af00-11e9-96c6-127d86546026.png)


**24. 왼쪽 탐색창에서 “보안그룹‘을 클릭합니다.**

**25. 보안그룹이 표시되면 아래창에 ![image](https://user-images.githubusercontent.com/48195985/61857950-1b09d200-af00-11e9-9649-a7addd1ddc96.png)
탭을 클릭합니다.**

> 보안그룹은 인바운드 및 아웃바운드 트래픽을 제어하기 위해 인스턴스에 대한 가상 방화벽 역할을 합니다. Amazon EC2 인스턴스를 VPC로 실행하면 최대 5개의 보안그룹을 인스턴스에 할당 할 수 있습니다. 보안그룹은 서브넷 수준이 아닌 인스턴스 수준에서 작동합니다. VPC에는 자동으로 기본 보안그룹이 제공됩니다. Amazon EC2 인스턴스를 시작할 때 다른 보안그룹을 지정하지 않으면 기본 보안그룹이 사용됩니다.

> 기본 보안그룹은 모든 트래픽이 연결된 리소스에 액세스 할 수 있도록 허용하지만 원본이 기본 보안그룹 인 경우에만 허용됩니다. 이 자체 참조는 이상하게 보일 수 있지만 이 구성은 기본 보안그룹과 연결된 모든 EC2 인스턴스가 기본 보안그룹과 연결된 다른 EC2 인스턴스와 통신 할 수 있다는 것을 의미합니다. 다른 모든 트래픽은 거부됩니다. 이는 다른 리소스의 액세스를 제한하기 때문에 매우 안전한 기본 설정입니다.

> VPC에 리소스를 추가 할 때 웹 서버, 응용 프로그램 서버 및 데이터베이스 서버와 같은 리소스에 대한 원하는 액세스를 허용하는 추가 보안그룹을 만들 수 있습니다.

> 이 실습에서 Amazon EC2 인스턴스를 시작하는 것은 실습 범위 밖에 있습니다. Amazon EC2 인스턴스를 시작하지 마십시오. 이 실습에서는 EC2 인스턴스를 시작할 수 없습니다.

<br>
<br>

## <span style="color:red"> 실습을 종료합니다. </span>
